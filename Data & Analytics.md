# **Section 22: Data & Analytics**

---

## **1) Amazon Athena**

- 📌 **What it is**
    - **Serverless** SQL query engine **on S3** (no infra to manage)
    - Built on **Presto**; ANSI SQL
    - Formats: **CSV, JSON, Parquet, ORC, Avro** (prefer **Parquet/ORC**)
- 💰 **Pricing**
    - **Per TB scanned** → optimize to **scan less**
- ⚡ **Performance & Cost Tips**
    - Use **columnar** formats (**Parquet/ORC**)
    - **Compress** data
    - **Partition** S3 datasets (e.g., `year=YYYY/month=MM/day=DD/`)
    - Prefer **larger files** (≥ 128 MB) over many small files
    - **Glue Data Catalog** for schemas
- 🔗 **Federated Query**
    - Query **S3 + external sources** via **Lambda connectors** (e.g., CloudWatch Logs, DynamoDB, RDS, on-prem)
    - Results land back to **S3**
- ✅ **Use cases**
    - Ad-hoc SQL on S3, BI with **QuickSight**, log analytics (VPC Flow, ALB, CloudTrail)
- 🧠 **Exam tip**
    - “**SQL on S3, serverless**” ⇒ **Athena**

---

## **2) Amazon Redshift (incl. Serverless)**

- 📌 **What it is**
    - **Fully managed data warehouse** (petabyte scale)
    - **MPP** architecture, **columnar** storage, **Query Optimizer**
- 🌟 **Key features**
    - **Materialized Views**
    - **Concurrency Scaling** (burst capacity)
    - **Redshift Serverless** (no cluster mgmt)
    - **Redshift ML** (build/deploy ML from SQL)
    - **Spectrum** (query data in S3 directly)
    - **Automated snapshots** to S3; **resize** without downtime
- 🔗 **Integrations**
    - Athena, EMR, Glue, QuickSight, S3, Kinesis, MSK
- ✅ **Use cases**
    - Enterprise BI, star/snowflake schemas, **dashboards at scale**
- 🧠 **Exam tip**
    - **Warehouse** workloads, **materialized views**, **MPP** ⇒ **Redshift**

---

## **3) Amazon OpenSearch Service**

- 📌 **What it is**
    - Successor to **Elasticsearch** (managed & **serverless** options)
    - **Search & analytics** on semi-structured docs; **partial match**, full-text
- 🌟 **Features**
    - OpenSearch Dashboards (viz)
    - Ingest from **Kinesis Firehose**, **CloudWatch Logs**, **DynamoDB Streams** (via Lambda), custom apps
    - Security with **Cognito/IAM**, encryption in transit/at rest
- 🧩 **Common pattern**
    - **DynamoDB** as source of truth → **Streams → Lambda → OpenSearch** for fast search → ID → read full item from DynamoDB
- ✅ **Use cases**
    - App/site **search**, log analytics, “Google-like” queries
- 🧠 **Exam tip**
    - Need **search across any field/partial match** ⇒ **OpenSearch** (not DynamoDB alone)

---

## **4) Amazon EMR (Elastic MapReduce)**

- 📌 **What it is**
    - Managed **big data** clusters (Hadoop/Spark/HBase/Presto/Flink)
- 🌟 **Cluster roles**
    - **Master** (control plane, long-running)
    - **Core** (HDFS + tasks, long-running)
    - **Task** (stateless compute, good for **Spot**)
- 💸 **Purchasing**
    - **On-Demand**, **Reserved** (good for Master/Core), **Spot** (great for Task)
- 🔁 **Patterns**
    - Long-running or **transient** clusters
- ✅ **Use cases**
    - ETL at scale, ML with Spark, web indexing, custom big-data pipelines
- 🧠 **Exam tip**
    - “**Hadoop/Spark cluster** on AWS” ⇒ **EMR**

---

## **5) Amazon QuickSight**

- 📌 **What it is**
    - Serverless **BI & dashboards**, embeddable, per-session pricing
- 🌟 **Features**
    - **SPICE** (in-memory engine) for imported data
    - **Enterprise**: column-level security (CLS)
    - Data sources: **Athena, Redshift, S3, RDS/Aurora, OpenSearch, Timestream**, plus SaaS (Salesforce, Jira), JDBC
- 🧩 **Artifacts**
    - **Analyses** (editable) → **Dashboards** (read-only share)
- ✅ **Use cases**
    - Interactive dashboards, ad-hoc analysis
- 🧠 **Exam tip**
    - Front-end for **Athena/Redshift**; “**dashboarding**” ⇒ **QuickSight**

---

## **6) AWS Glue**

- 📌 **What it is**
    - Serverless **ETL** + **Data Catalog**
- 🌟 **Features**
    - **ETL Jobs** (Spark): transform **CSV → Parquet** (common exam case)
    - **Job Bookmarks** (incremental)
    - **Data Catalog**/**Crawlers** (schemas for **Athena/Redshift Spectrum/EMR**)
    - **Glue Studio** (GUI), **DataBrew** (no-code cleaning)
    - **Streaming ETL** (Kinesis/MSK; Spark Structured Streaming)
- 🧩 **Automation**
    - S3 **events** or **EventBridge** → trigger Glue jobs
- ✅ **Use cases**
    - Data prep, format conversion, cataloging, batch/stream transforms
- 🧠 **Exam tip**
    - “Convert to **Parquet** for Athena” / “central **catalog**” ⇒ **Glue**

---

## **7) AWS Lake Formation**

- 📌 **What it is**
    - Managed **data lake setup & governance** on S3
- 🌟 **Benefits**
    - **Days vs months** to set up
    - **Centralized permissions**: **row/column-level** security
    - Ingest blueprints; dedupe with ML
    - Built **on Glue** (but you manage via **Lake Formation**)
- 🔗 **Consumers**
    - Athena, Redshift, EMR, Spark, QuickSight
- ✅ **Use cases**
    - Central lake on S3 with **fine-grained governance**
- 🧠 **Exam tip**
    - “**One place** to manage analytic data **permissions**” ⇒ **Lake Formation**

---

## **8) Amazon Managed Service for Apache Flink** *(formerly Kinesis Data Analytics for Apache Flink)*

- 📌 **What it is**
    - Managed **Apache Flink** for **real-time streaming** apps (Java/Scala/SQL)
- 🌟 **Sources**
    - **Kinesis Data Streams**, **Amazon MSK (Kafka)**
    - ❌ Not **Firehose** as a source
- 🌟 **Features**
    - Parallelism, autoscaling
    - **Checkpoints/Snapshots** (stateful streaming)
- ✅ **Use cases**
    - Real-time transforms, windowed aggregations, streaming joins
- 🧠 **Exam tip**
    - “**Flink on AWS**” / “**read from Kinesis/MSK**” ⇒ Managed Flink

---

## **9) Amazon MSK (Managed Streaming for Apache Kafka)**

- 📌 **What it is**
    - **Managed Kafka** (brokers/ZooKeeper managed in your VPC)
    - **MSK Serverless** available
- 🌟 **Kafka vs Kinesis (exam-level)**
    - **Kinesis**: shards; split/merge; fixed message size; TLS in-flight
    - **Kafka/MSK**: topics/partitions; can add partitions (not remove); retention **as long as EBS holds**; TLS/plain options
- 🔗 **Consumers/Producers**
    - Managed Flink, **Glue Streaming**, **Lambda event source**, custom apps (EC2/ECS/EKS)
- ✅ **Use cases**
    - Streaming pipelines, event backbones, long retention topics
- 🧠 **Exam tip**
    - “**Cassandra** ⇒ Keyspaces; **Kafka** ⇒ **MSK**; **Real-time** ⇒ Kinesis/MSK/Flink (depending on needs)”

---

## **10) Big Data Ingestion Pipeline (Reference Pattern)**

- 🧱 **End-to-end flow**
    1. **Producers / IoT Core**
    2. **Kinesis Data Streams** (real-time ingest)
    3. **Kinesis Data Firehose** → **S3 (ingestion bucket)**
       ↳ (optional **Lambda** transform in Firehose)
    4. **S3 event** or **SQS → Lambda** trigger
    5. **Athena** queries on S3 → results to **S3 (reporting)**
    6. **QuickSight** dashboards on S3/Athena or
    7. Load into **Redshift** for warehouse analytics
- 🎯 **Why it works**
    - Mostly **serverless**, scalable, cost-efficient
    - Clear separation: **ingest → transform → query → visualize**

---

## **11) Quick Comparison – Pick the Right Tool**

| Need | Best Service(s) | Why |
| --- | --- | --- |
| **SQL on S3, serverless** | **Athena** | Per-TB scanned; fast to start |
| **Enterprise BI / DW** | **Redshift / Redshift Serverless** | MPP, columnar, MV, Spectrum |
| **Search across fields / partial matches** | **OpenSearch** | Inverted index, full-text |
| **Hadoop/Spark clusters** | **EMR** | Managed big-data frameworks |
| **Dashboards** | **QuickSight** | Serverless BI, SPICE |
| **ETL + catalog** | **Glue** | Jobs, crawlers, Parquet, catalog |
| **Data lake governance** | **Lake Formation** | Central row/column-level ACLs |
| **Real-time stream processing** | **Managed Flink** | Stateful windows, checkpoints |
| **Kafka on AWS** | **MSK (Serverless/Managed)** | Native Kafka, VPC, long retention |

---

## **Key Takeaways**

- **Athena** = **Serverless SQL on S3**; cut scan costs with **Parquet/ORC**, **compression**, **partitions**.
- **Redshift** = **Data warehouse** (MPP, columnar, serverless option) for heavy analytics.
- **OpenSearch** = **Search & log analytics**, often **paired with DynamoDB**.
- **EMR** = **Managed Hadoop/Spark** clusters; mix On-Demand/Reserved/Spot.
- **QuickSight** = **Dashboards**; **SPICE** for fast in-memory queries.
- **Glue** = **ETL + Data Catalog**; streaming ETL; CSV→**Parquet** for Athena.
- **Lake Formation** = **Centralized governance** on S3 with row/column-level security.
- **Managed Flink** = **Real-time** stream processing from **Kinesis/MSK** (not Firehose).
- **MSK** = **Managed Kafka**; partitions/retention and Kafka ecosystem compatibility.
