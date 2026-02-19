# Azure Blob Storage Tiers vs AWS S3 Storage Classes

## 1. Azure Blob Tiers vs AWS S3 Storage Classes (Full Mapping)

| Azure Blob Tier | Closest AWS S3 Equivalent | How Similar? | Notes |
|---|---|---|---|
| Hot | S3 Standard | ⭐⭐⭐⭐⭐ | Same use case: frequent access, low latency |
| Cool | S3 Standard‑IA / S3 One Zone‑IA | ⭐⭐⭐⭐ | Same idea: infrequent access, higher read cost |
| Archive | S3 Glacier Flexible Retrieval / Glacier Deep Archive | ⭐⭐⭐ | Azure Archive sits between Glacier and Deep Archive |
| Premium | No direct S3 equivalent | ⭐ | SSD‑backed object storage; AWS doesn't offer this |

---

## 2. Azure Hot Tier (Like S3 Standard)

**Use cases:**
- Active apps
- Real‑time data
- High‑frequency reads/writes

**Characteristics:**
- Lowest latency
- Highest cost per GB
- Lowest access cost

**AWS analogy:** Exactly like S3 Standard

---

## 3. Azure Cool Tier (Like S3 Standard‑IA)

**Use cases:**
- Backup data
- Infrequently accessed documents
- Disaster recovery

**Characteristics:**
- Cheaper storage than Hot
- Higher read and write costs
- Minimum retention: 30 days

**AWS analogy:**
- S3 Standard‑IA (multi‑AZ)
- S3 One Zone‑IA (if Azure uses LRS)

Azure Cool is basically IA with a 30‑day minimum.

---

## 4. Azure Archive Tier (Like Glacier / Deep Archive)

**Use cases:**
- Long‑term retention
- Compliance storage
- Rarely accessed data

**Characteristics:**
- Very cheap storage
- High retrieval cost
- Retrieval latency: hours
- Minimum retention: 180 days

**AWS analogy:**
- Glacier Flexible Retrieval (similar retrieval time)
- Glacier Deep Archive (similar cost)

Azure Archive sits between the two AWS Glacier classes.

---

## 5. Azure Premium Tier (No S3 Equivalent)

**Use cases:**
- High‑performance workloads
- Event streaming
- Big data analytics
- Low‑latency transactional workloads

**Characteristics:**
- SSD‑backed object storage
- Very low latency
- High cost per GB

**AWS analogy:** AWS S3 does not offer SSD‑backed object storage. Closest conceptual match is EBS Provisioned IOPS, but that's block storage, not object storage.

---

## 6. Lifecycle Management: Azure vs AWS

### Azure Lifecycle Rules
- Move Hot → Cool → Archive
- Delete after X days
- Tier based on last access time (optional)

### AWS Lifecycle Rules
- Move Standard → IA → Glacier → Deep Archive
- Delete after X days
- Intelligent Tiering (Azure has no equivalent)

**Key difference:** AWS has Intelligent‑Tiering, Azure does not. Azure requires explicit rules.

---

## 7. Redundancy Mapping (Azure vs AWS)

Azure has more redundancy flavors than AWS. Here's the mapping:

| Azure Redundancy | AWS Equivalent | Notes |
|---|---|---|
| LRS (Local) | S3 One Zone | Single AZ |
| ZRS (Zone‑Redundant) | No direct equivalent | Azure stores across 3 AZs automatically |
| GRS (Geo‑Redundant) | S3 Cross‑Region Replication | Azure auto‑replicates to paired region |
| RA‑GRS (Read‑Access GRS) | S3 CRR + read from replica | Azure exposes a read‑only secondary endpoint |

**Note:** Azure's ZRS is unique — AWS doesn't have a multi‑AZ object storage class by default.

---

## 8. Retrieval Times Comparison

| Tier | Azure Retrieval | AWS Retrieval |
|---|---|---|
| Hot | Instant | Instant |
| Cool | Instant | Instant |
| Archive | Hours | Glacier: minutes–hours / Deep Archive: hours–days |

**Key point:** Azure Archive ≈ Glacier Flexible Retrieval. Azure Archive ≠ Deep Archive (Deep Archive is slower).

---

## 9. Cost Model Comparison

### Azure
- **Hot:** expensive storage, cheap access
- **Cool:** cheaper storage, expensive access
- **Archive:** extremely cheap storage, expensive access

### AWS
- **Standard:** expensive storage, cheap access
- **IA:** cheaper storage, expensive access
- **Glacier:** very cheap storage, expensive access
- **Deep Archive:** extremely cheap storage, very slow retrieval

Azure Archive is priced between Glacier and Deep Archive.

---

## 10. The Mental Model (The Shortcut)

If you want the simplest way to think about Azure tiers:

- **Hot** = S3 Standard
- **Cool** = S3 Standard‑IA
- **Archive** = Glacier / Deep Archive hybrid
- **Premium** = SSD object storage (AWS doesn't have this)

Everything else is just detail.
