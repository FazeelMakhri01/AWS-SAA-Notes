# AWS Docker & Container Services Notes

---

## **1. Docker Basics**

- 📌 **What is Docker?**
    - A **software deployment platform** that packages apps into **containers**.
    - Containers can run on **any OS** → portable & consistent.
    - ✅ **Use Cases**:
        - Microservices architecture
        - Lift & shift apps between OS/servers
- 📦 **Storage**
    - Docker images stored in **Docker Hub** (public repo)
    - AWS equivalent: **Amazon ECR** (Elastic Container Registry)

---

## **2. AWS Container Services**

- **ECS (Elastic Container Service)** → AWS-managed container platform
- **EKS (Elastic Kubernetes Service)** → Managed **Kubernetes** by AWS
- **Fargate** → Serverless compute engine for **ECS/EKS**

---

## **3. Amazon ECS (Elastic Container Service)**

### 🔹 **Launch Types**

1. **EC2 Launch Type**
    - User provisions & manages EC2 instances
    - ECS Agent runs on EC2 → registers with ECS Cluster
    - Manual **scale-in/out** of EC2 infra

2. **Fargate Launch Type**
    - Fully **serverless** → AWS manages infra
    - Only define **Task Definitions** (CPU, RAM, container details)
    - Scale by increasing **tasks** (not EC2)

---

### 🔹 **Core ECS Concepts**

- **Task** → Info about container + runtime needs (image, port, storage)
- **Task Definition** → Blueprint describing what the task does
- **ECS Service** → Ensures tasks run continuously (restarts on failure)

---

### 🔹 **IAM in ECS**

- **ECS Task Role**
    - Assigned per task (defined in task definition)
    - Provides **AWS service permissions** to containers
    - Example:
        - Task 1 → Read from S3
        - Task 2 → Write to RDS

- **EC2 Instance Profile**
    - Used by **ECS Agent** running on EC2
    - Permissions for:
        - Pull/push images from **ECR**
        - Send logs to **CloudWatch**
        - ECS API calls

⚖️ **Difference**

- **Task Role** → For tasks/containers (business logic access)
- **Instance Profile** → For ECS agent/infra management

---

### 🔹 **ECR (Elastic Container Registry)**

- AWS version of Docker Registry
- **Stores & manages images** (private/public)
- ✅ Supports **image scanning** & **vulnerability checks**

---

### 🔹 **ECS Data Volumes**

- For persistent storage → Use **Amazon EFS**
- Works with **ECS EC2** & **Fargate**
- **Fargate + EFS = Fully serverless persistent storage**
- Multi-AZ → Shared data across clusters

---

### 🔹 **ECS Cluster**

- A **logical grouping of containers (tasks/services)**

---

## **4. Amazon EKS (Elastic Kubernetes Service)**

- 📌 AWS-managed **Kubernetes** (open source, cloud-agnostic)
- **Scales Docker containers automatically**
- ✅ **Best for** migrating on-prem Kubernetes workloads

### 🔹 **EKS Core Concepts**

- **Pod** → Smallest deployable unit (runs containers)
- **Nodes** → Machines running pods

---

### 🔹 **EKS Node Types**

1. **Managed Node Groups**
    - AWS provisions & manages nodes (part of ASG)
    - Supports On-Demand & Spot

2. **Self-Managed Nodes**
    - User creates & manages nodes
    - Register nodes with EKS manually
    - Use pre-built **EKS AMIs**

3. **Fargate**
    - Fully serverless option for EKS

---

### 🔹 **EKS Data Volumes**

- Uses **Container Storage Interface (CSI)**
- Supported storage:
    - **EBS** → Block storage (per pod)
    - **EFS** → Shared filesystem (works with Fargate)
    - **FSx for Lustre / NetApp ONTAP** → High-performance file systems

---

## **5. AWS Fargate**

- 🚀 **Serverless compute engine** for ECS & EKS
- No infra to manage
- Tasks/Pods scale automatically
- Use with **EFS** for persistence

---

## **6. Summary Table**

| Service | Orchestration | Compute Model | Storage | Best Use Case |
| --- | --- | --- | --- | --- |
| **ECS** | AWS-native | EC2 or Fargate | EFS/EBS | Simple containerized apps |
| **EKS** | Kubernetes | EC2 or Fargate | CSI (EBS/EFS/FSx) | Migrating K8s workloads |
| **Fargate** | ECS/EKS backend | Serverless | EFS | Serverless apps |
