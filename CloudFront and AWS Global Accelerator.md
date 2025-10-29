# 🌍 Amazon CloudFront & AWS Global Accelerator

## 🚀 Amazon CloudFront (CDN)

### 🔹 1. Overview
Amazon **CloudFront** is a **Content Delivery Network (CDN)** that securely delivers static, dynamic, streaming, and API content to users globally with **low latency** and **high transfer speed**.

- Uses a global network of **edge locations** to cache and serve content closer to users.  
- Requests are routed to the **nearest edge location** for optimal performance.  
- If content isn’t cached, CloudFront retrieves it from the **origin** (e.g., S3, EC2, ALB).

---

### 🔹 2. Core Components
| Component | Description |
|------------|-------------|
| **Origin** | The original source of content (S3, EC2, ALB, or custom server). |
| **Distribution** | The CloudFront configuration defining how content is delivered. |
| **Edge Location** | Cache endpoint for delivering content to nearby users. |
| **Regional Edge Cache** | Intermediate cache layer between edge locations and origins for better performance. |

---

### 🔹 3. How CloudFront Works
1. Upload content to your **origin** (S3, EC2, or custom HTTP server).  
2. Create a **CloudFront distribution** linked to the origin.  
3. CloudFront provides a unique domain name (e.g., `d123abc.cloudfront.net`).  
4. Users access content via that domain.  
5. CloudFront routes the request to the **nearest edge location**.  
6. Cached content → served immediately; otherwise fetched from origin and cached.

💡 **CloudFront uses AWS’s private network** between edge locations and origins for secure, fast, and reliable delivery.

---

### 🔹 4. Supported Protocols & Methods
- **Protocols:** HTTP, HTTPS, WebSocket  
- **HTTP Methods:** `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `OPTIONS`, `PATCH`

---

### 🔹 5. Caching & Optimization
- CloudFront **caches content** at edge locations to minimize latency.  
- Controlled via **Cache-Control** or **Expires** headers.  
  - Example: `Cache-Control: max-age=3600` → cache for 1 hour.
- TTL Settings: **Min TTL**, **Max TTL**, and **Default TTL** (per behavior).  
- **Query Strings, Cookies, and Headers** can be used as cache keys.

**Invalidations:**  
- Use invalidation requests (e.g., `/*`) to remove cached objects manually.

---

### 🔹 6. Cache Behaviors
Define how CloudFront handles specific paths and origins.

| Configuration | Description |
|----------------|-------------|
| **Path Pattern** | Define route (e.g., `/images/*`) |
| **Origin Routing** | Choose which origin serves which paths |
| **Query String Forwarding** | Control how query parameters affect cache |
| **HTTPS Enforcement** | Require HTTPS-only access |
| **Signed URLs/Cookies** | Restrict access to private content |
| **Cache & Origin Policies** | Fine-tune TTL, cache keys, compression, and forwarding behavior |

---

### 🔹 7. Lambda@Edge
- Run **serverless functions** at edge locations.  
- Modify requests/responses in real time.  
- Common use cases:
  - Redirects / URL rewrites  
  - Header modifications  
  - Custom authentication  
  - A/B testing  
  - SEO optimizations

---

### 🔹 8. Price Classes
Control cost by choosing which regions serve your content.

| Price Class | Regions Included |
|--------------|------------------|
| **100** | US, Canada, Europe |
| **200** | Adds Asia & Oceania |
| **All Edge Locations** | Global (default) |

---

### 🔹 9. Security & Access Control
| Feature | Description |
|----------|-------------|
| **AWS WAF Integration** | Protects against SQLi, XSS, and HTTP floods |
| **AWS Shield** | DDoS protection (network & app layers) |
| **SSL/TLS** | Encrypts data in transit |
| **Geo-Restriction** | Restrict access by location |
| **Origin Access Control (OAC)** | Secure S3 origins (replaces OAI) |
| **Field-Level Encryption** | Encrypts sensitive form data (e.g., credit cards) |
| **Custom HTTP Headers** | Restrict origin access to CloudFront-only traffic |
| **Response Headers Policy** | Modify or strip response headers |
| **Managed Prefix List** | Limit origin inbound traffic to CloudFront IPs |

---

### 🔹 10. Compliance
✅ **PCI DSS** – for payment data  
✅ **HIPAA-eligible** – for healthcare data  
✅ **SOC 1 / SOC 2 / SOC 3** – compliance-ready

---

### 🔹 11. Pricing Overview
You pay for:
- **Data Transfer Out** to the internet or between regions  
- **HTTP/HTTPS Requests**  
- **Invalidation Requests** (first 1,000 free/month)  
- **Custom SSL (Dedicated IP)**  
- **Field-Level Encryption**  
- **S3 costs** if S3 is used as origin  

💡 Optimize with caching, compression, and limited invalidations to reduce costs.

---

### 🔹 12. CloudFront vs S3 Cross-Region Replication
| Feature | CloudFront | S3 CRR |
|----------|-------------|--------|
| **Purpose** | Global content delivery (read-only caching) | Cross-region redundancy (full data replication) |
| **Setup** | Single origin, multiple edge locations | Configure per target region |
| **Cost** | Pay for data transfer & requests | Pay for storage + replication |
| **Use Case** | Low-latency global access | Backup or compliance data replication |

---

## 🌐 AWS Global Accelerator

### 🔹 1. Overview
**AWS Global Accelerator** enhances **availability** and **performance** for both **HTTP and non-HTTP** applications.

- Operates at **Layer 4 (Transport Layer)**.  
- Provides **two static anycast IPv4 addresses** for global entry.  
- Routes traffic over AWS’s **low-latency global network** to the nearest healthy endpoint.

---

### 🔹 2. Key Features & Benefits
| Feature | Description |
|----------|-------------|
| **Performance Optimization** | Routes traffic via AWS backbone for low latency |
| **Instant Failover** | Redirects traffic within seconds (< 1 minute) |
| **High Availability** | Two static IPs served from independent zones |
| **No DNS Propagation** | IPs remain fixed even if endpoints change |
| **Traffic Dials** | Adjust regional traffic percentages |
| **Client Affinity** | Session stickiness for stateful apps |
| **Bring Your Own IP (BYOIP)** | Use your own IPs on AWS’s edge network |

---

### 🔹 3. Use Cases
- Multi-region, mission-critical applications  
- Gaming, media streaming, IoT  
- Disaster recovery / automatic failover  
- Blue-green or regional migration deployments  

---

## ⚖️ CloudFront vs Global Accelerator

| Aspect | **Amazon CloudFront** | **AWS Global Accelerator** |
|---------|------------------------|-----------------------------|
| **Purpose** | CDN for content delivery | Network performance optimization |
| **OSI Layer** | Layer 7 (Application) | Layer 4 (Transport) |
| **Traffic Type** | HTTP / HTTPS / WebSocket | TCP / UDP (any protocol) |
| **Caching** | ✅ Yes | ❌ No |
| **Routing Basis** | URL paths, headers, query strings | Latency & endpoint health |
| **Static IPs** | ❌ Uses DNS | ✅ Two fixed IPs |
| **Failover** | Basic (via caching/origin) | Fast regional failover |
| **Integrations** | S3, EC2, ALB, Lambda@Edge | ALB, NLB, EC2, EIPs |
| **Best For** | Websites, APIs, media delivery | Multi-region apps, IoT, games |

---

## 🧱 AWS WAF Integration (with CloudFront)
- **AWS WAF** protects web apps from exploits (SQLi, XSS, HTTP floods).  
- Works with **CloudFront**, **ALB**, **API Gateway**, and **AppSync**.  
- Supports **rate-based rules** (e.g., DDoS throttling).  
- Integrates with **AWS Shield Advanced** for enhanced DDoS protection.  
- Use **Web ACLs** (Access Control Lists) to define filtering rules.  

---

## 🎯 Exam Tips (AWS SAA)
✅ CloudFront = CDN at **Layer 7**, Global Accelerator = **Layer 4**.  
✅ CloudFront caches content; Global Accelerator only optimizes routing.  
✅ Global Accelerator provides **static IPs**; CloudFront uses DNS-based endpoints.  
✅ Use **OAC** (Origin Access Control) for private S3 origins.  
✅ CloudFront integrates with **AWS WAF + Shield** for security.  
✅ Lambda@Edge executes functions globally for low latency.  
✅ Price Classes control cost by limiting edge locations.  
✅ CloudFront works with **HTTP/S**, Global Accelerator supports **any TCP/UDP** app.  
✅ CloudFront invalidations remove cached objects manually.  
✅ Global Accelerator supports **instant failover** (<60s).  

---

## 📊 Summary Table
| Service | Main Goal |
|----------|------------|
| **Amazon CloudFront** | CDN that caches and serves content globally with low latency. |
| **AWS Global Accelerator** | Optimizes network routing for high availability and low latency using AWS’s global backbone. |

---
