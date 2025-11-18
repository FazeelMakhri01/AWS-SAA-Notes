# **Deploying and Managing Infrastructure at Scale**


## ☁️ **AWS CloudFormation**

**Definition:**

- **Declarative IaC (Infrastructure as Code)** service.
- Define, provision, and manage AWS infrastructure using **templates (YAML/JSON)**.

**How It Works:**

- Define resources (EC2, S3, RDS, ELB) in a **template**.
- CloudFormation **automatically creates** resources in correct order.

**✅ Benefits:**

- **Infrastructure as Code:** Version-controlled templates, no manual creation.
- **Code Review:** Changes go through review.
- **Cost Tracking:** Tags all stack resources.
- **Cost Optimization:** Automate creation/deletion schedules.
- **Automation:** Create/update/delete resources easily.
- **Dependency Management:** Automatic resource creation order.
- **Visualization:** **Infrastructure Composer** for architecture diagrams.

**📚 Exam Tip:**

- Use CloudFormation for **IaC** or **multi-environment replication**.

**Key Takeaways:**

- Declarative, template-based provisioning.
- Automates correct resource order.
- Enables cost tracking and review.
- Supports almost all AWS resources.

---

## 📧 **Amazon SES (Simple Email Service)**

**Definition:**

- Fully managed email sending service for **bulk or transactional emails**.

**Features:**

- Send outbound and receive inbound emails.
- Reputation Dashboard, delivery/open tracking.
- Anti-spam feedback & performance insights.
- Supports **DKIM** and **SPF** for authentication.

**Deployment Options:**

- Shared IPs
- Dedicated IPs
- Customer-owned IPs

**Use Cases:**

- Transactional emails: password reset, confirmations.
- Marketing campaigns & bulk communications.

**Access Methods:** API, AWS Console, SMTP.

**Key Takeaways:**

- Managed email sending/receiving with metrics & security.
- Supports shared, dedicated, or customer IPs.

---

## 📱 **Amazon Pinpoint**

**Definition:**

- Multi-channel communication service for **marketing and user engagement**.
- Channels: Email, SMS, push, voice, in-app.

**Primary Use Case:**

- **SMS messaging** with segmentation, personalization, and reply handling.

**Features:**

- Segmentation & personalization for audience targeting.
- Scalable → billions of messages/day.
- Event tracking → SNS, Kinesis Firehose, CloudWatch.

**Pinpoint vs SNS/SES:**

| Feature | SNS / SES | Pinpoint |
| --- | --- | --- |
| Audience mgmt | Manual | Managed (segments/templates) |
| Campaigns | No | Yes |
| Scheduling | Manual | Built-in |
| Use case | Notifications/Emails | Marketing automation |

**Key Takeaways:**

- Scalable inbound/outbound messaging.
- Supports multiple channels.
- Template, campaign, targeting, and analytics management.

---

## 🔒 **SSM Session Manager**

**Definition:**

- Part of **AWS Systems Manager**.
- Provides **secure shell access** to EC2/on-prem **without SSH keys or open ports**.

**How It Works:**

- Requires **SSM Agent** on instance.
- IAM role with `AmazonSSMManagedInstanceCore`.
- Connect via **Session Manager console** — no port 22, no bastion host.

**Access Methods:**

1. SSH → requires port 22 + key pair.
2. EC2 Instance Connect → temporary key, still needs port 22.
3. Session Manager → no open ports, IAM-based access.

**Features:**

- Linux, macOS, Windows.
- Logs sessions to S3 or CloudWatch.
- Auditable session history.

**Key Takeaways:**

- Secure EC2 access without SSH keys or open ports.
- Requires SSM Agent & IAM role.
- Logs stored in S3/CloudWatch.

---

## ⚙️ **Other SSM Services**

### 🧾 **Run Command**

- Execute scripts/commands on EC2/on-prem **without SSH**.
- Results → S3 or CloudWatch Logs.
- Integrates with **SNS**, **EventBridge**, **IAM**, **CloudTrail**.

### 🩹 **Patch Manager**

- Automates OS/application patching for EC2/on-prem.
- Supports Linux, macOS, Windows.
- On-demand or via Maintenance Windows.
- Generates patch compliance reports.

### 🕒 **Maintenance Windows**

- Schedule tasks on instances.
- Define frequency, duration, targets, and tasks (e.g., patching, updates).

### 🔁 **Automation**

- Automates operational tasks (create AMIs, restart instances).
- Uses **SSM Documents/runbooks**.
- Trigger via Console, CLI, SDK, EventBridge, or AWS Config.

**Key Takeaways:**

- Run Command → execute commands without SSH.
- Patch Manager → automate patching and compliance.
- Maintenance Windows → schedule system tasks.
- Automation → simplify and remediate AWS operations.

---

## 🧮 **AWS Batch**

**Definition:**

- Fully managed batch processing for **compute jobs at scale**.

**Batch Job:**

- Defined start and end time (vs continuous jobs).
- Example: image processing, report generation.

**How It Works:**

- Dynamically provisions EC2/Spot Instances.
- Jobs defined via Docker images & ECS task definitions.
- AWS Batch manages scaling & queues.

**Use Case Example:**

- S3 images → Batch Docker container → processed output stored.

**Batch vs Lambda:**

| Feature | AWS Lambda | AWS Batch |
| --- | --- | --- |
| Duration | Max 15 min | No limit |
| Runtime | Node, Python, etc. | Any Docker image |
| Compute | Serverless | EC2/Spot instances |
| Storage | 512 MB temp | Full EC2 storage |

**Key Takeaways:**

- Managed batch job execution.
- Dynamic EC2/Spot scaling.
- Ideal for long-running or heavy compute workloads.
- More flexible/persistent than Lambda.

---

✅ **Overall Section Summary**

- **CloudFormation:** IaC, template-driven provisioning, automated dependencies.
- **SES:** Managed email sending/receiving, DKIM/SPF, metrics.
- **Pinpoint:** Multi-channel messaging, marketing automation, analytics.
- **SSM:** Session Manager (secure access), Run Command, Patch Manager, Automation.
- **AWS Batch:** Scalable batch compute workloads, persistent & flexible.
