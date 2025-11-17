# Amazon S3 (Simple Storage Service)

## 1. Overview & Key Features

- S3 = Simple Storage Service, “building block of AWS.”
- Can host **static websites**.
- Known as **infinitely scaling storage**.
- Objects are stored **in buckets**; storage is virtually unlimited.

---

## 2. S3 Buckets

- Buckets are **containers for objects**.
- **Globally unique name** (DNS-compliant).
- Defined at **region level**; region **cannot be changed** after creation.
- **Naming rules**: lowercase, no uppercase, unique across all AWS accounts.
- Max **100 buckets per account** by default.
- Bucket URL visible in object URLs.
- **Static website hosting** possible via bucket configuration.
- **Deletion constraints**:
  - Console: cannot delete bucket with ≥100,000 objects.
  - CLI: cannot delete versioned buckets unless versions removed.

**Bucket Capabilities:**

- Control access (create, delete, list objects).
- View access logs.
- Choose storage region.

---

## 3. S3 Objects

- Files stored in buckets are **objects**.
- **Key** = unique path for an object.
- Upload limits:
  - Single operation ≤ 5 GB.
  - Multipart upload for 5 GB – 5 TB.
- **Object Lambda** supports: `HeadObject`, `ListObjects`, `ListObjectsV2`.

**Tagging:**

- Max 10 tags per object.
- Tag key: ≤128 Unicode chars, value: ≤256 chars, case-sensitive.

**Deletion:**

- Versioned bucket: specify version ID to delete specific version.
- MFA-enabled bucket: MFA required to delete versioned objects.

**Object Lock:**

- Prevents deletion/overwrite.
- Retention options: **Retention Period** or **Legal Hold**.
- Works **only in versioned buckets**.

**Ownership:**

- `bucket-owner-full-control` ACL allows bucket owner to assume object ownership.

---

## 4. S3 Security & Access

**Policies contain:**  
Resources, Actions, Effect (Allow/Deny), Principal.

**Resource-Based Policies:**

- **Bucket Policies**: centralized control, allow/deny by conditions (IP, requester, operation).
- Limit: 20 KB.

**ACLs (Legacy):**

- Grant permissions to AWS accounts only.
- Cannot deny or set conditions.
- Recommended use: **S3 Log Delivery group**.

**Bucket vs Identity Policies:**

| Type | Scope | Use Case |
| --- | --- | --- |
| Identity | Controls what identity can access | IAM users across account |
| Resource | Controls who can access resource | Cross-account or anonymous access |

💡 **Exam Tip:** Avoid ACLs unless absolutely necessary.

---

## 5. Versioning & MFA Delete

**Versioning:**

- Keep multiple versions, protects from overwrites/deletions.
- Default: **disabled**; must enable explicitly.
- PUT in versioned bucket: previous versions preserved.
- DELETE: inserts **delete marker**, versions remain.
- Outposts buckets: **Unversioned, Enabled, Suspended**.

**Backup:**

- AWS Backup supports continuous/periodic backups, automated scheduling, restoration.

**MFA Delete:**

- Requires MFA to change versioning state or permanently delete object versions.

---

## 6. Storage Classes & Lifecycle Management

**Frequently Accessed:**

- **STANDARD**: general purpose, 99.99% availability.
- **EXPRESS ONE ZONE**: single AZ, millisecond latency, reduces request costs.

**Infrequently Accessed:**

- **STANDARD_IA**: multi-AZ, ≥128 KB, ≥30 days.
- **ONEZONE_IA**: single AZ, lower cost, less resilient.

**Intelligent-Tiering:**

- Auto-moves data between **frequent/infrequent access tiers**.
- Archive tiers: 90 days → archive, 180 days → deep archive.
- No retrieval fees.

**Glacier Classes:**

| Class | Retrieval Time | Ideal Use |
| --- | --- | --- |
| **Instant Retrieval** | Milliseconds | Quarterly access |
| **Flexible Retrieval** | Minutes–hours | Rare access |
| **Deep Archive** | 12–48 hrs | Long-term archives |

**Object Lifecycle:**

- **Transition actions**: move storage classes over time.  
  Example: `STANDARD → STANDARD_IA → GLACIER → Deep Archive`
- **Expiration actions**: delete objects or old versions after a period.

---

## 7. Networking & Data Transfer

**Access Methods:**

- **Virtual Hosted–Style:** `bucketname.s3.amazonaws.com` or region-specific.
- **Path-Style:** `s3.amazonaws.com/bucket` or region-specific endpoint.
- **CNAME customization** allowed (bucket name must match).

**Transfer Acceleration:**

- Uses CloudFront edge locations for faster global transfers.
- URL: `bucket.s3-accelerate.amazonaws.com`.
- Cannot be disabled, only suspended.

**Requester Pays:**

- Requester pays transfer costs; bucket owner still pays storage.

---

## 8. Performance Optimization

**Single PUT Upload:**

- ≤ 5 GB, fails if stream fails.

**Multipart Upload:**

- ≥100 MB recommended.
- Max 10,000 parts, can retry parts independently.
- Improves throughput and upload speed.

**Transfer Acceleration:**

- Best for long-distance transfers.
- Requires bucket DNS-compatible name (no periods).

**Throughput:**

- 3,500 PUT/COPY/POST/DELETE  
- 5,500 GET/HEAD per second **per prefix**

---

## 9. Encryption

**Server-Side Encryption (SSE):**

| Type | Key Managed By | Description |
| --- | --- | --- |
| **SSE-S3** | AWS | AES-256 encryption (default) |
| **SSE-KMS** | AWS KMS | Enhanced security + audit trail |
| **SSE-C** | Customer | Client-provided keys, HTTPS required |

**Client-Side Encryption:**

- Encrypt locally before upload.
- Options: AWS KMS key or client-managed master key.

**Encryption in Transit:**

- HTTPS recommended.
- Bucket policy can enforce HTTPS using `aws:SecureTransport`.

---

## 10. Cross-Account Access

- **Resource-based policies** & **IAM** for programmatic access.  
- **Cross-account IAM roles** for console + programmatic access.  
- **Cross-account Access Points** supported.  
- **Requester Pays Buckets**: requester pays download/transfer costs.

---

## 11. Pricing Summary

- **Storage cost**: per GB/month.
- **Data transfer:**
  - **IN:** free.
  - **OUT:** charged per GB.
- **Operation cost:** per 1,000 requests.
- Static website hosting: minimal transfer charges.

---

## 12. Key Exam Takeaways

✅ Buckets = containers; objects = files.  
✅ Enable versioning explicitly to prevent data loss.  
✅ Lifecycle policies automate cost optimization (transition + expiration).  
✅ **SSE-S3** is default; **SSE-KMS** adds audit & control; **SSE-C** = external key mgmt.  
✅ Multipart uploads for >100 MB; Transfer Acceleration for global uploads.  
✅ ACLs are **legacy** — use IAM or bucket policies.  
✅ Intelligent-Tiering saves cost for unpredictable access.  
✅ Glacier classes = archival storage; retrieval time varies.  
✅ MFA Delete = protection for versioned buckets.
