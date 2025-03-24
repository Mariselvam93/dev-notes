## **AWS Solution Architect - Associate - Cheat Sheet** 🚀  

### **1. Compute Services**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon EC2** | Virtual machines (instances) for running applications in the cloud. | Web hosting, enterprise apps, databases, machine learning, gaming servers. |
| **AWS Lambda** | Serverless compute that runs code without provisioning servers. | Event-driven apps, API backends, automation scripts. |
| **Elastic Beanstalk** | PaaS for deploying and scaling web apps. | Rapidly deploying web apps without managing infrastructure. |
| **Amazon ECS** | Managed container service using Docker. | Running microservices, CI/CD pipelines, batch jobs. |
| **Amazon EKS** | Managed Kubernetes service. | Orchestrating containerized applications at scale. |
| **AWS Fargate** | Serverless compute engine for containers (ECS/EKS). | Running containers without managing EC2 instances. |

---

### **2. Storage Services**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon S3** | Object storage with high durability. | Static website hosting, backup & restore, big data analytics. |
| **Amazon EBS** | Block storage for EC2 instances. | Databases, transactional apps, high-performance workloads. |
| **Amazon EFS** | Scalable file storage accessible by multiple EC2 instances. | Shared storage for distributed applications. |
| **AWS Glacier (S3 Glacier)** | Low-cost archive storage. | Long-term data retention, compliance storage. |
| **AWS Storage Gateway** | Hybrid storage service for on-prem to AWS connectivity. | Extending on-premises storage to AWS, backup & disaster recovery. |

---

### **3. Networking Services**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon VPC** | Isolated network environment in AWS. | Securely hosting applications, hybrid cloud networking. |
| **Amazon Route 53** | Scalable DNS service. | Domain registration, load balancing across regions. |
| **Elastic Load Balancer (ELB)** | Distributes incoming traffic across instances. | Ensuring high availability for web applications. |
| **AWS Direct Connect** | Dedicated network connection to AWS. | High-speed, secure hybrid cloud connections. |
| **Amazon CloudFront** | Content Delivery Network (CDN) for low-latency content delivery. | Accelerating website/app performance, video streaming. |

---

### **4. Database Services**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon RDS** | Managed relational database (MySQL, PostgreSQL, SQL Server, etc.). | Enterprise databases, e-commerce, SaaS apps. |
| **Amazon DynamoDB** | NoSQL database with millisecond latency. | Real-time apps, IoT, gaming leaderboards. |
| **Amazon Aurora** | High-performance MySQL/PostgreSQL database. | Mission-critical databases, analytics workloads. |
| **Amazon Redshift** | Data warehouse for analytics. | Business intelligence, large-scale analytics. |
| **Amazon ElastiCache** | In-memory caching service (Redis, Memcached). | Accelerating database performance, session storage. |

---

### **5. Security, Identity, and Compliance**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **AWS IAM** | Manages user permissions and access. | Secure access control, role-based authentication. |
| **AWS KMS** | Encryption key management. | Encrypting S3, EBS, RDS, Lambda data. |
| **AWS Shield** | DDoS protection service. | Protecting websites & APIs from attacks. |
| **AWS WAF** | Web Application Firewall. | Preventing common web exploits (SQL injection, XSS). |
| **Amazon Cognito** | User authentication service. | Managing user sign-up & login for apps. |

---

### **6. Management and Governance**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **AWS CloudWatch** | Monitoring and logging for AWS resources. | Tracking performance metrics, error logging. |
| **AWS CloudTrail** | Tracks AWS API activity and logs it. | Compliance auditing, security monitoring. |
| **AWS Config** | Tracks AWS resource changes. | Ensuring compliance, troubleshooting configurations. |
| **AWS Trusted Advisor** | Best practice recommendations for AWS usage. | Cost savings, security improvements. |
| **AWS Systems Manager** | Automates operational tasks across AWS. | Patch management, remote administration. |

---

### **7. Application Integration**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon SNS** | Pub/Sub messaging service. | Sending notifications, event-driven architecture. |
| **Amazon SQS** | Message queueing for decoupling applications. | Asynchronous processing, order fulfillment. |
| **AWS Step Functions** | Orchestration of workflows. | Automating workflows, microservices coordination. |
| **Amazon EventBridge** | Event-driven service. | Connecting SaaS applications, real-time event processing. |

---

### **8. Migration and Transfer**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **AWS Snowball** | Physical data transfer device. | Moving petabytes of data to AWS. |
| **AWS DMS** | Migrate databases to AWS. | Migrating on-premises databases to RDS. |
| **AWS SMS** | Migrate on-prem servers to AWS. | Lift-and-shift migrations. |

---

### **9. Analytics**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **Amazon Kinesis** | Real-time data streaming. | Video analytics, log processing, IoT. |
| **AWS Glue** | ETL (Extract, Transform, Load) service. | Data preparation for analytics. |
| **Amazon QuickSight** | Business intelligence tool. | Interactive dashboards, data visualization. |

---

### **10. Developer Tools**  
| **Service** | **Description** | **Use Cases** |
|------------|--------------|------------|
| **AWS CodeStar** | Integrated development tool for CI/CD. | Managing software development workflows. |
| **AWS CodeCommit** | Git-based source control. | Secure version control for code. |
| **AWS CodeBuild** | Build & test service. | Automating application builds. |
| **AWS CodeDeploy** | Automating deployments. | Rolling out new features with minimal downtime. |

---

## **Real-time AWS Architecture Example**
### **Use Case: Scalable Web Application**
A company wants to deploy a **highly available** and **scalable** e-commerce website using AWS.

### **AWS Services Used:**
- **Compute:** Amazon EC2 (for hosting the web application) with Auto Scaling.
- **Storage:** Amazon S3 (for storing product images and user uploads).
- **Database:** Amazon RDS (for structured data like user accounts, orders).
- **Networking:** Amazon VPC (to isolate resources securely).
- **Security:** AWS IAM (for managing access control).
- **Load Balancing:** Elastic Load Balancer (to distribute traffic).
- **Monitoring:** AWS CloudWatch (for logging and alerts).

### **Architecture Flow:**
1. **User Requests:** A user visits the e-commerce website (hosted on EC2).
2. **Load Balancing:** The request is routed through **Elastic Load Balancer (ELB)**.
3. **Data Retrieval:** Product details and user information are fetched from **Amazon RDS**.
4. **Image Storage:** Product images are stored in **Amazon S3**.
5. **Auto Scaling:** If traffic spikes, **Auto Scaling Group** launches new EC2 instances.
6. **Security Controls:** IAM policies enforce **least privilege access** for developers.
7. **Monitoring:** **CloudWatch** tracks application health and performance.

---

This setup ensures:
✅ **High availability** (Multi-AZ RDS, ELB, Auto Scaling).  
✅ **Scalability** (Auto Scaling adjusts resources).  
✅ **Cost Optimization** (Use of S3 for static content, auto-scaling reduces unused capacity).  

