
# AWS Certified Solutions Architect – Associate (SAA-C03) Exam Overview and its Arichitectures

## Exam Overview
**Certification:** AWS Certified Solutions Architect – Associate (SAA-C03)  
**Purpose:** Validates ability to design secure, resilient, high-performing, and cost-optimized architectures on AWS.  
**Format:** 65 questions (multiple-choice, multiple-response), 130 minutes, passing score ~720/1000.  

### Domains (per the official exam guide):
- **Design Secure Architectures** (30%)  
- **Design Resilient Architectures** (26%)  
- **Design High-Performing Architectures** (24%)  
- **Design Cost-Optimized Architectures** (20%)  

---

### **1. Design Secure Architectures**

#### ✅ **Key Concepts:**

* **IAM (Identity and Access Management)**

  * Roles, Policies, Users, Groups, and Federated Access
  * Least Privilege Principle
  * IAM Policies: Inline vs Managed

* **VPC (Virtual Private Cloud)**

  * Subnets (Public vs Private)
  * NAT Gateway, Internet Gateway
  * Security Groups vs NACLs
  * VPC Peering, Transit Gateway

* **Encryption & Security**

  * KMS (Key Management Service)
  * S3 Server-Side Encryption (SSE-S3, SSE-KMS, SSE-C)
  * TLS/SSL
  * AWS Shield, WAF (Web Application Firewall)

* **Access Control**

  * SCPs (Service Control Policies)
  * AWS Organizations
  * Resource Policies vs Identity Policies

---

### **2. Design Resilient Architectures**

#### ✅ **Key Concepts:**

* **High Availability (HA)**

  * Multi-AZ, Multi-Region
  * Elastic Load Balancer (ALB/NLB)
  * Route 53 routing policies (Latency, Failover, Geolocation)

* **Disaster Recovery**

  * Backup and Restore
  * Pilot Light
  * Warm Standby
  * Multi-site Active/Active

* **Scalability**

  * Auto Scaling Groups (ASG)
  * Elastic Load Balancing (ELB)
  * Amazon CloudFront (CDN)

* **Storage**

  * S3 (Durability, Storage Classes: Standard, IA, Glacier)
  * EBS (GP2, GP3, IO1, ST1, SC1)
  * EFS (Elastic File System)

---

### **3. Design High-Performing Architectures**

#### ✅ **Key Concepts:**

* **Compute**

  * EC2 Instance Types (General, Compute-Optimized, Memory-Optimized)
  * Lambda (Event-driven compute)
  * ECS vs EKS vs Fargate

* **Caching**

  * Amazon CloudFront
  * Amazon ElastiCache (Redis, Memcached)

* **Database**

  * Amazon RDS (MySQL, PostgreSQL, Aurora)
  * DynamoDB (NoSQL, Partition Key, GSIs, LSIs)
  * Amazon Redshift (Data Warehousing)
  * Read Replicas, Multi-AZ deployments

* **Networking**

  * Enhanced Networking (ENA)
  * Placement Groups (Cluster, Spread, Partition)

---

### **4. Design Cost-Optimized Architectures**

#### ✅ **Key Concepts:**

* **Pricing Models**

  * EC2: On-Demand, Reserved, Spot Instances, Savings Plans
  * Lambda: Pay per request and execution time
  * S3 Storage Classes: Cost differences

* **Right-Sizing**

  * Trusted Advisor Recommendations
  * Compute Optimizer

* **Storage Optimization**

  * Lifecycle Policies
  * S3 Intelligent-Tiering
  * Glacier for archiving

* **Billing and Monitoring Tools**

  * AWS Budgets
  * Cost Explorer
  * Cost and Usage Reports (CUR)

---

### **General AWS Concepts**

* **Well-Architected Framework**

  * 5 Pillars: Operational Excellence, Security, Reliability, Performance Efficiency, Cost Optimization

* **Monitoring & Logging**

  * CloudWatch (Metrics, Logs, Alarms)
  * CloudTrail (API activity logging)
  * AWS Config (Compliance and change tracking)

* **Deployment Services**

  * Elastic Beanstalk
  * CodePipeline, CodeDeploy, CodeBuild
  * CloudFormation (IaC)

---
