# Decoupling Applications – SQS, SNS, Kinesis, Amazon MQ

## 1. Communication Models

> Synchronous vs Asynchronous

- **Synchronous** → Client interacts directly with the service.  
⚠️ *Problem*: Sudden spikes can overwhelm the service.  
*Example*: Encoding 1000 videos → Overload.

- **Asynchronous** → Middleware (Queue/Stream/Topic) between client & app.  
✅ *Solution*: Decouple apps using:
  - **SQS** → Queue model
  - **SNS** → Pub/Sub model
  - **Kinesis** → Real-time streaming model

---

## 2. Amazon SQS (Simple Queue Service)

### Concept

**Producer → Queue → Consumer**

- Multiple producers & consumers.
- Consumers poll messages, process, and delete them.

### Key Attributes

- **Unlimited throughput** & storage
- **Retention:** 4 days (default) → 14 days (max)
- **Message size:** 256 KB max
- **Delivery:** At least once → *possible duplicates*
- **Ordering:** Best-effort → *can be out of order*

### Message Lifecycle

1. **Producer** → Sends message via `SendMessage` API.  
   *Example*: Send order info.
2. **Consumer** → Polls messages via `ReceiveMessage` API.  
   *Example*: Process order & store in RDS.
3. **Delete** → After processing via `DeleteMessage` API.

### Scaling

- Multiple Consumers → Parallel processing.
- Auto Scaling via **CloudWatch**:
  - Metric: `ApproximateNumberOfMessages`
  - Example: If messages > 100 → Scale EC2 to 10 instances.

### Use Case Example

**Video upload site architecture:**

Client → Frontend (ASG) → **SQS** → Backend Processors → S3

- Acts as buffer for DB inserts
- Prevents data loss

### Security

- **In-flight encryption** → HTTPS
- **At-rest encryption** → AWS KMS
- **Client-side encryption**
- **IAM Policies** → Access control
- **SQS Policies** → Cross-account & service-level access

### Message Visibility Timeout

- Default: **30s** → Message hidden from others
- If not processed → Message reappears → *duplicate risk*
- **Fix:** Use `ChangeMessageVisibility` API

### Long Polling

- Wait **1–20s** for message arrival
- Reduces empty responses & API cost
- Configurable at queue level or via API (`WaitTimeSeconds`)

### FIFO Queue

- Strict **First-In-First-Out** ordering
- **Deduplication** via Message ID
- Requires **Group ID**
- **Throughput:**
  - 300 msg/s (no batching)
  - 3000 msg/s (with batching)
- Naming convention: `queue.fifo`

---

## 3. Amazon SNS (Simple Notification Service)

### Concept

**Producer → SNS Topic → Multiple Subscribers**

- Subscribers: Email, SMS, Push, Lambda, HTTP/S, SQS, etc.
- Supports **12M+ subscribers** per topic
- Up to **100,000 topics**

### How It Works

1. Create **SNS Topic**
2. Add **Subscriber(s)**
3. Publish message

*Example*: SDK → Create platform app → Endpoint → Publish

### SNS + SQS = Fanout Pattern

- Publish once → Replicates to multiple **SQS queues**
- Advantages:
  - Fully decoupled
  - No data loss
  - Cross-region delivery
- Ensure SQS policy allows SNS to write

### Message Filtering

- Apply **JSON Filter Policy** on subscription
- Without filter → All messages delivered

*Example*: Only `PlaceOrder` events → Sent to **SQS(PlaceOrder)** queue

---

## 4. Amazon Kinesis (Real-Time Streaming Data)

### Kinesis Data Streams (KDS)

- Real-time data collection & processing
- Example: **Apple Watch → KDS → Real-time analytics**

**Attributes:**

- Retention: **Up to 365 days**
- Replay & reprocess capability
- Immutable data → Cannot delete until expiry
- Message size: **1 MB**
- Ordering guaranteed → **Partition Key (Shard)**

**Libraries:**

- **KPL** → Producer library
- **KCL** → Consumer library

### Capacity Modes

**Provisioned Mode**

- Manual shard definition
- 1 MB/s in, 2 MB/s out per shard
- Pay per shard-hour

**On-Demand Mode**

- Auto-scales based on 30-day usage
- Default: 4 MB/s
- Pay per stream + data volume

### Kinesis Data Firehose

- Fully managed real-time delivery
- Streams → S3, Redshift, Elasticsearch, Splunk
- Auto-scaling
- Near real-time ingestion (buffered)

---

## 5. Amazon MQ

### Overview

- Managed **message broker** for traditional apps
- Supports **AMQP, MQTT, STOMP, OpenWire**
- Ideal for **migrating legacy apps**
- **High Availability** → Use **EFS** for active/standby failover

---

## 6. Comparison Table – SQS vs SNS vs Kinesis

| Service | Model | Push/Pull | Ordering | Best Use Case |
| --- | --- | --- | --- | --- |
| **SQS** | Queue | Pull | FIFO (optional) | Decouple apps, async processing |
| **SNS** | Pub/Sub | Push | FIFO via SQS FIFO | Notifications, fanout |
| **Kinesis** | Stream | Pull | Ordered by Shard | Real-time analytics, IoT, logs |
