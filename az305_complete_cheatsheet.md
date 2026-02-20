# AZ‑305 One‑Page Cheat Sheet (Use Case + Choose This, Not That)

---

## Identity, Governance & Security

### Entra ID

**Use case:** Central identity, SSO, MFA, Conditional Access.

**Choose this when:** You need cloud identity for users/apps.

**Not that:** Don't use AD DS unless you need legacy domain join.

### Managed Identities

**Use case:** Secure identity for apps without secrets.

**Choose this when:** Your app accesses Azure resources.

**Not that:** Don't store secrets in config files.

### Key Vault

**Use case:** Secrets, keys, certificates.

**Choose this when:** You need HSM or secret rotation.

**Not that:** Don't store secrets in App Service settings.

### Azure Policy

**Use case:** Enforce compliance at scale.

**Choose this when:** You need guardrails (locations, SKUs).

**Not that:** Don't rely on RBAC for compliance.

### Management Groups

**Use case:** Organise subscriptions.

**Choose this when:** You apply policy/RBAC across many subs.

**Not that:** Don't manage each subscription individually.

---

## Compute

### Virtual Machines

**Use case:** Full OS control, legacy workloads.

**Choose this when:** You need custom OS/networking.

**Not that:** Don't use VMs for simple web apps.

### VM Scale Sets

**Use case:** Auto-scaling VM workloads.

**Choose this when:** You need predictable VM scaling.

**Not that:** Don't use App Service if you need custom images.

### App Service

**Use case:** Managed web apps/APIs.

**Choose this when:** You want PaaS simplicity.

**Not that:** Don't use AKS unless you need orchestration.

### Azure Functions

**Use case:** Event-driven serverless compute.

**Choose this when:** You need scale-to-zero or triggers.

**Not that:** Don't use Functions for long-running jobs.

### AKS

**Use case:** Full Kubernetes orchestration.

**Choose this when:** You need microservices, sidecars, custom networking.

**Not that:** Don't use AKS for simple container hosting.

### Azure Container Apps

**Use case:** Serverless containers.

**Choose this when:** You want Dapr, scale-to-zero, no cluster mgmt.

**Not that:** Don't use AKS unless you need deep control.

---

## Storage & Databases

### Blob Storage

**Use case:** Unstructured data, data lakes.

**Choose this when:** You need cheap, scalable object storage.

**Not that:** Don't use Files for analytics.

### Azure Files

**Use case:** SMB/NFS file shares.

**Choose this when:** Lift-and-shift file servers.

**Not that:** Don't use Blob if you need SMB semantics.

### Cosmos DB

**Use case:** Global NoSQL, multi-region writes.

**Choose this when:** You need <10ms reads globally.

**Not that:** Don't use SQL DB for massive scale.

### Azure SQL Database

**Use case:** Managed relational DB.

**Choose this when:** You need modern SQL with minimal admin.

**Not that:** Don't use Managed Instance unless you need SQL Server features.

### SQL Managed Instance

**Use case:** Full SQL Server compatibility.

**Choose this when:** You need SQL Agent, cross-database queries.

**Not that:** Don't use SQL DB if you need SQL Server features.

### Azure Cache for Redis

**Use case:** Low-latency caching.

**Choose this when:** You need to reduce DB load.

**Not that:** Don't use Cosmos DB as a cache.

---

## Networking

### VNet

**Use case:** Private network boundary.

**Choose this when:** You need isolation and custom IP ranges.

**Not that:** Don't expose PaaS publicly if Private Link exists.

### VNet Peering

**Use case:** Low-latency VNet-to-VNet.

**Choose this when:** You need seamless Azure-to-Azure connectivity.

**Not that:** Don't use VPN Gateway for Azure-to-Azure.

### VPN Gateway

**Use case:** Site-to-site or point-to-site VPN.

**Choose this when:** You need encrypted internet connectivity.

**Not that:** Don't use ExpressRoute unless you need private circuits.

### ExpressRoute

**Use case:** Private dedicated connection.

**Choose this when:** You need predictable latency.

**Not that:** Don't use VPN for mission-critical workloads.

### Load Balancer

**Use case:** Layer 4 load balancing.

**Choose this when:** You need TCP/UDP balancing.

**Not that:** Don't use App Gateway for L4.

### Application Gateway (WAF)

**Use case:** Layer 7 + WAF.

**Choose this when:** You need URL routing, SSL termination.

**Not that:** Don't use Load Balancer for HTTP routing.

### Front Door

**Use case:** Global routing + CDN + WAF.

**Choose this when:** You need global acceleration.

**Not that:** Don't use Traffic Manager if you need CDN.

### Traffic Manager

**Use case:** DNS-based global routing.

**Choose this when:** You need geo-routing without CDN.

**Not that:** Don't use Front Door if you don't need edge acceleration.

### Private Link

**Use case:** Private access to PaaS.

**Choose this when:** You need traffic off the public internet.

**Not that:** Don't use Service Endpoints for strict isolation.

---

## Business Continuity & DR

### Azure Backup

**Use case:** Backup for VMs, SQL, files.

**Choose this when:** You need point-in-time restore.

**Not that:** Don't use ASR for backups.

### Azure Site Recovery

**Use case:** DR for VMs and servers.

**Choose this when:** You need region failover.

**Not that:** Don't use Backup for DR.

### Availability Zones

**Use case:** Datacenter-level redundancy.

**Choose this when:** You need high availability.

**Not that:** Don't rely on Availability Sets for zone-level resilience.

### Availability Sets

**Use case:** Hardware fault isolation.

**Choose this when:** You need FD/UD separation.

**Not that:** Don't use them for multi-datacenter redundancy.

---

## Integration & Messaging

### Service Bus

**Use case:** Enterprise messaging (queues, topics).

**Choose this when:** You need ordered, reliable, transactional messaging.

**Not that:** Don't use Event Grid for commands.

### Event Grid

**Use case:** Event routing.

**Choose this when:** You need lightweight pub/sub.

**Not that:** Don't use Service Bus for event notifications.

### Event Hubs

**Use case:** High-throughput ingestion.

**Choose this when:** You need millions of events/sec.

**Not that:** Don't use Service Bus for telemetry.

### Logic Apps

**Use case:** Workflow automation.

**Choose this when:** You need connectors to SaaS.

**Not that:** Don't use Functions if you need visual workflows.

### API Management

**Use case:** API gateway.

**Choose this when:** You need throttling, versioning, security.

**Not that:** Don't expose backend APIs directly.

---

## Flashcards (Exam-Style)

### Flashcard 1

**Q:** When should you choose Azure Functions?

**A:** Event-driven, serverless compute with scale-to-zero.

### Flashcard 2

**Q:** When should you choose Cosmos DB over SQL DB?

**A:** When you need global distribution and multi-region writes.

### Flashcard 3

**Q:** When should you choose Front Door?

**A:** When you need global routing + CDN + WAF.

### Flashcard 4

**Q:** When should you choose Private Link?

**A:** When you need private access to PaaS services.

### Flashcard 5

**Q:** When should you choose AKS?

**A:** When you need full Kubernetes control and microservices.

---

## Decision Trees (Choose This, Not That)

### Compute Decision Tree

```
Need full OS control?
├─ Yes → Virtual Machines
└─ No →
   Need container orchestration?
   ├─ Yes → AKS
   └─ No →
      Need serverless?
      ├─ Yes → Azure Functions
      └─ No →
         Need simple web hosting?
         ├─ Yes → App Service
         └─ No → Container Apps
```

### Database Decision Tree

```
Relational?
├─ Yes →
│  Need SQL Server features?
│  ├─ Yes → SQL Managed Instance
│  └─ No → Azure SQL Database
└─ No →
   Need global scale + low latency?
   ├─ Yes → Cosmos DB
   └─ No → Table Storage or Redis (cache)
```

### Networking Decision Tree

```
Need global routing?
├─ Yes →
│  Need CDN/WAF?
│  ├─ Yes → Front Door
│  └─ No → Traffic Manager
└─ No →
   Need HTTP routing?
   ├─ Yes → Application Gateway
   └─ No → Load Balancer
```

### Storage Decision Tree

```
Need SMB/NFS?
├─ Yes → Azure Files
└─ No →
   Need object storage?
   ├─ Yes → Blob Storage
   └─ No → Disk Storage
```
