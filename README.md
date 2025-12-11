

# ✅ **Snowflake SnowPro Certification – Complete Master Guide (Scratch → Advanced)**  
### *Based on your cheat sheet + expanded expert-level explanations*

---

# 🧊 **1. Snowflake Fundamentals**

## ✅ 1.1 What Snowflake Is  
Snowflake is a **cloud-native analytical data warehouse** with these characteristics:

### **Key Properties**
- Fully **SaaS** (no hardware, no installation, no patching)
- Runs only on **AWS, Azure, GCP** (no on-prem, no hybrid)
- Uses its own **VPC** inside the cloud provider
- **Separation of compute & storage**  
  → Scale each independently  
  → Pay only for what you use
- Zero maintenance (Snowflake handles tuning, vacuuming, indexing)

### **Architecture Type**
Hybrid of:
- **Shared-disk** → Centralized storage  
- **Shared-nothing** → Independent compute clusters (virtual warehouses)

---

# 🧊 **2. Snowflake Architecture (Exam Critical)**

## ✅ 2.1 Three-Layer Architecture

### **1. Storage Layer**
- Stores data in **micro-partitions** (compressed, columnar)
- Immutable files
- Optimized for analytical workloads
- Automatically encrypted

### **2. Compute Layer (Virtual Warehouses)**
- MPP clusters
- Independent compute
- No resource sharing between warehouses
- Can scale up/down anytime
- Can auto-suspend & auto-resume

### **3. Cloud Services Layer**
Controls:
- Authentication
- Metadata management
- Query parsing & optimization
- Access control
- Result cache
- Infrastructure management

This layer is **always on** and **shared** unless using VPS edition.

---

# 🧊 **3. Caching in Snowflake (High Exam Weight)**

| Cache Type | Layer | Purpose | Warehouse Required? |
|-----------|--------|----------|----------------------|
| **Metadata Cache** | Cloud Services | Speeds query compilation | ❌ No |
| **Result Cache** | Cloud Services | Returns results for identical queries within 24 hours | ❌ No |
| **Warehouse Cache** | Compute | Stores table data in SSD/memory | ✅ Yes |

---

# 🧊 **4. Data Loading (COPY, Stages, Snowpipe)**

## ✅ 4.1 File Locations
- Local → PUT → Internal Stage
- Cloud → External Stage (S3, Blob, GCS)

## ✅ 4.2 File Types
- CSV, TSV
- JSON, Parquet, ORC, XML (preview)
- Auto-detect compression (Snappy, Gzip)

## ✅ 4.3 Best Practices
- File size: **10–100 MB compressed**
- Parquet >3GB → split into 1GB
- VARIANT row limit: **16 MB compressed**

---

## ✅ 4.4 Stages (Internal vs External)

### **Internal Stages**
| Stage Type | Reference | Notes |
|------------|-----------|-------|
| User Stage | `@~` | Private to user |
| Table Stage | `@%table` | One per table |
| Named Stage | `@stage_name` | Most flexible |

### **External Stages**
- S3, Azure Blob, GCS
- Requires **Storage Integration** + IAM role

---

## ✅ 4.5 COPY INTO (Bulk Load)
Key features:
- Parallel loading
- Supports regex patterns
- Metadata stored for **64 days**
- Use `VALIDATION_MODE` to validate without loading
- Use `ON_ERROR` to control behavior

---

## ✅ 4.6 Snowpipe (Continuous Load)
Snowpipe = **serverless auto-ingest** using COPY under the hood.

| Feature | COPY | Snowpipe |
|---------|------|-----------|
| Warehouse | Required | ❌ Serverless |
| Load latency | Manual | Near real-time |
| Metadata retention | 64 days | 14 days |
| Cost | Warehouse credits | Snowpipe compute credits |

Automation via:
- S3 Event Notifications
- Azure Event Grid
- GCP Pub/Sub
- REST API

---

# 🧊 **5. Unloading Data (COPY INTO <location>)**

Supports:
- CSV
- JSON
- Parquet

Compression:
- gzip (default)
- bzip2
- Brotli
- Zstandard

Max file size:
- AWS/GCP: **5GB**
- Azure: **256MB**

---

# 🧊 **6. Virtual Warehouses (Compute Layer)**

## ✅ 6.1 Warehouse Sizes
XS → 4XL  
Performance scales **linearly**.

## ✅ 6.2 Multi-Cluster Warehouse
Modes:
- **Maximized** → All clusters always running
- **Auto-scale** → Scales between min/max

Policies:
- **Standard** → Reduce queuing (starts clusters quickly)
- **Economy** → Save credits (starts clusters slowly)

---

# 🧊 **7. Tables & Micro-Partitions**

## ✅ 7.1 Micro-Partition Characteristics
- 50–500 MB uncompressed
- 16 MB compressed
- Immutable
- Automatically clustered
- Metadata stored in Cloud Services

## ✅ 7.2 Clustering Keys
Use when:
- Table is **multi-terabyte**
- Natural clustering is poor
- Queries filter on specific columns

---

# 🧊 **8. Table Types**

| Type | Time Travel | Fail-safe | Use Case |
|------|-------------|-----------|----------|
| Temporary | 1 day | ❌ | Session-only |
| Transient | 1 day | ❌ | Cost-saving, staging |
| Permanent | 1–90 days | ✅ 7 days | Production |
| External | ❌ | ❌ | Query external data |

---

# 🧊 **9. Views**

| View Type | Stored? | Performance | Security | Cost |
|-----------|---------|-------------|----------|------|
| Regular View | ❌ | Slow | ✅ | ❌ |
| Materialized View | ✅ | Fast | ✅ | ✅ |
| Secure View | ❌/✅ | Slower | ✅✅ | ❌/✅ |

---

# 🧊 **10. Time Travel & Fail-safe**

## ✅ Time Travel
- 1–90 days
- Query, clone, restore historical data
- Useful for accidental deletes

## ✅ Fail-safe
- 7 days
- Only Snowflake can recover
- Disaster recovery only

---

# 🧊 **11. Data Sharing**

## ✅ Types
- **Share** → Between Snowflake accounts
- **Reader Account** → For users without Snowflake

## ✅ Key Points
- No data copying
- Metadata-based sharing
- Consumer pays compute

---

# 🧊 **12. Access Control (RBAC)**

## ✅ System Roles
- **ACCOUNTADMIN** → Full control
- **SECURITYADMIN** → Users, roles, grants
- **SYSADMIN** → Create warehouses, DB objects
- **PUBLIC** → Default minimal access

## ✅ RBAC Model
- Privileges → Roles → Users

---

# 🧊 **13. Security**

## ✅ Encryption
- End-to-end encryption
- Hierarchical key model:
  - Root Key
  - Account Master Key
  - Table Master Key
  - File Key

## ✅ Tri-Secret Secure (Business Critical)
- Customer key + Snowflake key + cloud provider key
