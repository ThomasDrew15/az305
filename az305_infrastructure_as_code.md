# AZ‑305: Infrastructure as Code (IaC) — Decision Tree & Cheat Sheet

---

## IaC Decision Tree (AZ‑305)

```
Do you need Azure‑native IaC?
                    /                    \
                  Yes                    No
                   |                      |
               Use Bicep          Do you need multi‑cloud?
                                          /         \
                                        Yes         No
                                        |            |
                                   Use Terraform   Do you need
                                                   governance +
                                                   RBAC + policy
                                                   bundled?
                                                   /            \
                                                 Yes            No
                                                 |               |
                                          Use Azure           Do you need
                                          Blueprints          to enforce
                                                              compliance?
                                                              /         \
                                                            Yes         No
                                                            |            |
                                                     Use Azure Policy   Do you need
                                                                         CI/CD for IaC?
                                                                         /          \
                                                                       Yes          No
                                                                       |             |
                                                             Use DevOps/GitHub     Do you need
                                                             Pipelines             AKS declarative
                                                                                   management?
                                                                                   /          \
                                                                                 Yes          No
                                                                                 |             |
                                                                          Use GitOps (Flux)   Use ARM
                                                                                               Templates
                                                                                               (legacy)
```

This tree covers every IaC‑related exam question.

---

## IaC Cheat Sheet (AZ‑305)

### 1. Bicep — "Azure‑Native IaC"

**Use When**

- You want Azure‑native IaC
- You want clean, modular templates
- You want ARM compatibility without JSON pain

**Exam Keywords**

- "Azure‑native IaC"
- "Declarative templates"
- "ARM‑based"
- "Modules"

**Key Features**

- Cleaner syntax than ARM JSON
- Full ARM Template compatibility
- Modular reusable components
- Easier to read and maintain

**Best For**

- New Azure projects
- Teams that prefer readability
- Azure-only deployments

---

### 2. ARM Templates — "Legacy JSON IaC"

**Use When**

- You need full Azure coverage
- You're maintaining existing ARM deployments
- You need features not yet supported in Bicep

**Exam Keywords**

- "ARM template"
- "JSON declarative deployment"
- "Native Azure deployment model"

**Key Features**

- Native Azure deployment format
- Full feature coverage
- JSON-based (verbose)
- Wide ecosystem support

**Best For**

- Maintaining legacy deployments
- Maximum feature coverage
- Deep Azure resource control

---

### 3. Terraform — "Multi‑Cloud IaC"

**Use When**

- You need multi‑cloud support
- You want a single IaC tool across Azure + AWS + GCP
- You want provider‑based extensibility

**Exam Keywords**

- "Multi‑cloud"
- "Cloud‑agnostic"
- "HCL"
- "State file"

**Key Features**

- Cloud-agnostic syntax
- Multi-cloud provider support
- State management
- Large community ecosystem

**Best For**

- Multi-cloud environments
- Organizations using multiple cloud providers
- Teams requiring flexibility across clouds

**Important Note**

- State file management is critical
- Requires understanding of Terraform state
- Not Azure-specific

---

### 4. Azure Blueprints — "Governance + IaC Bundle"

**Use When**

- You need to enforce standardised environments
- You want to deploy RBAC + Policy + ARM/Bicep together
- You need subscription‑level governance

**Exam Keywords**

- "Governance"
- "Standardised environment"
- "Subscription‑level deployment"
- "Policy + RBAC + templates"

**Key Features**

- Bundles templates, policies, and RBAC
- Enforces standards across subscriptions
- Repeatable environment creation
- Governance at scale

**Best For**

- Enterprise governance
- Multi-subscription deployments
- Enforcing organizational standards

---

### 5. Azure Policy — "Enforce Compliance"

**Use When**

- You need to enforce rules on resources
- You need to deny non‑compliant deployments
- You want auto‑remediation

**Exam Keywords**

- "Compliance"
- "Deny non‑compliant resources"
- "Allowed SKUs/regions"
- "Auto‑remediation"

**Key Features**

- Evaluate and enforce compliance
- Audit, Deny, or DeployIfNotExists modes
- Auto-remediation capabilities
- Organization-wide policy enforcement

**Best For**

- Compliance enforcement
- Preventing non-compliant resources
- Organization-wide governance

---

### 6. Azure DevOps / GitHub Actions — "Automated IaC Deployment"

**Use When**

- You need CI/CD for IaC
- You want approvals, gates, and pipelines
- You want Git‑based workflows

**Exam Keywords**

- "Pipeline"
- "Automated deployment"
- "CI/CD"
- "GitOps workflow"

**Key Features**

- Automated deployment pipelines
- Approval gates and reviews
- Git integration for version control
- Environment-based deployments

**Best For**

- Automating IaC deployments
- Team collaboration with Git
- Enforcing deployment practices

---

### 7. GitOps (Flux) — "AKS Declarative Management"

**Use When**

- You want AKS to sync state from Git
- You want drift correction
- You want declarative cluster management

**Exam Keywords**

- "GitOps"
- "Flux"
- "AKS declarative management"
- "Git as source of truth"

**Key Features**

- Git repository as source of truth
- Automatic drift correction
- Declarative cluster state
- Continuous reconciliation

**Best For**

- AKS cluster management
- Declarative Kubernetes deployments
- GitOps workflows

---

## IaC vs Other Tools (Exam Traps)

| Scenario | Choose | Why |
|---|---|---|
| Pure Azure deployment | Bicep | Cleaner, modern, Azure-native |
| Multi-cloud infrastructure | Terraform | Cloud-agnostic, multi-provider |
| Governance + IaC bundle | Azure Blueprints | Combines templates, policy, RBAC |
| Enforce compliance rules | Azure Policy | Compliance enforcement |
| Automated IaC pipelines | DevOps/GitHub | CI/CD automation |
| AKS cluster management | GitOps/Flux | Declarative, Git-based |
| Legacy ARM deployments | ARM Templates | Backward compatibility |

---

## Memory Tricks (Fast Recall)

### "Azure‑native → Bicep"

If the question is purely Azure, Bicep is almost always correct.

**Example:** "Deploy standardized VMs to Azure subscriptions" → Bicep

### "Multi‑cloud → Terraform"

If AWS or GCP appear anywhere, Terraform wins.

**Example:** "Deploy to Azure and AWS with same tool" → Terraform

### "Governance → Blueprints"

If the question mentions standards or subscription‑level provisioning, choose Blueprints.

**Example:** "Enforce standard environments across subscriptions" → Blueprints

### "Compliance → Azure Policy"

If the question mentions deny, audit, or enforce, it's Policy.

**Example:** "Prevent non-compliant resources from being deployed" → Azure Policy

### "Automation → DevOps/GitHub"

If the question mentions pipelines or approvals, choose DevOps/GitHub.

**Example:** "Automate IaC deployment with approval gates" → DevOps/GitHub

### "AKS state → GitOps"

If the question mentions AKS drift correction or Git as source of truth, choose GitOps.

**Example:** "Sync AKS cluster state from Git repository" → GitOps/Flux

### "Legacy JSON → ARM Templates"

If the question mentions ARM JSON specifically, that's ARM Templates.

**Example:** "Maintain existing ARM template deployments" → ARM Templates

---

## Quick Decision Guide

| Need | Solution |
|---|---|
| Deploy Azure resources from code | Bicep |
| Deploy to Azure + AWS + GCP | Terraform |
| Bundle governance with templates | Azure Blueprints |
| Enforce compliance on deployments | Azure Policy |
| Automate template deployments | DevOps/GitHub Actions |
| Manage AKS cluster declaratively | GitOps/Flux |
| Maintain legacy JSON templates | ARM Templates |

---

## Exam Scenarios You'll See

### Scenario 1: "We need to deploy standardized environments across 10 subscriptions with RBAC and policies."

**Answer:** Azure Blueprints

**Why:** Only Blueprints bundles templates, policy, and RBAC for subscription-level deployment.

### Scenario 2: "We deploy to Azure and AWS using the same IaC tool."

**Answer:** Terraform

**Why:** Terraform is cloud-agnostic and supports multiple providers.

### Scenario 3: "We need to deploy Azure resources but want cleaner syntax than JSON."

**Answer:** Bicep

**Why:** Bicep provides ARM compatibility with cleaner, more readable syntax.

### Scenario 4: "We need to prevent deployments of non-compliant resources."

**Answer:** Azure Policy

**Why:** Policy can audit or deny non-compliant resources.

### Scenario 5: "We want to automate Bicep deployments with approval gates."

**Answer:** Azure DevOps / GitHub Actions

**Why:** These provide CI/CD pipelines with approval workflows.

### Scenario 6: "We want AKS cluster configuration to sync automatically from Git."

**Answer:** GitOps (Flux)

**Why:** GitOps uses Git as the source of truth for cluster state.

---

## Relationship Between IaC Tools

```
┌─────────────────────────────────────────────────────┐
│                 Infrastructure as Code              │
└─────────────────────────────────────────────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    ┌────▼────┐     ┌────▼────┐    ┌────▼────┐
    │  Bicep  │     │Terraform│    │ARM Tmpl │
    │(Azure)  │     │(Multi)  │    │(Legacy) │
    └────┬────┘     └────┬────┘    └────┬────┘
         │               │               │
         └───────────────┼───────────────┘
                         │
            ┌────────────┴────────────┐
            │                         │
       ┌────▼─────┐           ┌──────▼────┐
       │ Blueprints│           │   Policy  │
       │(Governance)          │(Compliance)
       └────┬─────┘           └──────┬────┘
            │                         │
            │      Deployed via      │
            │                         │
            └────────────┬────────────┘
                         │
         ┌───────────────┼───────────────┐
         │               │               │
    ┌────▼────┐     ┌────▼────┐    ┌────▼────┐
    │ DevOps  │     │ GitHub  │    │  GitOps │
    │Pipelines│     │ Actions │    │  (Flux) │
    └─────────┘     └─────────┘    └─────────┘
```
