# Azure Resource Locks - Key Information

## Two Lock Types

- **CanNotDelete** (Delete in portal): Prevents deletion but allows reads and modifications
- **ReadOnly** (Read-only in portal): Prevents both deletion and updates—users can only read resources

## Scope & Inheritance

- Locks apply at subscription, resource group, or individual resource level
- Child resources inherit parent locks automatically
- Most restrictive lock in the inheritance chain takes precedence
- Locks do NOT prevent subscription cancellation

## Control Plane vs Data Plane

- Locks only protect **control plane operations** (management.azure.com)
- Do NOT restrict **data plane operations**
- Example: ReadOnly lock on SQL Database still allows data transactions
- This distinction matters significantly for services like storage accounts

## Common Gotchas

### Storage Account
- ReadOnly blocks listing keys (POST operation)
- Prevents RBAC assignments at that scope
- Prevents blob container creation
- Will NOT protect actual data deletion

### App Service
- ReadOnly blocks Visual Studio Server Explorer access
- Cannot scale up/out App Service Plan with ReadOnly on resource group

### Virtual Machines
- ReadOnly on resource group prevents VM restart operations (POST requests)
- Cannot start or restart VMs

### Deployment History
- CanNotDelete on resource group prevents automatic deployment history cleanup
- Can cause failures after 800 deployments

### Other Services
- ReadOnly on NSG prevents NSG flow log creation
- ReadOnly on Log Analytics prevents UEBA enablement
- ReadOnly on Application Gateway prevents backend health checks
- ReadOnly on AKS cluster limits portal access to cluster resources
- CanNotDelete on resource group prevents Azure RBAC assignment deletion

## Permissions Required

Only users with the following can create or delete locks:
- **Owner** role
- **User Access Administrator** role
- Custom roles with `Microsoft.Authorization/*` or `Microsoft.Authorization/locks/*` actions

## Configuration Methods

### Azure Portal
Settings → Locks → Add (provide name, level, optional notes)

### ARM Template / Bicep
```json
{
  "type": "Microsoft.Authorization/locks",
  "apiVersion": "2016-09-01",
  "name": "lockName",
  "properties": {
    "level": "CanNotDelete",
    "notes": "Optional notes"
  }
}
```

### PowerShell
```powershell
# Lock a resource
New-AzResourceLock -LockLevel CanNotDelete -LockName LockSite -ResourceName examplesite -ResourceType Microsoft.Web/sites -ResourceGroupName exampleresourcegroup

# Lock a resource group
New-AzResourceLock -LockName LockGroup -LockLevel CanNotDelete -ResourceGroupName exampleresourcegroup

# Get locks
Get-AzResourceLock

# Remove lock
Remove-AzResourceLock -LockId $lockId
```

### Azure CLI
```bash
# Lock a resource
az lock create --name LockSite --lock-type CanNotDelete --resource-group exampleresourcegroup --resource-name examplesite --resource-type Microsoft.Web/sites

# Lock a resource group
az lock create --name LockGroup --lock-type CanNotDelete --resource-group exampleresourcegroup

# List locks
az lock list

# Delete lock
az lock delete --ids $lockid
```

### REST API
```
PUT https://management.azure.com/{scope}/providers/Microsoft.Authorization/locks/{lock-name}?api-version=2016-09-01

Body:
{
  "properties": {
    "level": "CanNotDelete",
    "notes": "Optional text notes."
  }
}
```

## Key Exam Points for AZ-305

- Locks are a governance tool to prevent accidental infrastructure damage in production
- They work at control plane level only—data operations are unaffected
- Inheritance can create unexpected restrictions (most restrictive wins)
- ReadOnly has many non-obvious side effects (POST operations, RBAC assignments)
- CanNotDelete affects deployment history management at resource group level
- Different from RBAC—locks override all user permissions for those specific operations
- Essential for multi-tenant and enterprise governance strategies
