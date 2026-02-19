# Azure VM Compute Types - Quick Reference Guide

## VM Size Naming Convention

### Basic Format
```
Standard_D4s_v5

Where:
  D     = Family (General Purpose)
  4     = vCPU count
  s     = Premium Storage capable
  v5    = Generation/Version
```

### Key Characters
- **First letter(s)**: Family type (A, B, D, F, E, M, L, N, H, etc.)
- **Number**: Number of vCPUs
- **Letter suffix**: Features (s = premium storage, d = local disk, a = AMD, p = ARM/Cobalt)
- **Version**: v5, v6, etc.

---

## Quick Decision Tree

```
What's your use case?

1. General workloads?
   → General Purpose (D, A, B families)

2. Need lots of CPU power?
   → Compute Optimized (F, FX families)

3. Need lots of RAM/Memory?
   → Memory Optimized (E, M families)

4. Heavy disk I/O?
   → Storage Optimized (L family)

5. GPU for AI/Graphics?
   → GPU Accelerated (NC, ND, NV, NG families)

6. Need FPGA?
   → FPGA Accelerated (NP family)

7. Scientific/HPC workloads?
   → High Performance Compute (HB, HC, HX families)
```

---

## General Purpose VMs

### When to Use
- Development/testing environments
- Small to medium apps
- General web servers
- Business applications

### Best For
- Balanced CPU and memory
- Production workloads
- Enterprise applications

### Key Families

#### A-Family (Budget)
- **Entry-level, cheap**
- Small workloads only
- Dev/test only
- **Example**: Standard_A1
- **Use**: Proof of concept, light testing

#### B-Family (Burstable)
- **CPU credit system** - accumulate credits at idle, spend during spikes
- Unpredictable workloads
- Low-traffic applications
- **Example**: Standard_B2s
- **Use**: Blogs, dev machines, monitoring agents
- **Note**: Can throttle if you run out of credits

#### D-Family (Sweet Spot) ⭐
- **Most common for production**
- Balanced CPU-to-memory (1:4 ratio)
- Multiple generations (v5, v6, v7)
- **Example**: Standard_D4s_v5
- **Use**: Most production apps, databases, app servers
- **CPU options**: Intel (default), AMD (Dav5), ARM/Cobalt (Dpv6)

#### DC-Family (Confidential)
- D-family + **encryption/security**
- Hardware-based Trusted Execution Environment (TEE)
- **Example**: Standard_DC4s_v5
- **Use**: Sensitive data, financial, healthcare, regulated workloads
- **Cost**: Premium over standard D-series

---

## Compute Optimized VMs

### When to Use
- High CPU relative to memory
- Need processing speed, not storage

### CPU-to-Memory Ratio
- General Purpose: 1:4 (4GB RAM per vCPU)
- **Compute Optimized: 1:2 (2GB RAM per vCPU)**

### Key Families

#### F-Family
- **High CPU power, lower memory**
- **Example**: Standard_F8s_v2
- **Use**: 
  - Web servers (high traffic)
  - Batch processing
  - Analytics
  - Gaming servers
  - Lightweight databases

#### FX-Family (Premium Compute)
- **Very high single-core performance**
- Intel Ice Lake, high frequency
- Large L3 cache
- **Example**: Standard_FX4mdv5
- **Use**:
  - Electronic Design Automation (EDA)
  - Financial modeling
  - Scientific simulations
  - Real-time data processing

---

## Memory Optimized VMs

### When to Use
- Large amounts of RAM needed
- In-memory processing
- High-speed data operations

### Memory-to-CPU Ratio
- Compute Optimized: 1:2
- General Purpose: 1:4
- **Memory Optimized: 1:8 or higher**

### Key Families

#### E-Family
- **High memory with good CPU**
- Memory-to-CPU: 1:8
- **Example**: Standard_E8s_v5
- **Use**:
  - Large relational databases (SQL Server)
  - In-memory databases (SAP HANA, Redis)
  - Big data analytics
  - Data warehousing
  - Business intelligence

#### Eb-Family
- E-family + **high remote storage performance**
- Better for applications needing both memory and storage I/O
- **Example**: Standard_Eb8sds_v5

#### EC-Family
- E-family + **confidential computing**
- Hardware-based encryption
- **Example**: Standard_EC4ads_v5
- **Use**: Sensitive big data, regulated analytics

#### M-Family (Ultra-High Memory)
- **Extreme memory capacity**
- Memory-to-CPU: 1:16 or higher
- Highest cost
- **Example**: Standard_M208ms_v2
- **Use**:
  - Massive databases (>1TB)
  - In-memory SAP HANA
  - Heavy ERP systems
  - Real-time financial platforms

---

## Storage Optimized VMs

### When to Use
- High disk throughput needed
- Heavy sequential read/write
- Local disk I/O matters

### Characteristics
- Large local disk capacity
- High disk throughput (IOPS)
- Lower memory-to-CPU

### Key Families

#### L-Family
- **High disk throughput and capacity**
- **Example**: Standard_L8s_v3
- **Use**:
  - NoSQL databases (Cassandra, MongoDB)
  - Large transactional databases
  - Data warehousing
  - Big data processing
  - Log processing
  - Real-time analytics

---

## GPU Accelerated VMs

### When to Use
- Machine learning / AI
- Graphics rendering
- Video processing
- High-performance compute

### GPU Options
- **NVIDIA**: A100, H100, V100, T4 (most common)
- **AMD**: Radeon PRO

### Key Families

#### NC-Family (NVIDIA Compute)
- **AI/ML training and inference**
- NVIDIA GPUs (H100, A100, V100, T4)
- **Example**: Standard_NC24s_v3
- **Use**:
  - Deep learning training
  - AI/ML model development
  - HPC simulations
  - 3D rendering
  - Scientific visualization
- **Cost**: High

#### ND-Family (NVIDIA Deep Learning)
- **Large-scale deep learning**
- Massive NVIDIA GPUs (MI300X, H100)
- High memory for large models
- **Example**: Standard_ND_H100_v5
- **Use**:
  - Large AI model training
  - Research/academic AI
  - Foundation model work
  - Extreme compute needs
- **Cost**: Very high

#### NV-Family (NVIDIA Visualization)
- **Graphics and visualization**
- NVIDIA GPUs optimized for graphics
- **Example**: Standard_NV6_v4
- **Use**:
  - Virtual Desktop (VDI)
  - CAD/design work (AutoCAD, SolidWorks)
  - Video editing
  - 3D visualization
  - Remote graphics workstations

#### NG-Family (AMD Radeon)
- **Cloud gaming and remote desktop**
- AMD Radeon PRO GPUs
- **Example**: Standard_NGads_V620
- **Use**:
  - Cloud gaming
  - Remote desktops
  - Virtual workstations

---

## FPGA Accelerated VMs

### When to Use
- Custom hardware acceleration
- Specialized algorithms
- Real-time processing
- Specific inference tasks

### Key Families

#### NP-Family
- **Field Programmable Gate Arrays**
- Programmable hardware
- **Example**: Standard_NP10s
- **Use**:
  - Custom ML inference
  - Video transcoding
  - Real-time signal processing
  - Genomics acceleration
  - Database queries
  - Financial trading
- **Note**: Requires FPGA expertise, not common

---

## High Performance Compute (HPC) VMs

### When to Use
- Scientific simulations
- Research computing
- Massive parallel tasks
- Extreme performance needs

### Key Families

#### HB-Family (High Bandwidth)
- **AMD EPYC processors**
- Extreme memory bandwidth
- **Example**: Standard_HBv5
- **Use**:
  - Computational Fluid Dynamics (CFD)
  - Weather modeling
  - Seismic processing
  - Molecular dynamics
  - Scientific research

#### HC-Family (High Compute)
- **Intel Xeon Scalable**
- High single-thread performance
- **Example**: Standard_HC44rs
- **Use**:
  - Genomic sequencing
  - Finite Element Analysis (FEA)
  - Engineering simulations
  - Financial modeling
  - Molecular simulations

#### HX-Family (High Memory)
- **Massive memory + CPU power**
- Combines memory and compute
- **Example**: Standard_HX176rs
- **Use**:
  - Large in-memory databases
  - Big data analytics
  - Complex simulations
  - ERP systems
  - Real-time analytics

---

## Size Selection Cheat Sheet

| Need | Best Family | Example | Cost |
|------|-------------|---------|------|
| Development | A or B | A1, B2s | $ |
| Production web app | D | D4s_v5 | $$ |
| Database server | E | E8s_v5 | $$$ |
| Data warehouse | L | L8s_v3 | $$$ |
| Very large DB | M | M208ms_v2 | $$$$ |
| AI/ML training | ND | ND_H100_v5 | $$$$ |
| 3D rendering | NV | NV6_v4 | $$$ |
| Scientific compute | HC | HC44rs | $$$$ |
| Sensitive data | DC or EC | DC4s_v5 | $$$ |
| Budget test | A | A2_v2 | $ |

---

## CPU Generations (Versions)

### Naming Pattern
- **v5/v6/v7** = Newer generations
- **v7** = Latest (fastest, most efficient)
- **v5** = Proven, stable, good value

### Rule of Thumb
- **For new workloads**: Use latest (v7) for efficiency
- **For existing workloads**: Match current version or upgrade if budget allows
- **Cost difference**: v5 → v6 → v7 (each newer = slightly more expensive)

---

## CPU Vendors in Azure VMs

### Intel (Default)
- `Standard_D4s_v5` (no suffix = Intel)
- Widest application support
- **Example**: D, F, E, NC, NV families

### AMD
- `Standard_D4ads_v5` (has "a" = AMD)
- Good price/performance
- **Example**: Da, Fa, Ea families

### ARM-Based (Microsoft Cobalt)
- `Standard_D4ps_v6` (has "p" = ARM)
- Newest, most efficient power
- Limited availability
- **Example**: Dp, Fp families

---

## Quick Selection Guide

### "I need to save money"
→ **A-family** or **B-family** (burstable)

### "I need something that works for everything"
→ **D-family** (most popular, well-balanced)

### "I have a database"
→ Large: **M-family**
→ Medium: **E-family**
→ Small: **D-family**

### "I'm doing AI/ML"
→ Training: **ND-family**
→ Inference: **NC-family**

### "I need high throughput I/O"
→ **L-family** (storage)

### "I need extreme performance"
→ **HB/HC/HX families** (HPC)

### "I have sensitive/regulated data"
→ **DC or EC family** (confidential computing)

### "I'm doing graphics work remotely"
→ **NV-family** or **NG-family**

---

## Memory-to-CPU Ratios Summary

| Type | Ratio | Example | Use Case |
|------|-------|---------|----------|
| Compute | 1:2 | F4s (4 vCPU, 8GB RAM) | CPU-heavy tasks |
| General | 1:4 | D4s (4 vCPU, 16GB RAM) | Most workloads |
| Memory | 1:8 | E8s (8 vCPU, 64GB RAM) | Databases |
| Ultra Memory | 1:16 | M128s (128 vCPU, 2TB+ RAM) | Giant databases |

---

## Storage Options

### Local SSD (ephemeral)
- Fast, cheap per operation
- Lost if VM stops (not hibernated)
- Good for: Temp files, cache, logs

### Premium SSD (persistent)
- Fast, reliable
- Survives VM restart
- Recommended for: Databases, apps

### Standard HDD (persistent)
- Slow, cheap
- Survives VM restart
- Good for: Backups, archives, non-critical

---

## For AZ-305 Exam

### Know These
1. **D-family = default go-to** for general purpose
2. **E-family = memory-intensive** (databases, SAP HANA)
3. **F-family = compute-heavy** (batch, analytics)
4. **L-family = storage-heavy I/O** (databases with lots of I/O)
5. **GPU families**: NC (AI), NV (graphics), ND (deep learning)
6. **HPC families**: HC (compute), HB (bandwidth), HX (memory)

### Likely Exam Scenarios
- "Which VM for SQL Server with 500GB data?" → **E-family or M-family**
- "Which VM for AI model training?" → **ND-family**
- "Minimize cost for dev environment?" → **B-family (burstable)**
- "High-throughput database reads/writes?" → **L-family**
- "Confidential data processing?" → **DC or EC family**

### Avoid Common Mistakes
- ❌ Using F (compute) for database with big memory needs (use E instead)
- ❌ Using D (general) for AI training (use GPU family)
- ❌ Using A (entry-level) for production workloads (use D minimum)
- ❌ Forgetting about B (burstable) for intermittent workloads (saves money)

---

## Key Points to Remember

- **D-family is the Swiss Army knife**: Works for most things, good balance
- **B-family saves money**: Burstable is perfect for unpredictable workloads
- **Memory matters more than CPU for databases**: Use E/M families, not D
- **Compute-heavy needs F, not D**: Higher CPU-to-memory ratio
- **GPU is expensive**: Only for AI/ML/rendering work
- **Newer versions (v7) are better**: Faster, more efficient, but slightly pricier
- **Confidential computing adds cost**: DC/EC families for regulated workloads
- **HPC is extreme**: For research/scientific computing, not typical business
