# **Section 21: Databases in AWS**

## **1. Amazon RDS (Relational Database Service)**

- 📌 **Overview**
    - Managed **SQL databases**:
        - PostgreSQL, MySQL, MariaDB, Oracle, SQL Server, DB2
    - Provisioned → **Instance size + EBS volume type**
    - **Auto-scaling capacity** supported
    - **Read replicas** & **Multi-AZ** for HA
    - **Backups**:
        - Automated: up to **35 days**
        - Manual snapshots → Long-term
    - **Use cases**:
        - OLTP workloads
        - RDBMS apps
        - SQL + transactional queries

---

## **2. Amazon Aurora**

- 📌 **Overview**
    - Compatible with **PostgreSQL & MySQL**
    - **Storage**:
        - 6 copies across **3 AZs**
        - **Self-healing** + **auto-scaling**
    - **Compute**:
        - Clustered DB instances
        - **Reader & Writer endpoints**
        - Read replicas scale automatically
    - **Backups**: Automated + snapshots
- ⚡ **Advanced Features**
    - **Aurora Serverless** → Pay-per-use DB (for unpredictable workloads)
    - **Aurora Global Database** → Multi-region, <1s replication lag, failover to secondary
    - **Aurora Machine Learning** → Integrates with SageMaker & Comprehend
    - **Aurora Database Cloning** → Quick clone for testing/staging
- ✅ **Use cases**
    - Same as RDS, but with:
        - Less maintenance
        - Higher performance
        - Multi-region replication

---

## **3. Amazon ElastiCache**

- 📌 **Overview**
    - Managed **Redis** or **Memcached**
    - **In-memory cache** → Sub-ms latency
- ⚡ **Redis Features**
    - Clustering
    - Multi-AZ + replicas (sharding)
    - Backups, snapshots, PITR
    - Security: IAM, KMS encryption, Redis Auth
- ⚠️ **Exam Tip**
    - Requires **app code changes** to integrate
    - ❌ If exam asks for caching *without code change* → ElastiCache is **NOT the answer**
- ✅ **Use cases**
    - Key/Value store
    - Query caching
    - Session store
    - **Not SQL-compatible**

---

## **4. Amazon DynamoDB**

- 📌 **Overview**
    - **Managed, serverless, NoSQL**
    - **Millisecond latency**
    - Replicates across multiple AZs
- ⚡ **Capacity Modes**
    - **Provisioned** (+ auto scaling) → predictable workloads
    - **On-demand** → unpredictable workloads
- 🚀 **Features**
    - TTL → Auto-expire rows
    - Transactions supported
    - **DAX (DynamoDB Accelerator)** → Microsecond reads
    - Streams → Event-driven (integrates with Lambda)
    - Global Tables → Multi-region active-active
    - Export/Import from S3
    - Backups:
        - **PITR** (≤35 days)
        - **On-demand** backups
- ✅ **Use cases**
    - Flexible schema apps
    - Serverless web/mobile apps
    - Small docs (≤400 KB)
    - Distributed cache

---

## **5. Amazon DocumentDB**

- 📌 **Overview**
    - Managed **MongoDB-compatible** NoSQL DB
    - Stores & indexes **JSON objects**
    - Replicates across **3 AZs**
    - Auto-scales to millions of requests
- ✅ **Use cases**
    - MongoDB workloads
    - JSON-heavy apps
    - NoSQL storage

---

## **6. Amazon Neptune**

- 📌 **Overview**
    - Fully managed **graph database**
    - Example → **Social networks, relationships, knowledge graphs**
- ⚡ **Features**
    - Replicates across **3 AZs**
    - Up to **15 read replicas**
    - Stores **billions of relationships**
    - **Query latency** → milliseconds
    - Neptune Streams → Ordered log of changes (accessible via REST API)
- ✅ **Use cases**
    - Social graphs
    - Recommendation engines
    - Fraud detection
    - Knowledge graphs (e.g., Wikipedia)

---

## **7. Amazon Keyspaces (for Apache Cassandra)**

- 📌 **Overview**
    - Managed **Apache Cassandra**
    - **Serverless, scalable, highly available**
    - Replicates data **3x across AZs**
    - Uses **Cassandra Query Language (CQL)**
- ⚡ **Capacity Modes**
    - On-demand
    - Provisioned (with auto scaling)
- 🔒 **Security**
    - Encryption at rest & transit
    - Backup + PITR (≤35 days)
- ✅ **Use cases**
    - IoT data
    - Time-series data
    - High-throughput workloads
- ⚠️ **Exam Tip**
    - If you see **Cassandra** → Think **Amazon Keyspaces**

---

## **8. Amazon Timestream**

- 📌 **Overview**
    - **Time-series database**
    - Fully managed, serverless, cost-optimized
    - Auto-scales, stores **trillions of events/day**
- ⚡ **Features**
    - **SQL-compatible**
    - Scheduled queries
    - Time-series analytics (patterns, trends)
    - Recent data → in-memory
      Old data → cost-optimized storage
    - Encryption at rest & in transit
- 🔗 **Integrations**
    - AWS IoT, Kinesis, MSK, Prometheus, Telegraf
    - Amazon QuickSight (dashboards)
    - SageMaker (ML)
    - Grafana
- ✅ **Use cases**
    - IoT apps
    - Operational monitoring
    - Real-time analytics
    - Any time-series workloads

---

## **Comparison – AWS Databases**

| Service | Type | Best For | Exam Tip |
| --- | --- | --- | --- |
| **RDS** | Relational (SQL) | OLTP, transactions | Multi-AZ, backups, replicas |
| **Aurora** | Relational (SQL) | High perf, HA, global | Serverless, Global DB |
| **ElastiCache** | In-memory | Caching, session store | Requires code changes |
| **DynamoDB** | NoSQL (Key-Value/Doc) | Serverless, flexible schema | Streams, DAX, TTL |
| **DocumentDB** | NoSQL (MongoDB) | JSON storage | MongoDB-compatible |
| **Neptune** | Graph | Relationships, social networks | Graph queries, Streams |
| **Keyspaces** | NoSQL (Cassandra) | IoT, time-series | Cassandra → Keyspaces |
| **Timestream** | Time-series | IoT, analytics | SQL + time-series functions |
