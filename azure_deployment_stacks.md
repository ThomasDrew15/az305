# Azure Deployment Stacks - Complete Reference

## Overview

An Azure deployment stack is a resource that enables management of a group of Azure resources as a single, cohesive unit. It's a superset of traditional ARM template deployments with additional lifecycle and governance capabilities.

**Resource Type**: `Microsoft.Resources/deploymentStacks`

**Requirements**: Azure PowerShell 12.0.0+ or Azure CLI 2.61.0+

## Why Use Deployment Stacks?

- Streamlined provisioning and management across different scopes as a unified entity
- Prevention of undesired modifications via deny settings
- Efficient environment cleanup using delete flags during updates
- Infrastructure-as-code (IaC) pattern with automatic resource tracking
- Built-in protection against accidental deletions and modifications

## Scope Levels

Deployment stacks operate at three hierarchical levels:

### Resource Group Scope
- Stack exists in a resource group
- Manages resources in the same resource group
- Most granular control level

### Subscription Scope
- Stack exists at subscription level
- Can deploy resources to specific resource groups OR subscription scope
- Separates stack management from developer resource visibility
- Better for governance and RBAC isolation

### Management Group Scope
- Stack exists at management group level
- Can deploy resources to child subscriptions
- Highest level of organizational control
- Limited portal support currently

**Key Pattern**: Place stacks at parent scope while managing child-level resources. For example, subscription-level stack managing resource group resources prevents developers from accidentally modifying the stack itself.

## Deployment Modes

Stacks can be created and updated using the same commands:
- **New** command: Creates stack or overwrites existing (with confirmation)
- **Set** command (PowerShell only): Updates stack explicitly
- **Create** command (CLI): Works for both create and update operations

## ActionOnUnmanage Behavior

Controls what happens to resources when they're removed from the template during stack update or deletion:

### detachAll (Default)
- Resources and resource groups remain in Azure
- Stack no longer manages or tracks them
- Safest option for non-ephemeral infrastructure

### deleteResources
- Deletes only the resources
- Resource groups remain intact
- Useful when cleaning up specific components

### deleteAll
- Deletes both managed resources AND their resource groups
- Most aggressive cleanup option
- **Warning**: Permanently deletes all contained resources
- Ideal for temporary test environments

**Example Use Case**: Provision test VMs across multiple resource groups for a project, then delete everything with a single stack update using `deleteAll`.

## Deny Settings (Protection)

Deployment stacks can apply deny assignments to protect managed resources from unauthorized changes. This adds security beyond RBAC.

### DenySettingsMode Options

**None**
- No restrictions applied
- Default value
- Users with appropriate RBAC can modify resources

**DenyDelete**
- Prevents deletion of managed resources
- Users can still read and modify
- Useful for production infrastructure

**DenyWriteAndDelete**
- Prevents both modifications and deletions
- Users can only read resources
- Strongest protection level

### Deny Settings Customization

**DenySettingsApplyToChildScopes**
- Extends deny settings to child resources
- Example: Deny on SQL Server also affects SQL Databases
- Prevents adding new child resources when enabled

**DenySettingsExcludedActions** (max 200)
- List of specific RBAC operations exempt from deny rules
- Example: Allow `Microsoft.Compute/virtualMachines/write` even with DenyWriteAndDelete
- Provides granular exceptions

**DenySettingsExcludedPrincipal** (max 5)
- Specific Microsoft Entra users/groups exempt from deny rules
- Useful for break-glass emergency access
- Object IDs required

**Control Plane Only**
- Deny settings only protect control plane operations (management.azure.com)
- Do NOT restrict data plane operations
- Example: Storage account deny still allows data deletion via blob endpoints

### Permission Requirements

Creating/updating stacks with deny settings requires:
- `Microsoft.Resources/deploymentStacks/manageDenySetting/action` permission
- Enforced via new **Azure Deployment Stack Owner** role

## Built-in Roles

### Azure Deployment Stack Contributor
- Can create, update, delete deployment stacks
- Cannot create or manage deny-assignments
- Cannot change existing deny settings
- Suitable for development teams

### Azure Deployment Stack Owner
- Full permissions including deny-assignment management
- Can create, update, and delete stacks with deny settings
- Can modify existing deny configurations
- Suitable for infrastructure/security teams

## Managed Resources

### Explicit vs Implicit Resources

**Explicitly Managed** (defined in Bicep/template):
- Resources you define are tracked and managed
- Subject to deny settings
- Can be deleted/detached via ActionOnUnmanage

**Implicitly Created** (automatic child resources):
- Resources created automatically by parent resources
- Example: VMs created by AKS cluster
- NOT tracked or managed by stack
- NOT affected by deny settings
- Cannot be deleted via ActionOnUnmanage

### Viewing Managed Resources

PowerShell:
```powershell
(Get-AzResourceGroupDeploymentStack -Name "myStack" -ResourceGroupName "myRg").Resources
(Get-AzSubscriptionDeploymentStack -Name "myStack").Resources
(Get-AzManagementGroupDeploymentStack -Name "myStack" -ManagementGroupId "mgId").Resources
```

CLI:
```bash
az stack group list --name 'myStack' --resource-group 'myRg' --output 'json'
az stack sub show --name 'myStack' --output 'json'
az stack mg show --name 'myStack' --management-group-id 'mgId' --output 'json'
```

Portal:
- Navigate to Deployment stacks section
- Select stack to view managed resources list

## Creation Examples

### Resource Group Scope

PowerShell:
```powershell
New-AzResourceGroupDeploymentStack `
  -Name "myStack" `
  -ResourceGroupName "myRg" `
  -TemplateFile "template.bicep" `
  -ActionOnUnmanage "detachAll" `
  -DenySettingsMode "none"
```

CLI:
```bash
az stack group create \
  --name 'myStack' \
  --resource-group 'myRg' \
  --template-file 'template.bicep' \
  --action-on-unmanage 'detachAll' \
  --deny-settings-mode 'none'
```

### Subscription Scope

PowerShell:
```powershell
New-AzSubscriptionDeploymentStack `
  -Name "myStack" `
  -Location "eastus" `
  -TemplateFile "template.bicep" `
  -DeploymentResourceGroupName "myRg" `
  -ActionOnUnmanage "detachAll" `
  -DenySettingsMode "none"
```

CLI:
```bash
az stack sub create \
  --name 'myStack' \
  --location 'eastus' \
  --template-file 'template.bicep' \
  --deployment-resource-group 'myRg' \
  --action-on-unmanage 'detachAll' \
  --deny-settings-mode 'none'
```

### Management Group Scope

PowerShell:
```powershell
New-AzManagementGroupDeploymentStack `
  -Name "myStack" `
  -Location "eastus" `
  -TemplateFile "template.bicep" `
  -DeploymentSubscriptionId "subId" `
  -ActionOnUnmanage "detachAll" `
  -DenySettingsMode "none"
```

CLI:
```bash
az stack mg create \
  --name 'myStack' \
  --location 'eastus' \
  --template-file 'template.bicep' \
  --deployment-subscription 'subId' \
  --action-on-unmanage 'detachAll' \
  --deny-settings-mode 'none'
```

## Updating Stacks

### Standard Update Flow

1. Modify the Bicep template (add/remove/change resources)
2. Run create or set command with updated template
3. ActionOnUnmanage controls behavior for removed resources

PowerShell:
```powershell
Set-AzResourceGroupDeploymentStack `
  -Name "myStack" `
  -ResourceGroupName "myRg" `
  -TemplateFile "template.bicep" `
  -ActionOnUnmanage "detachAll"
```

CLI (uses create, no separate set):
```bash
az stack group create \
  --name 'myStack' \
  --resource-group 'myRg' \
  --template-file 'template.bicep' \
  --action-on-unmanage 'detachAll'
```

### With Deny Settings

PowerShell:
```powershell
New-AzResourceGroupDeploymentStack `
  -Name "myStack" `
  -ResourceGroupName "myRg" `
  -TemplateFile "template.bicep" `
  -ActionOnUnmanage "detachAll" `
  -DenySettingsMode "denyDelete" `
  -DenySettingsExcludedAction "Microsoft.Compute/virtualMachines/write" `
  -DenySettingsExcludedPrincipal "objectId1,objectId2"
```

CLI:
```bash
az stack group create \
  --name 'myStack' \
  --resource-group 'myRg' \
  --template-file 'template.bicep' \
  --action-on-unmanage 'detachAll' \
  --deny-settings-mode 'denyDelete' \
  --deny-settings-excluded-actions 'Microsoft.Compute/virtualMachines/write' \
  --deny-settings-excluded-principals 'objectId1 objectId2'
```

## Stack Out-of-Sync Errors

**Error Message**:
```
The deployment stack '{stack-name}' might not have an accurate list of managed resources. 
To prevent resources from being accidentally deleted, check that the managed resource list 
doesn't have any additional values.
```

**Causes**:
- External modifications to resources (outside stack)
- Portal or direct API changes to managed resources
- Sync mismatch between stack state and actual resources

**Resolution**:
1. View managed resources from portal or CLI
2. Verify list is correct and hasn't been accidentally modified
3. Rerun command with `BypassStackOutOfSyncError` flag (PowerShell) or `bypass-stack-out-of-sync-error` (CLI)
4. Only use bypass flag after thoroughly reviewing resource list

PowerShell:
```powershell
Set-AzResourceGroupDeploymentStack `
  -Name "myStack" `
  -ResourceGroupName "myRg" `
  -TemplateFile "template.bicep" `
  -BypassStackOutOfSyncError
```

CLI:
```bash
az stack group create \
  --name 'myStack' \
  --resource-group 'myRg' \
  --template-file 'template.bicep' \
  --bypass-stack-out-of-sync-error
```

## Deletion Workflow

### Before Deletion Decision

1. Review which resources will be affected
2. Choose appropriate ActionOnUnmanage flag
3. Verify no critical data will be lost

### Delete Operations

**Resource Group Scope**

PowerShell:
```powershell
Remove-AzResourceGroupDeploymentStack `
  -Name "myStack" `
  -ResourceGroupName "myRg" `
  -ActionOnUnmanage "deleteAll"  # or deleteResources, detachAll
```

CLI:
```bash
az stack group delete \
  --name 'myStack' \
  --resource-group 'myRg' \
  --action-on-unmanage 'deleteAll'
```

**Subscription Scope**

PowerShell:
```powershell
Remove-AzSubscriptionDeploymentStack `
  -Name "myStack" `
  -ActionOnUnmanage "deleteAll"
```

CLI:
```bash
az stack sub delete \
  --name 'myStack' \
  --action-on-unmanage 'deleteAll'
```

**Management Group Scope**

PowerShell:
```powershell
Remove-AzManagementGroupDeploymentStack `
  -Name "myStack" `
  -ManagementGroupId "mgId" `
  -ActionOnUnmanage "deleteAll"
```

CLI:
```bash
az stack mg delete \
  --name 'myStack' \
  --management-group-id 'mgId' \
  --action-on-unmanage 'deleteAll'
```

### Important Deletion Notes

- Even with `deleteAll`, unmanaged resources in the resource group prevent deletion
- Resource locks on any resource in the group block deletion
- Portal shows update behavior dialog for flag selection
- Subscription cancellation is NOT blocked by stacks

## Exporting Templates

Export managed resources as JSON for reference or migration.

PowerShell:
```powershell
Save-AzResourceGroupDeploymentStack -Name "myStack" -ResourceGroupName "myRg"
Save-AzSubscriptionDeploymentStack -Name "myStack"
Save-AzManagementGroupDeploymentStack -Name "myStack" -ManagementGroupId "mgId"
```

CLI:
```bash
az stack group export --name 'myStack' --resource-group 'myRg'
az stack sub export --name 'myStack'
az stack mg export --name 'myStack' --management-group-id 'mgId'
```

## Listing Stacks

**Resource Group Scope**

PowerShell:
```powershell
Get-AzResourceGroupDeploymentStack -ResourceGroupName "myRg"
```

CLI:
```bash
az stack group list --resource-group 'myRg'
```

**Subscription Scope**

PowerShell:
```powershell
Get-AzSubscriptionDeploymentStack
```

CLI:
```bash
az stack sub list
```

**Management Group Scope**

PowerShell:
```powershell
Get-AzManagementGroupDeploymentStack -ManagementGroupId "mgId"
```

CLI:
```bash
az stack mg list --management-group-id 'mgId'
```

## Limitations

- **800 stacks maximum** per single scope
- **2,000 deny assignments maximum** per scope
- **5 excluded principals maximum** in deny settings
- **200 excluded actions maximum** in deny settings
- Cannot manage implicitly created resources (no deny/cleanup for them)
- Cannot delete Key Vault secrets via stack deletion
- Deny assignments don't support tags
- Deny assignments not supported at management group scope (only subscription within MG stack)
- What-if deployment not yet supported
- Management group-scoped stack cannot deploy to another management group
- Azure portal support is limited (no GUI for all operations)
- Microsoft Graph provider doesn't support deployment stacks

## Known Issues

- Deleting resource groups bypasses deny-assignments (but locks still block deletion)
- Resource group deletion possible even with deny settings if no lock present
- Some template validation errors not clearly actionable in PowerShell/CLI
- Portal support incomplete for management group stacks

## Key Exam Points for AZ-305

**Governance & Lifecycle**
- Deployment stacks provide unified resource lifecycle management across multiple scopes
- Perfect for managing ephemeral environments (provision, update, cleanup in one workflow)
- ActionOnUnmanage enables automatic cleanup—critical for cost management

**Protection & Security**
- Deny settings add layer beyond RBAC—comprehensive protection strategy
- Stacks at parent scope manage child resources with developer isolation
- Perfect for enforcing guardrails without blocking legitimate operations

**IaC Pattern**
- Superset of traditional deployments with additional management capabilities
- Single source of truth for resource group (the Bicep template)
- Remove resource from template = automatic detach/delete based on settings

**Real-World Scenario**
- Provision multi-resource test environments with subscription-level stack
- Apply DenyDelete to prevent accidental changes
- Update stack to remove resources when testing complete
- ActionOnUnmanage handles cleanup automatically

**Scope Planning**
- Consider management lifecycle when deciding scope
- Resource group scope: Tight control, single RG only
- Subscription scope: Cross-RG management with isolation
- Management group scope: Enterprise-wide governance

## Comparison with Azure Resource Locks

| Feature | Locks | Deployment Stacks |
|---------|-------|-------------------|
| Scope | Any level | RG, Subscription, MG |
| Control Plane Only | Yes | Yes |
| Protection Type | Prevents delete/modify | Prevents delete/modify + lifecycle |
| Inheritance | Child resources inherit | Managed resources tracked |
| Cleanup | Manual deletion | Automatic with ActionOnUnmanage |
| RBAC Override | Yes | Yes |
| Cost Control | No | Yes (auto cleanup) |
| Use Case | Static protection | Lifecycle management |

Locks are simpler point protections; stacks are comprehensive lifecycle orchestration.
