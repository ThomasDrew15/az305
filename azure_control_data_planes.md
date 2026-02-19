# Azure Control Plane vs Data Plane - Complete Reference

## Fundamental Concept

Azure operations are divided into two distinct categories:

**Control Plane (Management Plane)** = Infrastructure management and lifecycle
**Data Plane (Operational Plane)** = Using and accessing resource capabilities

Think of it as the difference between managing a building (control plane) and living/working in it (data plane).

## Control Plane: The Management Layer

### Definition
The control plane is where you create, modify, configure, and delete Azure resources. It's the administrative decision-making layer handled by Azure Resource Manager (ARM).

### Single Endpoint
All control plane operations route through: **https://management.azure.com**

(Regional clouds have different URLs, but the pattern is consistent)

### Control Plane Operations (Examples)

**Resource Lifecycle**
- Creating a VM, storage account, SQL database, app service, key vault
- Modifying resource configurations (size, SKU, settings)
- Deleting resources
- Moving resources between resource groups or subscriptions

**Access & Security**
- Assigning RBAC roles
- Granting permissions to users/groups/service principals
- Creating or updating access policies

**Governance & Protection**
- Applying resource locks (CanNotDelete, ReadOnly)
- Configuring deny settings (from deployment stacks)
- Implementing Azure Policy
- Setting up Azure RBAC

**Metadata Management**
- Tagging resources
- Updating resource properties
- Configuring monitoring/diagnostics

**Deployment Operations**
- Deploying ARM templates
- Deploying Bicep files
- Creating deployment stacks
- Resource Manager operations through portal/CLI/PowerShell

### Control Plane Authentication
- Handled by **Azure Active Directory (Azure AD)**
- Permissions enforced via **Azure RBAC**
- Service principals, managed identities, and user accounts authenticate here

### Control Plane URL Pattern
```
PUT https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/{resourceProvider}/{resourceType}/{resourceName}?api-version={version}

Example (MySQL Database):
PUT https://management.azure.com/subscriptions/abc123/resourceGroups/myRg/providers/Microsoft.DBforMySQL/servers/myserver/databases/mydb?api-version=2017-12-01
```

### ARM Handles Control Plane Requests
When you send a control plane request, Azure Resource Manager automatically applies:
- Azure RBAC (role-based access control)
- Azure Policy (compliance and governance)
- Management locks (deletion/modification prevention)
- Activity logging (audit trail)
- Deny assignments (from deployment stacks)

## Data Plane: The Operational Layer

### Definition
The data plane is where you actually use and interact with resources after they're created. It's where the real work happens—the functional capabilities of deployed resources.

### Resource-Specific Endpoints
Each data plane operation goes to a resource-specific endpoint, NOT management.azure.com

Examples:
- Storage: `https://myaccount.blob.core.windows.net`
- SQL Database: `sqlserver.database.windows.net`
- Key Vault: `https://myvault.vault.azure.net`
- Cosmos DB: `https://mydb.documents.azure.com`

### Data Plane Operations (Examples)

**Storage Account**
- Reading/writing blobs (upload files, download files)
- Creating blob containers
- Managing queues and tables
- Accessing file shares

**Virtual Machine**
- RDP/SSH into the VM
- Running applications and services
- Installing software
- Managing operating system configurations

**SQL Database**
- Executing queries (SELECT, INSERT, UPDATE, DELETE)
- Creating tables and schemas
- Running stored procedures
- Managing database objects

**Key Vault**
- Reading secrets
- Retrieving certificates
- Managing cryptographic keys
- Using HSM operations

**Cosmos DB**
- Querying data (SQL API, MongoDB, Cassandra)
- Inserting/updating documents
- Running transactions

**Azure AI Services**
- Detect Language operation
- Translation operations
- Text analysis

**App Service**
- Deploying application code
- Accessing application endpoints
- Running background jobs

### Data Plane Authentication
- May use Azure AD, but not exclusively
- Often uses resource-specific credentials:
  - Storage account keys or SAS tokens
  - SQL database connection strings
  - Database server login credentials
  - API keys
  - Managed identities with appropriate data plane permissions

## Critical Architectural Distinction

### Independence of Planes

Control plane and data plane operate independently:

**Even if control plane (management.azure.com) is unavailable**, your data plane remains accessible. You can:
- Continue to access data in storage accounts via blob URLs
- Log into VMs and run applications
- Query databases
- Use App Service endpoints

**This is crucial for resilience**: Azure separates these concerns architecturally, ensuring infrastructure unavailability doesn't prevent you from using deployed services.

## Governance ONLY Covers Control Plane

This is the most important concept for AZ-305 exam and architecture design.

### What Governance Features Protect

- **Azure RBAC**: Controls who can perform control plane operations
- **Resource Locks**: Block deletion/modification of resource configuration
- **Azure Policy**: Enforces governance on control plane operations
- **Deployment Stack Deny Settings**: Prevents unauthorized control plane changes
- **Activity Logging**: Records control plane actions
- **Audit Logs**: Track management operations

### What Governance Features DO NOT Protect

Governance features do NOT restrict data plane operations because these are resource-specific and decoupled from management infrastructure.

### Real-World Examples

**Storage Account with ReadOnly Lock**
- ✓ Prevents: Deleting the storage account, modifying storage config
- ✓ Prevents: Assigning RBAC roles at storage account scope
- ✓ Prevents: Creating blob containers (control plane operation)
- ✗ Does NOT prevent: Reading blob data, Deleting blob data, Uploading blobs
- Why? Data operations go directly to blob endpoint, not management.azure.com

**SQL Database with DenyDelete Lock**
- ✓ Prevents: Deleting the SQL server or database
- ✓ Prevents: Modifying database configuration
- ✗ Does NOT prevent: Querying data, Deleting data from tables, Modifying data
- Why? Database queries are data plane operations

**Key Vault with DenyWriteAndDelete**
- ✓ Prevents: Deleting the key vault, Creating/updating secrets
- ✓ Prevents: Modifying key vault policies
- ✗ Does NOT prevent: Reading secrets if you have access
- Why? Secret retrieval is data plane

**Deployment Stack with DenyDelete**
- ✓ Prevents: Removing resources via stack update, Unmanaging resources
- ✗ Does NOT prevent: Modifying data within managed resources
- Why? Data modifications bypass control plane

### Security Implication

Governance is about infrastructure protection, not data protection. You need a layered approach:
- Control plane: Locks, policies, RBAC for infrastructure
- Data plane: Encryption, network controls, database security for actual data

## IaC and the Planes

### Infrastructure as Code Works on Control Plane

ARM templates, Bicep, Terraform, and Pulumi primarily operate on the control plane:
- Creating resources
- Configuring properties
- Setting up networks
- Establishing RBAC

### Data Plane Requires Separate Operations

IaC tools cannot (by design) populate your databases, load data into storage, or configure application-specific settings. You need separate steps:

**Bicep/ARM Template Example:**
```bicep
// Control Plane: Creates the database
resource database 'Microsoft.Sql/servers/databases@2019-06-01-preview' = {
  parent: server
  name: 'mydb'
  location: location
  sku: {
    name: 'Basic'
  }
}

// Data Plane: Requires separate pipeline step
// - Run SQL scripts via Azure DevOps/GitHub Actions
// - Use PowerShell to execute CREATE TABLE statements
// - Deploy application code that initializes database
```

### Tools and Planes

| Operation | Tool | Plane |
|-----------|------|-------|
| Create App Service | ARM/Bicep | Control |
| Deploy code to App Service | Azure DevOps/GitHub Actions | Data |
| Create SQL Database | ARM/Bicep | Control |
| Run SQL scripts | PowerShell/Azure DevOps | Data |
| Create Storage Account | ARM/Bicep | Control |
| Upload blobs | Azure Storage SDK/CLI | Data |
| Create Key Vault | ARM/Bicep | Control |
| Populate secrets | PowerShell/Azure CLI | Data |

## Common Confusion Points

### Misconception 1: "My resource is running, so everything is fine"
Control plane shows resource as "running" through portal, but data plane could have issues. A VM might be running but application inside is broken. Check both planes.

### Misconception 2: "Locks protect all my data"
Locks are control plane only. They don't prevent data deletion. Need encryption and other data plane security.

### Misconception 3: "RBAC controls everything"
RBAC is control plane. Someone with no RBAC permissions can still access data if they have direct credentials (storage key, connection string).

### Misconception 4: "Policy prevents data misuse"
Azure Policy enforces control plane compliance, not data governance. You need database-level policies and encryption for data.

## Authentication Summary

| Aspect | Control Plane | Data Plane |
|--------|---------------|-----------|
| Primary Auth | Azure AD | Varies (AD, keys, tokens, credentials) |
| Endpoint | management.azure.com | Resource-specific URLs |
| RBAC Applied | Yes | No (resource-specific auth) |
| Scope | All subscriptions/RGs | Resource-specific |
| Audit | Activity Log | Resource-specific logs |

## Cost Implications

### Control Plane Operations
- Generally minimal cost (some are free)
- Examples: RBAC assignments, policy evaluation, lock creation
- Occasionally billed per operation (deployment operations)

### Data Plane Operations
- Drive the majority of your Azure bill
- Storage transactions, compute usage, database queries
- Direct correlation to actual usage

**Optimization**: Control plane efficiency helps governance but impacts cost minimally. Data plane optimization (compression, caching, right-sizing) drives cost savings.

## Design Principles for AZ-305

### 1. Separate Concerns
Design solutions recognizing these are distinct architectural layers:
- Infrastructure layer (control plane)
- Application/data layer (data plane)

### 2. Layered Security
Don't rely solely on control plane governance:
- Governance: Locks, policies, RBAC
- Encryption: TDE, encryption at rest
- Network: Network security groups, firewalls
- Data access: Database-level permissions
- Auditing: Both control plane (Activity Log) and data plane (specific service logs)

### 3. Resilience Planning
Understand that control plane outages don't stop data access:
- Applications can continue serving data
- Plan recovery of infrastructure management separately

### 4. Monitoring Both Planes
- Control plane: Activity Log for infrastructure changes
- Data plane: Azure Monitor, application insights for usage and errors

### 5. Automation Considerations
- Use IaC for control plane (ARM, Bicep, Terraform)
- Use separate pipelines for data plane (scripts, SDKs, deployment tools)

## Exam Scenarios

### Scenario 1: Accidental Deletion Prevention
**Question**: How do you prevent accidental deletion of a production database?

**Answer involves both planes**:
- Control plane: Apply CanNotDelete lock or deployment stack deny setting
- Data plane: Enable automatic backups, point-in-time restore, read-only replicas

### Scenario 2: Governance Compliance
**Question**: Ensure developers can't modify production resources but can read data.

**Answer**:
- Control plane: RBAC role with read-only on resources
- Data plane: Grant read-only database access (SQL role, storage SAS with read-only)

### Scenario 3: Infrastructure as Code Pipeline
**Question**: Design IaC pipeline for application deployment.

**Answer includes both planes**:
- Control plane: ARM templates deploy infrastructure
- Data plane: Post-deployment scripts configure databases, load initial data

## Key Exam Points Summary

**Control Plane** (`https://management.azure.com`):
- Creates, modifies, deletes resources
- Handled by Azure Resource Manager
- Governed by RBAC, locks, policies
- Uses Azure AD authentication
- Provides audit trail via Activity Log

**Data Plane** (resource-specific endpoints):
- Uses resources for actual work
- Independent from control plane
- Not restricted by locks/policies
- May use resource-specific authentication
- Requires separate monitoring/logging

**Critical Relationship**:
- Governance protects infrastructure, not data
- Both planes needed for complete security
- Must handle both in IaC pipelines
- Independent resilience means separate failure scenarios

**Practical Impact**:
- A ReadOnly lock doesn't protect data
- Control plane outage ≠ data access lost
- IaC deploys infrastructure; scripts populate data
- RBAC doesn't control data access (only infrastructure)

## Common Service Examples

| Service | Control Plane | Data Plane |
|---------|---------------|-----------|
| **Storage Account** | Create/delete account | Read/write blobs |
| **SQL Database** | Create/modify database | Execute queries, insert/update data |
| **VM** | Create/resize VM | RDP/SSH, run applications |
| **Key Vault** | Create vault, set policies | Read secrets, create keys |
| **App Service** | Create/configure app | Deploy code, handle requests |
| **Cosmos DB** | Create database | Query/insert documents |
| **Function App** | Create function app | Execute functions |
