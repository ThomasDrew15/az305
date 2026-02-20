# AZ‑305: API Management Solution — The Practical Guide

Azure API Management (APIM) is all about publishing, securing, scaling, and governing APIs. The exam expects you to know when to use it, why, and which tier fits which scenario.

---

## 1. What Azure API Management Is

A fully managed service that sits in front of your APIs and provides:

- A unified API gateway
- Security (keys, OAuth2, JWT validation)
- Rate limiting & throttling
- Caching
- Transformation (headers, payloads, versioning)
- Developer portal
- Centralised API governance

**Think:** 🟦 "A front door for all your APIs."

---

## 2. When to Use API Management

### 🔐 Security

- Validate JWT tokens
- Enforce OAuth2
- Require subscription keys
- Hide backend endpoints

### 📊 Governance

- Centralised API catalog
- Versioning
- Policies
- Consistent standards across teams

### 🚦 Traffic Control

- Rate limiting
- Throttling
- Quotas
- IP filtering

### ⚙️ Transformation

- Convert XML ↔ JSON
- Rewrite URLs
- Modify headers
- Mask sensitive data

### 📣 Developer Experience

- Developer portal
- API documentation
- Try‑it console

### 🌍 Multi‑region API Delivery

- Global distribution
- Multi‑region gateways

---

## 3. When NOT to Use API Management

Avoid APIM when:

- You only need simple routing → use Application Gateway
- You only need global load balancing → use Front Door
- You only need internal microservice routing → use Service Mesh / Dapr / Envoy
- You need ultra‑low latency (APIM adds overhead)

---

## 4. API Management Tiers (Exam‑Critical)

This is where AZ‑305 loves to trip people up.

| Tier | Use Case | Notes |
|---|---|---|
| **Developer** | Dev/test only | Not for production |
| **Basic** | Small workloads | No VNET integration |
| **Standard** | Most production workloads | VNET support |
| **Premium** | Enterprise, multi‑region | Multi‑region gateways, VNET, hybrid |
| **Consumption** | Serverless, pay‑per‑call | No VNET, no self‑hosted gateway |

### Exam Keywords to Spot

- "Multi‑region" → **Premium**
- "Hybrid / on‑prem connectivity" → **Premium**
- "VNET integration" → **Standard or Premium**
- "Serverless, pay per use" → **Consumption**
- "Dev/test" → **Developer**

---

## 5. APIM Architecture Patterns

### Pattern 1: APIM in Front of App Services

```
Client → APIM → App Service API
```

**Use when:**
- You want to secure APIs
- You want rate limiting
- You want a developer portal

### Pattern 2: APIM + Functions (Serverless APIs)

```
Client → APIM → Azure Functions
```

**Use when:**
- You want serverless APIs with governance
- You want to hide Function URLs

### Pattern 3: APIM + AKS / Microservices

```
Client → APIM → Internal Load Balancer → AKS
```

**Use when:**
- You want a single public endpoint for many microservices
- You want consistent policies across services

### Pattern 4: APIM + Hybrid / On‑Prem APIs

```
Client → APIM (Premium) → VPN/ExpressRoute → On‑Prem API
```

**Use when:**
- You need to expose on‑prem APIs securely
- You need hybrid connectivity

---

## 6. APIM Policies (Exam‑Relevant)

Policies are the heart of APIM. The exam expects you to know the categories:

### Inbound Policies

- Validate JWT
- Check subscription key
- Rate limit
- Rewrite URL
- Transform request

### Backend Policies

- Retry
- Timeout
- Circuit breaker

### Outbound Policies

- Transform response
- Mask data
- Add/remove headers

### On‑Error Policies

- Custom error handling

---

## 7. APIM vs Other Services (Exam Traps)

| Requirement | Choose |
|---|---|
| Secure + govern APIs | API Management |
| Global load balancing | Front Door |
| WAF + routing | Application Gateway |
| Internal microservice routing | Service Mesh / Dapr |
| Simple reverse proxy | NGINX / App Gateway |
| Serverless API gateway | APIM Consumption |

---

## 8. Tier Selection Decision Tree

```
Do you need multi-region API gateways?
                    /                        \
                  Yes                        No
                   |                          |
            Use Premium APIM        Do you need VNET
                                   integration?
                                   /                \
                                  Yes               No
                                  |                  |
                           Use Standard or   Do you need to pay
                           Premium APIM      per call only?
                                            /                \
                                           Yes               No
                                           |                 |
                                    Use Consumption    Is it dev/test?
                                    APIM              /            \
                                                     Yes            No
                                                     |               |
                                              Use Developer    Use Basic or
                                              APIM            Standard APIM
```

---

## 9. Security in APIM

### JWT Validation

- Validate token signature
- Check expiration
- Extract claims
- Forward to backend

### Subscription Keys

- Required for API access
- Included in headers or query parameters
- Can be revoked per app

### OAuth2 / OpenID Connect

- Delegate to identity provider (Entra ID, Auth0)
- Validate token before allowing access

### IP Whitelisting

- Restrict to known IP ranges
- Block malicious traffic

### Rate Limiting

- Limit calls per second
- Prevent abuse
- Quota-based throttling

---

## 10. APIM Caching

### When to Cache

- GET requests (idempotent)
- Expensive backend operations
- Static responses

### How to Cache

- Set `cache-lookup` policy inbound
- Set `cache-store` policy outbound
- Cache key = URL + query parameters + headers

### Benefits

- Reduce backend load
- Improve response time
- Lower costs

---

## 11. Memory Tricks (Fast Exam Recall)

### "Govern APIs → APIM"

If the question mentions governance, versioning, or developer portal → **APIM**.

### "Global API delivery → Premium APIM"

Multi‑region = **Premium**. Always.

### "Serverless API gateway → Consumption APIM"

Pay‑per‑call = **Consumption**.

### "Hybrid APIs → Premium APIM"

On‑prem connectivity requires **Premium**.

### "Security policies → APIM"

JWT validation, rate limiting, quotas → **APIM**.

### "Simple routing → Application Gateway"

Don't overkill with APIM if you only need L7 routing.

### "Microservice mesh → Dapr / Service Mesh"

If it's internal communication, don't use APIM (adds latency).

---

## 12. Quick Reference: When to Choose What

| Scenario | Solution | Why |
|---|---|---|
| Publish APIs with developer portal | APIM Standard | Full governance + portal |
| Multi-region API distribution | APIM Premium | Built-in multi-region gateways |
| Secure on-prem API access | APIM Premium | VNET + hybrid connectivity |
| Serverless API with minimal cost | APIM Consumption | Pay-per-call, no infrastructure |
| Simple request routing | Application Gateway | Cheaper, less overhead |
| Global CDN + WAF | Front Door | Better for edge acceleration |
| Microservice internal routing | Service Mesh / Dapr | Lower latency, no external gateway |
| JWT token validation | APIM | Built-in, configurable policies |
| Rate limiting at edge | Front Door | Cheaper than APIM for global throttling |

---

## 13. Exam Scenarios You'll See

### Scenario 1: "We need to expose internal microservices with rate limiting and a developer portal."

**Answer:** APIM Standard (or Premium if multi-region needed)

**Why:** Provides gateway, governance, and portal without premium cost.

### Scenario 2: "We have on-premises APIs and need to expose them securely to cloud apps."

**Answer:** APIM Premium + VPN/ExpressRoute

**Why:** Only Premium supports VNET and hybrid connectivity.

### Scenario 3: "We need an API gateway that scales automatically with pay-per-call billing."

**Answer:** APIM Consumption

**Why:** Serverless, auto-scales, no VNET overhead needed.

### Scenario 4: "We're just doing basic URL routing and don't need API governance."

**Answer:** Application Gateway or Front Door

**Why:** APIM is overkill; lighter solutions exist.

### Scenario 5: "We need to validate JWT tokens and enforce quotas on API calls."

**Answer:** APIM with inbound policies

**Why:** JWT validation and quotas are built-in APIM policies.
