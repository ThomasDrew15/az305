# Azure Hierarchy Cheat Sheet (Expanded + Exam‑Ready)

Here's the hierarchy again, but now with what belongs where:

```
Tenant
└── Management Groups
      └── Subscriptions
            └── Resource Groups
                  └── Resources
```

Now let's attach the right mental model to each layer.

---

## 1. Tenant — Organization Boundary

### What It Is

Your Azure AD / Entra ID directory. Everything lives under one tenant.

### Use When You See

- Identity (Azure AD / Entra ID)
- Global administrators
- Enterprise‑wide identity governance
- Root management group
- Tenant-level policies

### Key Point

Policies are almost never applied here unless you're building a strict enterprise baseline for the entire organization.

### Exam Keywords

- "Root management group"
- "Enterprise baseline"
- "Global policy"
- "Tenant-wide"

---

## 2. Management Groups — Governance Layer

### What They Are

A grouping layer above subscriptions that allows you to apply policies and governance across multiple subscriptions.

### Use Management Groups For

- Landing zones
- Security baselines
- Compliance standards
- Connectivity standards
- Feature restrictions
- Guardrails
- Policies that apply to multiple subscriptions
- Enterprise‑wide governance

### Key Concept

Management Groups are **NOT** for application organization. They're for **governance**.

### Exam Triggers (When to Choose Management Groups)

- "Apply to all workloads"
- "Apply to all subscriptions"
- "Standardize security"
- "Standardize compliance"
- "Landing zone"
- "Governance"
- "Guardrails"
- "Enterprise policy"
- "Multiple subscriptions"
- "Organizational standards"

**If you see ANY of these → Management Group is likely correct**

### Real-World Example

```
Management Group: Production
  ├── Policy: Require encryption on all storage
  ├── Policy: Require NSGs on all VMs
  ├── Policy: Require tags on all resources
  └── Applies to all subscriptions under this MG
```

---

## 3. Subscription — Environment Boundary

### What It Is

A billing and quota boundary. Each subscription is isolated and has its own quotas.

### Use Subscriptions For

- Billing separation
- Environment separation (prod/test/dev)
- Quota boundaries
- Access isolation
- Delegating ownership to teams
- Cost tracking per environment

### Key Concept

Subscriptions are **boundaries**, not governance layers.

### Exam Triggers (When Subscriptions Are the Answer)

- "Separate billing"
- "Separate environments"
- "Quota limits"
- "Team isolation"
- "Cost center separation"
- "Different departments"

### Important Note

Policies applied at a subscription level apply to everything inside that subscription, but not beyond to other subscriptions.

### Real-World Example

```
Subscription: Production (for production workloads)
Subscription: Development (for dev/test)
Subscription: Sandbox (for experimentation)
```

Each has separate billing, quotas, and cost tracking.

---

## 4. Resource Group — Workload Boundary

### What It Is

A logical container for resources that are deployed, managed, and deleted together.

### Use Resource Groups For

- Grouping resources that share a lifecycle
- Deploying resources together
- Deleting resources together
- Application‑level RBAC
- Organizing a single workload
- Organizational clarity

### Key Concept

Resource Groups are for **application/workload organization**, NOT governance.

### What NOT to Use Resource Groups For

- ❌ Governance
- ❌ Enterprise‑wide policy
- ❌ Landing zones
- ❌ Compliance standards
- ❌ Security baselines

### Exam Triggers (When Resource Groups Are the Answer)

- "Group related resources"
- "Share a lifecycle"
- "Deploy together"
- "Delete together"
- "Application boundary"
- "Workload boundary"
- "Organizational clarity"

### Real-World Example

```
Resource Group: WebAppRG
  ├── App Service (web app)
  ├── SQL Database
  ├── Storage Account
  ├── App Service Plan
  └── All deployed/deleted together

Resource Group: DataProcessingRG
  ├── Data Factory
  ├── Synapse Analytics
  ├── Storage Account
  └── All deployed/deleted together
```

---

## 5. Resources — Individual Services

### What They Are

Individual Azure services: VMs, storage accounts, databases, NICs, etc.

### Use This Level For

- Resource‑specific locks
- Resource‑specific tags
- Resource‑specific RBAC
- Resource-specific policies
- Cost allocation per resource

---

## The Golden Rule: Pick the Right Level Instantly

### Rule 1: Multi-Subscription Requirement?

**If the requirement applies to multiple subscriptions** → **MUST be a Management Group**

### Rule 2: Multi-Workload Requirement?

**If the requirement applies to multiple workloads** → **NOT a Resource Group**

### Rule 3: Governance Requirement?

**If the requirement is about governance, compliance, or security** → **Management Group**

### Rule 4: Application Organization?

**If the requirement is about application organization** → **Resource Group**

### Rule 5: Billing or Environment?

**If the requirement is about billing or environment separation** → **Subscription**

---

## Decision Table (Quick Reference)

| Requirement | Level | Why |
|---|---|---|
| Enforce encryption across org | Management Group | Multi-subscription governance |
| Require tags on all resources | Management Group | Enterprise-wide standard |
| Separate prod/test billing | Subscription | Environment boundary |
| Group related resources | Resource Group | Workload organization |
| Resource-level RBAC | Resource | Individual service access |
| Require NSG on all VMs | Management Group | Security baseline |
| Cost center separation | Subscription | Billing boundary |
| Deploy web app components | Resource Group | Shared lifecycle |
| Landing zones | Management Group | Governance structure |
| Restrict VM sizes | Management Group | Resource constraint |

---

## Policy Placement Decision Tree

```
Does the requirement apply to the entire organization?
            |
        ┌───┴───┐
        Yes     No
        |       |
    Tenant or  Does it apply to multiple subscriptions?
    Root MG    |
            ┌───┴───┐
            Yes     No
            |       |
        Management  Does it apply to a single
        Group       environment (prod/test/dev)?
                    |
                ┌───┴───┐
                Yes     No
                |       |
            Subscription  Does it apply to a single
                          workload or app?
                          |
                      ┌───┴───┐
                      Yes     No
                      |       |
                  Resource    Resource
                  Group       (individual)
```

---

## Hierarchy Governance Pattern (Best Practices)

### Typical Enterprise Structure

```
Tenant (Root Management Group)
│
├── Management Group: Landing Zones
│   │
│   ├── Management Group: Production
│   │   ├── Subscription: Prod-1
│   │   ├── Subscription: Prod-2
│   │   └── Policy: Require encryption, NSGs, tags
│   │
│   ├── Management Group: Non-Production
│   │   ├── Subscription: Dev-1
│   │   ├── Subscription: Test-1
│   │   └── Policy: Allow experimental features
│   │
│   └── Management Group: Sandbox
│       └── Subscription: Sandbox-1
│       └── Policy: Minimal restrictions
│
├── Management Group: Platform
│   └── Subscription: Connectivity (shared networking)
│
└── Management Group: Security
    └── Subscription: Security (shared security tools)
```

### Policies Applied at Each Level

| Level | Policies |
|---|---|
| Root MG | Global baselines (encryption, audit logging) |
| Landing Zone MG | Environment-specific (prod vs non-prod) |
| Production MG | Production-only requirements |
| Subscription | Environment-specific quotas, cost tracking |
| Resource Group | Application organization (not policy) |

---

## Exam Scenarios: Hierarchy Placement

### Scenario 1: "Enforce encryption on all storage accounts"

**Question Level:** Management Group

**Why:** Applies to multiple subscriptions, multiple workloads, security standard

---

### Scenario 2: "Group our web app resources"

**Question Level:** Resource Group

**Why:** Single workload, shared lifecycle, organizational clarity

---

### Scenario 3: "Separate production and development billing"

**Question Level:** Subscription

**Why:** Environment boundary, billing separation

---

### Scenario 4: "Require NSGs on all VMs in the organization"

**Question Level:** Management Group

**Why:** Organization-wide security baseline, multiple subscriptions

---

### Scenario 5: "Create a policy that allows experimental features only in sandbox"

**Question Level:** Management Group (Sandbox MG)

**Why:** Sandbox-specific governance, but still multi-resource policy

---

### Scenario 6: "Organize database, app service, and storage for our e-commerce app"

**Question Level:** Resource Group

**Why:** Single application, shared lifecycle, resources deployed together

---

### Scenario 7: "Restrict VM SKUs to a maximum cost per month"

**Question Level:** Management Group or Subscription

**Why:** Quota/cost restriction applies to multiple resources
- If across org → Management Group
- If per environment → Subscription

---

### Scenario 8: "Apply a resource lock to prevent accidental deletion"

**Question Level:** Resource

**Why:** Individual resource protection, not organizational

---

## Memory Tricks (Fast Recall)

### "Governance → Management Group"

Any time you hear:
- "Apply to all..."
- "Standardize..."
- "Enterprise..."
- "Security baseline..."
- "Landing zone..."

→ **Management Group**

### "Environment → Subscription"

Any time you hear:
- "Separate billing..."
- "Production vs test..."
- "Team isolation..."
- "Cost center..."

→ **Subscription**

### "Workload → Resource Group"

Any time you hear:
- "Group resources..."
- "Deploy together..."
- "Single application..."
- "Shared lifecycle..."

→ **Resource Group**

### "Individual → Resource"

Any time you hear:
- "Lock this VM..."
- "Tag this storage..."
- "RBAC for this database..."

→ **Resource**

---

## The One Principle That Rules Them All

**Ask yourself: "Does this apply to multiple subscriptions or is it a governance standard?"**

- **Yes → Management Group**
- **No → Subscription/Resource Group/Resource**

If it's governance or applies to multiple subscriptions, it belongs in Management Groups. Everything else is either environmental (subscription), organizational (resource group), or individual (resource).

---

## Final Exam Checklist

- [ ] Is this about governance or security baseline? → Management Group
- [ ] Does it apply to multiple subscriptions? → Management Group
- [ ] Is it about environment separation? → Subscription
- [ ] Is it about billing separation? → Subscription
- [ ] Is it about grouping resources that work together? → Resource Group
- [ ] Is it about a single resource? → Resource
- [ ] Is it about a landing zone? → Management Group
- [ ] Is it about organizational clarity for an app? → Resource Group
- [ ] Is it about enforcing enterprise standards? → Management Group
- [ ] Is it about resource lifecycle management? → Resource Group
