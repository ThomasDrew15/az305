# AZ‑305 Azure Services Cheat Sheet

---

## 1. Identity, Governance & Security

### Identity & Access Management

| Service | Purpose |
|---|---|
| Entra ID | Identity & access management for users, apps, devices |
| Conditional Access | Enforce MFA, location/device-based access |
| Privileged Identity Management (PIM) | Just‑in‑time admin roles |
| Identity Protection | Risk-based sign‑in detection |
| Managed Identities | Identity for apps without secrets |

### Governance

| Service | Purpose |
|---|---|
| Management Groups | Organise subscriptions |
| Azure Policy | Enforce rules (e.g., allowed locations, SKUs) |
| Blueprints | Deploy governance packages |
| Cost Management + Budgets | Track and control spend |
| Tags | Metadata for resources |

### Security

| Service | Purpose |
|---|---|
| Key Vault | Secrets, keys, certificates |
| Defender for Cloud | Security posture management |
| Azure Firewall | Network-level filtering |
| NSG / ASG | Subnet/VM traffic rules |
| DDoS Protection | Network-level protection |

---

## 2. Compute Services

| Service | Use Case |
|---|---|
| Virtual Machines | Full control, legacy workloads |
| VM Scale Sets | Auto-scaling VMs |
| Azure App Service | Web apps, APIs, managed platform |
| Azure Functions | Serverless compute |
| Azure Kubernetes Service (AKS) | Container orchestration |
| Azure Container Apps | Serverless containers |
| Azure Batch | HPC and parallel workloads |

---

## 3. Storage & Data Services

### Storage

| Service | Purpose |
|---|---|
| Blob Storage | Unstructured data, data lakes |
| File Shares (SMB/NFS) | Lift‑and‑shift file servers |
| Queue Storage | Lightweight messaging |
| Table Storage | NoSQL key-value store |
| Disk Storage | VM disks |

### Databases

| Service | Purpose |
|---|---|
| Azure SQL Database | Managed SQL |
| SQL Managed Instance | Near 100% SQL Server compatibility |
| Cosmos DB | Global NoSQL, multi‑model |
| Azure Database for PostgreSQL/MySQL | Managed open-source DBs |
| Azure Synapse Analytics | Data warehousing & analytics |
| Azure Cache for Redis | Low‑latency caching |

---

## 4. Networking Services

| Service | Purpose |
|---|---|
| Virtual Network (VNet) | Private network boundary |
| Subnets | Network segmentation |
| VNet Peering | Connect VNets |
| VPN Gateway | Site‑to‑site or point‑to‑site VPN |
| ExpressRoute | Private dedicated connection |
| Application Gateway (WAF) | Layer 7 load balancing + WAF |
| Load Balancer | Layer 4 load balancing |
| Traffic Manager | DNS-based global routing |
| Front Door | Global CDN + WAF + routing |
| Private Link / Endpoints | Private access to PaaS services |
| Service Endpoints | Secure PaaS access via VNet |

---

## 5. Business Continuity & Disaster Recovery

| Service | Purpose |
|---|---|
| Azure Backup | VM, SQL, file backup |
| Site Recovery (ASR) | DR for VMs and servers |
| Availability Sets | Protect from hardware failure |
| Availability Zones | Datacenter-level redundancy |
| Geo‑redundant Storage (GRS) | Cross‑region replication |

---

## 6. Integration & Messaging

| Service | Purpose |
|---|---|
| Service Bus | Enterprise messaging, queues, topics |
| Event Grid | Event routing |
| Event Hubs | Big data ingestion |
| Logic Apps | Workflow automation |
| API Management | API gateway, throttling, security |

---

## 7. Monitoring & Management

| Service | Purpose |
|---|---|
| Azure Monitor | Metrics + logs |
| Log Analytics Workspace | Central log store |
| Application Insights | App performance monitoring |
| Alerts & Action Groups | Automated notifications |
| Automation Account | Runbooks, patching |
| Azure Advisor | Recommendations |

---

## 8. What You Must Know for AZ‑305

### Design Principles

- High availability vs disaster recovery
- Cost optimisation
- Security by design
- Zero trust
- Scalability (vertical vs horizontal)
- Multi‑region architecture

### Typical Exam Scenarios

- Choosing between App Service, AKS, Functions
- Designing hybrid networks (VPN vs ExpressRoute)
- Selecting the right database for a workload
- Designing identity and access strategy
- Securing PaaS services with Private Link
- Designing backup and DR plans
- Architecting multi‑tier applications
