# Amazon EC2 (Elastic Compute Cloud)


## 🧭 Overview

Amazon Elastic Compute Cloud (EC2) provides secure, resizable compute capacity in the cloud.  
You can provision **Linux**, **Windows**, or **Mac** virtual servers.

### ⚙️ Limits
- **On-Demand Instances:** Limited by your vCPU-based On-Demand limit.  
- **Reserved Instances:** Max 20 per account.  
- **Spot Instances:** Limited by dynamic Spot limit per region.  
- **New AWS accounts** may have lower default limits.

---

## 🧩 Instance Types

| Type | Examples | Use Case |
| --- | --- | --- |
| General Purpose | M7g, T4g, M7i, T3, M7a, M7gd | Balanced compute, memory, networking (e.g., web servers) |
| Compute Optimized | C7g, C7i, C7a, C7gd | CPU-intensive tasks (e.g., batch processing, ML) |
| Memory Optimized | R7iz, X2idn, X2iedn, R7a, R7g, U7i, I7i, I7ie | Large in-memory datasets, databases, real-time processing |
| Accelerated Computing | P4d, G5, VT1, P5en, Trn2 | GPU-intensive workloads, HPC, ML |
| Storage Optimized | Im4gn, Is4gen, I4i | High-frequency local storage access, OLTP, data warehousing |

**Naming Convention:**  
Instance class (M) + generation (5) + size (2xlarge)

---

## 💰 Pricing Models

| Model | Description |
| --- | --- |
| On-Demand | Pay per second, no long-term commitment |
| Reserved Instances | 1–3 year term, capacity reservation, discounts available |
| Savings Plans | 1–3 year commitment, discount based on committed spend |
| Spot Instances | Up to 90% discount, can be interrupted |
| Dedicated Hosts | Physical servers for your use |
| Dedicated Instances | Single-tenant hardware |
| Capacity Reservations | Reserve EC2 capacity for any duration |

**Analogy:**
- **On-Demand:** Pay as you go  
- **Reserved:** Plan ahead, get discount  
- **Savings Plan:** Commit monthly spend  
- **Spot:** Grab discounted unused capacity  
- **Dedicated Host:** Own the entire server  
- **Capacity Reservation:** Reserve space even if unused  

---

## ⚡ Key Features

- **Nitro System:** Enhanced security & near bare-metal performance  
- **Elastic Fabric Adapter (EFA):** Low-latency networking for HPC/ML  
- **Capacity Blocks for ML:** Reserve GPU capacity for specific times  
- **EC2 Instance Connect:** Browser-based SSH access  
- **EC2 Fleet / Spot Fleet:** Mix On-Demand, Reserved, Spot instances  

### 🧠 Best Practices
- Use **IMDSv2** for metadata access  
- Enable **Detailed Monitoring** (<1-min metrics)  
- Use **Placement Groups** (Cluster, Spread, Partition)  
- Enable **Auto Scaling** for elasticity & high availability  
- Use **AWS Systems Manager** for automation  
- Monitor costs with **Cost Explorer** & **Compute Optimizer**

---

## 🔁 Instance States

| State | Description | Billing |
| --- | --- | --- |
| Start | Runs normally | Billed continuously |
| Stop | Shuts down, EBS persists | No usage charges |
| Hibernate | Writes memory to EBS | Billed for EBS & Elastic IPs |
| Terminate | Deletes instance | Billing stops; EBS optional persistence |

**Notes:**
- **Instance Store-backed** instances cannot be stopped, only started/terminated.  
- Enable **termination protection** to prevent accidental deletions.

---

## 💾 Root Device Volumes

| Volume Type | Description |
| --- | --- |
| Instance Store | Temporary; deleted on stop/terminate |
| EBS-backed | Persistent; supports stop/start |

**Modifications:**
- **EBS-backed:** Change instance type, kernel, RAM disk (while stopped)  
- **Instance Store:** Fixed attributes; no stop state

---

## 🧱 Amazon Machine Images (AMI)

| Feature | EBS-backed | Instance Store-backed |
| --- | --- | --- |
| Boot Time | <1 min | <5 min |
| Root Volume Size | Up to 64 TiB | 10 GiB |
| Data Persistence | EBS persists | Lost on termination |
| Modifications | Supported | Not supported |
| Stopped State | Supported | Not supported |

**Details:**
- AMI includes root volume template, permissions, block device mapping.  
- AMIs can be copied across regions.  
- Deleted AMIs can be restored from **Recycle Bin**.  
- Supports **UEFI Secure Boot** and **IMDSv2** configuration.

---

## 🧰 EC2 Image Builder

- Automates creation and management of AMIs.  
- Custom images are owned by your account.  
- Supports automatic **patching, updating, and pipelines**.

---

## 💵 EC2 Pricing Details

- **On-Demand:** No upfront cost, per-second billing.  
- **Reserved Instances (RI):** Standard (max discount) or Convertible (flexible).  
- **Spot Instances:** Up to 90% off, but interruptible.  
- **Dedicated Hosts/Instances:** Physical isolation, for compliance.  
- **Capacity Reservations:** Hold capacity, combine with RI/Savings Plans for discount.

---

## 🔒 Security

- Manage access using **IAM**.  
- **Security Groups:** Virtual firewalls for instances.  
  - Stateful (allow rules only).  
  - Default: All outbound allowed.  
  - Common ports:  
    - SSH: 22  
    - FTP: 21  
    - SFTP: 22  
    - HTTP: 80  
    - HTTPS: 443  
    - RDP: 3389  
- Use **key pairs**, not passwords.

---

## 🌐 Networking

- **Elastic IP:** Static IPv4 address.  
- **ENI (Elastic Network Interface):** Logical NIC for network traffic.  
  - Default: `eth0` (non-detachable).  
  - Attach additional ENIs for multi-subnet setups.  
- **Enhanced Networking (SR-IOV):** Improves performance, reduces latency.  
- **Elastic Fabric Adapter (EFA):** Optimized inter-instance comms for HPC/ML.

---

## 📊 Monitoring

- **Metrics:** CPU, network, disk, memory (via agent).  
- **Tools:** CloudWatch Alarms, Logs, Events, Status Checks.  
- **Detailed Monitoring:** 1-min intervals (default: 5-min).

---

## 🧠 Instance Metadata & User Data

- **Metadata:** Info about the instance.  
- **User Data:** Initialization scripts (16 KB limit, runs once).  

Access via:
- Metadata → `http://169.254.169.254/latest/meta-data/`  
- User Data → `http://169.254.169.254/latest/user-data`

---

## 🗺️ Placement Groups

| Type | Description | Use Case |
| --- | --- | --- |
| Cluster | Pack instances close in 1 AZ | Low-latency HPC |
| Spread | Spread instances across hardware | Critical small workloads |
| Partition | Spread across logical partitions | Distributed systems (HDFS, Cassandra) |

**Rules:**
- Add instances only at launch time.  
- Host tenancy instances not supported.  
- Cannot merge placement groups.

---

## 🗄️ Storage Options

| Type | Description |
| --- | --- |
| EBS | Persistent block storage |
| Instance Store | Temporary block storage |
| EFS | Elastic file storage (multi-instance) |
| FSx | Managed file systems (Windows, Lustre, NetApp, OpenZFS) |
| S3 | Object storage for snapshots & AMIs |

---

## 🏷️ Resources & Tagging

| Resource | Scope | Notes |
| --- | --- | --- |
| AWS Account | Global | Access all regions |
| Key Pairs | Regional | Required for SSH/RDP |
| AMIs | Regional | Copyable across regions |
| Elastic IPs | Regional | Must be in same region as instance |
| Security Groups | Regional | Region-bound |
| EBS Volumes | AZ | Attach only in same AZ |
| Instances | AZ | Instance ID tied to region |

**Tags:**  
Key-value metadata pairs to organize resources.

---

## ✅ Summary Takeaways

- **EC2 = Core AWS compute service**.  
- Provides flexible compute, networking, storage, and monitoring options.  
- Offers multiple **instance types** for optimized performance.  
- Multiple **pricing models** for cost efficiency.  
- Supports **security**, **scaling**, and **automation** best practices.  

---
