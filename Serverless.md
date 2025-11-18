# **Section 19: Serverless**

## **1. Serverless Overview**

- 📌 **Concept**
    - Developers **don’t manage servers** → Just deploy code/services.
    - Serverless = **FaaS (Function as a Service)**
    - ⚠️ Servers still exist → But **abstracted away** (no provisioning, patching, scaling headaches).
- ✅ **AWS Serverless Services**
    - **Lambda Functions**
    - **Fargate** (serverless containers)
    - **API Gateway**
    - **S3** (serverless storage)
    - **SQS & SNS** (messaging)
    - **Kinesis Data Firehose** (streaming delivery)
    - **Aurora Serverless** (DB on-demand)
    - **Step Functions** (workflow orchestration)
    - **Cognito** (authentication & identity)

---

## **2. AWS Lambda**

- 📌 **Key Features**
    - Virtual function → **no server mgmt**
    - **Event-driven** & runs **on-demand**
    - **Scaling** → Seamless & automatic
    - **Short execution** (≤ 15 min)
    - **Monitoring** → CloudWatch
    - **Languages supported** → Python, Node.js, Java, .NET, Go, Ruby, etc.
    - ⚡ More RAM → Better CPU/network performance
    - Use **ECS/Fargate** for **long-running workloads**
- 🔗 **Integrations**
    - API Gateway, SQS, SNS, DynamoDB, S3, Cognito, CloudWatch, EventBridge, CloudFront, Kinesis, etc.
- ⚠️ **Limitations**
    - **Memory**: 128 MB – 10 GB
    - **Max execution**: 900s (15 min)
    - **Env vars**: 4 KB
    - **Ephemeral disk**: 512 MB – 10 GB
    - **Concurrency**: 1000 by default
    - **Deployment**:
        - 50 MB (zip, compressed)
        - 250 MB (uncompressed with deps)
        - `/tmp` dir available at runtime
- 🧩 **Concurrency Control**
    - Default: 1000 executions
    - **Reserved concurrency** → Avoids throttling errors
- ⚡ **Lambda SnapStart**
    - Pre-initializes functions for **10x performance boost** (Java, .NET, Python)
    - Skips cold-start lifecycle (`Init → Invoke → Shutdown`)

---

## **3. Edge Compute with CloudFront**

- 📌 **Edge Logic**
    - Runs code **closer to users** → Minimize latency
    - **Types**:
        - **CloudFront Functions** → Lightweight JS (sub-ms latency)
        - **Lambda@Edge** → Full Node.js/Python runtime (ms latency, 3rd-party libs)
- ⚡ **CloudFront Functions**
    - Used for:
        - Cache key normalization
        - URL rewrites
        - Header manipulation
    - Native CloudFront feature, **ultra-low latency**
- ⚡ **Lambda@Edge**
    - Supports **Viewer & Origin (Req/Resp)** events
    - ✅ Use when:
        - Need CPU/memory adjustments
        - 3rd-party libs required
        - File system / request body access needed

---

## **4. Lambda in VPC**

- 🔒 By default, Lambda **not in private VPC**
- To connect to **RDS/EC2**, must configure:
    - VPC ID
    - Subnets
    - Security Groups
- Creates **ENI (Elastic Network Interface)** in subnet.

**Architecture**:

Lambda → ENI → Private Subnet → RDS/EC2

---

## **5. Lambda + RDS Proxy**

- ⚠️ **Problem**: Direct Lambda → RDS = too many DB connections
- ✅ **Solution**: Use **RDS Proxy**
    - Connection pooling & sharing
    - Reduces failover time by **66%**
    - Uses IAM + Secrets Manager for security
    - Must run in VPC

---

## **6. Invoking Lambda from Databases**

- **Aurora MySQL & RDS PostgreSQL** can trigger Lambda
- Useful for **data-driven events**
- ⚠️ Must configure DB permissions for Lambda invocation

**Flow Example**:

User → Register → DB → Lambda → SQS → Send Email

---

## **7. RDS Event Notifications**

- Covers **DB events** (not row-level data)
- Categories: DB Instance, Snapshot, Parameter group, Security group, RDS Proxy, CEV
- Delivered via **SNS / EventBridge**
- Near real-time

---

## **8. DynamoDB**

- 📌 **Features**
    - Fully managed, highly available, multi-AZ replication
    - **NoSQL** → Infinite rows
    - **Item size max**: 400 KB
    - Data types: scalar, documents, sets
    - Schema flexibility → Rapid evolution
- ⚡ **Read/Write Modes**
    - **Provisioned** (predictable workload, cheap)
    - **On-demand** (unpredictable workload, expensive)
- 🚀 **Performance**
    - Scales to **millions of RPS**
    - No patching/maintenance
    - **DAX** → In-memory cache for acceleration

---

## **9. API Gateway**

- 📌 **Concept**
    - Fully managed → Build, deploy, manage APIs
    - Supports **REST, WebSocket, HTTP, gRPC APIs**
- ⚡ **Features**
    - Authentication: IAM, Cognito, OAuth 2.0
    - Monitoring: CloudWatch, X-Ray
    - Caching: Improve performance
    - Versioning: Stages (dev, test, prod)
    - Usage Plans & Throttling
    - WAF integration for security
    - Supports **private APIs** in VPC
- 🔗 **Integrations**
    - Lambda, S3, DynamoDB, Step Functions, AppSync

---

## **10. Step Functions**

- 📌 **Serverless Workflow Orchestration**
    - Visual workflow: sequence, parallel, wait, error handling
    - **Use cases**: order fulfillment, ETL/data processing, web apps
- 🔗 Orchestrates → Lambda, ECS tasks, DynamoDB, SQS, Glue, etc.

---

## **11. Amazon Cognito**

- 📌 **Purpose**
    - Provides **identity management** for web & mobile apps
    - Works with **API Gateway** & **ALB**
- ⚡ **Components**
    - **User Pools (CUP)**: Auth database (signup/sign-in, MFA, password reset, email/phone verification, social login)
    - **Identity Pools (Federated Identities)**: Temporary AWS credentials for direct resource access
- 🔗 **Integration with API Gateway**
    1. User authenticates → CUP issues token
    2. Token → API Gateway
    3. Gateway validates → Passes user identity to Lambda/backend
- 🔗 **Integration with ALB**
    - ALB verifies login via CUP
    - Forwards request with **user identity headers** to backend
- ⚡ **Use Cases**
    - **CUP** → Auth users for APIs & apps
    - **Identity Pools** → Direct AWS access (S3/DynamoDB) with fine-grained IAM roles

---

## **12. Comparison – Lambda vs CloudFront Functions vs Lambda@Edge**

| Feature | **AWS Lambda** | **CloudFront Functions** | **Lambda@Edge** |
| --- | --- | --- | --- |
| **Runtime** | Multiple languages (Python, Node.js, Java, .NET, Go, Ruby, etc.) | **JavaScript only** | **Node.js & Python** |
| **Execution Time** | Up to **15 min (900s)** | **< 1 ms (sub-ms latency)** | A few **ms** |
| **Location** | AWS Regions (inside VPC optionally) | **At CloudFront edge locations** | **At CloudFront edge locations** |
| **Scaling** | Up to **1000+ concurrent executions** | Scales to **millions of req/s** | Scales automatically to **thousands of req/s** |
| **Use Cases** | Event-driven apps, backend logic, API handling, ETL, DB triggers | Lightweight CDN customization:<br>• Header rewrites<br>• Cache key normalization<br>• Simple auth | Advanced CDN logic:<br>• Request/response modification<br>• Call external services<br>• Use 3rd-party libs |
| **Startup** | Cold start possible (ms–seconds) ⚡ SnapStart reduces in some runtimes | **Always ultra-fast** (no cold starts) | Slower than CF Functions (init + execution) |
| **Access to AWS Services** | Full AWS integration (S3, DynamoDB, RDS, SQS, etc.) | ❌ No direct AWS service calls | ⚠️ Limited (but can access network + 3rd-party APIs) |
| **File System Access** | ✅ `/tmp` up to 10 GB | ❌ Not supported | ✅ Supports FS access |
| **Cost** | Pay per request + execution time | Extremely low-cost (optimized for high-volume traffic) | Higher cost than CF Functions (regional compute) |
