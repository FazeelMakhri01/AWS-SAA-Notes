# Amazon EC2 Notes


Comprehensive notes for **Amazon EC2**, covering instance types, pricing, key features, storage, networking, security, monitoring, and best practices.


---


## 📖 Table of Contents


1. [Overview](#overview)
2. [Instance Types](#instance-types)
3. [Pricing Models](#pricing-models)
4. [Key Features](#key-features)
5. [Instance States](#instance-states)
6. [Root Device Volumes](#root-device-volumes)
7. [Amazon Machine Images (AMI)](#amazon-machine-images-ami)
8. [EC2 Image Builder](#ec2-image-builder)
9. [EC2 Pricing Details](#ec2-pricing-details)
10. [Security](#security)
11. [Networking](#networking)
12. [Monitoring](#monitoring)
13. [Instance Metadata & User Data](#instance-metadata--user-data)
14. [Placement Groups](#placement-groups)
15. [Storage Options](#storage-options)
16. [Resources & Tagging](#resources--tagging)
17. [Summary Takeaways](#summary-takeaways)


---


<details>
<summary>Overview</summary>


Amazon Elastic Compute Cloud (EC2) provides secure, resizable compute capacity in the cloud. You can provision Linux-based, Windows-based, or Mac-based virtual servers.


**Limits:**
- On-Demand Instances: Limited by your vCPU-based On-Demand limit.
- Reserved Instances: Max 20 per account.
- Spot Instances: Limited by dynamic Spot limit per region.
- New AWS accounts may have lower default limits.


</details>


<details>
<summary>Instance Types</summary>


| Type | Examples | Use Case |
| --- | --- | --- |
| General Purpose | M7g, T4g, M7i, T3, M7a, M7gd | Balanced compute, memory, networking (e.g., web servers) |
| Compute Optimized | C7g, C7i, C7a, C7gd | CPU-intensive tasks (e.g., batch processing, ML) |
| Memory Optimized | R7iz, X2idn, X2iedn, R7a, R7g, U7i, I7i, I7ie | Large in-memory datasets, databases, real-time processing |
| Accelerated Computing | P4d, G5, VT1, P5en, Trn2 | GPU-intensive workloads, HPC, ML |
| Storage Optimized | Im4gn, Is4gen, I4i | High-frequency local storage access, OLTP, data warehousing |


**Naming Convention:** Instance class (M) + generation (5) + size (2xlarge)


</details>


<details>
<summary>Pricing Models</summary>


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
- On-Demand: Pay full price as you go
- Reserved: Plan ahead, get discount
- Savings Plan: Commit monthly spend
- Spot: Last-minute discounted capacity
- Dedicated Host: Entire building
- Capacity Reservation: Book a room you may/may not use


</details>


<details>
<summary>Key Features</summary>


- **Nitro System:** Enhanced security & performance, near bare-metal speed.
- **Elastic Fabric Adapter (EFA):** Low-latency network for HPC/ML workloads.
- **Capacity Blocks for ML:** Reserved GPU capacity for specific durations.
- **EC2 Instance Connect:** Browser-based SSH access.
- **EC2 Fleet / Spot Fleet:** Combine On-Demand, Reserved, Spot instances.
- **Best Practices:**
- Use IMDSv2 for metadata access
- Enable Detailed Monitoring for <1-min metrics
- Use Placement Groups (Cluster, Spread, Partition)
- EC2 Auto Scaling for elasticity & HA
| Instances | AZ | Instance ID tied to region |
