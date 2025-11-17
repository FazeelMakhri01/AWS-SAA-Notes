# AWS Storage Services: EBS & EFS 

## 💾 Amazon Elastic Block Store (EBS)

### 🔹 1. Overview
EBS (Elastic Block Store) is a block-level storage service for Amazon EC2 instances.

- Provides persistent storage that remains even when an EC2 instance stops or terminates (if configured).  
- Attached virtually over a private network to EC2 instances.  
- Replicated within the same Availability Zone (AZ) for durability.  
- Acts as a network drive mounted to one instance at a time (except Multi-Attach).

#### 📊 Default Limits
| Resource | Limit |
|-----------|--------|
| EBS Volumes | 5,000 per account |
| Snapshots | 10,000 per account |

---

### 🔹 2. Key Characteristics
| Feature | Description |
|----------|--------------|
| **Persistence** | Data persists independently of EC2 instance lifecycle |
| **AZ-Bound** | Each volume is tied to one AZ; move across AZs using snapshots |
| **Multiple Volumes** | One EC2 instance can attach multiple EBS volumes |
| **Delete on Termination** | Root volumes deleted by default; can disable |
| **Performance Configuration** | Provision size & IOPS in advance; can adjust later |
| **Volume Initialization** | New volumes perform at full speed; restored ones need initialization |
| **Termination Protection** | Disabled by default; must enable manually |

---

### 🔹 3. EBS Volume Types
| Type | Storage | Description | Best For |
|------|----------|--------------|-----------|
| **gp3** | SSD | General purpose; 3,000 IOPS & 125 MB/s baseline, scalable to 16,000 IOPS & 1,000 MB/s | Default choice for most workloads |
| **gp2 (Legacy)** | SSD | Older version; performance scales with volume size | Legacy systems |
| **io1 (Legacy)** | SSD | Provisioned IOPS SSD (up to 64,000 IOPS) | Critical apps (legacy) |
| **io2 / io2 Block Express** | SSD | Highest performance (up to 256,000 IOPS, 4,000 MB/s) | Databases, enterprise workloads |
| **st1** | HDD | Throughput-optimized HDD for frequent sequential access | Big data, log processing |
| **sc1** | HDD | Low-cost HDD for infrequent access | Cold data, archives |

⚠️ **Important:** HDD volumes (st1, sc1) cannot be used as boot volumes.

---

### 🔹 4. EBS Snapshots
- **Definition:** Point-in-time backup of an EBS volume stored in Amazon S3.  
- **Use Cases:** Backup, migration, and disaster recovery.  
- **Portable** across AZs and Regions.

**Snapshot Archive:**
- 75% cheaper than standard snapshots.  
- 24–72 hours restore time.

---

### 🔹 5. EBS Multi-Attach
- Attach one EBS volume to multiple EC2 instances (up to 16) in the same AZ.  
- Supported only for **io1** and **io2** volumes.  
- Requires a cluster-aware file system (e.g., GFS2, OCFS2).  
- Enables shared data access for high-availability applications.

---

### 🔹 6. EBS Encryption
- Encrypts data **at rest**, **in transit**, **snapshots**, and **derived volumes**.  
- Uses **AES-256 encryption** via AWS KMS keys (CMKs).  
- Minimal latency impact.  
- Encryption propagates through all copies.

**🔄 Encryption Process for Unencrypted Volume:**
1. Create a snapshot  
2. Copy snapshot with encryption enabled  
3. Create new encrypted volume

---

### 🔹 7. EC2 Instance Store (vs EBS)
| Feature | EBS | Instance Store |
|----------|-----|----------------|
| **Type** | Network-attached | Physical (on host) |
| **Persistence** | Persistent | Ephemeral |
| **Performance** | High | Very High |
| **Durability** | Replicated in AZ | Temporary |
| **Use Case** | Databases, apps | Caching, temp storage |

---

### 🔹 8. AMI (Amazon Machine Image)
- A template containing OS, app code, and configuration.  
- Can be customized for consistent infrastructure.

**To move across AZs:**
1. Create snapshot  
2. Customize if needed  
3. Launch in target AZ

---

### 🔹 9. Monitoring and Health
Amazon CloudWatch offers Basic and Detailed monitoring.

**Volume Status Checks:**
| Status | Meaning |
|---------|----------|
| ✅ OK | Normal volume |
| ⚠️ Warning | Degraded performance |
| ❌ Impaired | Stalled/failing volume |
| ⏳ Insufficient Data | Unknown state |

---

### 🔹 10. Backup & Automation
- **AWS Backup:** Automates backup across AWS services (EBS, RDS, DynamoDB, EFS, Storage Gateway).  
- **Amazon DLM (Data Lifecycle Manager):** Automates creation, retention, and deletion of snapshots to ensure compliance and cost efficiency.

---

### 🔹 11. Use Cases
**Primary storage for:**
- File systems  
- Databases  
- Applications requiring block-level access  

**Suitable for:**
- Random I/O (databases)  
- Sequential I/O (big data, logs)

---

## 📁 Amazon Elastic File System (EFS)

### 🔹 1. Overview
Amazon EFS (Elastic File System) is a fully managed, elastic **network file system (NFS)** that can be mounted by multiple EC2 instances across different Availability Zones (AZs).

- Scalable, durable, and highly available file storage without manual provisioning.  
- Can be accessed from on-premises servers via **AWS Direct Connect** or **VPN**.  
- Supports **Linux-based AMIs only**.

---

### 🔹 2. Key Features
| Feature | Description |
|----------|--------------|
| **Fully Managed** | No provisioning, management, or scaling required |
| **Scalable** | Automatically scales to petabytes |
| **Durable & Available** | Data replicated across multiple AZs |
| **Strong Consistency** | Supports file locking and consistent reads/writes |
| **POSIX-Compliant** | Works like a traditional Linux file system |
| **Encryption** | Supports encryption at rest (via KMS) and in transit |
| **Mount Targets** | Can be mounted across multiple instances in multiple AZs |
| **Integration** | Compatible with EC2, ECS, EKS, Lambda, and Fargate |

---

### 🔹 3. Storage Classes & Performance Modes
| Category | Options | Description |
|-----------|----------|-------------|
| **Performance Modes** | General Purpose / Max I/O | Choose based on latency and throughput needs |
| **Throughput Modes** | Bursting / Provisioned | Bursting scales with size; provisioned for steady workloads |
| **Storage Classes** | Standard / Infrequent Access / One Zone | Balance between cost and availability |

💡 **EFS One Zone** provides cost-optimized storage for non-critical workloads in a single AZ.  
**EFS Replication** offers cross-region data protection.

---

### 🔹 4. Use Cases
- Shared web content and static assets  
- Containerized or serverless workloads (ECS, EKS, Lambda)  
- Machine learning model storage  
- Data analytics and reporting  
- Content management systems  
- Database backups and logs  

---

## 📊 Comparison Tables

### EBS vs EFS vs Instance Store
| Feature | EFS | EBS | Instance Store |
|----------|-----|-----|----------------|
| **Type** | Network file system | Block storage | Local physical disk |
| **Access** | Multiple instances across AZs | Single instance, single AZ | Single instance only |
| **Scalability** | Auto-scaling | Must provision size | Fixed to instance hardware |
| **Persistence** | Persistent | Persistent | Ephemeral (data lost on stop/terminate) |
| **OS Support** | Linux (NFS) | Linux & Windows | Depends on instance type |
| **Cost** | Higher (pay-per-use) | Moderate | Included in instance cost |
| **Encryption** | KMS (at rest & transit) | KMS (at rest & transit) | Not supported |
| **Best Use** | Shared file storage | Databases, apps | Temporary/cache data |

---

## 🎯 Exam Tips (AWS SAA)

### EBS Exam Tips
✅ EBS is AZ-scoped → move via snapshots  
✅ Root volume deleted on termination by default  
✅ HDD volumes cannot be boot volumes  
✅ Multi-Attach → only io1/io2  
✅ EBS = Persistent | Instance Store = Ephemeral  
✅ Snapshots stored in S3 (not visible)  
✅ Use KMS for AES-256 encryption  
✅ CloudWatch for performance monitoring  
✅ DLM automates snapshot lifecycle  
✅ EFS is shared, while EBS is dedicated to one instance  
✅ Uses NFS protocol and POSIX compliance  
✅ Works across multiple AZs (unlike EBS)  
✅ Supports Linux-based workloads only  
✅ Scales automatically—no provisioning required  
✅ Choose EFS One Zone for cost savings  
✅ EFS Replication provides cross-region resilience  
✅ Encrypt data at rest with AWS KMS  
