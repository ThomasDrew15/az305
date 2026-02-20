# AZ‑305: Azure App Configuration — The Practical Guide

Azure App Configuration is all about centralising application settings so you don't scatter config across code, VMs, containers, Function Apps, App Services, or Kubernetes.

**Think of it as:**

🟦 "A single source of truth for application settings."

---

## 1. What Azure App Configuration Is

A fully managed service for:

- Centralising application settings
- Managing feature flags
- Versioning configuration
- Dynamically updating config without redeploying apps
- Keeping config out of code and out of environment variables

It's designed for cloud‑native, distributed applications.

---

## 2. When to Use Azure App Configuration

### 🔧 Centralised Configuration

- Multiple apps need the same settings
- Microservices need shared config
- You want to avoid config drift

### 🚀 Dynamic Configuration

- Change settings without redeploying
- Roll out config changes gradually
- Override settings per environment

### 🟢 Feature Flags

- Gradual rollouts
- Canary releases
- A/B testing
- Kill switches

### 🔐 Secure Configuration

- Store non‑secret config here
- Store secrets in Key Vault
- Combine both in your app

### 🌍 Multi‑environment Support

- Dev / Test / Prod
- Region‑specific settings
- App‑specific overrides

---

## 3. When NOT to Use It

Avoid App Configuration when:

- You need to store secrets → use **Key Vault**
- You need application data → use **a database**
- You need infrastructure config → use **IaC (Bicep/Terraform)**
- You need VM configuration → use **DSC**
- You need cluster configuration → use **GitOps**

**App Configuration is not a replacement for:**

- Key Vault
- Cosmos DB
- App Settings
- ARM/Bicep
- GitOps

**It's purely for application settings + feature flags.**

---

## 4. Key Features (Exam‑Relevant)

### 1. Centralised Key‑Value Store

- Hierarchical keys
- Labels for versioning
- Environment‑specific values

### 2. Feature Flags

- On/off toggles
- Percentage rollout
- Targeted rollout
- Time‑based activation

### 3. Dynamic Refresh

- Apps can auto‑refresh config
- No redeploy required
- Works with .NET, Java, Python, Node

### 4. Integration with Key Vault

- Reference secrets
- Keep secrets out of App Config
- Unified config + secret model

### 5. Revision History

- Roll back config changes
- Audit changes

---

## 5. Architecture Patterns

### Pattern 1: App Config + Key Vault

```
App → App Configuration → Key Vault (for secrets)
```

**Use when:**
- You want config + secrets managed separately
- You want dynamic config refresh

### Pattern 2: App Config + Microservices

```
Microservice A → 
Microservice B → App Configuration
Microservice C → 
```

**Use when:**
- Multiple services share config
- You want consistent settings across services

### Pattern 3: App Config + Feature Flags

```
App → App Configuration (Feature Flags)
```

**Use when:**
- You want canary releases
- You want to toggle features without redeploying

### Pattern 4: App Config + App Service

```
App Service → App Configuration (reference)
             → Key Vault (reference)
```

**Use when:**
- App Service apps need shared config
- Config needs to be dynamic
- Secrets are managed separately

---

## 6. App Configuration vs Key Vault (Exam Trap)

| Requirement | Choose |
|---|---|
| Application settings | App Configuration |
| Feature flags | App Configuration |
| Secrets, keys, certificates | Key Vault |
| Rotate secrets | Key Vault |
| Dynamic config refresh | App Configuration |

**Memory Trick:** 🟦 "Settings → App Config, Secrets → Key Vault."

---

## 7. App Configuration vs App Settings (App Service)

| Requirement | Choose |
|---|---|
| Per‑app config | App Settings |
| Shared config across apps | App Configuration |
| Dynamic refresh | App Configuration |
| Feature flags | App Configuration |
| Secure secrets | Key Vault |

**Key Distinction:**
- **App Settings** = specific to that App Service instance
- **App Configuration** = shared across multiple apps, dynamic, versioned

---

## 8. App Configuration vs Other Config Methods

| Use Case | Solution | Why |
|---|---|---|
| Share config across microservices | App Configuration | Designed for this |
| Feature flags and rollouts | App Configuration | Built-in feature flag management |
| Store application secrets | Key Vault | Secure key management |
| Environment-specific app settings | App Settings | Per-instance configuration |
| Infrastructure as code settings | Bicep/Terraform | IaC-managed |
| Container orchestration settings | GitOps/Kubernetes ConfigMaps | Cluster-level config |
| Database connection strings | Key Vault + App Config | Key Vault for secret part, App Config for server |

---

## 9. Exam Keywords to Watch For

If you see any of these phrases, the answer is **Azure App Configuration:**

- "Centralise application settings"
- "Feature flags"
- "Dynamic configuration"
- "Avoid redeploying apps to change config"
- "Shared configuration across microservices"
- "Roll out features gradually"
- "Canary release"
- "A/B testing"
- "Gradual rollout"
- "Configuration versioning"

---

## 10. Memory Tricks (Fast Exam Recall)

### "Settings live in App Config. Secrets live in Key Vault."

Always remember this distinction. It's the most common exam trap.

**Example:** "We need to store API keys and database connection strings."
- API keys → Key Vault
- Connection string server (non-secret part) → App Configuration
- Connection string password → Key Vault

### "Feature flags = App Configuration."

Any time the exam mentions gradual rollouts, canary releases, or feature toggles, it's App Configuration.

### "If you want to change config without redeploying → App Configuration."

This is the core value proposition. No redeployment needed.

### "If multiple apps need the same config → App Configuration."

App Configuration solves the "shared config across services" problem.

---

## 11. Real Exam Scenarios

### Scenario 1: "We have 5 microservices that need the same API endpoint and need to change it without redeploying."

**Answer:** Azure App Configuration

**Why:** Centralized, shared, dynamic, no redeploy needed.

### Scenario 2: "We want to gradually roll out a new feature to 10% of users first."

**Answer:** Azure App Configuration (feature flags)

**Why:** Built-in percentage rollout capability.

### Scenario 3: "We need to store database passwords securely."

**Answer:** Azure Key Vault

**Why:** Designed for secrets, not settings.

### Scenario 4: "We need to store non-secret connection string details and feature toggles that apply to all apps."

**Answer:** Azure App Configuration

**Why:** Non-secret config + feature flags = App Configuration.

### Scenario 5: "We need per-App-Service instance settings that vary by environment."

**Answer:** App Settings (on App Service)

**Why:** Per-instance configuration is what App Settings are for.

### Scenario 6: "We want to version configuration changes and roll back if needed."

**Answer:** Azure App Configuration

**Why:** Built-in revision history and rollback.

---

## 12. Integration with Other Azure Services

### App Configuration + App Service

```
App Service → reads → App Configuration → references → Key Vault
                      (non-secrets)                   (secrets)
```

### App Configuration + Azure Functions

```
Function App → reads → App Configuration → references → Key Vault
```

### App Configuration + AKS

```
Pod → reads → App Configuration
Pod → reads → Key Vault (via workload identity)
```

### App Configuration + Logic Apps

```
Logic App → reads settings → App Configuration
```

---

## 13. Quick Reference Table

| Question Pattern | Answer |
|---|---|
| "Centralize settings for multiple apps" | App Configuration |
| "Feature flags and gradual rollout" | App Configuration |
| "Store API keys/passwords securely" | Key Vault |
| "Rotate secrets automatically" | Key Vault |
| "Change config without redeploy" | App Configuration |
| "Per-App-Service instance settings" | App Settings |
| "Shared config across services" | App Configuration |
| "Version control for configuration" | App Configuration |
| "A/B testing or canary releases" | App Configuration |
| "Audit config changes" | App Configuration |

---

## 14. Decision Tree (Config Storage)

```
Is it a secret, key, or certificate?
                /                    \
              Yes                    No
               |                      |
         Use Key Vault        Is it application config?
                              /                      \
                            Yes                      No
                             |                        |
                    Is it shared across       Is it infrastructure
                    multiple apps?            config (IaC)?
                    /              \              /          \
                  Yes              No           Yes           No
                   |                |            |              |
            Use App Config   Use App Settings   Use Bicep/  Use Database
                            (per-instance)     Terraform   or other storage
```

---

## 15. Why App Configuration Matters for AZ-305

Azure App Configuration is tested because:

1. **Cloud-native architecture** — Microservices need shared configuration
2. **Zero-downtime deployments** — Config changes without app redeploy
3. **Feature management** — Canary releases and gradual rollouts
4. **Separation of concerns** — Config ≠ secrets ≠ code
5. **Multi-environment scaling** — Dev/Test/Prod with different settings

Understanding when to use App Configuration (vs Key Vault, App Settings, or IaC) is a key differentiator on the exam.
