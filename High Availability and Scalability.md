# ⚙️ High Availability and Scalability

---

## 📈 What is Scalability?

**Scalability** means that your application system can handle a greater load by adapting accordingly.  

There are two types of scalability:

1. **Vertical scalability**  
2. **Horizontal scalability (elasticity)**

---

### 🧱 Vertical Scalability

Vertical scalability means **increasing the size or capacity of a single instance**.  

**Example (Phone operator analogy):**
- A junior operator can take **5 calls/min**
- A senior operator can take **10 calls/min**

Upgrading from junior → senior = **vertical scaling**.

---

### ⚙️ Horizontal Scalability

Horizontal scalability means **increasing the number of instances or systems**.  

**Example (Phone operator analogy):**
- Hire a second operator → double capacity
- Hire six operators → further increase capacity

---

## 🏢 High Availability

**High availability (HA)** ensures your application runs **across multiple data centers or AZs** to survive failures.  

**Example:**
- 3 operators in New York, 3 in San Francisco  
- If NY goes down, SF operators continue serving calls  

### Types of High Availability:
1. **Passive:** Standby instance exists but is not serving (e.g., RDS Multi-AZ)
2. **Active:** Multiple instances actively serve traffic simultaneously

**Key:** HA is often implemented alongside horizontal scaling.

---

## 🌐 Load Balancers

**Function:** Knows which instances are healthy to maintain HA  
**Health check port:** `4567`  

### Elastic Load Balancer (ELB)

- Distributes traffic across multiple EC2 instances  
- Provides single endpoint for users  
- Handles health checks, SSL termination, and HA  
- AWS Managed Load Balancers:
  - Classic Load Balancer (deprecated)
  - Application Load Balancer (ALB)
  - Network Load Balancer (NLB)
  - Gateway Load Balancer (GLB)  

**Security:** Configure SGs to allow traffic to ELB and restrict EC2 traffic from only the ELB SG.

---

### Application Load Balancer (ALB)

- Layer **7**: HTTP, HTTPS, WebSocket  
- Fixed DNS name  
- Uses **target groups** to route traffic  
- Routing options:
  - Hostname
  - Pathname
  - Query string  
- **Connection termination:** Client → ALB → target group  
- Uses **X-Forwarded header** to get client IP  

---

### Network Load Balancer (NLB)

- Layer **4**: TCP, UDP  
- Handles **millions of requests per second**  
- Only in **one AZ**  
- Requires **Elastic IP**  
- Use case: **Extreme high-performance scenarios**  

---

### Gateway Load Balancer (GWLB)

- Works like a **watchman** for third-party services  
- Single entry/exit point for traffic  
- Supports **Geneve protocol (6081)**  
- Attachable to IPs only  

---

## 🍪 Sticky Sessions & Cookies

- Keeps user interaction tied to one machine  
- Uses **load balancer/application-based cookies**  

**Reserved cookie names:**
- AWSALB, AWSALBAPP, AWSALBTG (target-generated)
- AWSLB, AWSELB (duration-based, load balancer generated)

---

### 🔄 Cross-Zone Load Balancing

- Balances load **equally among all target groups**  

---

### 🔑 SNI (Server Name Indication)

- Enables multiple SSL certificates on a single server  
- Client must indicate the hostname  

---

## 📦 Auto Scaling Groups (ASG)

- Enables **automatic scaling** of EC2 instances based on load  
- Reduces manual scaling  
- Requires a **Launch Template**:
  1. Min instances
  2. Max instances
  3. Desired instances  

**Scaling Policies:**
- **Dynamic:**
  1. Target Tracking
  2. Simple Step Up
  3. Scheduled
- **Predictive:** Scale based on **forecasted load**

**Metrics for scaling:**
1. CPU utilization
2. Request count per target
3. Average Network In/Out
4. Custom metrics  

---

### 📝 Default Termination Policy

Ensures **AZs are balanced** when scaling in:

1. Choose AZ with **most instances** & at least one unprotected instance  
2. Terminate instance with **oldest launch template**  
3. If multiple candidates, terminate instance **closest to next billing hour**  
4. If still multiple, choose **randomly**  

**Launch Template:** Contains configuration for launching EC2 instances:
- AMI ID
- Instance type
- Key pair
- Security groups
- Block device mapping  

---

### ⏱ Cooldown Period

- Prevents **additional scaling before previous action completes**  
- Default: **300 seconds**  
- Configurable per ASG
