# Design Data Integration — Dummies Guide

## 1. What It's About

It's all about moving data between systems, transforming it, and making sure it arrives reliably. Azure gives you tools to do this at scale.

---

## 2. The Core Tools (In Plain English)

### Azure Data Factory (ADF)

- The "data mover"
- Moves data between systems (SQL, Blob, SaaS apps, on‑prem)
- Can schedule, orchestrate, and transform data

### Azure Synapse Pipelines

- Same idea as ADF but built into Synapse
- Good when your analytics live in Synapse

### Azure Data Lake Storage (ADLS)

- The "big bucket" where raw and processed data lives
- Cheap, scalable, great for analytics

### Azure Event Hub

- The "firehose" for streaming data
- Use it when data arrives continuously (IoT, logs, telemetry)

### Azure Stream Analytics

- The "real‑time calculator"
- Runs live queries on streaming data

### Azure Logic Apps

- The "workflow glue"
- Good for lightweight integrations and automations

---

## 3. The Big Decisions You Make

### Batch or Real‑Time?

- **Batch** → ADF / Synapse Pipelines
- **Real‑time** → Event Hub + Stream Analytics

### Where Does the Data Land?

- Usually ADLS for analytics
- Databases (SQL, Cosmos DB) for apps

### Do You Need Transformation?

- **Simple** → ADF mapping data flows
- **Complex** → Databricks / Synapse Spark

### Do You Need Orchestration?

- Use ADF or Synapse Pipelines to coordinate everything

---

## 4. The Golden Rules

- Put raw data in ADLS first ("bronze layer")
- Transform into cleaned ("silver") and curated ("gold") layers
- Use Event Hub for streaming, ADF for batch
- Keep compute and storage separate for flexibility and cost control
