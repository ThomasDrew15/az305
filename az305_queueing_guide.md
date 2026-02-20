# AZ‑305: Queueing Services — The Practical Guide

Azure has three queueing technologies you need to know for the exam:

1. Azure Queue Storage
2. Azure Service Bus (Queues + Topics)
3. Event Grid / Event Hub (not queues, but often confused with them)

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

## How to Choose the Right Queue (Exam Logic)

Here's the cheat sheet the exam is secretly testing:

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

This tree covers every queue‑related exam question.

---

## Queueing Cheat Sheet (AZ‑305)

### 1. Azure Queue Storage — "Simple Queue"

**Use when:**
- You need basic async messaging
- You want the cheapest option
- You don't need ordering or transactions

**Think:** 🟦 "Simple, cheap, basic queue."

**Exam keywords:**
- Decouple components
- Asynchronous processing
- Low cost
- Basic messaging

### 2. Azure Service Bus — "Enterprise Queue"

**Use when:**
- You need FIFO (via sessions)
- You need transactions
- You need dead‑letter queues
- You need pub/sub (Topics)
- You need guaranteed delivery

**Think:** 🟪 "Enterprise messaging with rules."

**Exam keywords:**
- Ordered messages
- Transactions
- Dead‑letter queue
- Message sessions
- Pub/sub with filtering

### 3. Event Grid — "Event Router"

**Use when:**
- You need pub/sub
- You need to react to events from Azure services
- You need fan‑out

**Think:** 🟩 "Push events everywhere instantly."

**Exam keywords:**
- Event-driven
- Pub/sub
- Fan-out
- React to resource changes

### 4. Event Hub — "Telemetry Firehose"

**Use when:**
- You need high‑throughput ingestion
- You need streaming analytics
- You need real‑time dashboards

**Think:** 🟥 "Massive stream ingestion."

**Exam keywords:**
- Telemetry
- Streaming
- High throughput
- Real-time ingestion

### 5. IoT Hub — "Device Gateway"

**Use when:**
- You have physical devices
- You need device identity
- You need device twins
- You need command/control

**Think:** 🟧 "Devices need passports."

**Exam keywords:**
- Sensors
- Devices
- Production line
- Device management
- Bi-directional communication

---

## Memory Tricks (These REALLY Help in the Exam)

### 1. "SIMPLE → Queue Storage"

If the question sounds simple, the answer is simple.

"Just decouple components" → Queue Storage.

### 2. "RULES → Service Bus"

If you see ordering, transactions, sessions, dead‑letter, or guaranteed delivery:

"Rules? Use Service Bus."

### 3. "EVENTS → Event Grid"

If the question mentions events, notifications, or fan‑out:

"Events? Grid."

### 4. "STREAMS → Event Hub"

If the question mentions telemetry, streaming, real‑time, or high throughput:

"Streams? Hub."

### 5. "DEVICES → IoT Hub"

If the question mentions sensors, PLCs, production line, or device identity:

"Devices? IoT Hub."
