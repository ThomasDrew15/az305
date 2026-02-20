# AZ‑305 Data Integration Services — Exam Cheat Sheet

Below are the major ingestion, processing, and movement services you'll see in AZ‑305, each with:
- What it does
- When to choose it
- Exam keywords that point to it
- When not to choose it

---

## 1. Azure Event Hubs

### What It Is

High‑throughput, low‑latency ingestion for telemetry and streaming data.

### Use When

- You need to ingest massive event streams
- Data comes from apps, services, logs, clickstreams
- You need real‑time or near‑real‑time processing

### Exam Keywords

- "Telemetry"
- "Streaming data"
- "High throughput"
- "Real‑time ingestion"
- "Application events"

### Not For

- Device identity
- Device management
- Command/control (That's IoT Hub)

---

## 2. Azure IoT Hub

### What It Is

A device gateway for secure ingestion + device management.

### Use When

- You have physical devices
- You need device identity
- You need device twins, commands, provisioning
- You're dealing with sensors, PLCs, factory equipment

### Exam Keywords

- "Devices"
- "Sensors"
- "Production line equipment"
- "Device management"
- "Bi‑directional communication"

### Not For

- Generic telemetry
- App logs
- Clickstreams (Event Hub is simpler)

---

## 3. Azure Stream Analytics

### What It Is

A real‑time analytics engine for streaming data.

### Use When

- You need real‑time processing
- You want to run SQL‑like queries on streams
- You need sub‑second latency
- You want to push results to Power BI

### Exam Keywords

- "Real‑time analytics"
- "Streaming queries"
- "Low latency"
- "Time window analysis"

### Not For

- Batch ETL
- Heavy ML
- Long‑running transformations

---

## 4. Azure Data Factory

### What It Is

A data movement + orchestration service.

### Use When

- You need scheduled pipelines
- You need to copy data between systems
- You need on‑prem → cloud ingestion
- You need ETL/ELT orchestration

### Exam Keywords

- "Copy data"
- "Move data"
- "Pipeline"
- "Scheduled ingestion"
- "Hybrid/on‑prem integration"

### Not For

- Real‑time
- Analytics
- ML
- Big data processing

---

## 5. Azure Databricks

### What It Is

A Spark‑based analytics and ML platform.

### Use When

- You need big data processing
- You need machine learning
- You need complex transformations
- You're working with JSON, Parquet, large datasets
- You need predictive analytics

### Exam Keywords

- "Predictive modelling"
- "Machine learning"
- "Time‑series analysis"
- "Large datasets"
- "Spark"

### Not For

- Simple ETL
- Basic SQL analytics
- Real‑time ingestion

---

## 6. Azure Synapse Analytics

### What It Is

A unified analytics platform (SQL + Spark + pipelines).

### Use When

- You need data warehousing
- You need SQL analytics at scale
- You need Spark + SQL together
- You need Power BI integration
- You're building a lakehouse

### Exam Keywords

- "Data warehouse"
- "Analytics workspace"
- "SQL + Spark"
- "Lakehouse"
- "Power BI integration"

### Not For

- Pure ML (Databricks is preferred)
- Simple ETL (ADF is simpler)

---

## 7. Azure Data Lake Storage (ADLS Gen2)

### What It Is

A storage layer optimized for analytics.

### Use When

- You need to store large raw datasets
- You need hierarchical namespace
- You need Hadoop/Spark compatibility
- You need POSIX ACLs

### Exam Keywords

- "Hierarchical file system"
- "Hadoop integration"
- "Big data storage"
- "Analytics workloads"

### Not For

- Simple blob storage
- Images, videos, random files (Blob Storage is fine)

---

## 8. Azure Blob Storage

### What It Is

General‑purpose object storage.

### Use When

- You need to store unstructured data
- You don't need analytics features
- You need cheap, scalable storage

### Exam Keywords

- "Images"
- "Videos"
- "Unstructured files"
- "Object storage"

### Not For

- Hadoop/Spark workloads
- Hierarchical namespace
- Analytics pipelines

---

## 9. Azure Queue Storage

### What It Is

A simple message queue.

### Use When

- You need to decouple components
- You need asynchronous messaging

### Exam Keywords

- "Message queue"
- "Decouple services"
- "Asynchronous processing"

### Not For

- Analytics
- ETL
- Ingestion
- Big data
- Real‑time processing

---

## 10. Azure SQL Database / SQL Server

### What It Is

A relational database.

### Use When

- You need structured, relational data
- You need transactional workloads
- You need SQL queries on structured data

### Exam Keywords

- "Transactional"
- "Relational"
- "OLTP"

### Not For

- Big data
- JSON lakes
- ML
- Real‑time ingestion
