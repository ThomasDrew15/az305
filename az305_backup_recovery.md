# AZ‑305 Backup & Recovery — Ultimate Cheat Sheet

---

## 1. First Principle: Backup ≠ Disaster Recovery

### Backup = Restore Data

- Restore a VM
- Restore a file
- Restore a database
- Long‑term retention
- Point‑in‑time restore

### Disaster Recovery (DR) = Keep the App Running

- Failover
- Secondary region
- Low RPO/RTO
- Business continuity

### Exam Trigger Words

**If the question says "failover"** → ASR

**If the question says "restore"** → Backup

---

## 2. The Decision Tree (Architect's Flow)

### STEP 1 — What Do You Need?

**A. Restore data?**
- → Azure Backup
- → SQL Automated Backups
- → Azure Files Backup

**B. Keep the app running during an outage?**
- → Azure Site Recovery (ASR)

---

### STEP 2 — What Are You Protecting?

#### Azure VM

- **Backup** → Azure Backup (RSV)
- **DR** → ASR

#### Azure Files

- **Backup** → Azure Backup for Azure Files
- **Quick restore** → Snapshots

#### Azure SQL Database / Managed Instance

- **Backup** → Built‑in automated backups
- **DR** → Geo‑replication

#### On‑prem Servers

- **Backup** → MARS/MABS
- **DR** → ASR

#### Blob Storage

- **Accidental deletion** → Soft Delete
- **Versioning** → Blob versioning
- **Long-term retention** → Archive tier

---

### STEP 3 — Special Requirements

**Need long-term retention (years)?**
- Azure Backup LTR
- SQL LTR

**Need file-level restore?**
- Azure Backup (VM)
- Azure Files Backup
- MARS agent

**Need region-to-region failover?**
- ASR
- SQL Geo-replication

**Need protection against accidental deletion?**
- Soft Delete (Blobs, Files, Key Vault, Backup vaults)

---

## 3. The Exam Traps (and How to Avoid Them)

### TRAP 1 — Confusing Backup with DR

**Wrong:** "We need disaster recovery, so use Azure Backup"
**Right:** 
- "Failover", "secondary region", "RPO/RTO" → ASR
- "Restore", "retention", "point‑in‑time" → Backup

---

### TRAP 2 — Thinking Azure SQL Needs Azure Backup

**Wrong:** "Back up Azure SQL with Azure Backup"
**Right:** Azure SQL has built-in:
- Automatic backups
- PITR (Point-in-Time Restore)
- Geo‑restore
- LTR (Long-Term Retention)

**Correct answer = built‑in SQL backups**

---

### TRAP 3 — Thinking Azure Backup Protects Everything

**Wrong:** "Azure Backup backs up all my resources"
**Right:** Azure Backup does NOT protect:
- Cosmos DB
- Azure AD
- Key Vault
- Storage accounts (except Azure Files)
- AKS
- App Service content (unless configured)

**PaaS data** → Use the service's built‑in backup features

---

### TRAP 4 — Confusing Azure Files Snapshots with Azure Backup

**Wrong:** "Snapshots provide retention policies"
**Right:**
- "Retention policy", "backup", "file-level restore" → Azure Backup
- "Quick snapshot", "instant restore" → Snapshots

---

### TRAP 5 — Thinking ASR Protects Data

**Wrong:** "ASR backs up my data"
**Right:** ASR replicates, it does NOT:
- Keep long-term retention
- Provide PITR
- Protect against corruption
- Protect against accidental deletion

**ASR ≠ Backup**

---

### TRAP 6 — Forgetting Soft Delete Exists

**Wrong:** "Use backup to recover deleted files"
**Right:** If the question says:
- "Accidental deletion"
- "Recover deleted blob/file/secret"
- → Soft Delete, not Backup

---

### TRAP 7 — Thinking Snapshots = Backups

**Wrong:** "Snapshots provide long-term retention"
**Right:** Snapshots are:
- Local (not geographically redundant)
- Not durable for extended periods
- Not regionally redundant
- Not for long-term retention

**Snapshots ≠ Backup**

---

## 4. What Each Service Actually Does

### Azure Backup (Recovery Services Vault)

**Capabilities:**
- VM backup
- Azure Files backup
- SQL in VM backup
- On‑prem backup (MARS/MABS)
- Retention policies (days/weeks/months/years)
- File-level restore
- Long-term retention
- Cross-region restore

**Use For:** Data restore, file-level recovery, long-term retention

---

### Azure Site Recovery (ASR)

**Capabilities:**
- VM replication (source → target)
- Region-to-region failover
- Low RPO/RTO
- Orchestrated recovery
- Business continuity
- Automated failover

**Use For:** Disaster recovery, keeping apps online during regional outages

---

### Azure SQL Automated Backups

**Capabilities:**
- PITR (Point-in-Time Restore) — up to 35 days
- Geo‑restore (to different region)
- LTR (Long-Term Retention) — multi-year
- No vault required
- Built-in, automatic
- No cost for PITR

**Use For:** SQL PaaS restore, compliance retention

---

### Azure Files Backup

**Capabilities:**
- Snapshot-based
- File-level restore
- Retention policies
- Share-level restore

**Use For:** Azure Files protection with retention policies

---

### Soft Delete

**Protects:**
- Blobs
- Azure Files
- Key Vault
- Backup vaults

**What It Does:** Recovers accidental deletions (within retention period)

**Use For:** Safety net against deletion

---

### Snapshots (VMs, Blobs, Azure Files)

**Capabilities:**
- Instant creation
- Point-in-time image
- Local storage

**Limitations:**
- Not long-term
- Not regionally redundant
- Not a backup strategy alone

**Use For:** Quick rollback, not long-term backup

---

## 5. The One‑Sentence Summary

- **Backup** = Restore data
- **ASR** = Keep the app alive
- **SQL PaaS** = Built-in backups
- **Azure Files** = Snapshot + Backup
- **Soft Delete** = Safety net

---

## Backup & DR Scenario Guide (10 Real Exam Questions)

### Scenario 1 — "We Deleted a VM File Yesterday"

**Situation:** Your team accidentally deleted a critical file from an Azure VM's OS disk. You need to restore just that file, not the whole VM.

**What do you use?**

**Correct Answer:** 👉 **Azure Backup (VM backup) — File-level restore**

**Why:**
- VM backups allow file-level restore
- Snapshots don't give file-level restore
- ASR is not backup
- Soft delete doesn't apply to VM disks

---

### Scenario 2 — "Region Outage, Keep the App Running"

**Situation:** Your production app must stay online even if the entire Azure region fails. You need automatic failover to a secondary region.

**What do you use?**

**Correct Answer:** 👉 **Azure Site Recovery (ASR)**

**Why:**
- ASR = DR + failover
- Backup cannot keep apps running
- Snapshots are local
- SQL PITR is not DR

---

### Scenario 3 — "Restore SQL Database to Last Tuesday"

**Situation:** You have an Azure SQL Database (PaaS). A developer corrupted data yesterday. You need to restore the database to a point in time from last week.

**What do you use?**

**Correct Answer:** 👉 **Azure SQL Automated Backups (PITR)**

**Why:**
- Azure SQL has built‑in PITR
- Azure Backup does NOT back up PaaS SQL
- ASR is not for data restore
- Snapshots don't apply to SQL PaaS

---

### Scenario 4 — "Protect Azure Files Share with Retention"

**Situation:** You have an Azure Files share storing HR documents. You need daily backups with retention policies and file-level restore.

**What do you use?**

**Correct Answer:** 👉 **Azure Backup for Azure Files**

**Why:**
- Azure Files snapshots alone don't give retention
- Backup vault gives retention + restore
- ASR doesn't protect file shares
- Soft delete only protects against deletion

---

### Scenario 5 — "Recover a Deleted Blob"

**Situation:** A user accidentally deleted a blob from a storage account. You need to restore it quickly.

**What do you use?**

**Correct Answer:** 👉 **Soft Delete for Blobs**

**Why:**
- Soft delete protects against accidental deletion
- Backup doesn't apply to blobs
- Snapshots might exist, but soft delete is the intended mechanism
- ASR irrelevant

---

### Scenario 6 — "Long-Term Retention for SQL (7 Years)"

**Situation:** Your compliance team requires 7‑year retention for Azure SQL Database backups.

**What do you use?**

**Correct Answer:** 👉 **Azure SQL LTR (Long-Term Retention)**

**Why:**
- Built-in SQL LTR supports multi‑year retention
- Azure Backup does not back up SQL PaaS
- ASR is not backup
- Snapshots don't apply

---

### Scenario 7 — "On-Prem Server Backup to Azure"

**Situation:** You have an on‑prem Windows Server. You need to back up files to Azure.

**What do you use?**

**Correct Answer:** 👉 **MARS agent (Microsoft Azure Recovery Services agent)**

Or 👉 **MABS (Microsoft Azure Backup Server)** for larger environments

**Why:**
- MARS = file/folder backup to Azure
- MABS = full on‑prem backup suite
- ASR is DR, not backup
- Azure Backup VM backup only works for Azure VMs

---

### Scenario 8 — "VM Corruption, Restore Entire VM"

**Situation:** Your Azure VM becomes corrupted. You need to restore the entire VM to a previous point.

**What do you use?**

**Correct Answer:** 👉 **Azure Backup (VM restore)**

**Why:**
- VM backup supports full VM restore
- ASR is for failover, not restore
- Snapshots are not long-term
- Soft delete doesn't apply

---

### Scenario 9 — "Need Instant Rollback for Azure Files"

**Situation:** You want to quickly revert an Azure Files share to a previous state from 10 minutes ago.

**What do you use?**

**Correct Answer:** 👉 **Azure Files Snapshots**

**Why:**
- Snapshots = instant
- Backup = scheduled
- ASR irrelevant
- Soft delete only protects deletions

---

### Scenario 10 — "Protect VM + Failover to Another Region"

**Situation:** You need both backup and DR for an Azure VM.

**What do you use?**

**Correct Answer:** 👉 **Azure Backup + Azure Site Recovery**

**Why:**
- Backup = restore
- ASR = failover
- They solve different problems
- **Exam loves this combo**

---

## Quick Reference Table (Services at a Glance)

| Service | Purpose | Scope | Best For | Note |
|---|---|---|---|---|
| **Azure Backup** | Data restore | VM, Files, SQL in VM, on-prem | File/VM/DB restore, LTR | Requires RSV |
| **Azure Site Recovery** | Disaster recovery | VM, servers | Failover, business continuity | Low RPO/RTO |
| **SQL PITR** | Point-in-time restore | Azure SQL PaaS | SQL restore | Built-in, free |
| **SQL LTR** | Long-term retention | Azure SQL PaaS | Compliance retention | Multi-year |
| **Azure Files Backup** | File share backup | Azure Files | Share protection | Snapshot-based |
| **Soft Delete** | Accidental deletion | Blobs, Files, Key Vault | Deletion recovery | Limited retention |
| **Snapshots** | Point-in-time image | VMs, Blobs, Files | Quick rollback | Not long-term |
| **MARS Agent** | On-prem backup | Windows Server | On-prem to Azure | File/folder level |
| **MABS** | Full on-prem backup | Large on-prem | Enterprise backup suite | Workload protection |

---

## Memory Tricks (Fast Recall)

### "Failover = ASR"

Any time you hear "failover", "secondary region", or "RTO" → **Azure Site Recovery**

### "Restore = Backup"

Any time you hear "restore", "retention", "PITR" → **Azure Backup** or **service-specific backups**

### "SQL does itself"

Azure SQL Database/Managed Instance have **built-in backups**. Don't use Azure Backup for SQL PaaS.

### "Oops, I deleted it = Soft Delete"

Accidental deletion → **Soft Delete**, not Backup

### "Quick snapshot = Snapshots"

Need instant rollback → **Snapshots**

Need retention and scheduling → **Backup**

### "On-prem = MARS/MABS"

On-premises backup to Azure → **MARS (small)** or **MABS (large)**

### "App keeps running = ASR"

Need business continuity → **Azure Site Recovery (ASR)**

### "Data comes back = Azure Backup"

Need to restore files/VMs/databases → **Azure Backup**

---

## Final Exam Checklist

- [ ] Is this about failover or keeping the app running? → ASR
- [ ] Is this about restoring data? → Azure Backup or service-specific
- [ ] Is this about Azure SQL? → Built-in backups (PITR/LTR)
- [ ] Is this about accidental deletion? → Soft Delete
- [ ] Is this about instant rollback? → Snapshots
- [ ] Is this about on-prem backup? → MARS/MABS
- [ ] Is this about Azure Files? → Azure Backup for Azure Files
- [ ] Do we need both backup AND DR? → Use both services
- [ ] Is this PaaS (Cosmos, AD, etc.)? → Use service-specific backups
- [ ] Is this long-term retention? → Azure Backup LTR or SQL LTR

---

## ⭐ The Only Rule You Need (RPO/RTO)

**If it's BACKUP** → RPO is hours, RTO is hours

**If it's ASR** → RPO is seconds/minutes, RTO is minutes

Everything else is just detail.

Let's break it down so it sticks.

---

## 1. Azure Backup — "Restore Later"

Azure Backup is scheduled, not continuous.

### RPO (How Much Data You Lose)

**Hours** — Because backups run daily or every few hours

### RTO (How Long to Restore)

**Hours** — Because restoring a VM or file takes time

### Easy Memory Trick

**Backup = Batch = Hours**

If it's a batch job, it's not real‑time.

---

## 2. Azure Site Recovery (ASR) — "Keep the App Alive"

ASR is continuous replication, not scheduled.

### RPO

**Seconds to Minutes** — Because it's constantly replicating changes

### RTO

**Minutes** — Because failover is orchestrated and fast

### Easy Memory Trick

**ASR = Always Sending Replicas**

If it's always sending replicas, RPO is tiny.

---

## 3. Azure SQL Database (PaaS) — "Built‑In PITR"

### RPO

**5–10 Minutes (Typical)** — Because SQL takes frequent log backups

### RTO

**Minutes** — Because restoring a PaaS DB is fast

### Easy Memory Trick

**SQL = Small Gaps Log‑based**

It's not continuous, but it's close.

---

## 4. Azure Files Snapshots — "Instant Rollback"

### RPO

**Depends on When You Took the Snapshot** — Could be minutes, hours, days

### RTO

**Seconds** — Snapshots are instant to revert

### Easy Memory Trick

**Snapshots = "Photos"**

You can only go back to when the photo was taken.

---

## 5. Soft Delete — "Accidental Deletion Protection"

### RPO

**Zero** — Because the deleted object is still there

### RTO

**Seconds** — Just undelete it

### Easy Memory Trick

**Soft Delete = Recycle Bin**

No data loss, instant recovery.

---

## The Architect's RTO/RPO Table (The One to Memorise)

| Service | RPO | RTO | Why |
|---|---|---|---|
| **Azure Backup** | Hours | Hours | Scheduled backups |
| **ASR** | Seconds–minutes | Minutes | Continuous replication |
| **SQL PaaS PITR** | 5–10 min | Minutes | Frequent log backups |
| **Azure Files Snapshots** | Snapshot interval | Seconds | Instant revert |
| **Soft Delete** | Zero | Seconds | Just undelete |

---

## The One‑Sentence Memory Trick (EXAM GOLD)

**Backup = Hours / Hours**

**ASR = Minutes / Minutes**

**SQL = Minutes / Minutes**

**Snapshots = Depends / Instant**

**Soft Delete = Zero / Instant**

If you remember that, you'll never miss an RTO/RPO question again.

---

## RPO/RTO Exam Scenarios

### Scenario: "We need RPO < 5 minutes and RTO < 1 hour"

**Answer:** Azure Site Recovery (ASR)

**Why:** Only ASR gives RPO of minutes; backups are hours

---

### Scenario: "Database must be recoverable to within 10 minutes of failure"

**Answer:** Azure SQL PITR (built-in)

**Why:** SQL PITR has 5-10 min RPO, which meets requirement

---

### Scenario: "We can tolerate losing 4 hours of data, but need restore in < 1 hour"

**Answer:** Azure Backup

**Why:** 4 hours RPO fits backup schedule; hours RTO is typical

---

### Scenario: "Critical app needs RPO < 1 minute, RTO < 5 minutes"

**Answer:** Azure Site Recovery (ASR)

**Why:** Only ASR meets sub-minute RPO and sub-5-minute RTO

---

## Final RPO/RTO Checklist

- [ ] RPO < 1 minute? → ASR only
- [ ] RPO < 10 minutes? → ASR or SQL PITR
- [ ] RPO = hours? → Azure Backup
- [ ] RTO < 5 minutes? → ASR, Snapshots, or Soft Delete
- [ ] RTO = hours? → Azure Backup
- [ ] Accidental deletion? → Soft Delete (RTO = seconds)
- [ ] Quick rollback? → Snapshots (RTO = seconds)
- [ ] Long-term retention? → Azure Backup LTR (RPO/RTO don't matter for LTR)
