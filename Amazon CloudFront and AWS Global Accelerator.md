# 🪣 Amazon CloudFront & AWS Global Accelerator

## 1. Overview

- **AWS CloudFront** is a **Content Delivery Network (CDN)** that delivers web content (static, dynamic, streaming, APIs) globally with **low latency** and **high transfer speed**.
- Uses a **global network of edge locations** to cache content close to users.
- Workflow:
  1. User requests content.
  2. If cached in nearest edge → delivered immediately.
  3. If not cached → retrieved from **origin**, cached, and delivered.
- **AWS Global Accelerator** improves **availability** and **performance** for applications (HTTP & non-HTTP) at **Layer 4**, using two static IPs and AWS global network routing.

---

## 2. Core Components

| **Component** | **Description** |
| --- | --- |
| **Origin** | Source of content (S3, EC2, ALB, custom HTTP server). |
| **Distribution** | CloudFront configuration defining delivery behavior. |
| **Edge Location** | Global cache point where content is stored. |
| **Regional Edge Cache** | Larger cache between edge locations & origins to improve hit ratios. |

---

## 3. How CloudFront Delivers Content

1. Upload files to **origin**.
2. Create **CloudFront distribution** linked to origin.
3. CloudFront assigns a unique **domain** (`d123abc.cloudfront.net`).
4. Users access via domain → routed to **nearest edge location**.
5. Cached → served; otherwise → fetched, cached, delivered.
6. Edge-to-origin communication uses **AWS private network**.

---

## 4. Supported Protocols & Methods

- Protocols: **HTTP**, **HTTPS**, **WebSocket**.
- Methods: `GET`, `HEAD`, `POST`, `PUT`, `DELETE`, `OPTIONS`, `PATCH`.

---

## 5. Caching & Optimization

- Content cached close to users → reduced latency.
- **Cache-Control / Expires Headers:** define cache duration.
  - Example: `Cache-Control: max-age=3600` → cached 1 hour.
  - Minimum expiration: Web → 0 sec, RTMP → 3600 sec.
- **Query Strings:** affect cache keys.
- **TTL Settings:** Min, Max, Default TTL configurable.

---

## 6. Cache Behavior Settings

- Define **path patterns** (`/images/*`).
- Specify **origin**.
- Control **query strings, cookies, headers**.
- Enforce **HTTPS-only**.
- Require **signed URLs/cookies** for restricted access.
- Set **minimum TTL**.
- Use **cache & origin request policies** for fine-grained control.

**Invalidations:** Remove cached items manually (`/*` clears all).

---

## 7. Lambda@Edge

- Run **serverless functions** at edge locations.
- Modify requests/responses in real time.
- Supports custom authentication, A/B testing, SEO redirects.

---

## 8. Price Classes

- **Price Class 100:** US, Canada, Europe.
- **Price Class 200:** Adds Asia & Oceania.
- **All Edge Locations:** Global (default).

---

## 9. Security & Access Control

- **AWS WAF:** protects against SQL injection, XSS, HTTP floods.
- **AWS Shield:** DDoS protection (network & application layer).
- **SSL/TLS:** encrypts data in transit.
- **Geo-Restriction:** block/allow by user location.
- **Origin Access Control (OAC):** restrict S3 access to CloudFront.
- **Field-Level Encryption:** encrypt sensitive form data.
- **Custom HTTP Headers:** restrict origin access.
- **Response Headers Policy:** modify/remove headers in responses.
- **Managed Prefix List:** restrict origin traffic to CloudFront IPs.

---

## 10. Compliance

- **PCI DSS** (payment data)
- **HIPAA-eligible** (healthcare data)
- **SOC 1, SOC 2, SOC 3**

---

## 11. Pricing

- **Data Transfer Out** (internet & AWS regions)
- **HTTP/HTTPS Requests**
- **Invalidation Requests** (first 1,000 free)
- **Dedicated IP Custom SSL Certificates**
- **Field-Level Encryption**
- **S3 costs** if used as origin

---

## 12. CloudFront vs S3 Cross-Region Replication

| **Feature** | **CloudFront** | **S3 CRR** |
| --- | --- | --- |
| **Purpose** | Global content delivery (read-only caching) | Replicate entire datasets across regions |
| **Setup** | One origin → multiple edge locations | Configure each target region manually |
| **Cost** | Lower (transfer & requests) | Higher (storage & replication) |
| **Use Case** | Fast global content delivery | Backup / compliance data replication |

---

## 13. AWS Global Accelerator

### Overview

- Improves **availability & performance** for apps.
- **Layer 4 (Transport Layer)**.
- Provides **two static IPv4 addresses** as global fixed entry.
- Routes traffic via **AWS global backbone** to **nearest healthy endpoint** (ALB, NLB, EC2).

### Key Features & Benefits

| **Feature** | **Description** |
| --- | --- |
| Performance Optimization | Traffic enters nearest AWS edge, routed via low-latency backbone. |
| Instant Regional Failover | Redirects traffic within seconds if endpoint unhealthy. |
| High Availability | Two static IPs, independent zones for fault isolation. |
| No DNS Propagation Delays | IPs stay same even if endpoints/regions change. |
| Traffic Dials | Adjust traffic % per region (blue/green testing). |
| Client Affinity | Ensures requests go to same endpoint (stateful apps). |
| Bring Your Own IP (BYOIP) | Use own IP range on AWS edge network. |

### Use Cases

- Multi-region/global web apps
- Gaming, media, IoT needing ultra-low latency
- Disaster recovery & automatic failover
- Region migrations & performance testing

---

## 14. CloudFront vs Global Accelerator

| **Aspect** | **CloudFront** | **Global Accelerator** |
| --- | --- | --- |
| **Purpose** | CDN for content delivery | Network accelerator for routing |
| **OSI Layer** | Layer 7 (Application) | Layer 4 (Transport) |
| **Traffic Type** | HTTP/HTTPS/WebSocket | TCP/UDP (any protocol) |
| **Caching** | ✅ Yes | ❌ No |
| **Routing Basis** | URL, headers, query strings | Network latency & endpoint health |
| **Static IPs** | ❌ DNS-based | ✅ Two fixed IPs |
| **Failover** | Limited | Fast automatic regional failover |
| **Integration Targets** | S3, EC2, ALB, Lambda@Edge | ALB, NLB, EC2, EIPs |
| **Use Case** | Websites, APIs, media delivery | Multi-region apps, gaming, IoT, API acceleration |

---

## 15. AWS WAF Integration (with CloudFront)

- Protects against web exploits (SQL injection, XSS, HTTP floods)
- Works with **CloudFront**, **ALB**, **API Gateway**, **AppSync**
- **Rate-based rules:** block IPs sending too many requests
- Integrated with **AWS Shield Advanced**
- Define **Web ACLs** with filtering rules

---

## 16. Summary Table

| **Service** | **Main Goal** |
| --- | --- |
| **CloudFront** | CDN that caches & serves content globally with low latency |
| **Global Accelerator** | Optimizes network routing for high availability & low latency using AWS global backbone |
