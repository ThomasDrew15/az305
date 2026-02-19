# Azure Monitor Log Tables: What Each Captures

## Summary Table

| Log Table | Source | What It Captures | Relevance to Security Events Report |
|-----------|--------|------------------|--------------------------------------|
| **Syslog** | Linux servers | Guest OS logs: authentication, system events, security alerts, kernel messages | ✓ YES - Contains Linux server security events |
| **Event** | Windows servers | Guest OS logs: Windows Event Log data (System, Security, Application events) | ✓ YES - Contains Windows server security events |
| **AzureDiagnostics** (Resource Log) | Azure services | Diagnostic logs from Azure resources (Key Vault, Storage, SQL, etc.) | ✗ NO - Not about server security events |
| **AzureActivity** (Activity Log) | Azure control plane | Who did what in Azure (deployments, role assignments, resource changes) | ✗ NO - Not about server security events |

---

## Detailed Breakdown

### Syslog Table
**Source:** Linux operating system (guest OS)

**What it captures:**
- Authentication attempts (successful and failed logins)
- User account management
- System startup/shutdown events
- Service start/stop events
- Security alerts
- Kernel messages
- Sudo command execution
- SSH connection attempts

**Example log entry:**
```
TimeGenerated: 2024-02-19T10:15:32Z
Computer: linux-vm-01
Facility: auth
SeverityLevel: Warning
SyslogMessage: Failed password for user john from 192.168.1.100 port 22 ssh2
```

**Relevance to question:** ✓ **REQUIRED** for Linux server security events

---

### Event Table
**Source:** Windows operating system (guest OS)

**What it captures:**
- Windows Event Log data
- System events (startup, shutdown, driver loading)
- Security events (failed logins, privilege escalation, object access)
- Application events
- User account changes
- Group policy application results

**Example log entry:**
```
TimeGenerated: 2024-02-19T10:15:32Z
Computer: windows-vm-01
EventID: 4625
Level: Error
RenderedDescription: An account failed to log on
SourceIP: 192.168.1.100
Account: DOMAIN\user
```

**Relevance to question:** ✓ **REQUIRED** for Windows server security events

---

### AzureDiagnostics (Resource Log)
**Source:** Azure service resources (NOT the servers themselves)

**What it captures:**
- Key Vault: secret access, key operations
- Storage Account: read/write operations, access attempts
- SQL Database: query execution, login attempts
- Application Gateway: request details
- API Management: API calls

**Example log entry:**
```
TimeGenerated: 2024-02-19T10:15:32Z
ResourceType: VAULTS
OperationName: SecretGet
ResultSignature: 200 OK
Identity: user@contoso.com
```

**Relevance to question:** ✗ **NOT NEEDED** - This is about Azure services, not server security events

---

### AzureActivity (Activity Log)
**Source:** Azure control plane (management operations)

**What it captures:**
- Who deployed a VM
- Who changed a storage account's firewall
- Who assigned RBAC roles
- Who created a virtual network
- Who modified a policy
- Resource creation/deletion/modification at Azure level

**Example log entry:**
```
TimeGenerated: 2024-02-19T10:15:32Z
OperationName: Microsoft.Compute/virtualMachines/write
Caller: admin@contoso.com
ResourceType: Microsoft.Compute/virtualMachines
Status: Succeeded
```

**Relevance to question:** ✗ **NOT NEEDED** - This is about who managed Azure resources, not what happened inside the servers

---

## Key Distinction: Guest OS vs Azure Control Plane

```
┌─────────────────────────────────────────────────────────────┐
│                    Your Windows VM                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌──────────────────────────────────────────────────────┐  │
│  │         GUEST OPERATING SYSTEM (Inside VM)           │  │
│  │  ✓ Security events (failed logins, privilege change) │  │
│  │  ✓ User account changes                              │  │
│  │  ✓ Service start/stop                                │  │
│  │  → Captured in: EVENT table                          │  │
│  └──────────────────────────────────────────────────────┘  │
│                                                              │
│  Monitored by: Azure Monitor Agent (installed on VM)        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
              ↓
        ┌─────────────┐
        │  Log        │
        │ Analytics   │
        │ Workspace   │
        └─────────────┘


┌─────────────────────────────────────────────────────────────┐
│               AZURE CONTROL PLANE (Outside VM)              │
├─────────────────────────────────────────────────────────────┤
│  Who deployed this VM?                                       │
│  Who changed the VM size?                                    │
│  Who assigned RBAC roles?                                    │
│  → Captured in: ACTIVITY LOG table                          │
│                                                              │
│  NOT related to what happens inside the VM                  │
└─────────────────────────────────────────────────────────────┘
```

---

## Question Analysis

**Scenario:** "Create a report about security-related events using Azure Monitor Logs"
- You have 100 Windows and Linux servers
- Azure Monitor Agent is installed on each

**What security events are you looking for?**
- Failed login attempts (in the servers)
- User account changes (in the servers)
- Privilege escalation attempts (in the servers)
- Process execution (in the servers)

**NOT:**
- Who deployed the VM (that's Activity Log)
- Who assigned roles (that's Activity Log)
- What happened to Azure resources like Key Vault (that's Resource Log)

---

## The Correct Answer

**Syslog** and **Event** tables

**Why:**
- **Event** table = Windows guest OS security events
- **Syslog** table = Linux guest OS security events
- Together, they cover all 100 servers (Windows + Linux) for security-related events

**Why NOT the others:**
- **AzureDiagnostics:** Not about server events, about Azure service diagnostics
- **AzureActivity:** Not about server events, about Azure management operations
