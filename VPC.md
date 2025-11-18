# **AWS VPC (Virtual Private Cloud)**


## **1. CIDR (Classless Inter-Domain Routing)**

- **CIDR** defines IP address ranges using:
    1. **Base IP:** Starting IP (e.g., 10.0.0.0)
    2. **Subnet Mask:** Defines how many bits can vary.
- **Examples:**

| CIDR | Meaning | Example Range | # of IPs |
| --- | --- | --- | --- |
| /32 | No change | 192.168.0.0 | 1 |
| /24 | Last octet changes | 192.168.0.0 → 192.168.0.255 | 256 |
| /16 | Last 2 octets change | 192.168.0.0 → 192.168.255.255 | 65,536 |
| /8 | Last 3 octets change | 10.0.0.0 → 10.255.255.255 | 16M+ |

- **AWS Note:** Each subnet reserves **5 IPs** (network, broadcast, etc.).

---

## **2. Private vs Public IPs**

- **Private IP Ranges:**
    - 10.0.0.0/8 → Large enterprise networks
    - 172.16.0.0/12 → AWS default range
    - 192.168.0.0/16 → Home networks
- All others are **Public IPs**.

---

## **3. Subnets**

- A **subnet** is a sub-range of IPs within a VPC.
- Used to isolate and manage network resources.

---

## **4. Internet Connectivity**

### **Internet Gateway (IGW)**
- Enables internet access for subnets.
- Scales horizontally, redundant.

### **Bastion Host**
- Deployed in **public subnet** to SSH into private EC2s.
- Only **SSH (port 22)** allowed from internet.
- Acts as a **jump box**.

### **NAT Instance / NAT Gateway**
- Allow **private subnet resources** to access internet.
- **NAT Instance:** Self-managed, acts like a proxy.
- **NAT Gateway:** Managed, scalable, HA; one per AZ for redundancy.
    - Cannot be used as a Bastion host.

---

## **5. NACLs vs Security Groups**

| Feature | Security Group | NACL |
| --- | --- | --- |
| Level | Instance | Subnet |
| Type | Stateful | Stateless |
| Rules | Allow only | Allow & Deny |
| Evaluation | All rules | Rule order (lowest # first) |
| Return Traffic | Auto allowed | Must be explicitly allowed |
| Default | Allow all | Allow all |

- **Ephemeral Ports:** Temporary client-side ports for return traffic.
- Always allow ephemeral ranges for response traffic.

---

## **6. VPC Peering**

- Connects **two VPCs privately** (same or different regions).
- Traffic stays on AWS backbone.
- **No transitive peering** (A↔B↔C not auto-connected).

---

## **7. VPC Endpoints**

Allow private AWS service access **without internet**.

| Type | Backed By | Use Case | Cost |
| --- | --- | --- | --- |
| Gateway Endpoint | Route table target | S3, DynamoDB | Free |
| Interface Endpoint | ENI + PrivateLink | Most AWS services | Hourly + Data |

- Gateway Endpoint → simple, free → preferred for S3/DynamoDB.
- Interface Endpoint → supports SGs, useful for hybrid/on-prem.

---

## **8. VPC Flow Logs**

- Capture **IP traffic metadata** at VPC/Subnet/ENI level.
- Includes source/dest IPs, ports, protocol, action (ACCEPT/REJECT).
- Stored in **S3**, **CloudWatch**, or **Kinesis**.
- Useful for diagnosing SG/NACL issues.

---

## **9. Site-to-Site VPN**

### Components:
- **Virtual Private Gateway (VGW)** – AWS VPN concentrator.
- **Customer Gateway (CGW)** – On-prem router/software.

### Notes:
- VPN encrypted over internet.
- Enable **route propagation** in route tables.
- ICMP/ping allowed via EC2 SG.
- **CloudHub:** Multi-site VPNs with single VGW (hub-and-spoke).

---

## **10. Direct Connect**

- **Dedicated private line** between on-prem & AWS.
- **Types:**
    - Dedicated Connection: Physical 1–100 Gbps.
    - Hosted Connection: Partner-managed.
- Benefits: Low latency, consistent performance, lower egress cost.
- Integrates with **VPCs**, **Transit Gateway**, VPNs.

---

## **11. VPC Traffic Mirroring**

- Capture/analyze **live network traffic** from ENIs.
- Mirror to **ENI** or **NLB** for inspection.
- Filters & aggregation supported.
- Use cases: **threat monitoring, content inspection**.
- Works within or across VPCs (peering).

---

## **12. Transit Gateway**

### Purpose:
- Hub for connecting:
    - Multiple VPCs
    - VPNs
    - Direct Connect
    - On-prem networks
- Simplifies hub-and-spoke topology.

### Features:
- Regional resource with cross-region peering.
- Route tables control communication.
- Supports **IP multicast**.
- **ECMP:** parallel tunnels for higher throughput.
- Integrates with **Direct Connect Gateway**.

### Throughput Example:
- VGW VPN: 1.5 Gbps
- Transit GW VPN: 2.5 Gbps per connection.

---

## **13. Egress-Only Internet Gateway (IPv6)**

| Gateway Type | Purpose | Traffic | Direction |
| --- | --- | --- | --- |
| Internet Gateway | IPv4/IPv6 | Inbound & Outbound | Public |
| NAT Gateway | IPv4 | Outbound only | Private |
| Egress-Only IGW | IPv6 | Outbound only | Private |

- Prevents inbound IPv6 traffic.
- Route `::/0` for outbound IPv6.

---

## **14. AWS Networking Costs**

- **Inbound EC2:** Free
- **Same AZ (Private IP):** Free
- **Cross-AZ:** $0.01/GB
- **Cross-Region:** $0.02/GB
- **Public IP:** More expensive

**Best Practices:**
- Use private IPs intra-region.
- Keep high-traffic services in same AZ.
- Use **Gateway Endpoints** to avoid NAT costs.

**S3 Example Costs:**
- Upload: Free
- Download: $0.09/GB
- CloudFront → S3: Free
- Cross-region replication: $0.02/GB
- Gateway Endpoint → S3: $0.01/GB vs NAT ($0.045/GB + hourly)

---

## **15. AWS Network Firewall**

- **Fully managed L3–L7 firewall** for VPCs.
- Inspects:
    - Inbound/outbound
    - VPC ↔ VPC
    - Direct Connect / VPN

### Features:
- Uses **AWS Gateway Load Balancer**.
- Supports IP, port, protocol, domain & regex filtering.
- Configurable: **allow / drop / alert**.
- Logs → S3, CloudWatch, Kinesis.
- Managed centrally via **Firewall Manager**.

---

## **🔥 Quick Exam Tips**

- /32 = 1 IP, /24 = 256 IPs
- Private IPs: 10.0.0.0/8, 172.16.0.0/12, 192.168.0.0/16
- 5 IPs reserved in every subnet
- NACL = Stateless, SG = Stateful
- Gateway Endpoint = Free, Interface Endpoint = Costly
- Transit Gateway = Hub-and-spoke for multiple VPCs
- Egress-only IGW = IPv6 NAT equivalent
- VPC Flow Logs → Debug SG/NACL issues
- Inbound = Free, Outbound = Charged
- AWS Network Firewall = L3–L7 VPC protection
