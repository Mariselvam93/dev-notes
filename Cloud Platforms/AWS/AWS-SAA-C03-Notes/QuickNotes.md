---

### ✅ **High Availability**

High availability requires that the application or database remains accessible even if one instance or one Availability Zone fails.
Deploying across multiple Availability Zones ensures resiliency and fault tolerance — minimizing downtime during failures.

---

### ✅ **Auto Scaling**

Auto Scaling automatically adjusts the number of EC2 instances based on demand.
It ensures that your application has enough capacity during spikes and scales down during idle times — improving performance and cost-efficiency.

---

### ✅ **Elastic Load Balancing (ELB)**

ELB automatically distributes incoming traffic across multiple targets (e.g., EC2 instances).
It improves availability and fault tolerance by routing traffic only to healthy instances across multiple Availability Zones.

---

### ✅ **Amazon RDS Multi-AZ**

RDS Multi-AZ provides high availability by maintaining a standby replica in another Availability Zone.
In case of failure, it automatically fails over to the standby — ensuring database continuity with minimal disruption.

---

### ✅ **Amazon S3 (Simple Storage Service)**

Amazon S3 is durable object storage designed for 11 9s of durability.
It stores data redundantly across facilities, supports versioning, and is ideal for backups, big data, and static website hosting.

---

### ✅ **Amazon CloudFront**

CloudFront is a CDN that caches content at global edge locations.
It improves latency and performance by delivering content from the edge closest to the user — enhancing user experience.

---

### ✅ **Amazon Route 53**

Route 53 is a scalable DNS and traffic routing service.
It supports domain registration, health checks, and routing policies (e.g., latency-based, failover) — improving availability and performance.

---

### ✅ **Amazon VPC (Virtual Private Cloud)**

VPC lets you provision a logically isolated section of AWS.
You can define subnets, route tables, and gateways — offering full control over your virtual networking environment.

---

### ✅ **Security Groups**

Security Groups act as virtual firewalls controlling inbound and outbound traffic for EC2 instances.
They help secure resources by allowing only trusted IPs, ports, and protocols.

---

### ✅ **IAM (Identity and Access Management)**

IAM manages users and their permissions to AWS resources.
You can create roles, enforce least privilege, and use MFA — ensuring secure and controlled access.

---

### ✅ **CloudWatch**

CloudWatch monitors metrics, logs, and alarms in real time.
It helps detect issues, trigger actions, and gain operational insight — essential for observability and automated responses.

---

### ✅ **Elastic Beanstalk**

Elastic Beanstalk handles the deployment and scaling of applications.
Just upload code — it automatically provisions infrastructure like EC2, ELB, and Auto Scaling — simplifying operations.

---

### ✅ **CloudFormation**

CloudFormation automates infrastructure provisioning using templates (IaC).
It allows consistent, repeatable deployments — supporting version control and rollback.

---

### ✅ **Amazon EBS (Elastic Block Store)**

EBS provides persistent block-level storage for EC2 instances.
It’s automatically replicated within the same AZ — ensuring data durability and high performance for transactional workloads.

---

### ✅ **Amazon Aurora**

Aurora is a high-performance, MySQL- and PostgreSQL-compatible managed database.
It offers up to 5x better performance with high availability, automatic failover, and replication — ideal for enterprise applications.

---

### ✅ **Amazon CloudTrail**

CloudTrail records all API activity in your AWS account.
It supports auditing, compliance, and forensic investigation by tracking who did what and when.

---

### ✅ **AWS Global Accelerator**

Global Accelerator improves availability and performance by routing traffic through AWS’s global backbone.
It detects unhealthy endpoints and reroutes traffic — ensuring global users always connect to the closest healthy endpoint.

---

### ✅ **Amazon S3 Glacier**

S3 Glacier is a low-cost archival storage solution.
It’s ideal for long-term backups and compliance data with infrequent access — retrieval times range from minutes to hours.

---

### ✅ **NAT Gateway**

A NAT Gateway allows private subnet instances to access the internet for updates or patches — without exposing them publicly.
It preserves security while enabling outbound traffic.

---

### ✅ **Bastion Host**

A Bastion Host enables secure SSH or RDP access to instances in private subnets.
It acts as a jump box — helping you manage internal resources without opening them to the internet.

---

### ✅ **Placement Groups**

Placement Groups influence how EC2 instances are placed on underlying hardware.
Types like “cluster” provide low-latency, high-bandwidth communication — useful for high-performance computing.

---

### ✅ **Spot Instances**

Spot Instances let you buy unused EC2 capacity at a discount of up to 90%.
They are ideal for flexible, fault-tolerant workloads — but can be interrupted when capacity is needed elsewhere.

---

### ✅ **Reserved Instances (RIs)**

Reserved Instances offer significant discounts for long-term EC2 usage.
They are perfect for predictable, steady-state workloads — reducing costs with 1- or 3-year commitments.

---

### ✅ **Elastic IP Address**

An Elastic IP is a static, public IPv4 address for dynamic cloud computing.
You can remap it instantly to another instance — useful for failover and fault tolerance.

---

### ✅ **Amazon SNS (Simple Notification Service)**

SNS is a fully managed pub/sub service for messaging and alerts.
It pushes messages to multiple subscribers (email, SMS, Lambda, SQS) — ideal for fan-out patterns and notification systems.

---

### ✅ **Amazon SQS (Simple Queue Service)**

SQS is a managed message queuing service.
It decouples application components, supports retries, and ensures message durability — making systems more resilient and scalable.

---

### ✅ **AWS Lambda**

Lambda lets you run code without provisioning servers (serverless).
You pay only for execution time — perfect for event-driven and microservice architectures.

---

### ✅ **Amazon API Gateway**

API Gateway is a managed service for building and securing APIs.
It integrates with Lambda, IAM, Cognito, and throttling — enabling you to create scalable and secure RESTful or WebSocket APIs.

---

### ✅ **Auto Recovery (EC2)**

Auto Recovery automatically recovers an EC2 instance when it becomes impaired.
It uses CloudWatch alarms to detect failure and recover the instance — reducing manual intervention and downtime.

---
Sure! Here’s an **expanded list** of additional AWS topics, including **ECS**, **EKS**, **AWS WAF**, **CloudFront signed URLs**, and more:

---

### ✅ **Amazon ECS (Elastic Container Service)**

ECS is a fully managed container orchestration service for Docker containers.
It simplifies running and managing containers on AWS, providing easy integration with other services like ELB and CloudWatch — ideal for microservices architectures.

---

### ✅ **Amazon EKS (Elastic Kubernetes Service)**

EKS is a fully managed Kubernetes service that makes it easy to run Kubernetes clusters on AWS.
It provides automated infrastructure management, including scaling and patching, and integrates seamlessly with AWS services like IAM, VPC, and CloudWatch.

---

### ✅ **AWS WAF (Web Application Firewall)**

AWS WAF is a managed firewall that helps protect web applications from common threats and exploits.
It allows you to define custom rules to block malicious traffic, such as SQL injection and cross-site scripting (XSS), to protect your applications.

---

### ✅ **CloudFront Signed URLs**

CloudFront Signed URLs enable access to private content stored in S3 or other CloudFront-distributed resources.
By generating signed URLs, you can restrict access to content based on time or other criteria — useful for serving content securely to authorized users only.

---

### ✅ **Amazon Elastic File System (EFS)**

Amazon EFS provides scalable file storage that can be accessed by multiple EC2 instances simultaneously.
It supports both NFS (Network File System) and is ideal for applications that require shared file access across multiple instances.

---

### ✅ **AWS Direct Connect**

Direct Connect allows you to establish a dedicated network connection from your on-premises data center to AWS.
It provides more reliable, lower-latency, and secure connectivity compared to standard internet connections, making it ideal for hybrid cloud environments.

---

### ✅ **Amazon Lightsail**

Amazon Lightsail offers simplified cloud computing services with virtual private servers, networking, and storage.
It’s designed for simpler applications and developers who need a low-cost and easy-to-use alternative to EC2.

---

### ✅ **Amazon Redshift**

Redshift is a fully managed data warehouse solution for performing fast SQL queries and analytics on large datasets.
It supports parallel query processing and integrates well with AWS data lakes, making it ideal for business intelligence workloads.

---

### ✅ **AWS Snowball**

AWS Snowball is a physical device that enables secure, fast, and large-scale data transfer to AWS.
It is typically used when you need to migrate large amounts of data to S3 or other AWS services but cannot rely on internet bandwidth due to limitations.

---

### ✅ **AWS Lambda\@Edge**

Lambda\@Edge allows you to run Lambda functions at AWS CloudFront edge locations.
It enables you to customize content delivery based on the request, such as modifying headers or redirecting traffic, all with low latency for a better user experience.

---

### ✅ **AWS Secrets Manager**

AWS Secrets Manager is a service that securely stores and manages sensitive information like API keys, passwords, and database credentials.
It integrates with IAM and can automatically rotate secrets for enhanced security.

---

### ✅ **AWS Cost Explorer**

AWS Cost Explorer allows you to analyze your AWS spending patterns and optimize costs.
It provides detailed insights and cost forecasts, helping you track your usage and make informed decisions about your AWS resources.

---

### ✅ **Amazon QuickSight**

Amazon QuickSight is a fully managed business intelligence (BI) service that lets you create and publish interactive dashboards.
It integrates with AWS data sources and supports machine learning-powered insights for better decision-making.

---

### ✅ **Amazon S3 Transfer Acceleration**

S3 Transfer Acceleration speeds up the transfer of files to and from Amazon S3 by utilizing CloudFront’s globally distributed edge locations.
It reduces latency and enhances performance when uploading large files or transferring over long distances.

---

### ✅ **AWS Batch**

AWS Batch is a fully managed batch processing service that runs large-scale parallel and high-performance computing (HPC) workloads.
It automatically provisions and scales EC2 instances based on the workload — making it ideal for compute-heavy jobs like simulations and data analysis.

---

### ✅ **AWS Systems Manager**

AWS Systems Manager helps automate operational tasks across AWS resources.
It includes capabilities for patch management, configuration compliance, and resource monitoring, enabling you to maintain and control your infrastructure at scale.

---

### ✅ **Amazon Macie**

Amazon Macie is a security service that uses machine learning to discover, classify, and protect sensitive data in AWS.
It helps identify personally identifiable information (PII) and ensures compliance with data privacy regulations like GDPR and CCPA.

---

### ✅ **AWS Glue**

AWS Glue is a fully managed ETL (Extract, Transform, Load) service that prepares and loads data for analytics.
It automates data transformation and scheduling, helping you process large datasets without managing the underlying infrastructure.

---

### ✅ **AWS Elastic Inference**

Elastic Inference allows you to attach GPU-powered inference acceleration to EC2 instances.
It enables you to run machine learning models at a fraction of the cost of traditional GPU instances, making it ideal for deep learning and inference workloads.

---

### ✅ **AWS X-Ray**

AWS X-Ray helps analyze and debug distributed applications by providing insights into performance bottlenecks, errors, and service issues.
It allows you to trace requests through your application, providing end-to-end visibility to improve application reliability.

---

### ✅ **Amazon FSx**

Amazon FSx provides fully managed file systems, such as Windows File Server and Lustre, for specific use cases.
It is ideal for workloads requiring a traditional file system, like enterprise applications, media processing, and high-performance computing.

---

### ✅ **AWS Data Pipeline**

AWS Data Pipeline is a web service for processing and moving data between different AWS compute and storage services.
It allows you to schedule regular data processing tasks, such as ETL operations, ensuring that your data is always fresh and ready for analysis.

---

### ✅ **Amazon EventBridge**

EventBridge is a serverless event bus that enables real-time event-driven architectures.
It makes it easy to connect applications with various AWS services or external sources and take automated actions in response to events.

---

### ✅ **AWS Transit Gateway**

Transit Gateway enables you to connect multiple VPCs, on-premises networks, and other AWS resources through a central hub.
It simplifies network management, reduces complexity, and increases scalability for large, multi-VPC environments.

---

### ✅ **AWS Control Tower**

AWS Control Tower automates the setup of a secure, multi-account AWS environment.
It provides governance and compliance controls, including AWS Organizations and Service Control Policies (SCPs), ensuring centralized management.

---

### ✅ **AWS Marketplace**

AWS Marketplace is an online store for purchasing software and services that integrate with AWS.
It offers a wide range of third-party solutions for security, networking, DevOps, machine learning, and more, simplifying procurement and deployment.

---
