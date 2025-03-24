The AWS Certified Solutions Architect – Associate exam doesn’t require you to memorize *every* AWS service (there are over 200 services, and that number keeps growing!). Instead, it focuses on understanding key services, their use cases, and how they integrate to design scalable, cost-effective, and reliable architectures on AWS. Below, I’ll list the **core AWS services** commonly emphasized for the exam, based on the exam guide and typical architectural scenarios. I’ll group them by category for clarity.

---

### **Compute Services**
1. **Amazon EC2 (Elastic Compute Cloud)** - Virtual servers in the cloud.
2. **AWS Lambda** - Serverless compute for running code without managing servers.
3. **Elastic Beanstalk** - Platform-as-a-Service (PaaS) for deploying and scaling applications.
4. **Amazon ECS (Elastic Container Service)** - Container management service.
5. **Amazon EKS (Elastic Kubernetes Service)** - Managed Kubernetes service.
6. **AWS Fargate** - Serverless compute engine for containers.

---

### **Storage Services**
7. **Amazon S3 (Simple Storage Service)** - Object storage for scalable data storage.
8. **Amazon EBS (Elastic Block Store)** - Block storage for EC2 instances.
9. **Amazon EFS (Elastic File System)** - Scalable file storage for multiple EC2 instances.
10. **AWS Glacier** - Low-cost storage for archiving and long-term backup (now part of S3 Glacier).
11. **AWS Storage Gateway** - Hybrid cloud storage integration.

---

### **Networking Services**
12. **Amazon VPC (Virtual Private Cloud)** - Isolated cloud network for resource control.
13. **Amazon Route 53** - Scalable DNS and domain name registration.
14. **Elastic Load Balancer (ELB)** - Distributes incoming traffic (includes ALB, NLB, CLB).
15. **AWS Direct Connect** - Dedicated network connection to AWS.
16. **Amazon CloudFront** - Content Delivery Network (CDN) for low-latency content delivery.

---

### **Database Services**
17. **Amazon RDS (Relational Database Service)** - Managed relational databases (e.g., MySQL, PostgreSQL).
18. **Amazon DynamoDB** - NoSQL database for low-latency, scalable applications.
19. **Amazon Aurora** - High-performance relational database compatible with MySQL/PostgreSQL.
20. **Amazon Redshift** - Data warehousing for analytics.
21. **Amazon ElastiCache** - In-memory caching (Redis, Memcached).

---

### **Security, Identity, and Compliance**
22. **AWS IAM (Identity and Access Management)** - Manage user access and permissions.
23. **AWS KMS (Key Management Service)** - Create and manage encryption keys.
24. **AWS Shield** - DDoS protection.
25. **AWS WAF (Web Application Firewall)** - Protect applications from common web exploits.
26. **Amazon Cognito** - User authentication and authorization.

---

### **Management and Governance**
27. **AWS CloudWatch** - Monitoring and logging for AWS resources.
28. **AWS CloudTrail** - Audit and track API calls and user activity.
29. **AWS Config** - Track resource configurations and changes.
30. **AWS Trusted Advisor** - Cost optimization and best practice recommendations.
31. **AWS Systems Manager** - Manage and automate operational tasks.

---

### **Application Integration**
32. **Amazon SNS (Simple Notification Service)** - Pub/sub messaging and notifications.
33. **Amazon SQS (Simple Queue Service)** - Message queuing for decoupling applications.
34. **AWS Step Functions** - Coordinate workflows and microservices.
35. **Amazon EventBridge** - Event-driven service for connecting applications.

---

### **Migration and Transfer**
36. **AWS Snowball** - Physical device for large-scale data transfer.
37. **AWS Database Migration Service (DMS)** - Migrate databases to AWS.
38. **AWS Server Migration Service (SMS)** - Migrate on-premises servers to AWS.

---

### **Analytics**
39. **Amazon Kinesis** - Real-time data streaming and analytics.
40. **AWS Glue** - ETL (Extract, Transform, Load) service for data preparation.
41. **Amazon QuickSight** - Business intelligence and data visualization.

---

### **Developer Tools**
42. **AWS CodeStar** - Develop, build, and deploy applications.
43. **AWS CodeCommit** - Source control (Git-based).
44. **AWS CodeBuild** - Build and test code.
45. **AWS CodeDeploy** - Automate application deployments.

---

### **Other Key Services**
46. **AWS Auto Scaling** - Automatically scale resources based on demand.
47. **AWS CloudFormation** - Infrastructure as Code (IaC) for provisioning resources.
48. **Amazon API Gateway** - Create, manage, and monitor APIs.
49. **AWS Billing and Cost Management** - Monitor and optimize AWS costs.

---

### **Key Focus for AWS Solutions Architect – Associate**
The exam emphasizes designing architectures based on the **AWS Well-Architected Framework**, which includes five pillars: 
- Operational Excellence
- Security
- Reliability
- Performance Efficiency
- Cost Optimization

### Well-Architected Framework
- **Security**: IAM, KMS, Security Groups, WAF, Shield, GuardDuty.
- **Reliability**: Auto Scaling, Multi-AZ, Backup & Restore.
- **Performance Efficiency**: Caching (CloudFront, ElastiCache), Database optimization.
- **Cost Optimization**: Reserved Instances, Spot Instances, Compute Savings Plans.
- **Operational Excellence**: AWS CloudFormation, AWS Config, CloudWatch.


You’ll need to know how to combine services like EC2, S3, RDS, VPC, and IAM to build solutions, as well as understand high availability (e.g., multi-AZ deployments), scalability (e.g., Auto Scaling), and cost management.

---

### **Notable Omissions**
While AWS has many more services (e.g., Amazon SageMaker for ML, AWS IoT, Amazon WorkSpaces), they’re typically not core to the Associate-level exam unless part of a specific scenario. The focus is on foundational services and architectural best practices.
