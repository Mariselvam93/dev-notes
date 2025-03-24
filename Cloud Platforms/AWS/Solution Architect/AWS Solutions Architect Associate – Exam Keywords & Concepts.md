# **AWS Solutions Architect Associate – Exam Keywords & Concepts**  

Understanding **key terms** in exam questions will help you choose the best AWS service for a given scenario. Below is a categorized list of **keywords** and their corresponding AWS solutions:  

---

## **1. High Availability & Fault Tolerance**
- **Keywords:**  
  *"High availability"*, *"Fault tolerant"*, *"No downtime"*, *"Minimal disruption"*, *"Ensure resilience"*, *"Handle failures automatically"*.  
- **AWS Services to Consider:**  
  - **Multi-AZ** (RDS, DynamoDB Global Tables, EFS)  
  - **Route 53** (Failover Routing, Latency-based Routing)  
  - **Auto Scaling + ELB** (Load balancing & scaling EC2 instances)  
  - **CloudFront** (Distributes traffic globally)  
  - **SQS & SNS** (Decoupling & event-driven architectures)  

---

## **2. Cost Optimization**
- **Keywords:**  
  *"Low cost"*, *"Minimize cost"*, *"Cost-effective"*, *"Reduce operational overhead"*, *"Optimize storage costs"*.  
- **AWS Services to Consider:**  
  - **EC2 Spot Instances** (Cheapest for non-critical workloads)  
  - **Reserved Instances & Savings Plans** (Lower cost for long-term workloads)  
  - **S3 Lifecycle Policies** (Move data to cheaper tiers like Glacier)  
  - **DynamoDB On-Demand** (Avoid over-provisioning)  
  - **AWS Lambda & Fargate** (Serverless computing reduces costs)  

---

## **3. Security & Compliance**
- **Keywords:**  
  *"Least privilege"*, *"Restrict access"*, *"Encrypt data at rest/in transit"*, *"Secure API access"*, *"Meet compliance requirements"*.  
- **AWS Services to Consider:**  
  - **IAM Roles & Policies** (Control access using the least privilege principle)  
  - **KMS & CloudHSM** (Encryption & key management)  
  - **AWS WAF & Shield** (DDoS protection)  
  - **Cognito** (Secure user authentication)  
  - **AWS Artifact, Config, & CloudTrail** (Compliance & audit logging)  

---

## **4. Performance Optimization**
- **Keywords:**  
  *"Improve latency"*, *"Reduce response time"*, *"High read/write performance"*, *"Optimize for global users"*.  
- **AWS Services to Consider:**  
  - **CloudFront** (Caching for low latency)  
  - **ElastiCache (Redis/Memcached)** (Speeds up database queries)  
  - **RDS Read Replicas & Aurora Global Databases** (For read-heavy workloads)  
  - **DynamoDB Global Tables** (Multi-region availability)  
  - **AWS Global Accelerator** (Improves routing for low latency)  

---

## **5. Scalability & Elasticity**
- **Keywords:**  
  *"Scale automatically"*, *"Handle variable workloads"*, *"Accommodate unpredictable traffic"*.  
- **AWS Services to Consider:**  
  - **Auto Scaling Groups** (Adjust EC2 instances based on demand)  
  - **Lambda & API Gateway** (Serverless scaling)  
  - **DynamoDB On-Demand** (Handles spikes in traffic without provisioning)  
  - **ECS with Fargate** (Containerized auto-scaling without managing servers)  
  - **Kinesis Data Streams** (For real-time scalable data processing)  

---

## **6. Disaster Recovery**
- **Keywords:**  
  *"Recover quickly"*, *"Ensure business continuity"*, *"Minimize data loss"*, *"Backup & restore"*.  
- **AWS Services to Consider:**  
  - **S3 Cross-Region Replication (CRR)** (For multi-region backup)  
  - **RDS Multi-AZ & Read Replicas** (Database failover & recovery)  
  - **AWS Backup & AWS Disaster Recovery** (Automated backups & failover)  
  - **S3 Versioning & Glacier** (Long-term backups)  

---

## **7. Networking & Connectivity**
- **Keywords:**  
  *"Secure communication"*, *"Expose a service publicly"*, *"Connect on-premises to AWS"*.  
- **AWS Services to Consider:**  
  - **VPC Peering & Transit Gateway** (Private network connectivity)  
  - **AWS PrivateLink** (Secure cross-service communication)  
  - **API Gateway & ELB** (Public and private service exposure)  
  - **AWS Direct Connect & VPN** (On-prem to AWS connectivity)  
  - **Route 53** (Domain resolution & traffic routing)  

---

## **8. Monitoring & Logging**
- **Keywords:**  
  *"Track user activity"*, *"Audit logs"*, *"Monitor system health"*, *"Send alerts on failures"*.  
- **AWS Services to Consider:**  
  - **AWS CloudTrail** (Tracks API calls for auditing)  
  - **AWS Config** (Tracks configuration changes)  
  - **CloudWatch Metrics & Logs** (Monitoring & alerts)  
  - **AWS X-Ray** (Tracing API requests & debugging)  
  - **Trusted Advisor** (Best practices & cost/security recommendations)  

---

### **📌 Exam Tip: Identify the Key Requirement First!**
- If the **question asks for cost savings** → Look for **Spot Instances, Reserved Instances, S3 Glacier, etc.**  
- If the **question is about high availability** → Think **Multi-AZ, Load Balancing, Route 53 Failover**  
- If the **question involves security** → Focus on **IAM, KMS, WAF, CloudTrail**  
- If the **question is about performance** → Consider **CloudFront, ElastiCache, DynamoDB Indexing**  




