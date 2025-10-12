# AWS IAM & Organizations

Comprehensive notes for **AWS IAM & Organizations**, including IAM users, roles, policies, AWS Organizations, and related services. Ideal for solution architecture prep, interviews, or reference.

---

## 📖 Table of Contents

1. [IAM Overview](#1-iam-overview-identity--access-management)  
2. [Root User](#2-root-user)  
3. [IAM Users](#3-iam-users)  
4. [IAM Groups](#4-iam-groups)  
5. [IAM Policies](#5-iam-policies)  
6. [Types of IAM Policies](#6-types-of-iam-policies)  
7. [Policy Types by Target](#7-policy-types-by-target)  
8. [Policy Evaluation Order](#8-policy-evaluation-order)  
9. [IAM Roles](#9-iam-roles)  
10. [AWS STS](#10-aws-sts-security-token-service)  
11. [IAM Access Analyzer](#11-iam-access-analyzer)  
12. [IAM Identity Center](#12-iam-identity-center-aws-sso)  
13. [AWS Directory Services](#13-aws-directory-services)  
14. [AWS Organizations](#14-aws-organizations)  
15. [Consolidated Billing](#15-consolidated-billing)  
16. [Organizational Units (OUs)](#16-organizational-units-ous)  
17. [Service Control Policies (SCPs)](#17-service-control-policies-scps)  
18. [Tag Policies](#18-tag-policies)  
19. [IAM Conditions](#19-iam-conditions)  
20. [Permission Boundaries](#20-permission-boundaries)  
21. [IAM Roles vs Resource-Based Policies](#21-iam-roles-vs-resource-based-policies)  
22. [IAM + Organizations Integration Summary](#22-iam--organizations-integration-summary)  
23. [Motion Recap Flow](#23-motion-recap-flow)  
24. [Quick Memory Aids](#24-quick-memory-aids)  

---

<details>
<summary>🧠 1. IAM Overview (Identity & Access Management)</summary>

**Concept:**  
IAM = Manage *who* can access *what* in AWS.

**Key Points:**  
- Global service (not region-based)  
- Root → Users → Groups → Roles → Policies  
- Root user = full control (use only for setup)

**Example:**  
Root creates “AdminUser” and gives permissions to manage EC2 & S3.

</details>

<details>
<summary>👤 2. Root User</summary>

- Created automatically with AWS account.  
- Use only for billing & account setup.  
- Always protect with **MFA**.

**Example:**  
Root creates new users, sets password policy, and never used again for daily operations.

</details>

<details>
<summary>👥 3. IAM Users</summary>

- Represent individuals or applications.  
- Auth via:  
  - Console: username + password  
  - CLI/SDK: access keys

**Example:**  

`dev_user` → EC2:StartInstances only

```json
{
  "Effect": "Allow",
  "Action": "ec2:StartInstances",
  "Resource": "*"
}
```

</details>

<details>
<summary>👪 4. IAM Groups</summary>

- **Container for users**, no login of its own.  
- Simplifies permission management.  
- No nesting (group-in-group ❌).

**Example:**  
Group: “Developers” → Policy: Full access to EC2  
All users in Developers group inherit EC2 permissions.

</details>

<details>
<summary>📜 5. IAM Policies</summary>

- **JSON documents** defining what’s allowed/denied.  
- Attached to users, groups, or roles.  
- Follows **least privilege** principle.

**Structure:**

| Field    | Meaning          | Example                     |
| -------- | ---------------- | --------------------------- |
| Version  | Policy version   | “2012-10-17”               |
| Sid      | Statement ID     | “S3ReadOnly”               |
| Effect   | Allow / Deny     | Allow                       |
| Action   | What can be done | “s3:GetObject”             |
| Resource | Where            | “arn:aws:s3:::my-bucket/*” |

**Example:**  

Allow S3 read-only access:

```json
{
  "Effect": "Allow",
  "Action": "s3:GetObject",
  "Resource": "arn:aws:s3:::my-bucket/*"
}
```

</details>

<details>
<summary>🧩 6. Types of IAM Policies</summary>

### a) Managed Policy
- Reusable, centrally maintained.  
- Can attach to many users/groups/roles.

### b) Inline Policy
- Attached directly to one entity.  
- Deleted if the entity is deleted.

**Example:**  
- `AdminPolicy` → Managed  
- `CustomLambdaPolicy` → Inline (for one Lambda role)

</details>

<details>
<summary>🧱 7. Policy Types by Target</summary>

| Policy Type       | Attached To         | Example                  |
| ----------------- | ----------------- | ------------------------ |
| **Identity-based** | User, Group, Role  | Allow EC2 access          |
| **Resource-based** | Resource (e.g., S3 bucket) | Bucket policy granting access |
| **Trust policy**   | IAM Role           | Defines who can assume the role |

</details>

<details>
<summary>⚖️ 8. Policy Evaluation Order</summary>

1. **Explicit Deny** → Always wins 🚫  
2. **Explicit Allow** → Works if no Deny ✅  
3. **Implicit Deny** → Default for all users ❌

</details>

<details>
<summary>🔑 9. IAM Roles</summary>

**Purpose:**  
Grant AWS services or users temporary access.

**Two policy types:**  
- **Trust Policy:** Who can assume the role  
- **Permissions Policy:** What the role can do

**Example:**  

Role: `EC2S3FullAccessRole`  
- Trust: EC2 can assume it  
- Permissions: Full access to S3

```json
"Action": "s3:*",
"Resource": "*"
```

</details>

<details>
<summary>🕒 10. AWS STS (Security Token Service)</summary>

**Purpose:**  
Issue **temporary credentials** (short-term, secure).

**Common APIs:**

| API                     | Use Case                |
| ----------------------- | ---------------------- |
| `AssumeRole`            | Cross-account access    |
| `AssumeRoleWithSAML`    | Enterprise SSO          |
| `AssumeRoleWithWebIdentity` | Social login        |
| `GetFederationToken`    | Internal app access     |
| `GetSessionToken`       | MFA temporary access    |

**Example:**  
App uses `AssumeRoleWithWebIdentity` via Cognito to access DynamoDB.

</details>

<details>
<summary>🧠 11. IAM Access Analyzer</summary>

- Checks & validates policies for risks.  
- Detects public or overly broad access.  
- Monitors: S3, KMS, SQS, IAM roles, Lambda, Secrets Manager.

**Example:**  
Warns if S3 bucket policy allows `"Principal": "*"`.

</details>

<details>
<summary>🔐 12. IAM Identity Center (AWS SSO)</summary>

**Purpose:**  
Manage **workforce sign-in** to AWS accounts & apps.

**Features:**  
- One **identity source** (AD, external IdP, or internal).  
- **Permission Sets** per account.  
- Supports **ABAC** (attribute-based access).  
- Works with AWS CLI v2.

**Example:**  
User logs in once → Access multiple AWS accounts & Salesforce via SSO.

</details>

<details>
<summary>🧱 13. AWS Directory Services</summary>

| Service                     | Description           | Example                         |
| --------------------------- | ------------------- | ------------------------------- |
| **AWS Managed Microsoft AD** | Full AD in AWS       | Integrate with on-prem AD       |
| **AD Connector**             | Proxy to on-prem AD  | Corporate users use on-prem credentials |
| **Simple AD**                | Standalone AD        | Small AWS-only setup            |

</details>

<details>
<summary>🏢 14. AWS Organizations</summary>

**Purpose:**  
Manage multiple AWS accounts centrally.

**Concepts:**  
- **Management Account:** Root admin  
- **Member Accounts:** Joined accounts  
- **OU (Organizational Unit):** Logical grouping  
- **SCP (Service Control Policy):** Restrictions

**Example:**  
OU “Dev” denies S3 delete actions; “Prod” allows all.

</details>

<details>
<summary>💰 15. Consolidated Billing</summary>

- One payment for all accounts.  
- **Shared discounts** (EC2, S3, RIs).  
- Unused RIs shared across org.

**Example:**  
Account A’s unused EC2 RI applied to Account B’s usage.

</details>

<details>
<summary>🏗️ 16. Organizational Units (OUs)</summary>

- Root OU → Sub-OUs for dev, prod, test.  
- Each OU can hold multiple accounts.  
- SCPs cascade down the OU tree.

</details>

<details>
<summary>🚫 17. Service Control Policies (SCPs)</summary>

**Purpose:**  
Restrict actions accounts or OUs can perform.  
Works *in addition* to IAM policies.

**Example:**  
- Root OU: Full access  
- Dev OU: Deny `s3:*`  
Result: Dev accounts can’t use S3.

<
