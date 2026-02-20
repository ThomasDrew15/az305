# AZ‑305: Logging & Monitoring — Cheat Sheet

Azure's monitoring stack looks huge, but AZ‑305 only cares about six core components:

- Azure Monitor
- Log Analytics Workspace
- Application Insights
- Activity Log
- Diagnostic Settings
- Network Watcher

Everything else is just flavour.

---

## 1. Azure Monitor — "The Umbrella"

### What It Is

The platform that collects, stores, analyses, and visualises telemetry.

### Use When

- You need a unified monitoring solution
- You want to correlate logs + metrics + traces
- You want alerts, dashboards, workbooks

### Exam Keywords

- "Unified monitoring"
- "Metrics and logs"
- "Platform for observability"

### Key Components

- **Metrics** — Time-series data (CPU, memory, latency)
- **Logs** — Structured event data
- **Alerts** — Automated notifications
- **Dashboards** — Custom visualizations
- **Workbooks** — Interactive reports

---

## 2. Log Analytics Workspace — "Central Log Store"

### What It Is

A workspace that stores logs and supports KQL queries.

### Use When

- You need to query logs
- You need cross‑resource analysis
- You need long‑term retention
- You want alerts based on logs

### Exam Keywords

- "KQL"
- "Centralised logging"
- "Log Analytics Workspace"

### Key Features

- **KQL queries** — Kusto Query Language for log analysis
- **Long-term storage** — Months to years of logs
- **Log-based alerts** — Alert on KQL query results
- **Cross-resource** — Query logs from multiple resources
- **Retention policies** — Customize log retention

### Common Use Cases

- Analyzing application logs
- Querying security events
- Trending analysis
- Compliance auditing

---

## 3. Application Insights — "Application Performance Monitoring (APM)"

### What It Is

Telemetry for applications:

- Requests
- Dependencies
- Exceptions
- Traces
- Distributed tracing
- Availability tests

### Use When

- You need end‑to‑end tracing
- You need app performance monitoring
- You want to detect slow dependencies

### Exam Keywords

- "APM"
- "Distributed tracing"
- "Application performance"
- "End-to-end tracing"

### Key Features

- **Request tracking** — See every HTTP request
- **Dependency tracking** — Track calls to databases, APIs, services
- **Exception tracking** — Automatic error capture
- **Performance counters** — CPU, memory, disk usage
- **Availability tests** — Synthetic monitoring
- **Distributed tracing** — Track requests across microservices

### Best For

- Microservices architectures
- API performance monitoring
- End-user experience tracking
- Dependency health

---

## 4. Azure Activity Log — "Control‑Plane Audit Log"

### What It Is

Tracks who did what in Azure (control plane).

### Use When

- You need governance
- You need to audit resource changes
- You need to track admin actions

### Exam Keywords

- "Control plane"
- "Audit"
- "Resource creation/deletion"
- "Who deployed what"

### What It Tracks

- Resource creation
- Resource deletion
- Configuration changes
- Role assignments
- Policy changes
- Deployment operations
- Access changes

### Key Characteristics

- **Automatic** — All Azure actions are logged
- **90-day retention** — Default (can export for longer)
- **Searchable** — Via Azure Monitor or exported to Log Analytics
- **Free** — No additional cost

---

## 5. Diagnostic Settings — "Data‑Plane Logs from Resources"

### What It Is

Resource‑level logs emitted by:

- Key Vault
- Storage
- SQL
- App Service
- Event Hub
- Cosmos DB
- Many other services

### Use When

- You need data‑plane logs
- You want to send logs to Log Analytics, Storage, or Event Hub
- You want to enable deep auditing

### Exam Keywords

- "Diagnostic settings"
- "Data plane logs"
- "Send to Log Analytics"
- "Enable logging on resource"

### Key Features

- **Per-resource configuration** — Configure for each resource
- **Multiple destinations** — Send to Log Analytics, Storage, or Event Hub
- **Selective logging** — Choose which log categories to enable
- **Long-term archival** — Send to Storage for compliance

### Common Scenarios

- SQL Server query logging
- Key Vault access auditing
- Storage account access logs
- App Service detailed logging

---

## 6. Network Watcher — "Network Monitoring & Troubleshooting"

### What It Is

Network‑level diagnostics:

- NSG flow logs
- Connection monitor
- Packet capture
- Topology view
- IP flow verify

### Use When

- You need to troubleshoot network issues
- You need traffic flow visibility
- You need to monitor connectivity

### Exam Keywords

- "NSG flow logs"
- "Packet capture"
- "Connection monitor"
- "Network troubleshooting"

### Key Tools

- **NSG Flow Logs** — See allowed/denied traffic through NSGs
- **Connection Monitor** — Monitor connectivity between resources
- **Packet Capture** — Capture network traffic for analysis
- **IP Flow Verify** — Test if traffic is allowed by security rules
- **Next Hop** — See routing path for packets
- **VNet Flow Logs** — Advanced network monitoring

---

## Side‑by‑Side Comparison (Exam‑Ready)

| Service | Purpose | Scope | Best For | Data Type |
|---|---|---|---|---|
| **Azure Monitor** | Unified monitoring platform | Everything | Correlation of metrics/logs | Metrics + Logs |
| **Log Analytics Workspace** | Store + query logs | Logs | KQL, alerts, dashboards | Logs only |
| **Application Insights** | App performance & tracing | Applications | APM, distributed tracing | App telemetry |
| **Activity Log** | Audit control‑plane actions | Azure resources | Governance, admin actions | Control plane events |
| **Diagnostic Settings** | Data‑plane logs | Resource-level | Deep auditing, log export | Resource-specific logs |
| **Network Watcher** | Network monitoring | Network | NSG logs, packet capture | Network traffic |

---

## Logging & Monitoring Decision Tree (AZ‑305)

### Application vs Infrastructure vs Network

```
What do you need to monitor?
           /          |           \
      Application   Infrastructure   Network
          |              |              |
   Use Application   Do you need      Use Network
    Insights        logs or metrics?   Watcher
                   /                \
                Logs               Metrics
                 |                   |
    Use Log Analytics WS       Use Azure Monitor
```

### Auditing Decision Tree

```
Do you need to track admin actions?
             /        \
           Yes        No
           |           |
   Use Activity Log   Do you need
                      resource-level logs?
                      /             \
                    Yes             No
                    |                |
         Use Diagnostic Settings   Azure Monitor
```

### Complete Monitoring Decision

```
Do you need application performance monitoring?
              /                             \
            Yes                            No
             |                              |
   Use Application Insights        Do you need to audit
                                  or track changes?
                                  /                 \
                                Yes               No
                                 |                  |
                    Use Activity Log +          Do you need
                    Diagnostic Settings         network visibility?
                                               /              \
                                             Yes             No
                                             |                |
                                   Use Network Watcher    Use Azure
                                                          Monitor
                                                          (metrics)
```

---

## Memory Tricks (Fast Exam Recall)

### "Logs → Log Analytics"

Any time you see "query logs" or "KQL" → Log Analytics Workspace

**Example:** "We need to query application logs for errors"
→ Send app logs to Log Analytics → Query with KQL

### "App telemetry → App Insights"

Any time you see "distributed tracing" or "end-to-end" → Application Insights

**Example:** "We need to track requests across our microservices"
→ Application Insights for distributed tracing

### "Audit actions → Activity Log"

Any time you see "who did what" or "admin actions" → Activity Log

**Example:** "We need to audit when users are created"
→ Activity Log tracks user creation

### "Network issues → Network Watcher"

Any time you see "NSG logs" or "packet capture" → Network Watcher

**Example:** "We need to see if traffic is being blocked by NSG"
→ Network Watcher NSG Flow Logs

### "Everything ties into Azure Monitor"

Azure Monitor is the umbrella—all other services feed into it or are accessed through it.

---

## Quick Reference: When to Use What

| Question | Answer | Why |
|---|---|---|
| "Query application logs" | Log Analytics | KQL for log analysis |
| "Track request latency" | Application Insights | APM for performance |
| "Audit who deleted a VM" | Activity Log | Control plane audit |
| "See Key Vault access logs" | Diagnostic Settings + Log Analytics | Data plane + query |
| "Monitor if NSG is blocking traffic" | Network Watcher | Network-level diagnostics |
| "Create dashboard of metrics" | Azure Monitor | Metrics visualization |
| "Alert on slow database queries" | Log Analytics | Log-based alerts |
| "End-to-end distributed tracing" | Application Insights | Tracing across services |

---

## Exam Scenarios (Full Context)

### Scenario 1: "We need to track all administrative changes in Azure"

**Answer:** Azure Activity Log

**Why:** Activity Log tracks control plane operations (who deployed what, who changed roles, etc.)

**Follow-up:** Export to Log Analytics for long-term retention and querying

### Scenario 2: "Our application is slow. We need to identify which dependency is causing it."

**Answer:** Application Insights

**Why:** Application Insights tracks dependencies and shows performance bottlenecks

**Follow-up:** Use distributed tracing to see the full request path

### Scenario 3: "We need to query logs from all our resources and create alerts"

**Answer:** Send logs to Log Analytics Workspace, then query with KQL and create alerts

**Why:** Log Analytics is the central store for querying and alerting on logs

**Included services:** Application Insights, Diagnostic Settings, Activity Log can all send to Log Analytics

### Scenario 4: "We need to see if network traffic is being blocked by security rules"

**Answer:** Network Watcher NSG Flow Logs

**Why:** NSG Flow Logs show allowed/denied traffic at the network layer

**Scenario 5: "We need to enable logging on our SQL Database to track all queries"

**Answer:** Diagnostic Settings on the SQL Database

**Why:** Diagnostic Settings sends data-plane logs (queries) to Log Analytics or Storage

---

## Integration Pattern (How It All Works Together)

```
┌─────────────────────────────────────────────────────────┐
│                   Azure Monitor                         │
│        (Unified monitoring platform)                    │
└─────────────────────────────────────────────────────────┘
        ↑              ↑              ↑              ↑
        │              │              │              │
    ┌───▼──┐      ┌───▼─────┐   ┌──▼───────┐   ┌──▼──────┐
    │Metrics│      │  Logs   │   │ Alerts  │   │Dashboards
    │(CPU,  │      │(KQL)    │   │(Rules)  │   │(Workbooks)
    │Memory)│      │         │   │         │   │
    └───┬──┘      └────┬────┘   └────┬────┘   └──────────┘
        │              │              │
        │         ┌─────▼──────────┐  │
        │         │ Log Analytics  │  │
        │         │   Workspace    │  │
        │         │  (Central       │  │
        │         │   Store)        │  │
        │         └────────────────┘  │
        │                              │
    ┌───▼────────────────────────────▼────┐
    │  Data Sources:                       │
    │  • Application Insights              │
    │  • Diagnostic Settings               │
    │  • Activity Log                      │
    │  • Network Watcher                   │
    │  • Custom sources                    │
    └────────────────────────────────────┘
```

---

## Common Exam Traps

### Trap 1: Confusing Activity Log with Diagnostic Settings

- **Activity Log** = Control plane (who deployed, who assigned roles)
- **Diagnostic Settings** = Data plane (what happened inside the resource)

### Trap 2: Thinking Application Insights is for Infrastructure

- **App Insights** = Application performance monitoring
- **Azure Monitor** = Infrastructure and resource monitoring

### Trap 3: Forgetting Log Analytics is Just Storage + Query

- Log Analytics doesn't automatically analyze
- You query it with KQL
- You must configure what gets sent to it

### Trap 4: Not Knowing Network Watcher Shows Only NSG Traffic

- Network Watcher NSG Flow Logs show traffic **through NSGs only**
- Other network traffic (routing, gateways) needs different tools

### Trap 5: Assuming All Logs Go to Activity Log

- Only **control plane** operations go to Activity Log
- **Data plane** logs need Diagnostic Settings
- **Application** logs need Application Insights or custom setup

---

## Final Exam Checklist

- [ ] Do I need to query logs? → Log Analytics
- [ ] Do I need to monitor app performance? → Application Insights
- [ ] Do I need to audit admin actions? → Activity Log
- [ ] Do I need data-plane logs? → Diagnostic Settings
- [ ] Do I need network visibility? → Network Watcher
- [ ] Do I need to correlate metrics and logs? → Azure Monitor
- [ ] Is this about control plane? → Activity Log
- [ ] Is this about application telemetry? → Application Insights
- [ ] Is this about network traffic? → Network Watcher
