# **Section 24: AWS Monitoring and Audit: CloudWatch, CloudTrail, and Config**

## **1) Amazon CloudWatch Metrics**

- 📌 **What it is**
    - Monitors AWS service metrics (CPU, network, disk, etc.)
    - Metrics belong to **namespaces**
    - **Dimensions** = attributes (e.g., Instance ID)
    - **Timestamped**, up to 30 metrics per namespace
    - Create **dashboards** from multiple metrics
    - Supports **custom metrics**
- ✅ **Use cases**
    - Track **CPU utilization, memory, disk I/O**
    - Monitor **application performance**
- ⚙️ **Advanced**
    - **Metric Streams**: near real-time delivery to destinations
        - Flow: Metric Stream → **Kinesis Data Firehose** → S3 → Athena/Redshift
        - Optional **filtering** to stream subsets

---

## **2) CloudWatch Logs**

- 📌 **Structure**
    - **Log Groups** → apps or services
    - **Log Streams** → instances/containers
    - **Retention:** 1–10 years (configurable)
    - **Encrypted**, can use **KMS**
- ⚙️ **Log Insights**
    - Query logs across accounts
    - Find patterns or top contributors
- 🛠️ **Agents**
    - **CloudWatch Logs Agent** – legacy, logs only
    - **Unified Agent** – system-level metrics + logs, central config via SSM

---

## **3) CloudWatch Alarms**

- 📌 **What it is**
    - Notify or trigger actions on metric thresholds
    - Delivery: **Email, SMS, SNS**, or **automated actions** (Auto Scaling, EC2)
- ✅ **Use cases**
    - Alert when CPU > 80%
    - Auto-scale EC2 when traffic spikes

---

## **4) Amazon EventBridge**

- 📌 **What it is**
    - **Serverless event bus** connecting apps via events
    - Detect events from **AWS services, apps, SaaS, IoT**
    - Route events to **Lambda, Step Functions, Kinesis, SNS**
- 🌟 **Key Features**
    - Decouples microservices and legacy apps
    - Highly **scalable**, low-latency
    - **Schema Registry**: manage event schemas

---

## **5) CloudWatch Container Insights**

- 📌 **What it is**
    - Collects, aggregates, summarizes metrics/logs from **ECS, EKS, Kubernetes on EC2/Fargate**
    - Uses **containerized agent**
- ✅ **Use cases**
    - Monitor container performance
    - Troubleshoot ECS/EKS apps

---

## **6) Lambda Insights**

- 📌 **What it is**
    - Monitoring/troubleshooting for **AWS Lambda**
    - Metrics: CPU, memory, disk, network, **cold starts**, worker shutdowns
    - Provided as a **Lambda Layer**
- ✅ **Use cases**
    - Performance monitoring of serverless apps
    - Dashboard: **Lambda Insights**

---

## **7) CloudWatch Contributor Insights**

- 📌 **What it is**
    - Identifies **top contributors** impacting performance
    - Works with **CloudWatch Logs**
- ✅ **Use cases**
    - Find **heaviest network users**
    - Detect URLs generating most errors

---

## **8) CloudWatch Application Insights**

- 📌 **What it is**
    - Automated **dashboard for app health**
    - Uses **ML (SageMaker)** to detect potential issues
    - Integrates with EC2, Lambda, RDS, ELB, SQS, DynamoDB, S3, ECS/EKS
- ✅ **Use cases**
    - Auto-detect issues in monitored applications
    - Reduce troubleshooting time

---

## **9) AWS CloudTrail**

- 📌 **What it is**
    - Provides **governance, auditing, compliance**
    - Tracks **API calls/events** in your AWS account
    - Logs delivered to **S3** or **CloudWatch Logs**
- 🌟 **Key Features**
    - Retention: **90 days default**, archive to S3 for longer
    - **CloudTrail Insights**: detects unusual activity
- ✅ **Use cases**
    - Audit user actions
    - Investigate resource deletion/modification

---

## **10) AWS Config**

- 📌 **What it is**
    - Monitors **resource configurations** and **compliance**
    - Tracks changes and evaluates against **rules**
- ✅ **Use cases**
    - Ensure **security policies** are followed
    - Audit configuration changes

---

## **11) CloudWatch vs CloudTrail vs Config**

| Feature | CloudWatch | CloudTrail | Config |
| --- | --- | --- | --- |
| Purpose | Monitor **performance metrics & logs** | Track **API calls** | Track **resource configurations** |
| Data | Metrics, logs | API calls/events | Resource state changes |
| Retention | Customizable | 90 days default, archive S3 | Configurable |
| Example (ELB) | Monitor connections, error codes | Who changed security rules/SSL | Track SSL cert, security group compliance |

- ⚡ **Key takeaway**
    - **CloudWatch** = performance & logs
    - **CloudTrail** = auditing & API history
    - **Config** = compliance & resource state
    - Combined: complete **monitoring & audit solution**
