# 🗄️ AWS Database Services — RDS & Amazon Aurora



## 1️⃣ Amazon RDS (Relational Database Service)

### 🔹 Overview
- Fully managed **relational database service** by AWS.
- Supports **MySQL, PostgreSQL, MariaDB, Oracle, SQL Server**.
- Automates:
  - Provisioning
  - Patching
  - Backups
  - Scaling
- Supports **point-in-time restore**.
- **Read replicas** improve read performance.
- **Multi-AZ deployments** for **high availability (HA)** and **disaster recovery (DR)**.

---

### ⚙️ Auto Scaling Feature
- Automatically scales storage when enabled.
- Define a **maximum threshold**.
- RDS triggers auto-scaling if:
  1. Low storage persists ≥ 5 minutes.
  2. Free storage < 10%.
  3. ≥ 6 hours since the last modification.

---

### 🧩 Read Replicas vs Multi-AZ

#### **Read Replicas**
- Improve **read performance** for read-heavy workloads.
- **Asynchronous replication** from the main DB.
- **Same-AZ replicas** are free; **cross-AZ replicas** are chargeable.
- Up to **15 read replicas** per database.
- Can be **promoted** to standalone DBs.
- **Use case:** Offload analytics/reporting from production.
- **Read-only** capability.

#### **Multi-AZ**
- Provides **high availability** and **failover** capability.
- Uses **synchronous replication** to standby DB.
- Offers a **single DNS endpoint** with automatic failover.
- **No downtime** when enabling Multi-AZ.
- Workflow:
  - Main DB → Snapshot → Create Standby → Sync Replication.

---

### 🧠 RDS Custom
- Provides **OS-level access** (for Oracle & SQL Server only).
- Allows **SSH / SSM** access to underlying EC2.
- Best practice:
  - Disable **automation mode**.
  - Take a **snapshot** before custom changes.
- **RDS Custom:** Full admin access.
- **Standard RDS:** Fully managed, no OS access.

---

### 🧱 Basic Structure
- **DB instance** = basic building block (isolated DB environment).
- Up to **40 DB instances**.
- Each instance runs one **DB engine**.

---

## 2️⃣ Amazon Aurora

### 🔹 Overview
- AWS’s **cloud-optimized relational database**.
- Compatible with **MySQL** and **PostgreSQL**.
- Up to **15+ read replicas**.
- **5× faster** than RDS MySQL, **3× faster** than RDS PostgreSQL.
- **Auto-scales** from **10GB → 128TB**.
- **20% more expensive** than RDS.
- Maintains **6 copies of data** across **3 AZs**:
  - 4/6 for writes, 3/6 for reads.
- **Self-healing**, **fault-tolerant**, **cross-region replication**.

---

### 🧩 Aurora Architecture
- Composed of an **Aurora DB Cluster**:
  - One **writer endpoint** (for writes).
  - One or more **reader endpoints** (for reads).
- Uses **shared storage volume** across instances.
- Auto-scales both **reads** and **writes** based on demand.

---

### 🧭 Endpoints
- **Writer Endpoint:** Handles all writes.
- **Reader Endpoint:** Load-balances read traffic.
- **Custom Endpoints:** Define subsets of replicas for specific workloads (e.g. analytics).

---

### 📈 Auto Scaling
- **Replica auto-scaling:** Adds/removes read replicas automatically.
- Helps reduce **CPU load** and manage **read-heavy traffic**.

---

### ☁️ Aurora Serverless
- On-demand, **auto-scaling** Aurora configuration.
- Ideal for **sporadic or unpredictable workloads**.
- **No capacity planning** needed.
- **Pay-per-second** billing while active.
- Uses **proxy fleet** to handle scaling seamlessly.

---

### 🌍 Aurora Global Database
- Enables **cross-region replication**.
- **Primary region:** Handles reads/writes.
- **Up to 10 secondary regions**, each with **16 read replicas**.
- **Replication lag:** <1 second.
- **Failover time:** <1 minute.
- Improves **global read latency** and **DR resilience**.

---

### 🤖 Aurora Machine Learning Integration
- Integrates with:
  - **Amazon SageMaker** (custom ML models)
  - **Amazon Comprehend** (NLP & sentiment)
- Perform **ML predictions via SQL**.
- Common use cases:
  - Fraud detection
  - Ad targeting
  - Sentiment analysis
  - Product recommendations

---

### 🧩 Babelfish for Aurora PostgreSQL
- Adds **Microsoft SQL Server (T-SQL)** compatibility.
- Allows apps using SQL Server drivers to connect **without major code changes**.
- Simplifies migration using:
  - **AWS SCT (Schema Conversion Tool)**
  - **AWS DMS (Database Migration Service)**

---

## 3️⃣ RDS Security

- **Encryption at rest:** AWS KMS.
- **In-transit encryption:** TLS enabled by default.
- **Authentication options:**
  - Username/password
  - IAM authentication
- **Access control:** Managed via **Security Groups**.
- **Auditing:** Send logs to **CloudWatch Logs**.

---

## 4️⃣ RDS Proxy

- **Connection pooling** service for RDS & Aurora.
- Reduces **failover time by ~66%**.
- Enforces **IAM authentication** (Secrets Manager).
- **VPC-only access** for enhanced security.
- Ideal for **Lambda functions** or applications with many concurrent connections.

**Key Benefits:**
- Serverless, Multi-AZ, auto-scaling.
- No code changes required.
- Improves efficiency and reduces DB overhead.

---

## 5️⃣ Amazon ElastiCache Overview

### 🔹 Purpose
- Managed **in-memory caching service** for:
  - **Redis**
  - **Memcached**
- Reduces **RDS load** for read-heavy workloads.
- Enables **low-latency**, **high-performance** responses.
- Can store **session data** for stateless apps.

---

### ⚙️ How It Works
- **Cache hit:** Data retrieved from cache (fast).
- **Cache miss:** Data fetched from DB and then cached.
- Requires **cache invalidation** strategy to maintain accuracy.

---

### 🧰 Use Cases
- Reduce read load on RDS.
- Store web application **user sessions** for scalability.

---

### 🔄 Redis vs Memcached

| Feature | Redis | Memcached |
|----------|--------|-----------|
| Availability | Multi-AZ with failover | No replication |
| Replication | Supported | Not supported |
| Persistence | AOF (Append-Only File) | No persistence |
| Backup/Restore | Supported | Limited |
| Architecture | Single-node + replicas | Multi-node sharded |
| Data Types | Sets, Sorted Sets, Lists | Simple key-value |
| Performance | High, single-threaded | Multi-threaded |

---

## ⚖️ Summary — RDS vs Aurora

| Feature | Amazon RDS | Amazon Aurora |
|----------|-------------|---------------|
| **Engine Type** | Standard managed DB engines | AWS-optimized (MySQL/PostgreSQL compatible) |
| **Storage Scaling** | Manual / Limited Auto | Auto 10GB → 128TB |
| **Performance** | Standard | 3–5× faster |
| **Availability** | Multi-AZ (Sync) | 6 copies across 3 AZs |
| **Replication** | Async read replicas | Sync + cross-region async |
| **Failover Time** | ~1–2 min | <30 sec |
| **Cost** | Lower | ~20% higher |
| **Serverless Option** | ❌ No | ✅ Aurora Serverless |
| **Global DB Support** | Manual | ✅ Aurora Global DB |

---

## ✅ Key Takeaways

- **RDS** = Simplicity & compatibility.
- **Aurora** = Performance, resilience, and automation.
- **RDS Proxy** optimizes connection handling.
- **ElastiCache** complements both for caching and latency reduction.
