# 🌐 Amazon Route 53 (DNS Service)

## 1. Overview

- **Amazon Route 53** is a **highly available and scalable Domain Name System (DNS)** web service.
- It is AWS’s **only 100% available service**.
- Provides **domain registration**, **DNS routing**, and **health checking** capabilities.
- Also functions as a **domain registrar**.

---

## 2. Core Concepts

- **Domain → Hosted Zone → AWS Assets**
- **Hosted Zone:** Container for DNS records that control traffic routing.
- **Name Server (NS):** Resolves the domain name to the correct hosted zone.
- **A Record:** Maps a domain to an **IPv4 address**.
- **CNAME Record:** Maps a domain name to **another domain name**.
- **TTL (Time To Live):** Duration DNS clients cache the response.

---

## 3. Key Features

- DNS Resolver  
- Traffic Flow Management  
- Latency-Based Routing  
- Geo DNS & Geolocation Routing  
- Private DNS for Amazon VPC  
- DNS Failover  
- Health Checks & Monitoring  
- Domain Registration  
- CloudFront & S3 Zone Apex Support  
- Integration with Amazon ELB  

---

## 4. Routing Policies in Route 53

> Routing policies define **how DNS queries are answered**, not how traffic is routed at the network layer.

---

### **4.1 Simple Routing Policy**

- Routes traffic to a **single resource**.
- Returns one or more IPs; clients randomly choose one.
- Does **not** support health checks.
- **Alias records** can point only to one AWS resource.

**Use Case:**  
Direct traffic to a single EC2 instance or endpoint.

**Key Points:**  
- Quick and straightforward to configure.  
- Multiple IPs allowed; client picks one randomly.  
- TTL controls caching duration.

---

### **4.2 Weighted Routing Policy**

- Distributes traffic **based on weights** assigned to multiple resources.
- Traffic % = (Record Weight / Sum of Weights).
- **Weights do not need to total 100**.
- Supports **health checks**.

**Use Cases:**  
- Gradual traffic migration.  
- Blue/green deployments.  
- A/B testing.

**Key Points:**  
- Records must share the same name and type.  
- Weight = 0 → No traffic routed.  
- Useful for controlled rollouts.

---

### **4.3 Latency-Based Routing**

- Routes users to the **region with the lowest latency**.
- Enhances user experience by reducing response time.
- Supports **health checks**.

**Use Case:**  
Global user base with latency-sensitive workloads.

**Testing Tip:**  
Use VPNs to simulate global traffic and validate routing results.

---

### **4.4 Failover Routing Policy**

- Enables **automatic DNS failover**.
- **Primary record** must have an associated **health check**.
- Failover logic:  
  - Primary unhealthy → Route traffic to secondary.  
  - Primary recovers → Automatically fail back.

**Use Cases:**  
- High availability.  
- Disaster recovery.  
- Backup systems.

**Key Points:**  
- One **primary** and one **secondary** per pair.  
- Fully automatic failover and failback.

---

### **4.5 Geolocation Routing Policy**

- Routes traffic based on the **geographic location** of the user.  
- Supports continent, country, and state-level routing.
- **Most specific location wins**.
- Includes a **default record**.
- Supports **health checks**.

**Use Cases:**  
- Region-specific content.  
- Regulatory/local restrictions.  
- Country-targeted services.

---

### **4.6 Geoproximity Routing Policy**

- Routes traffic based on **location of users and resources**.
- Supports **bias** adjustments:  
  - **Positive bias:** Increase region influence.  
  - **Negative bias:** Decrease region influence.
- Works via **Traffic Flow** only.
- For non-AWS resources → manual latitude/longitude required.

**Use Cases:**  
- Gradual region migrations.  
- Geographical load distribution.

---

### **4.7 IP-Based Routing Policy**

- Routes traffic based on **client IP ranges (CIDRs)**.
- Maps each IP range to a specific AWS or on-prem resource.
- Helps optimize performance or control access.

**Use Cases:**  
- Corporate networks.  
- ISP-based routing.  
- Traffic engineering.

---

### **4.8 Multi-Value Answer Routing**

- Returns **up to 8 healthy records** per DNS query.
- Each record can have a **health check**.
- Provides **basic client-side load balancing**.
- **Not a replacement** for an Elastic Load Balancer.

**Use Cases:**  
- Lightweight load balancing.  
- Redundant multi-instance routing.

**Key Points:**  
- Only **healthy** records are returned.  
- TTL defines how frequently clients refresh data.

---

## 5. Route 53 Resolvers & Hybrid DNS

### **5.1 Route 53 Resolver**

Default resolver handles:
- Private hosted zone records.
- Public DNS query resolution.
- Internal DNS for EC2 instances.

---

### **5.2 Hybrid DNS Setup**

Enables DNS resolution between **AWS** and **on-prem** networks.

---

### **5.3 Resolver Endpoints**

**Inbound Endpoint:**  
- On-prem resolvers query AWS private hosted zones.

**Outbound Endpoint:**  
- AWS resources query on-prem DNS servers.

**Requirement:**  
- Secure connectivity: **VPN** or **Direct Connect**.

**Examples:**  
- On-prem server resolving `internal.aws.local`  
- EC2 instance resolving `web.onprem.private`

---

## 6. Alias Records

- Special Route 53 records pointing to AWS resources:
  - S3 buckets (static website hosting)
  - CloudFront distributions
  - Load Balancers (ELB/ALB/NLB)
  - API Gateway
- **Alias = free**, no cost per query.
- **No IP address required**.

---

## 7. Summary Table

| **Routing Policy** | **Description** | **Health Checks** | **Use Case** |
| --- | --- | --- | --- |
| Simple | Directs traffic to one/many IPs randomly | ❌ | Basic routing |
| Weighted | Routes traffic by weight | ✅ | Gradual shifting, A/B tests |
| Latency | Routes by lowest latency | ✅ | Global performance |
| Failover | Primary → Secondary failover | ✅ (Primary req.) | HA & DR |
| Geolocation | Based on user region | ✅ | Local content/compliance |
| Geoproximity | Location + bias | ✅ | Traffic shifting by region |
| IP-Based | Based on client IP ranges | ✅ | Corporate/ISP routing |
| Multi-Value | Multiple healthy records | ✅ | Simple load balancing |

---

## 8. Final Key Points

- **Route 53** is a global, scalable, and reliable DNS service.  
- **TTL** controls DNS caching behavior.  
- **Health checks** ensure traffic is routed only to healthy endpoints.  
- **Alias Records** seamlessly integrate AWS services.  
- Supports both **public** and **private** hosted zones.  
- **Hybrid DNS** supported using **inbound/outbound resolver endpoints**.

