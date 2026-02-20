# AZ‑305 Network Infrastructure — Ultimate Cheat Sheet (Fully Updated)

Azure networking for AZ-305 boils down to five pillars:

- Connectivity (VNet Peering, VPN, ExpressRoute)
- Load Balancing & Global Routing
- Security & Access Control
- Private Access to PaaS (Private Link vs Service Endpoints)
- Network Architecture (Hub‑and‑Spoke, DNS, Firewalls)

Below is the complete, unified version.

---

## 1. Virtual Networks & Subnets

### Use When

- You need network isolation
- You need subnet segmentation
- You need custom routing (UDRs)
- You need custom DNS

### Exam Keywords

- "Subnet isolation"
- "Custom DNS"
- "User‑defined routes"

### Key Concepts

- **VNet** — Private network space with custom IP range
- **Subnets** — Segmentation within VNet
- **User-Defined Routes (UDRs)** — Custom routing rules
- **Network Security Groups (NSGs)** — Firewall at subnet/NIC level
- **Service Endpoints** — Restrict PaaS access via VNet

---

## 2. Connectivity Options

### VNet Peering

**Characteristics:**
- Private, fast, low‑latency
- Same or different regions
- Not transitive (exam trap!)

**Use for:** Hub‑and‑spoke, cross‑region private connectivity

**Key Point:** Traffic between peered VNets does NOT go through the internet

---

### VPN Gateway

**Characteristics:**
- IPSec tunnels
- S2S (Site-to-Site), P2S (Point-to-Site), VNet‑to‑VNet
- Cheaper, slower than ExpressRoute
- Goes over the internet

**Use for:** Hybrid connectivity for small/medium workloads

**Best for:** On-premises to Azure connectivity on a budget

---

### ExpressRoute

**Characteristics:**
- Private dedicated circuit (no internet)
- High throughput, low latency
- SLA‑backed
- No internet traversal
- More expensive than VPN

**Use for:** Enterprise hybrid connectivity, compliance-critical workloads

**Best for:** Mission-critical, high-bandwidth, predictable latency

---

## 3. Global Routing & Load Balancing (Updated)

### Azure Traffic Manager (DNS‑based Global Routing)

**Use When:**
- Failover between regions
- DR failover (with ASR)
- Simple global routing
- DNS‑level routing is acceptable
- No performance acceleration needed

**Think:** Global DNS routing + failover

**Key Characteristics:**
- DNS-level routing (not data path)
- Fast failover
- Geo-routing
- Weighted routing
- Performance routing

---

### Azure Front Door (Anycast Global Edge + Acceleration)

**Use When:**
- Improve global performance
- Reduce latency
- Route users to nearest POP
- Global WAF needed
- TLS offload required
- Global HTTP load balancing
- Edge caching needed

**Think:** Global performance + acceleration + smart routing

**Key Characteristics:**
- Anycast to edge POPs
- Global load balancing
- Built-in WAF
- Edge caching
- Performance acceleration
- Low latency to users

**vs Traffic Manager:**
- Front Door = Performance acceleration + smart routing
- Traffic Manager = DNS failover only

---

### Azure Application Gateway (Regional L7 Load Balancer)

**Use When:**
- Regional WAF
- URL‑based routing
- SSL termination
- Regional web load balancing
- Path-based routing
- Host-based routing

**Think:** Regional L7 load balancing + WAF

**Key Characteristics:**
- Layer 7 (Application layer)
- URL/path routing
- SSL termination
- Web app firewall
- Session affinity

---

### Azure Load Balancer (Regional L4 Load Balancer)

**Use When:**
- TCP/UDP load balancing
- High‑performance regional balancing
- Internal or external L4 load balancing
- Non-HTTP protocols

**Think:** Regional L4 load balancing

**Key Characteristics:**
- Layer 4 (Transport layer)
- Ultra-high performance
- TCP/UDP support
- Internal or public
- Low latency

---

## 4. Access & Security Controls (Updated)

### Azure Bastion

**Use When:**
- Secure VM access without public IPs
- Browser‑based RDP/SSH
- Reduce attack surface

**Important:** Bastion does NOT replace NSGs, JIT, firewalls—it supplements them

**Think:** Secure RDP/SSH without exposing VMs

**Key Benefits:**
- No public IP needed
- Browser-based access
- Centralized management
- Integrated with Azure Portal

---

### Just‑In‑Time (JIT) VM Access

**Use When:**
- Minimize exposure of RDP/SSH
- Time‑bound access needed
- Reduce attack surface
- Temporary admin access

**Think:** Open RDP/SSH only when needed, close automatically

**Key Benefits:**
- Temporary port opening
- Audit logging
- IP whitelisting
- Automatic closure
- Reduces attack surface

---

### Network Security Groups (NSG)

**Use When:**
- Restrict VM‑to‑VM traffic
- Subnet or NIC‑level filtering
- Basic stateful firewall

**Think:** Basic L3/L4 firewall (subnet or NIC level)

**Key Characteristics:**
- Stateful filtering
- Allow/Deny rules
- Works at NIC or subnet level
- Inbound/outbound rules

---

### Azure Firewall

**Use When:**
- Centralized firewalling
- DNAT/SNAT (network translation)
- Threat intelligence
- FQDN filtering
- Hub‑and‑spoke architecture

**Think:** Enterprise firewall for hub‑and‑spoke

**Key Characteristics:**
- Stateful firewall
- Threat intelligence
- FQDN filtering (domain names)
- DNAT/SNAT
- Scales better than NSGs

---

### Web Application Firewall (WAF)

**Use When:**
- Protect web apps from OWASP attacks
- SQL injection, XSS, DDoS protection
- Use with App Gateway or Front Door

**Think:** Web app protection at L7

**Deployment Options:**
- Application Gateway with WAF
- Front Door with WAF
- Azure CDN with WAF

---

## 5. PaaS Access Control (Private Link vs Service Endpoints)

### Decision Table

| Scenario | Correct Tool |
|---|---|
| Restrict access to Azure SQL / Storage / PaaS | Service Endpoint |
| Make PaaS fully private (no public IP) | Private Link / Private Endpoint |
| Restrict VM‑to‑VM traffic | NSG |
| Centralized firewalling across VNets | Azure Firewall |
| Secure VM access without public IPs | Azure Bastion |
| Temporarily open RDP/SSH only when needed | JIT Access |

### Service Endpoints

**What They Do:**
- Restrict PaaS access to specific VNet/subnet
- Still uses public IP (but routes through Azure backbone)
- Free

**Use When:**
- Simple PaaS restriction needed
- Cost is a concern
- Full private IP not required

**Limitations:**
- Doesn't hide public IP
- PaaS still has public endpoint
- Regional only (mostly)

---

### Private Link / Private Endpoints

**What They Do:**
- PaaS service gets private IP inside your VNet
- No public IP exposure
- Requires Private DNS Zone

**Use When:**
- Full private connectivity needed
- Compliance requires no public IP
- Hybrid scenarios (on-prem access)

**Advantages:**
- Complete privacy
- Hybrid scenarios
- Works across regions

**Note:** Requires Private DNS Zone configuration

---

## 6. Azure CDN (Updated)

### Use When

- Accelerate static content
- Cache images, CSS, JS globally
- Reduce latency for static assets
- Offload origin servers

### Important Note

**CDN is NOT a load balancer.** It caches static content at edge POPs.

**Think:** Global caching for static content

### Key Features

- Global edge POPs
- Static content caching
- Origin offloading
- Performance acceleration

---

## 7. Hub‑and‑Spoke Architecture

### Hub

- Shared services
- Azure Firewall
- VPN/ExpressRoute
- Central DNS

### Spokes

- Workload VNets
- Peered to hub
- All traffic through hub firewall

### Use When

- Enterprise‑scale networks
- Central security policies needed
- Multi-team environments
- Centralized management

### Benefits

- Single point of control
- Centralized firewall
- Shared services
- Scalable design

---

## 8. DNS in Azure

### Azure DNS

**Purpose:** Public DNS hosting

**Use:** For external domains

---

### Private DNS Zones

**Purpose:** Private DNS for Azure resources

**Critical:** Required for Private Endpoints

**Use:** Internal DNS resolution within VNet

**Exam Trap:** Private Endpoints require Private DNS Zones for resolution

---

### Custom DNS

**Purpose:** Custom DNS servers in Azure

**Use:** For domain-joined VMs, AD DS

**When Needed:** On-premises AD integration

---

## 9. Global Routing & Load Balancing Decision Tree

```
Is this GLOBAL routing?
        |
    ┌───┴───┐
    No      Yes
    |       |
  Step 4   Step 2: Is performance acceleration required?
           |
           Clues: "slow page loads", "global users", "nearest POP", 
           "edge caching"
           |
       ┌───┴────┐
       Yes      No
       |        |
   Front Door  Step 3: Is this DNS‑based failover or DR?
               |
               Clues: "ASR failover", "primary/secondary",
               "redirect users after failover"
               |
           ┌───┴────┐
           Yes      No
           |        |
      Traffic    Traffic Manager
      Manager    (simple global routing)
      (with ASR)


REGIONAL ROUTING (if NO to global):

Step 4: Is this REGIONAL routing?
        |
    ┌───┴───┐
    No      Yes
    |       |
 Step 6   Step 5: Do you need L7 features?
          |
          Clues: WAF, URL routing, SSL offload
          |
          ┌───┴────┐
          Yes      No
          |        |
      App Gateway  Load Balancer


SPECIAL CASES:

Step 6: Is this about VM access or management?
        |
    ┌───┴───┐
    Yes     No
    |       |
  Step 7  Step 8

Step 7: VM Access Requirement?
    No public IPs → Azure Bastion
    Minimize RDP/SSH → JIT Access
    VM-to-VM traffic → NSG

Step 8: Static content performance?
    Yes → Azure CDN
    No → Azure Firewall, NSG, Private Link
```

---

## Load Balancing Cheat Sheet

| Service | Layer | Scope | Best For |
|---|---|---|---|
| **Azure Load Balancer** | L4 (TCP/UDP) | Regional | Ultra-high performance, non-HTTP |
| **App Gateway** | L7 (HTTP) | Regional | URL routing, WAF, SSL termination |
| **Traffic Manager** | DNS | Global | DNS failover, DR, geo-routing |
| **Front Door** | L7 + Edge | Global | Performance acceleration, WAF, CDN |
| **Azure CDN** | Content | Global | Static content caching |

---

## Access Control & Security Cheat Sheet

| Service | Purpose | Scope | Best For |
|---|---|---|---|
| **NSG** | L3/L4 filtering | Subnet/NIC | Basic firewall rules |
| **Azure Firewall** | Centralized firewall | Hub | Enterprise firewalling |
| **WAF** | L7 protection | App Gateway/FD | Web app attacks |
| **Bastion** | Secure VM access | Regional | RDP/SSH without public IP |
| **JIT Access** | Temporary access | VM | Minimize exposure |
| **Service Endpoint** | PaaS restriction | VNet | Simple PaaS access control |
| **Private Link** | Private PaaS access | Global | Full privacy, hybrid |

---

## PaaS Access Control Decision Tree

```
Do you need to restrict PaaS access?
                |
            ┌───┴───┐
            Yes     No
            |       |
         Step 2   No action needed

Step 2: Does PaaS need fully private IP (no public endpoint)?
            |
        ┌───┴───┐
        Yes     No
        |       |
   Private Link  Service Endpoint
   (requires     (cheaper, simpler)
   Private DNS)
```

---

## Memory Hooks (Fast Exam Recall)

### Routing & Load Balancing

**Front Door** = Global acceleration + WAF + POP routing

**Traffic Manager** = DNS failover (for DR)

**App Gateway** = Regional L7 + WAF

**Load Balancer** = Regional L4 (ultra-high performance)

---

### Access & Security

**Bastion** = Secure VM access (no public IP)

**JIT** = Temporary RDP/SSH (minimize exposure)

**NSG** = Basic firewall (subnet/NIC)

**Azure Firewall** = Enterprise firewall (hub-and-spoke)

**WAF** = Web app protection (L7)

---

### PaaS Access

**Private Endpoint** = Private IP to PaaS (requires Private DNS Zone)

**Service Endpoint** = Public backbone but restricted (cheaper)

---

### Global vs Regional

- **Global Performance** → Front Door
- **Global Failover** → Traffic Manager
- **Regional Web** → App Gateway
- **Regional Anything** → Load Balancer

---

## Exam Traps to Avoid

### Trap 1: Front Door vs Traffic Manager

**Wrong:** "Use Traffic Manager for global performance"
**Right:** Use Front Door for performance acceleration

---

### Trap 2: VNet Peering Transitivity

**Wrong:** "Traffic flows transitively through peered VNets"
**Right:** VNet Peering is NOT transitive—use Azure Firewall in hub

---

### Trap 3: Private Endpoints Without DNS

**Wrong:** "Create Private Endpoint, traffic works"
**Right:** Private Endpoints require Private DNS Zone for resolution

---

### Trap 4: NSG vs Azure Firewall

**Wrong:** "NSGs are enough for enterprise security"
**Right:** Use Azure Firewall for centralized, threat-intelligent firewalling

---

### Trap 5: Bastion Replaces NSGs

**Wrong:** "Bastion eliminates the need for NSGs"
**Right:** Bastion supplements NSGs—both are needed

---

## Final Checklist (Network Questions)

- [ ] Is this global routing? → Front Door or Traffic Manager
- [ ] Is this regional load balancing? → App Gateway (L7) or Load Balancer (L4)
- [ ] Is this about VM access? → Bastion or JIT
- [ ] Is this about PaaS access? → Private Link or Service Endpoint
- [ ] Is this about VNet security? → NSG or Azure Firewall
- [ ] Is this about web app attacks? → WAF
- [ ] Is this about private connectivity? → VNet Peering or ExpressRoute
- [ ] Is this about on-premises connectivity? → VPN or ExpressRoute
- [ ] Is this about DNS? → Azure DNS or Private DNS Zone
- [ ] Is this about static content? → Azure CDN
