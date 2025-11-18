# **IAM Advanced Notes**

## **1) AWS Organizations**

- 📌 **What it is**
    - Global service to manage multiple AWS accounts
    - **Management Account** = main account
    - **Member Accounts** = part of org, only in one org
    - Accounts can **share resources** and **get discounts**
    - API available to automate account creation
    - Define **Service Control Policies (SCPs)**

---

## **2) Organizations - Tag Policies**

- 📌 **What it is**
    - Standardizes **tags** across all accounts
    - Ensures consistent **tagging, auditing, resource categorization**
    - Define **tag keys & allowed values**
    - Prevents non-compliant tagging
    - Reports on **tagged, non-tagged, non-compliant** resources
- ✅ **Use cases**
    - Enforce **cost allocation tags**
    - Support **attribute-based access control**
- ⚡ **Exam Tip**
    - Maintaining consistent tags across accounts ⇒ **Tag Policies**

---

## **3) IAM Conditions**

- 📌 **What it is**
    - Restrict resources based on conditions
    - Examples:
        - Allow only specific IP ranges
        - S3 object-level restrictions
        - `aws:PrincipalOrgID` to restrict actions to org accounts
- ✅ **Use cases**
    - Fine-grained access control
    - Enforce organizational boundaries

---

## **4) IAM Roles vs Resource-Based Policies**

- 📌 **Key Differences**
    - **IAM Role:** Principal assumes the role, **adopts role permissions**, original permissions lost
    - **Resource-Based Policy:** Principal **retains original permissions**, access granted directly to resource
- ✅ **Use case example**
    - User in Account A scans DynamoDB and writes to S3 in Account B
    - Resource-based policy preferred (no need to assume role)
- 🌟 **EventBridge Behavior**
    - Lambda, SNS, SQS, S3 → **resource-based policies**
    - Kinesis, EC2 Auto Scaling, ECS tasks → **IAM roles**

---

## **5) IAM Permission Boundaries**

- 📌 **What it is**
    - Restrict maximum permissions for IAM users/roles
    - Users can manage permissions **within boundaries** but not exceed them
- ✅ **Use case**
    - Developers manage resources without gaining full admin rights

---

## **6) IAM Identity Center**

- 📌 **What it is**
    - Successor to AWS SSO
    - Single sign-on to **multiple AWS accounts & apps**
    - Users stored in **built-in store** or **external IdPs** (AD, OneLogin, Okta)
    - **Permission sets** define access across accounts
    - Supports **Attribute-Based Access Control** (ABAC)
- ✅ **Use case**
    - Centralized login management for AWS and business apps

---

## **7) AWS Directory Services**

- 📌 **What it is**
    - Manage **Microsoft Active Directory** (AD) in AWS
    - Centralized authentication & resource management
- 🌟 **Options**
    - **AWS Managed Microsoft AD**
        - Create AD in AWS, supports **MFA** and **trust with on-prem AD**
    - **AD Connector**
        - Proxy to on-prem AD, MFA supported, users not stored in AWS
    - **Simple AD**
        - Standalone, AD-compatible, no on-prem integration
- ✅ **Use cases**
    - Proxy users to on-prem AD → AD Connector
    - Cloud-based AD with MFA → Managed AD
    - Standalone AD in AWS → Simple AD
- 🔗 **Integration with IAM Identity Center**
    - Managed AD → direct integration
    - On-prem AD → two-way trust or AD Connector

---

## **Key Takeaways**

- **AWS Organizations:** central management, SCPs, cost/resource sharing
- **Tag Policies:** enforce consistent tagging and support ABAC
- **IAM Roles:** assume permissions, original lost; Resource Policies: direct access, original retained
- **Permission Boundaries:** restrict maximum permissions
- **IAM Identity Center:** single sign-on across accounts/apps
- **Directory Services:** AWS Managed AD, AD Connector, Simple AD for different scenarios
