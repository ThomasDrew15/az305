# AZ‑305: Authentication & Authorization — Decision Tree + Cheat Sheet + Side‑by‑Side Comparison

---

## Identity & Access Decision Tree (AZ‑305)

### Primary User Type Decision

```
Who needs to authenticate?
            /              |                \
       Employees       Customers          Partners
          |               |                  |
       Use Entra ID    Use B2C            Use B2B
```

### Workload Identity Decision

```
Do workloads need identity?
            /  \
          Yes   No
          |      |
   Use Managed   Do you need to secure APIs?
   Identity            /           \
                     Yes           No
                     |              |
            Use APIM + OAuth2    Do you need
                                authorization
                                inside the app?
                                /           \
                              Yes           No
                              |              |
                    Use App Roles/Scopes   Use RBAC
                    (application-level)    (Azure resources)
```

### Governance & Policy Decision

```
Do you need MFA, device rules, or risk-based access?
                     /                     \
                   Yes                     No
                   |                        |
         Use Conditional Access       Standard Entra ID
```

---

## Authentication & Authorization Cheat Sheet (AZ‑305)

### 1. Entra ID (Azure AD) — Authentication for Employees

**What it is:** Enterprise identity provider

**Capabilities:**
- OAuth2 / OIDC / SAML
- MFA, Conditional Access, SSO
- Multi-tenant support

**Use for:** Internal users, enterprise apps

**Key Features:**
- SSO across applications
- Directory services
- Application proxy for hybrid
- Passwordless sign-in

---

### 2. Azure AD B2C — Authentication for Customers

**What it is:** Customer identity and access management

**Capabilities:**
- Social logins (Google, Facebook, LinkedIn)
- Custom user flows
- Email verification
- Password policies

**Use for:** Public-facing apps, customer-facing applications

**Key Features:**
- Branded login experience
- User self-service
- Custom attributes
- API connectors for workflows

---

### 3. Azure AD B2B — Authentication for Partners

**What it is:** Guest user management for external collaboration

**Capabilities:**
- Invite external users
- Grant access to resources
- Maintain relationships with partner orgs

**Use for:** Suppliers, contractors, external partners

**Key Features:**
- No separate accounts needed
- Leverage existing identities
- Conditional access for guests
- Access reviews

---

### 4. Managed Identities — Authentication for Workloads

**What it is:** Identity for Azure resources (no secrets required)

**Capabilities:**
- System-assigned (tied to resource lifecycle)
- User-assigned (standalone resource)
- Token-based authentication

**Use for:** Service-to-service authentication

**Key Features:**
- No secret management
- Automatic token refresh
- Works with Key Vault, Storage, SQL, etc.
- Reduced operational overhead

**Two Types:**
- **System-assigned** — Created with resource, deleted with resource
- **User-assigned** — Independent resource, can be shared

---

### 5. Azure RBAC — Authorization for Azure Resources

**What it is:** Role-based access control for Azure resources

**Capabilities:**
- Built-in roles (Contributor, Reader, Owner, etc.)
- Custom roles
- Scope-based (subscription, RG, resource)
- Inheritance through hierarchy

**Use for:** Infrastructure authorization (who can deploy, modify, delete resources)

**Key Characteristics:**
- Cascades through management groups
- Resource-level granularity
- Audit trail of assignments

---

### 6. App Roles & OAuth Scopes — Authorization Inside Applications

**What it is:** Application-level authorization model

**Capabilities:**
- Define app roles (Admin, Reader, Contributor)
- Define API scopes for microservices
- Check roles/scopes in code

**Use for:** Application authorization, API access control

**Key Characteristics:**
- Defined in app manifest
- Claims included in tokens
- Fine-grained app-level control

---

### 7. API Management + OAuth2 — API Security

**What it is:** API gateway with OAuth2/JWT enforcement

**Capabilities:**
- JWT token validation
- OAuth2 flow enforcement
- Scope validation
- Rate limiting and throttling

**Use for:** Securing backend APIs, API authorization

**Key Characteristics:**
- Centralized API security
- Policy-based enforcement
- Developer portal

---

### 8. Conditional Access — Policy-Based Authentication

**What it is:** Risk and policy-based access control

**Capabilities:**
- Require MFA
- Check device compliance
- Location-based access
- Risk-based authentication
- Session controls

**Use for:** Enforcing authentication policies enterprise-wide

**Key Characteristics:**
- Real-time policy evaluation
- Automatic remediation
- Integration with Identity Protection

---

### 9. Identity Protection — Risk-Based Authentication

**What it is:** Threat detection and automated response

**Capabilities:**
- Detect risky sign-ins
- Identify compromised credentials
- Automated remediation
- Risk levels (Low, Medium, High)

**Use for:** Identity threat detection and prevention

**Key Characteristics:**
- Machine learning-based detection
- Automated user blocking or MFA requirement
- Risk reports and analytics

---

## Side‑by‑Side Comparison (Authentication Methods)

| Capability / Need | Entra ID | B2C | B2B | Managed Identity | RBAC | App Roles / Scopes | APIM + OAuth2 | Conditional Access |
|---|---|---|---|---|---|---|---|---|
| **Primary User Type** | Employees | Customers | Partners | Workloads | N/A | App Users | API Clients | Any User |
| **Authentication Type** | OAuth2/OIDC/SAML | OAuth2/OIDC | OAuth2/OIDC | Token-based | N/A | OAuth2 scopes | OAuth2/JWT | Policy-based |
| **Authorization Type** | N/A | N/A | N/A | N/A | Azure resources | Application roles | API access | Access conditions |
| **Secrets Required?** | No | No | No | No | No | No | No | No |
| **Use for APIs?** | Indirectly | No | No | Yes (backend) | No | Yes | Yes | No |
| **Use for Apps?** | Yes | Yes | Yes | No | No | Yes | Yes | Yes |
| **External Users?** | No | Yes | Yes | No | No | Yes | Yes | No |
| **Enforce MFA?** | Via CA | Yes | Yes | No | No | No | No | Yes |
| **Device Compliance?** | Via CA | No | No | No | No | No | No | Yes |
| **Primary Purpose** | Identity provider | Customer identity | Partner identity | Workload identity | Resource access | App-level access | API security | Authentication policy |
| **Multi-tenant?** | Yes | Yes | Yes | No | No | No | Yes | Yes |
| **Scalable to Millions?** | Yes | Yes | Yes | N/A | Yes | Yes | Yes | Yes |

---

## Entra ID Licensing Tiers (Exam‑Critical)

This is a **heavily tested** area on AZ-305—know these tiers cold.

| Feature / Capability | Entra ID Free | Entra ID P1 | Entra ID P2 |
|---|---|---|---|
| **SSO** | ✔️ | ✔️ | ✔️ |
| **User & group management** | ✔️ | ✔️ | ✔️ |
| **Hybrid identity (AD Connect)** | ❌ | ✔️ | ✔️ |
| **Conditional Access** | ❌ | ✔️ | ✔️ |
| **Full MFA policies** | ❌ | ✔️ | ✔️ |
| **Dynamic groups** | ❌ | ✔️ | ✔️ |
| **Identity Protection** | ❌ | ❌ | ✔️ |
| **Risk‑based Conditional Access** | ❌ | ❌ | ✔️ |
| **Privileged Identity Management (PIM)** | ❌ | ❌ | ✔️ |
| **Full Identity Governance** | ❌ | Limited | ✔️ |

### What Each Tier Includes

**Entra ID Free**
- Basic identity and SSO
- User and group management
- No advanced features
- No hybrid identity

**Entra ID P1**
- Everything in Free, plus:
- Conditional Access (basic)
- Hybrid identity (AD Connect)
- Dynamic groups
- MFA policies
- Self-service password reset

**Entra ID P2**
- Everything in P1, plus:
- Identity Protection (risk detection)
- Privileged Identity Management (just-in-time admin access)
- Risk-based Conditional Access
- Full identity governance
- Advanced threat protection

### Exam Scenarios with Entra ID Tiers

**Scenario 1: "We need hybrid identity with on-premises AD"**
- Answer: **Entra ID P1** (minimum for AD Connect)

**Scenario 2: "We need to enforce MFA based on sign-in risk"**
- Answer: **Entra ID P2** (risk-based Conditional Access)

**Scenario 3: "We need just-in-time elevated permissions for admins"**
- Answer: **Entra ID P2** (Privileged Identity Management)

**Scenario 4: "We need basic SSO for our cloud apps"**
- Answer: **Entra ID Free** (that's all you need)

**Scenario 5: "We need to detect compromised credentials automatically"**
- Answer: **Entra ID P2** (Identity Protection)

---

## Entra ID Licensing Memory Tricks

### Quick Recall

**Free → Basic**
- Just identity and SSO
- No advanced features

**P1 → Enterprise**
- Hybrid identity
- Conditional Access
- Policies and governance

**P2 → Zero‑Trust Security**
- Risk detection
- Just-in-time access
- Full identity protection

### The Progression

```
Free
 ↓
P1 (Add: Hybrid + Conditional Access + Policies)
 ↓
P2 (Add: Risk Detection + PIM + Advanced Governance)
```

### Exam Trigger Words

| Keyword | Tier |
|---|---|
| "Hybrid identity" | P1+ |
| "Conditional Access" | P1+ |
| "Risk-based" | P2 |
| "PIM" or "just-in-time" | P2 |
| "Identity Protection" | P2 |
| "Detect risky sign-ins" | P2 |
| "Dynamic groups" | P1+ |
| "MFA policies" | P1+ |
| "Basic SSO" | Free |

---

## Common Exam Traps (Licensing)

### Trap 1: Assuming Free Supports Conditional Access

**Wrong:** "We use Entra ID Free and enable Conditional Access"
**Right:** Conditional Access requires P1 or P2

### Trap 2: Forgetting Hybrid Identity Costs

**Wrong:** "We need AD Connect, so Free is enough"
**Right:** AD Connect (hybrid identity) requires P1 or P2

### Trap 3: Thinking PIM is in P1

**Wrong:** "We'll use P1 for just-in-time admin access"
**Right:** PIM requires P2

### Trap 4: Assuming All MFA is the Same

**Wrong:** "P1 has full MFA policies"
**Right:** P1 has basic MFA; P2 has risk-based MFA

### Trap 5: Not Considering Identity Protection

**Wrong:** "We don't need to detect compromised accounts"
**Right:** Identity Protection (P2) is critical for zero-trust security

---

## Decision Guide by Scenario

### Scenario 1: "Employees need to access cloud apps with MFA"

**Answer:** Entra ID + Conditional Access

**Why:**
- Entra ID provides enterprise identity
- Conditional Access enforces MFA policy

### Scenario 2: "We need to expose APIs to external partners securely"

**Answer:** APIM + OAuth2 + B2B (for user authentication)

**Why:**
- APIM validates OAuth2 tokens
- B2B manages partner user access

### Scenario 3: "Azure Functions need to access Key Vault"

**Answer:** Managed Identity

**Why:**
- No secrets needed
- Automatic token management
- Built-in Azure service support

### Scenario 4: "Public-facing app with Google/Facebook login"

**Answer:** Azure AD B2C

**Why:**
- B2C supports social identity providers
- Custom user flows
- Customer-friendly experience

### Scenario 5: "We need fine-grained control over who can modify resources"

**Answer:** Azure RBAC

**Why:**
- Resource-level access control
- Inheritance through hierarchy
- Audit trail

### Scenario 6: "Microservices need to validate API caller permissions"

**Answer:** App Roles + OAuth Scopes

**Why:**
- Token includes role/scope claims
- Application-level authorization
- Microservice-friendly

### Scenario 7: "Block sign-ins from risky locations or compromised accounts"

**Answer:** Conditional Access + Identity Protection

**Why:**
- Conditional Access enforces policy
- Identity Protection detects risks
- Automated response

---

## Memory Tricks (Fast Exam Recall)

### "Authentication = Identity (Entra ID / B2C / B2B / Managed Identity)"

Focus on **who** the user/workload is:
- Employees → Entra ID
- Customers → B2C
- Partners → B2B
- Workloads → Managed Identity

### "Authorization = Access (RBAC / App Roles / Scopes)"

Focus on **what** they can do:
- Azure resources → RBAC
- Applications → App Roles/Scopes
- APIs → APIM + OAuth2

### "Workload identity = Managed Identity"

Whenever you see "service-to-service" or "app accessing Azure resources" → Managed Identity

### "Customer identity = B2C"

Public-facing apps with external logins → B2C

### "Partner identity = B2B"

Collaborating with other organizations → B2B

### "API security = APIM + OAuth2"

Protecting backend APIs from unauthorized callers → APIM with OAuth2/JWT

### "MFA/device rules = Conditional Access"

Any time you need to enforce authentication policies → Conditional Access

### "Risk detection = Identity Protection"

Compromised credentials, risky sign-ins → Identity Protection

---

## Quick Reference: When to Use What

| Need | Solution |
|---|---|
| Employee authentication | Entra ID |
| Customer login | B2C |
| Partner collaboration | B2B |
| Workload-to-resource access | Managed Identity |
| Control Azure resource access | RBAC |
| Control application access | App Roles / Scopes |
| Secure APIs | APIM + OAuth2 |
| Enforce MFA/device rules | Conditional Access |
| Detect risky logins | Identity Protection |
| Passwordless sign-in | Windows Hello, FIDO2 |

---

## Exam Traps to Avoid

### Trap 1: Confusing RBAC with App Roles

- **RBAC** = Who can deploy/modify Azure resources
- **App Roles** = Who can use the application (inside the app)

### Trap 2: Using Secrets Instead of Managed Identity

- Don't store connection strings in code
- Use Managed Identity for service-to-service auth

### Trap 3: Confusing B2C with B2B

- **B2C** = Customer identity (public apps)
- **B2B** = Partner collaboration (external users)

### Trap 4: Forgetting Conditional Access

- MFA alone isn't enough
- Conditional Access enforces **policies** (device compliance, location, risk)

### Trap 5: Not Using Identity Protection

- Conditional Access is reactive (policies)
- Identity Protection is proactive (threat detection)

---

## Final Checklist for AZ-305 Auth Questions

- [ ] Identify user/workload type (employee, customer, partner, workload)
- [ ] Choose authentication method (Entra ID, B2C, B2B, Managed Identity)
- [ ] Identify authorization need (RBAC, App Roles, Scopes, policies)
- [ ] Check if MFA/device rules needed (Conditional Access)
- [ ] Check if risk detection needed (Identity Protection)
- [ ] Check if APIs need security (APIM + OAuth2)
- [ ] Verify no secrets in code (use Managed Identity or Key Vault)
