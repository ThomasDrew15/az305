# AZ‑305: Queueing Services — The Practical Guide

Azure has five key messaging and ingestion technologies you need to know for the exam:

1. Azure Queue Storage
2. Azure Service Bus (Queues + Topics)
3. Event Grid
4. Event Hubs
5. IoT Hub

Let's break them down.

---

## 1. Azure Queue Storage

### What It Is

A simple, lightweight, cloud‑native queue for basic message passing.

### Use When

- You need simple asynchronous communication
- You're decoupling components
- You don't need ordering, transactions, or advanced features
- You want the cheapest option

### Key Characteristics

- At‑least‑once delivery
- No FIFO guarantee
- No transactions
- No sessions
- Max message size: 64 KB
- Very cheap

### Exam Keywords

- "Simple queue"
- "Decouple components"
- "Asynchronous processing"
- "Low cost"
- "Basic messaging"

### When NOT to Choose It

- When you need ordering
- When you need guaranteed delivery semantics
- When you need pub/sub
- When you need enterprise messaging features

---

## 2. Azure Service Bus (Queues + Topics)

### What It Is

Enterprise‑grade messaging with advanced features.

### Use When

- You need FIFO
- You need sessions
- You need dead‑letter queues
- You need transactions
- You need exactly‑once processing
- You need pub/sub (use Topics)

### Key Characteristics

- FIFO (with sessions)
- Duplicate detection
- Scheduled delivery
- Transactions
- Dead‑letter queues
- Message size up to 256 KB (Standard) or 1 MB (Premium)
- Topics for pub/sub

### Exam Keywords

- "Enterprise messaging"
- "Guaranteed order"
- "Transactions"
- "Dead-letter queue"
- "Pub/sub with filtering"
- "Message sessions"

### When NOT to Choose It

- When you need high‑throughput telemetry (Event Hub is better)
- When you need simple, cheap queues (Queue Storage is enough)

---

## 3. Event Grid (Not a Queue, But Often Confused)

### What It Is

A serverless event routing system.

### Use When

- You need pub/sub
- You need event-driven architecture
- You need to react to events from Azure services

### Key Characteristics

- Push model
- Very low latency
- Massive fan‑out
- No message persistence

### Exam Keywords

- "Event-driven"
- "Pub/sub"
- "Fan-out events"
- "React to resource changes"

### Not For

- Queueing
- Message durability
- Ordering

---

## 4. Event Hubs (Also Not a Queue, But Appears in Similar Questions)

### What It Is

A high‑throughput event ingestion service.

### Use When

- You need to ingest telemetry
- You need streaming data
- You need real‑time analytics

### Key Characteristics

- Partitioned log
- Very high throughput
- Low latency
- Not a queue (no FIFO, no transactions)

### Exam Keywords

- "Telemetry"
- "Streaming"
- "High throughput"
- "Real-time ingestion"

---

## 5. IoT Hub (Not a Queue, But Critical for Device Telemetry)

### What It Is

A secure, bi‑directional communication gateway for physical devices.

### Use When

- You have sensors, PLCs, embedded devices, factory equipment
- You need device identity
- You need device twins
- You need command/control
- You need device provisioning
- You need to ingest device telemetry

### Key Characteristics

- Per‑device authentication
- Device twins (state management)
- Bi‑directional messaging
- Integration with DPS (Device Provisioning Service)
- Supports millions of devices

### Exam Keywords

- "Devices"
- "Sensors"
- "Production line"
- "Device management"
- "Bi-directional communication"
- "Device identity"

### Not For

- Generic telemetry (Event Hub is simpler)
- Event routing (Event Grid is better)
- Simple messaging (Queue Storage or Service Bus)

---

## How to Choose the Right Service (Exam Logic)

| Requirement | Choose |
|---|---|
| Simple async messaging | Queue Storage |
| Enterprise messaging | Service Bus |
| FIFO | Service Bus (sessions) |
| Pub/sub | Service Bus Topics |
| High throughput telemetry | Event Hub |
| Event routing | Event Grid |
| Device telemetry | IoT Hub |

### The Exam Trick

Microsoft LOVES to test whether you can distinguish:
- Queue Storage (simple)
- Service Bus (enterprise)
- Event Grid (event routing)
- Event Hub (telemetry ingestion)
- IoT Hub (device gateway)

If you can map the requirement → service, you'll nail every question in this area.

---

## Queueing Decision Tree (AZ‑305)

```
Do you need to ingest telemetry or streams?
                    /                        \
                  Yes                        No
                   |                          |
            Use Event Hub              Do you need device
                                       identity/commands?
                                       /                \
                                      Yes               No
                                      |                  |
                                 Use IoT Hub       Do you need
                                                   enterprise-grade
                                                   messaging?
                                                   /            \
                                                  Yes            No
                                                  |               |
                                         Use Service Bus    Do you need
                                         (Queues/Topics)    simple async
                                                            messaging?
                                                            /        \
                                                           Yes        No
                                                           |           |
                                                 Use Queue Storage   Use Event Grid
                                                 (simple queue)      (event routing)
```

---

## Queueing Cheat Sheet (AZ‑305)

### Azure Queue Storage — "Simple Queue"

🟦 Simple, cheap, basic queue.

**Use when:** Basic async messaging, decoupling components, lowest cost priority

### Azure Service Bus — "Enterprise Queue"

🟪 Enterprise messaging with rules.

**Use when:** FIFO, transactions, ordering, dead-letter queues, pub/sub

### Event Grid — "Event Router"

🟩 Push events everywhere instantly.

**Use when:** Event-driven, pub/sub, react to Azure resource events, fan-out

### Event Hub — "Telemetry Firehose"

🟥 Massive stream ingestion.

**Use when:** Telemetry, streaming data, real-time analytics, high throughput

### IoT Hub — "Device Gateway"

🟧 Devices need passports.

**Use when:** Physical devices, sensors, device identity, device twins, command/control

---

## Memory Tricks (These REALLY Help in the Exam)

These five rules will instantly guide you to the right answer:

1. **SIMPLE → Queue Storage**
   - Question sounds basic? Simple async messaging? → Queue Storage

2. **RULES → Service Bus**
   - See ordering, transactions, sessions, dead-letter, guaranteed delivery? → Service Bus

3. **EVENTS → Event Grid**
   - Mentions events, notifications, fan-out, react to changes? → Event Grid

4. **STREAMS → Event Hub**
   - Says telemetry, streaming, real-time, high throughput? → Event Hub

5. **DEVICES → IoT Hub**
   - Mentions sensors, PLCs, production line, device identity? → IoT Hub

---

## Quick Reference Table

| Scenario | Best Choice |
|---|---|
| Background jobs decoupling web/worker roles | Queue Storage |
| Order processing with guaranteed delivery | Service Bus Queues |
| Publish events to multiple subscribers | Service Bus Topics |
| React to blob storage creation events | Event Grid |
| Stream 1M+ events per second | Event Hub |
| Manage thousands of temperature sensors | IoT Hub |
| Simple notification system | Event Grid |
| Enterprise workflow with retries | Service Bus |
| Cheap message queue | Queue Storage |
| Device command and control | IoT Hub |
