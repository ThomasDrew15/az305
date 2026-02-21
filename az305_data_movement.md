# Azure Data Movement Cheat Sheet

AzCopy • Data Box Family • Import/Export • ADF • Storage Explorer • Online vs Offline

---

## The Big Picture

Azure gives you two categories of data movement:

### ONLINE (Network-Based)

- AzCopy
- Storage Explorer
- Data Factory / Synapse pipelines
- Data Box Gateway (continuous sync)
- REST APIs / SDKs

### OFFLINE (Ship Hardware)

- Data Box (100 TB)
- Data Box Heavy (1 PB)
- Data Box Disk (up to 40 TB)
- Import/Export (your own disks)

---

## AzCopy — The Workhorse

### What It Is

A command-line tool for high-speed uploads/downloads to Azure Storage.

### Best For

- Moving large amounts of data online
- Blob Storage (block blobs)
- Azure Files
- Syncing folders
- Automation scripts
- Migration pipelines

### Strengths

- Fast (parallel, multi-threaded)
- Free
- Cross-platform
- Supports SAS tokens, MSI, OAuth
- Resume support
- Can sync deltas
- Scriptable

### Limitations

- Network-bound (WAN speed is your bottleneck)
- Not ideal for petabyte-scale
- Not for offline transfer
- Not for databases or VM disks
- Not for managed disks

### Memory Trick

**AzCopy = "Copy over the network, but fast"**

### Use Cases

- 50 GB upload → AzCopy ✅
- 500 GB upload → AzCopy ✅
- 5 TB upload → Consider Data Box

---

## Data Box Family — Microsoft Ships You Hardware

Azure's modern offline transfer solution.

### Data Box (100 TB Appliance)

**Characteristics:**
- Rugged, tamper-resistant
- 40–100 TB usable capacity
- SMB + NFS
- For large migrations
- You fill it → ship back → Azure ingests

**Best For:**
- 100 TB – 1 PB migrations
- One-time bulk transfers
- Regional migrations

**Cost:** Moderate (per order)

### Data Box Heavy (1 PB Appliance)

**Characteristics:**
- For massive migrations
- 1 petabyte usable
- Forklift required
- Same workflow as Data Box

**Best For:**
- Petabyte-scale migrations
- Data center consolidations
- Massive data lake ingestion

**Cost:** Higher (per order)

### Data Box Disk (Up to 40 TB)

**Characteristics:**
- Set of encrypted SSDs
- Lighter, cheaper
- Good for distributed offices
- You fill the disks → ship back

**Best For:**
- Smaller locations (branch offices)
- Limited space
- Regional transfers

**Cost:** Lower than Data Box

### Data Box Family Strengths

- Fast (local copy speeds)
- Secure (AES-256, tamper-proof)
- Microsoft handles logistics
- Ideal for TB–PB scale migrations
- No bandwidth charges
- Traceable and audited

### Data Box Family Limitations

- One-time bulk transfer (no continuous)
- Not for databases directly
- Not for VM disks directly
- Only supports Blob Storage + Azure Files
- Not for managed disks
- Not for page blobs

### Memory Trick

**Data Box = "Microsoft sends you the box"**

### When to Use Data Box

- Transferring 100 GB+ offline
- Network too slow/unreliable
- Large one-time migration
- Data center evacuation
- Backup/archive ingestion

---

## Data Box Gateway — Continuous Hybrid Sync

### What It Is

A virtual appliance you deploy on-prem that syncs data to Azure continuously.

### Best For

- Ongoing hybrid workloads
- Continuous ingestion
- Replacing tape
- Edge → cloud pipelines
- File server modernization

### How It Works

```
On-Premises Data
    ↓
Data Box Gateway (virtual appliance)
    ↓
Azure Storage (continuous sync)
```

### Strengths

- Continuous, not one-time
- Hybrid-friendly
- Ongoing sync
- Like a virtual Data Box
- Uses SMB/NFS

### Limitations

- Not for one-time bulk (use Data Box)
- Not for PB-scale volumes
- Requires stable network
- Gateway capacity limits
- Slower than AzCopy for sheer speed

### Memory Trick

**Gateway = "Data Box, but online and continuous"**

---

## Import/Export — Ship Your Own Disks

### What It Is

The older offline transfer method. You send your own BitLocker-encrypted disks to Azure.

### Best For

- One-time migrations
- When you already have disks
- When Data Box isn't available in your region

### How It Works

```
You encrypt your disks with BitLocker
    ↓
Ship disks to Azure data center
    ↓
Azure ingests into Blob Storage
    ↓
Your disks returned (or destroyed)
```

### Strengths

- Use existing hardware
- No Microsoft hardware cost
- Can bring your own disks

### Limitations

- You must supply disks
- Manual process
- Only supports Blob Storage + Azure Files
- No managed disks
- No databases
- No PaaS services
- Slower logistics than Data Box

### Memory Trick

**Import/Export = "You ship your disks"**
**Data Box = "Microsoft ships theirs"**

### Comparison: Data Box vs Import/Export

| Aspect | Data Box | Import/Export |
|---|---|---|
| Hardware | Microsoft sends | You provide |
| Logistics | Managed by Microsoft | Managed by you |
| Security | Tamper-proof | Encrypted disks |
| Cost | Per order | Per disk usage |
| Speed | Optimized (data center hardware) | Your disk speed |
| Destinations | Blob + Azure Files | Blob + Azure Files |
| Databases | No | No |
| Managed Disks | No | No |

---

## Azure Data Factory (ADF) — ETL/ELT Pipelines

### What It Is

A data integration service for orchestrating data movement and transformation.

### Best For

- Structured data (databases, files)
- Database migrations
- SaaS connectors
- Data transformations
- Scheduled pipelines
- Complex workflows

### How It Works

```
Source (SQL DB, Blob, API, etc.)
    ↓
ADF Pipeline
    ↓
Transformation (Mapping Data Flows)
    ↓
Destination (Azure services, databases)
```

### Strengths

- Supports many connectors (60+)
- Transformation capabilities
- Scheduled/triggered pipelines
- Data mapping features
- Orchestration

### Limitations

- Not for PB-scale raw file migration (use AzCopy)
- Not for offline transfer
- Not for VM disks
- Slower than AzCopy for raw files
- Requires pipeline setup

### Memory Trick

**ADF = "Move + transform data"**

### Use Cases

- Migrate SQL Database → SQL Managed Instance
- Copy data from Salesforce → Azure SQL
- ETL from multiple sources
- Data warehouse population

---

## Storage Explorer — GUI for Humans

### What It Is

A graphical tool for browsing and managing Azure Storage.

### Best For

- Small transfers
- Browsing storage
- Manual uploads/downloads
- Developers/admins exploring data
- Interactive work

### Strengths

- Visual, intuitive
- Free
- Cross-platform
- Easy for non-technical users

### Limitations

- Not for large migrations
- Not automated
- Not fast (single-threaded)
- Manual process

### Memory Trick

**Explorer = "Click, drag, drop"**

---

## The Ultimate Decision Table

| Scenario | Best Tool | Why |
|---|---|---|
| Upload 50 GB of files | AzCopy | Fast, online |
| Upload 500 GB | AzCopy | Still fine, network acceptable |
| Upload 5 TB | Data Box | WAN too slow |
| Upload 50 TB | Data Box | Bulk offline transfer |
| Upload 500 TB | Data Box Heavy | PB-scale |
| Upload 10 TB from branch offices | Data Box Disk | Portable, distributed |
| Continuous sync from on-prem | Data Box Gateway | Ongoing ingestion |
| Move SQL Database | Azure Database Migration Service (DMS) | Structured data, databases |
| Move VM disks | Azure Migrate | Not blobs, managed disks |
| Restore deleted blob | Soft Delete | Instant recovery |
| Move data offline using your own disks | Import/Export | Bring-your-own-disk |
| Manual browsing/upload | Storage Explorer | GUI interface |

---

## Data Movement Decision Tree

```
What data are you moving?

Structured (databases, tables)
    ↓
Use ADF or Azure Database Migration Service


Unstructured (files, blobs)
    ↓
How much data?

< 1 TB
    ↓
Network fast enough?
    ├─ Yes → AzCopy or Storage Explorer
    └─ No → Data Box Disk


1 TB – 100 TB
    ↓
Use Data Box


100 TB – 1 PB
    ↓
Use Data Box Heavy


Continuous sync needed?
    ↓
Data Box Gateway


Your own disks?
    ↓
Import/Export
```

---

## Exam Traps (Memorise These)

### ❌ Data Box Does NOT Support

- Managed disks
- Page blobs (only block blobs)
- Databases
- VM migration
- Continuous sync (except Data Box Gateway)
- SQL Server direct copy
- Cosmos DB

### ❌ Import/Export Does NOT Support

- Managed disks
- Page blobs
- SQL Database
- Cosmos DB
- Anything PaaS
- Continuous sync

### ❌ AzCopy Is NOT

- Offline (network-based only)
- For PB-scale (network bottleneck)
- For databases
- For VM disks
- For managed disks
- For page blobs

### ❌ ADF Is NOT

- A direct file mover for massive blobs (AzCopy faster)
- An offline solution
- A file sync tool (not Delta Sync for simple cases)
- A backup solution

### ✔ What They Actually Are

- **Data Box** = Microsoft sends hardware
- **Import/Export** = You send hardware
- **AzCopy** = Online, fast, free
- **ADF** = Pipelines + transformations
- **Storage Explorer** = GUI browsing
- **Data Box Gateway** = Continuous sync appliance

---

## Quick Reference: Online vs Offline

| Characteristic | Online | Offline |
|---|---|---|
| Speed | Network-limited | Local speed |
| Scale | Good for TB | Good for PB |
| Cost | Bandwidth charges | Hardware + shipping |
| Continuous | Yes (ADF, Gateway) | One-time |
| Setup | Quick | Weeks for PB |
| Ideal Size | < 100 TB | > 100 TB |
| Tools | AzCopy, ADF, Explorer | Data Box, Import/Export |

---

## Exam Scenarios: Data Movement

### Scenario 1: "Move 2 TB of files from on-prem to Blob Storage quickly"

**Answer:** AzCopy

**Why:** < 1 TB per day, online solution acceptable, fast, free

---

### Scenario 2: "We have a 500 TB data center to migrate offline"

**Answer:** Data Box Heavy

**Why:** Offline, PB-scale, Microsoft-managed hardware

---

### Scenario 3: "Branch office needs continuous file sync to Azure"

**Answer:** Data Box Gateway

**Why:** Ongoing, hybrid, continuous ingestion

---

### Scenario 4: "Migrate SQL Server to Azure SQL Database"

**Answer:** Azure Database Migration Service (DMS)

**Why:** Structured data, database-aware, minimal downtime

---

### Scenario 5: "We have BitLocker-encrypted disks we want to ship"

**Answer:** Import/Export

**Why:** Bring-your-own-disk, offline transfer

---

### Scenario 6: "Upload 10 TB in 1 week over slow WAN"

**Answer:** Data Box or Data Box Disk

**Why:** Offline transfer, WAN too slow for AzCopy

---

## Memory Tricks (Fast Exam Recall)

### "Bulk offline → Data Box"
If you're moving hundreds of TB and have time → Data Box family

### "Quick online → AzCopy"
If you need it fast and have good network → AzCopy

### "Continuous sync → Gateway"
If you need ongoing data flow → Data Box Gateway

### "Transform data → ADF"
If you need ETL/transformation → Azure Data Factory

### "Your disks → Import/Export"
If you have your own hardware → Import/Export

### "Your files → Storage Explorer"
If you're clicking around → Storage Explorer

---

## Final Data Movement Checklist

- [ ] Is this a database migration? → Azure DMS (not blob tools)
- [ ] Is this > 100 TB? → Data Box (offline)
- [ ] Is this < 100 TB online? → AzCopy
- [ ] Is this continuous sync? → Data Box Gateway
- [ ] Are you using your own disks? → Import/Export
- [ ] Do you need transformation? → ADF
- [ ] Are you exploring data? → Storage Explorer
- [ ] Is this for VM disks? → Azure Migrate (not blobs)
- [ ] Do you need Managed Disks? → Azure Migrate (not Data Box)
- [ ] Is this PaaS-to-PaaS? → ADF or service-specific tools
