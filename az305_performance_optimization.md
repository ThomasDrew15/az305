# AZ‑305: Performance Optimisation Cheat Sheet

Performance optimisation in Azure is all about reducing latency, improving throughput, and offloading expensive operations from your core systems. These are the services and patterns the exam expects you to know cold.

---

## 1. Azure Cache for Redis — "Low‑Latency Cache"

### What It Is

A fully managed, in‑memory data store used to accelerate application performance.

### Use When

- You need sub‑millisecond reads
- You want to cache database queries
- You need session state storage
- You want to reduce load on SQL/Cosmos
- You need rate limiting or distributed locks
- You want lightweight pub/sub inside an app

### Key Characteristics

- In‑memory (extremely fast)
- Supports strings, hashes, lists, sets, sorted sets
- Optional persistence
- Supports clustering and replicas
- High throughput, low latency

### Exam Keywords

- "Caching"
- "Improve performance"
- "Reduce database load"
- "Session state"
- "Low latency"
- "In‑memory store"

### Not For

- Durable messaging (use Service Bus)
- Telemetry ingestion (use Event Hub)
- Event routing (use Event Grid)

---

## 2. Azure Front Door — "Global Edge Accelerator"

### Use When

- You need global load balancing
- You want edge caching
- You need WAF
- You want to accelerate static + dynamic content

### Exam Keywords

- "Global routing"
- "Edge POPs"
- "WAF"
- "SSL offload"
- "CDN caching"

---

## 3. Azure CDN — "Static Content Accelerator"

### Use When

- You need to cache static assets globally
- You want to reduce load on origin servers

### Exam Keywords

- "Static content"
- "Images, CSS, JS"
- "Global caching"

---

## 4. Azure SQL Performance Optimisation

### Use When

- You need to scale relational workloads

### Techniques

- Scale up (DTUs/vCores)
- Read replicas
- In‑memory OLTP
- Query performance insights
- Index tuning

### Exam Keywords

- "Read scale-out"
- "In-memory tables"
- "Elastic pools"

---

## 5. Cosmos DB Performance Optimisation

### Use When

- You need low‑latency global NoSQL

### Techniques

- Increase RU/s
- Use partition keys correctly
- Use serverless for spiky workloads
- Multi-region writes

### Exam Keywords

- "RU/s"
- "Partitioning"
- "Global distribution"
- "Multi-region write"

---

## 6. App Service / API Performance Optimisation

### Techniques

- Autoscale rules
- Deployment slots
- Offload sessions to Redis
- Use managed identity for faster auth
- Use App Service Environment (ASE) for isolation

### Exam Keywords

- "Autoscale"
- "Session offloading"
- "ASE for high performance"

---

## 7. AKS Performance Optimisation

### Techniques

- Horizontal Pod Autoscaler (HPA)
- Cluster autoscaler
- Node pools (GPU, CPU, spot)
- Use Dapr sidecars for performance patterns

### Exam Keywords

- "Autoscaling"
- "Node pools"
- "HPA"

---

## 8. Storage Performance Optimisation

### Techniques

- Premium SSD / Ultra Disk for VMs
- Premium tiers for Blob Storage
- ADLS Gen2 for analytics workloads
- Increase throughput units

### Exam Keywords

- "Premium storage"
- "High IOPS"
- "Hierarchical namespace"

---

## 9. Messaging Performance Optimisation

### Queue Storage

- Simple, cheap, low‑latency messaging
- Not for high throughput or ordering

### Service Bus

- Use Premium tier for high throughput
- Sessions for FIFO
- Prefetching for faster reads

### Event Hub

- Increase partitions
- Use Capture for offloading storage
- Use consumer groups for parallelism

### IoT Hub

- Use partitions + routing
- Offload to Event Hub-compatible endpoint

---

## 10. Memory Tricks (Fast Recall)

These shortcuts will instantly guide you to the right optimization:

### "Speed → Redis"

If the problem is latency or performance, Redis is the answer.

### "Global → Front Door"

If the problem is global performance, Front Door wins.

### "Static → CDN"

If the content doesn't change, cache it globally.

### "Relational → Scale SQL"

If SQL is slow, scale up or add replicas.

### "NoSQL → Partition Cosmos"

If Cosmos is slow, fix the partition key.

### "Streaming → Event Hub"

If the data is continuous, use Event Hub.

### "Devices → IoT Hub"

If the data comes from hardware, use IoT Hub.

---

## Quick Performance Decision Table

| Performance Problem | Solution |
|---|---|
| Database queries are slow | Add Redis cache |
| Database is overloaded | Read replicas + scale up |
| Global users experience high latency | Front Door + CDN |
| Static assets load slowly | CDN |
| Application latency is high | Redis for session state |
| Cosmos reads are expensive | Optimize partition key |
| SQL reads are expensive | In-memory OLTP tables |
| App needs to autoscale | Autoscale rules |
| Containers need to autoscale | HPA + Cluster autoscaler |
| Streaming data is slow | Increase Event Hub partitions |
| Device commands are delayed | IoT Hub routing + partitions |

---

## Performance Patterns (Exam-Focused)

### Pattern 1: Cache-Aside (Cache-Aside Pattern)

```
1. App reads from cache
2. Cache miss → read from database
3. Update cache
4. Return to user
```

**Use:** When you want app-controlled caching with Redis.

### Pattern 2: Session Offloading

```
1. App stores session in Redis (not memory)
2. Scale horizontally without losing sessions
3. Multiple instances share session state
```

**Use:** For horizontally scaled web apps.

### Pattern 3: Global Read Distribution

```
1. Write to primary region
2. Replicate to secondary regions
3. Read from nearest region
```

**Use:** Cosmos DB multi-region, SQL read replicas.

### Pattern 4: Partition-Based Scaling

```
1. Increase partitions/RU-s
2. Distribute load across partitions
3. Improve throughput
```

**Use:** Event Hub, Cosmos DB, Service Bus.

### Pattern 5: Edge Caching

```
1. Front Door caches at edge POPs
2. Static + dynamic content cached
3. Reduced origin server load
```

**Use:** Global applications needing CDN + WAF.
