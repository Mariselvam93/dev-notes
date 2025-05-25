U# 📘 AWS DHCP Options Set

## 🧾 What is it?
A **DHCP Options Set** in AWS is a configuration attached to a **VPC** that defines how DHCP (Dynamic Host Configuration Protocol) provides network configuration parameters to instances.

---

## 🔧 Key Components

| Option Name        | Description                                                                 |
|--------------------|-----------------------------------------------------------------------------|
| `domain-name`      | The DNS domain name (e.g., `ec2.internal`, `example.com`)                   |
| `domain-name-servers` | IP addresses of DNS servers (e.g., AmazonProvidedDNS or custom IPs)     |
| `ntp-servers`      | Network Time Protocol servers IP addresses (optional)                      |
| `netbios-name-servers` | IP addresses for NetBIOS name servers (for Windows networking)         |
| `netbios-node-type` | Type of NetBIOS node (1: broadcast, 2: peer-peer, 8: hybrid - recommended) |

---

## 🗂️ How It Works

- Instances in a VPC **automatically use** the associated DHCP options set when they are launched.
- AWS provides a **default DHCP options set** per region.
- You can **create custom sets** for use cases like:
  - Using **on-premises DNS** with hybrid cloud setups.
  - Defining a **custom domain** for internal resolution.
  - Overriding the **default Amazon DNS** settings.

---

## ⚙️ Usage Examples

- **Hybrid environments**: Point EC2 instances to on-prem DNS (e.g., Active Directory).
- **Custom domain**: Use a company’s private DNS suffix.
- **Time sync**: Set custom NTP servers for consistent timekeeping.

---

## 🚧 Considerations

- You can **associate only one DHCP options set per VPC**.
- Changing the DHCP options set will affect **new DHCP leases only** (e.g., after reboot/reconnect).
- Cannot directly modify a DHCP options set — must **create a new one** and associate it with the VPC.

---

## ✅ Best Practices

- Use **AmazonProvidedDNS** unless you have specific requirements.
- Always test custom options with a **staging VPC**.
- Document changes when associating a new DHCP options set.

---

## 📝 AWS CLI Tip

```bash
aws ec2 create-dhcp-options \
  --dhcp-configurations \
    "Key=domain-name,Values=example.com" \
    "Key=domain-name-servers,Values=10.0.0.2"

---

## 🌐 VPC and Subnet Creation via CLI

```bash
# Create a VPC
aws ec2 create-vpc --cidr-block 10.0.0.0/16

# Create a subnet
aws ec2 create-subnet --vpc-id <vpc-id> --cidr-block 10.0.1.0/24
```

---

## 🔄 Elastic IP Association with EC2

```bash
aws ec2 associate-address --instance-id <instance-id> --allocation-id <eip-alloc-id>
```

---

## 🔀 VPC Endpoints vs VPC Peering

| Feature                        | Overlapping CIDRs Allowed? | Notes                                              |
| ------------------------------ | -------------------------- | -------------------------------------------------- |
| **VPC Peering**                | ❌ No                       | Requires route table entries, no overlapping CIDRs |
| **Transit Gateway**            | ❌ No                       | Scalable alternative to peering                    |
| **PrivateLink / VPC Endpoint** | ✅ Yes                      | Uses ENI & DNS; does not rely on IP routing        |
| **VPN / Direct Connect**       | ❌ No                       | Overlaps cause routing issues                      |

---

## 📡 Site-to-Site VPN Requirements

* **Virtual Private Gateway (VGW)**
* **Customer Gateway (CGW)**

---

## 🔐 VPN Authentication Options (Client VPN)

* **Mutual Authentication** (PKI Certificates)
* **Federated Authentication** (Microsoft AD/Active Directory)

---

## 📘 Route Table Associations

* Route Tables are associated with **Subnets** in a VPC.

---

## 🚀 AWS Direct Connect Throughput

* Ranges from **50 Mbps to 100 Gbps**.

---

## 🌍 Egress-Only Internet Gateway

* Allows **IPv6-only** instances to **access the internet** without enabling inbound traffic.

---

## 🧾 DNS Zone Records – Reverse Lookups

* **PTR** record is used for **Reverse DNS Lookups**.

| Type            | DNS Record | Function                                  |
| --------------- | ---------- | ----------------------------------------- |
| **Forward DNS** | A / AAAA   | Domain → IP (`example.com` → `192.0.2.1`) |
| **Reverse DNS** | PTR        | IP → Domain (`192.0.2.1` → `example.com`) |

---

## 🛡️ Route 53 DNS Firewall – Managed Domain Lists

* **Botnet Command and Control domains**
* **Malware domains**

---

## 💰 Savings Plans vs Reserved Instances

| Feature     | Savings Plans                         | Reserved Instances                         |
| ----------- | ------------------------------------- | ------------------------------------------ |
| Commitment  | 1 or 3 years                          | 1 or 3 years                               |
| Flexibility | Applies across instance types/regions | Limited to a specific instance type/region |
| Billing     | Based on hourly spend                 | Upfront/Partial/No upfront options         |

---

## 💸 S3 Requestor Pays Option

* When enabled, **requestor** pays for data transfer and **request costs**.
* **Bucket owner** still pays for **storage costs**.

---

## 🖥️ PowerShell AWS Cmdlets

| Action                        | Cmdlet              |
| ----------------------------- | ------------------- |
| Create S3 Bucket              | `New-S3Bucket`      |
| Create EC2 Instance           | `New-EC2Instance`   |
| Create AWS Credential Profile | `Set-AWSCredential` |

---

## 💤 EC2 Hibernation Requirements

* **Encrypted Root Volume** must be enabled.

---

## 🔍 Show EC2 Instance Type via CLI

```bash
aws ec2 describe-instance-attribute --instance-id <id> --attribute instanceType
```

---

## 🔄 Auto Scaling Use Case

* Best applied for **Rapid Elasticity**: automatically adds/removes instances based on load.

---

## 📡 Common Database Management Ports

| Service              | Port  |
| -------------------- | ----- |
| MySQL                | 3306  |
| Microsoft SQL Server | 1433  |
| Amazon DocumentDB    | 27017 |

---

## 💽 EBS Partitioning

* Use **GPT (GUID Partition Table)** to initialize large disks (>2TB).

---

## 📦 S3 Glacier Deep Archive Retrieval Time

* **Default retrieval time**: **12 hours**.

---

## 🧠 S3 Lifecycle Rules

* Available under the **Management** tab of the S3 console.

---

## 💾 AWS Backup – Initial Backup Type

* The **first backup** is always a **Full Backup**.

---

## 🗄️ RDS Snapshots

* Back up the **entire storage volume**.
* Snapshots **cannot** be restored into **existing instances**.

---

## 🌍 AWS Global Infrastructure – Regions

* **Currently 26 to 35 Regions** (frequently updated; check AWS Global Infrastructure page).

---

## 🧱 Web Application Firewall

* Operates at **Layer 7 (Application Layer)** in the **OSI Model**.

---

## 🕷️ Botnet

* A **collection of infected computers** controlled by a single attacker.

---

## 🛡️ GuardDuty Finding

* Can help determine the **geographic location** of potentially malicious activity.

---

## 🛡️ Data Privacy Regulation (EU)

* **GDPR** (General Data Protection Regulation) protects EU citizen data.

---

## 🔐 AWS KMS Key Rotation

* **Automatic key rotation** occurs **annually** (every 365 days).

---

## 👤 IAM User Access Options

* **Console Access**
* **Programmatic Access** (via CLI, SDK, or API)

---

## 🔑 IAM Password Policy – Required Setting

* **Minimum password length** must be enforced and **cannot be disabled**.

---

## 🧑‍💼 Create Users in Managed AD – Requirements

* Management instance must be:

  * **Domain-joined**
  * **Have Active Directory tools installed**

---

## 🔐 Identity Provider Token Validation

* **Public key** of the **identity provider** is used by resource apps to verify token authenticity.

---

## 🔧 Network ACL Rule Creation (CLI)

```bash
aws ec2 create-network-acl-entry \
  --network-acl-id <acl-id> \
  --rule-number 100 \
  --protocol tcp \
  --rule-action allow \
  --egress \
  --cidr-block 0.0.0.0/0 \
  --port-range From=443,To=443
```

---

## 🔒 Create Security Group Rule for RDP

```bash
aws ec2 authorize-security-group-ingress \
  --group-id <sg-id> \
  --protocol tcp \
  --port 3389 \
  --cidr 0.0.0.0/0
```

---

## 🩹 Find EC2 Instances Missing Patches

* Use **AWS Systems Manager Advanced Query** or **Patch Manager**:

  * Systems Manager → **Compliance → Patch compliance**

---
## Read Replica and Multi-AZ
---
| Feature       | Read Replica           | Multi-AZ          |
| ------------- | ---------------------- | ----------------- |
| **Purpose**   | Read scalability       | High availability |
| **Sync Type** | Asynchronous           | Synchronous       |
| **Failover**  | Manual (unless Aurora) | Automatic         |
| **Readable**  | ✅ Yes                  | ❌ No              |
| **Cost**      | Additional cost        | Higher base cost  |
---

---
Here are clear and concise **study notes** on IAM policy global condition context keys like `aws:PrincipalOrgID`, `aws:PrincipalAccount`, `aws:PrincipalOrgPaths`, and others.

---

## 🔐 IAM Global Condition Context Keys: Study Notes

These condition keys are used in AWS IAM policies to **control access based on the identity of the requester**, particularly for **cross-account** or **organization-level** access control.

---

### 1. **`aws:PrincipalOrgID`**

* **Purpose**: Ensures that only principals (users, roles, etc.) from a specific **AWS Organization** can access a resource.

* **Value Type**: String

* **Example Value**: `"o-1234567890"`

* **Use Case**: Grant access only to entities from your own AWS Organization.

* **Example Policy Snippet**:

  ```json
  "Condition": {
    "StringEquals": {
      "aws:PrincipalOrgID": "o-1234567890"
    }
  }
  ```

* **Common Usage**: On S3 bucket policies, KMS keys, Lambda, and API Gateway to allow org-level access.

---

### 2. **`aws:PrincipalAccount`**

* **Purpose**: Restricts access to a specific AWS **account ID**.

* **Value Type**: String

* **Example Value**: `"123456789012"`

* **Use Case**: Allow access only from a particular AWS account (useful for cross-account access control).

* **Example Policy Snippet**:

  ```json
  "Condition": {
    "StringEquals": {
      "aws:PrincipalAccount": "123456789012"
    }
  }
  ```

* **Note**: This works well for tightly controlling who can assume a role or access a resource across accounts.

---

### 3. **`aws:PrincipalOrgPaths`**

* **Purpose**: Grants access based on the **organizational unit (OU)** hierarchy (organization path) in AWS Organizations.

* **Value Type**: String or array of strings

* **Example Value**: `"/o-1234567890/r-exampleroot/ou-abcd-1111/ou-abcd-2222/"`

* **Use Case**: Grant permissions only to principals from specific **OUs** (Organizational Units) inside your AWS Organization.

* **Example Policy Snippet**:

  ```json
  "Condition": {
    "StringLike": {
      "aws:PrincipalOrgPaths": "/o-1234567890/r-exampleroot/ou-abcd-1111/*"
    }
  }
  ```

* **Advanced Use**: Great for large orgs where different OUs have different trust levels or responsibilities.

---

## 🔄 Comparison Summary

| Condition Key           | Scope               | Typical Use Case                         | Example Value               |
| ----------------------- | ------------------- | ---------------------------------------- | --------------------------- |
| `aws:PrincipalOrgID`    | Entire Organization | Allow any account/user from your AWS Org | `o-1234567890`              |
| `aws:PrincipalAccount`  | Specific Account    | Allow only a specific AWS account        | `123456789012`              |
| `aws:PrincipalOrgPaths` | OU Path Hierarchy   | Fine-grained control within org OUs      | `/o-123/r-root/ou-abc-xyz/` |

---

## ⚠️ Important Notes

* These condition keys are often used in **resource policies** (like S3 bucket policies, KMS key policies), not IAM identity policies.
* They **require AWS Organizations** to be set up and the resource/account must be a **member** of that organization.
* **`aws:PrincipalOrgPaths`** requires AWS Organizations to **support OU path resolution**, so it may have performance implications in large orgs.

---

## 📚 Use Case Example: S3 Bucket Policy

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::my-secure-bucket/*",
      "Condition": {
        "StringEquals": {
          "aws:PrincipalOrgID": "o-1234567890"
        }
      }
    }
  ]
}
```

* This ensures **only identities from your AWS Organization** can access your S3 bucket—even if the bucket is public or accessible cross-account.

---
Here are **concise study notes** on **AWS Network Firewall** to help you understand its purpose, architecture, and key concepts.

---

## 🔥 AWS Network Firewall – Study Notes

### ✅ **What is AWS Network Firewall?**

**AWS Network Firewall** is a **managed network security service** that allows you to **filter traffic** entering or leaving your **Amazon VPC**. 

* Built for **east-west (VPC to VPC)** and **north-south (internet or on-prem)** traffic filtering.
* **Integrates with VPC traffic routing** via **Gateway Load Balancer (GWLB)** or **route tables**.
* **Trafice Flow Inspection** and **Traffic Filtering**

---

## 🏗️ Core Use Cases

* **Prevent malicious traffic** from entering/leaving your VPC.
* Enforce **compliance rules** (block unwanted domains, restrict ports).
* Implement **intrusion prevention** and **detection (IPS/IDS)**.
* Centralized control via **AWS Firewall Manager**.

---

## 🔑 Key Features

| Feature                          | Description                               |
| -------------------------------- | ----------------------------------------- |
| **Stateless and Stateful Rules** | Allows both types of packet filtering     |
| **Domain Name Filtering**        | Block/allow based on DNS names            |
| **Suricata Rule Support**        | Use open-source Suricata rules            |
| **Deep Packet Inspection (DPI)** | Inspect and control payloads              |
| **VPC-level protection**         | Integrates directly with your VPC routing |

---

## 🧱 Key Components

### 1. **Firewall Policy**

* Defines **rule groups** and **default actions**.
* Stateless and stateful rule groups can be included.

### 2. **Rule Groups**

* Contain filtering rules.
* Types:

  * **Stateless Rule Group**: Basic packet filtering (5-tuple match).
  * **Stateful Rule Group**: Deep inspection with session context.
* You can import **Suricata rules** in stateful groups.

### 3. **Firewall**

* A resource you deploy to your **VPC** subnet.
* Attached to **subnets** in each AZ for high availability.

### 4. **Logging**

* Send logs to **Amazon CloudWatch**, **S3**, or **Kinesis Data Firehose**.
* Types:

  * **Alert** logs: Show blocked or flagged packets.
  * **Flow** logs: Track allowed and denied traffic flows.

---

## 🔄 Traffic Flow Architecture

1. **Create a VPC firewall**.
2. Attach it to **public or private subnets**.
3. Route traffic **through the firewall** using VPC **route tables**.
4. Define rules in **firewall policy** to control traffic.

---

## 📘 Example: Blocking a Domain

You can block a domain (like social media) using Suricata rule in a **stateful rule group**:

```suricata
alert http any any -> any any (msg:"Block Facebook"; content:"facebook.com"; http_host; sid:1000001;)
```

---

## 🧠 Tips for Remembering

| Term            | Think Of...    | Analogy                        |
| --------------- | -------------- | ------------------------------ |
| Firewall        | Guard tower    | Watches and filters traffic    |
| Rule Group      | Rulebook       | Defines what's allowed/blocked |
| Firewall Policy | Strategy guide | Combines all rulebooks         |

---

## 🛠️ Integration

* Works with **Transit Gateway** for centralized control.
* **AWS Firewall Manager** can help manage firewalls **across accounts and regions**.
* Monitor via **CloudWatch metrics and logs**.

---

## 🔐 Cost Considerations

* **Per firewall endpoint/hour** (per AZ).
* **Data processing charges** per GB.
* **Logging** and **CloudWatch** costs apply separately.

---
## ⚡️ EBS Fast Snapshot Restore (FSR) – Study Notes

### ✅ **What is EBS Fast Snapshot Restore (FSR)?**

**Fast Snapshot Restore (FSR)** is an **Amazon EBS feature** that enables you to **launch volumes from snapshots with full performance instantly**, without needing to pre-warm or initialize the data.

> Normally, when restoring a volume from an EBS snapshot, the first read from each block can have high latency. FSR eliminates this "lazy loading."

---

## 🧠 Why It Matters

| Without FSR                          | With FSR                               |
| ------------------------------------ | -------------------------------------- |
| Volume takes time to initialize      | Immediate, full performance            |
| Reads have high latency until warmed | Low latency from first I/O             |
| Good for long-term archival          | Great for rapid boot/test environments |

---

## 🏗️ How It Works

* You **enable FSR on a snapshot** in specific Availability Zones (AZs).
* AWS pre-initializes the snapshot in the background.
* Once ready, **volumes created from the snapshot** in that AZ perform **as if they were freshly created volumes**.

---

## 💡 Key Points

* FSR is **not automatic**; you must enable it **per snapshot per AZ**.
* It may take **a few minutes to become active**.
* FSR does **not replicate** the snapshot—it **accelerates access** in the AZ.
* **Additional cost** applies (hourly per AZ per snapshot).

---

## 🧪 Example Scenario

1. You create a **snapshot** of a pre-configured EC2 instance with software.
2. You enable FSR in **`us-east-1a`**.
3. Later, you launch **test environments** from that snapshot in **`us-east-1a`**.
4. The new volumes **perform immediately** without initialization latency.

---

## 📘 Exam Tip (AWS Solutions Architect)

> FSR improves **boot time** and **data access performance** from EBS snapshots — ideal for **mission-critical, time-sensitive workloads**.

---

---
## 🚀 AWS Serverless Architecture for Scalable, Low-Latency Web Applications
### 📈 **Scalability**

* **S3 + CloudFront**: Scales to millions of static requests with low latency.
* **API Gateway + Lambda**: Serverless, auto-scales for traffic bursts.
* **DynamoDB**: Handles millions of requests/sec with millisecond latency.

---

### ⚙️ **Operational Overhead**

* **No infrastructure management**: Serverless, no provisioning/patching needed.
* **High availability**: Built-in resilience and regional HA.

---

### ⚡ **Performance**

* **CloudFront**: Edge caching for global low latency.
* **DynamoDB**: Single-digit millisecond performance at scale.
* **Lambda + API Gateway**: Fast response, supports provisioned concurrency.

---
Here are **concise study notes** on **AWS Systems Manager Session Manager**, useful for understanding and revising the key features:

---

## 🔐 **AWS Systems Manager Session Manager** – 

### ✅ **What is Session Manager?**

**Session Manager** is a feature of **AWS Systems Manager** that enables secure and auditable **remote shell access** to Amazon EC2 instances and on-premises servers — **without the need for SSH keys, bastion hosts, or open inbound ports**.

---

### 🔑 **Key Features**

* **Secure shell access** to instances via browser or AWS CLI.
* **No need for SSH or RDP**, eliminating the risk of key management.
* Works **without opening inbound ports** — enhances security.
* **Logging and auditing** of sessions via Amazon S3 or CloudWatch Logs.
* Supports **multi-factor authentication (MFA)** and **IAM-based permissions**.

---

### 🧰 **Common Use Cases**

| Use Case              | Benefit                         |
| --------------------- | ------------------------------- |
| EC2 administration    | Secure and direct access        |
| No SSH key management | Simpler ops and better security |
| Audit and compliance  | Session logs for accountability |
| Private subnet access | No need for bastion hosts       |

---

### ⚙️ **How It Works**

1. **SSM Agent** must be installed and running on the instance.
2. Instance must have an **IAM role** with `ssm:StartSession` and related permissions.
3. Session Manager connects via **AWS Systems Manager endpoints** — **no inbound traffic required**.

---

### 🧪 **Command Line Example**

```bash
aws ssm start-session --target i-0123456789abcdef0
```

---

### 📝 IAM Policy Sample

```json
{
  "Effect": "Allow",
  "Action": [
    "ssm:StartSession",
    "ssm:TerminateSession",
    "ssm:DescribeSessions"
  ],
  "Resource": "*"
}
```

---

### 🧠 Exam Tip

> Session Manager is **ideal for secure, auditable instance access** without needing to manage SSH keys or expose your network.

---
Here are **concise study notes** on **Amazon FSx File Gateway**, ideal for quick revision or interview prep:

---

## 📘 **Amazon FSx File Gateway – Study Notes**

### 🧩 **What is FSx File Gateway?**

**Amazon FSx File Gateway** is a feature of **AWS Storage Gateway** that provides **low-latency access** to data stored in **Amazon FSx for Windows File Server** from **on-premises environments**.

It acts as a **hybrid cloud file storage solution**, enabling seamless access and caching of FSx file shares on-premises.

---

### 🔑 **Key Features**

* Provides **on-premises access** to **Amazon FSx for Windows File Server** shares.
* Supports **SMB protocol** (ideal for Windows-based applications).
* Uses a **local cache** to reduce latency and improve performance.
* Automatically **syncs data** to FSx in AWS.
* **Integrates with Active Directory** for access control and authentication.

---

### 🧰 **Common Use Cases**

| Use Case                    | Benefit                                      |
| --------------------------- | -------------------------------------------- |
| Lift-and-shift Windows apps | Retain on-premises access with cloud storage |
| Branch office file access   | Local caching reduces latency                |
| Centralize file shares      | Store in FSx, access from multiple sites     |
| Backup or disaster recovery | Cloud-based storage with on-prem access      |

---

### 🏗️ **How It Works**

1. Deploy the **FSx File Gateway** as a virtual appliance (VM, EC2, or hardware).
2. Connect it to your **Amazon FSx for Windows File Server**.
3. Mount SMB shares on client machines using the gateway.
4. Frequently accessed files are **cached locally** for faster access.
5. File changes are **automatically synced** to FSx in AWS.

---

### ⚙️ **Integration and Management**

* **Active Directory (AD)**: Uses AD credentials and permissions.
* **AWS Backup**: FSx files can be backed up centrally.
* **AWS CloudWatch**: Monitors gateway health and metrics.

---

### 📌 Key Benefits

* **Low-latency file access** from on-premises to cloud-hosted FSx.
* **Reduces storage costs** by keeping cold data in AWS.
* **Simplifies operations** by centralizing data in FSx.
* **Improves security** with AWS-managed infrastructure.

---

### 🔐 Security

* Supports **Kerberos-based authentication** via AD.
* Data is encrypted **in transit (SMB, HTTPS)** and **at rest**.
* Fine-grained access control through **NTFS permissions**.

---

### 📝 Notes

* **FSx File Gateway ≠ File Gateway**: FSx File Gateway is specifically for FSx for Windows File Server.
* **Only SMB supported** — not compatible with NFS clients.

---

### 🧠 Exam Tip

> FSx File Gateway is ideal when you need **Windows file share access** from **on-prem** to **cloud-based FSx** with **low latency and seamless user experience**.

---

Here are **short and effective study notes** on **Amazon DynamoDB Point-In-Time Recovery (PITR)** and **RPO (Recovery Point Objective):**

---

## 🗃️ **DynamoDB Point-In-Time Recovery (PITR)**

### ✅ **What is PITR?**

**PITR** is a feature that **automatically backs up** your DynamoDB table data continuously and allows you to **restore to any second in time** within the **last 35 days**.

---

### 🕒 **Key Features**

* **Continuous backups** (not snapshots).
* Restore data to **any second** within the past **35 days**.
* **No performance impact** on table operations.
* PITR can be **enabled or disabled** per table.
* Supports **full table restore** (not partial).

---

### 🧪 **Restore Example**

```bash
aws dynamodb restore-table-to-point-in-time \
  --source-table-name MyTable \
  --target-table-name MyTable-Restored \
  --restore-date-time 2024-05-01T15:30:00Z
```

---

## 🔁 **Recovery Point Objective (RPO)**

### 📌 **What is RPO?**

**RPO** defines **how much data you can afford to lose** in case of a failure — in time terms (e.g., 5 minutes of data).

---

### 🕐 **PITR & RPO**

* DynamoDB with PITR offers **near-zero RPO** — since it can restore to any second.
* **Max RPO ≈ few seconds**, depending on how fast data is committed.

---

### 🧠 Exam Tip

> Use **PITR** when your application requires **minimal data loss (low RPO)** and the ability to **recover from accidental writes or deletes**.

---

### 📈 Summary Table

| Feature                 | Description                                    |
| ----------------------- | ---------------------------------------------- |
| **PITR**                | Continuous backup with up to 35-day retention  |
| **Restore granularity** | Per second                                     |
| **Max retention**       | 35 days                                        |
| **Performance impact**  | None                                           |
| **RPO**                 | Near-zero (few seconds of potential data loss) |
| **Use case**            | Data recovery from accidental writes/deletes   |

---

Here are concise study notes on **AWS Config Rule to check certificate expiry**, including how it works and how to implement it:

---

## ✅ **AWS Config Rule to Check Certificate Expiry**

### 📘 **What is AWS Config?**

**AWS Config** is a service that **monitors and records AWS resource configurations** and lets you **evaluate compliance** against desired configurations using **Config Rules**.

---

### 🔎 **Use Case: Monitor SSL/TLS Certificate Expiry**

You want to ensure that your **ACM (AWS Certificate Manager)** certificates are **not expiring soon**, to avoid service disruptions.

---

## 🛠️ **How to Implement Certificate Expiry Rule**

### Option 1: **Use AWS Managed Rule**

* **Rule Name:** `acm-certificate-expiration-check`
* **Scope:** Checks **ACM certificates** for expiration within a configurable number of days.

### 🔧 **Configurable Parameters**

| Parameter          | Description                                                            |
| ------------------ | ---------------------------------------------------------------------- |
| `daysToExpiration` | Number of days before expiration to trigger noncompliance (e.g., `30`) |

### Example:

```json
{
  "ConfigRuleName": "acm-certificate-expiration-check",
  "SourceIdentifier": "ACM_CERTIFICATE_EXPIRATION_CHECK",
  "InputParameters": {
    "daysToExpiration": "30"
  }
}
```

---

### 🧠 What It Does:

* Evaluates **ACM certificates**.
* If a certificate **expires within the next X days**, it is marked **NON\_COMPLIANT**.
* Supports **CloudWatch and SNS notifications** for alerts.

---

## 🔐 **Permissions Needed**

To use the rule, AWS Config must have permission to read ACM certificate details:

```json
{
  "Effect": "Allow",
  "Action": [
    "acm:ListCertificates",
    "acm:DescribeCertificate"
  ],
  "Resource": "*"
}
```

---

### 🚨 Alerts & Remediation (Optional)

* **SNS**: Trigger an email/Slack alert when a cert is close to expiration.
* **Automation**: Trigger a **Lambda** function to renew/notify on certificate expiry.

---

### 🧠 Exam Tip:

> Use **`acm-certificate-expiration-check`** to ensure ACM certificates don’t expire without notice. This is a **managed rule**, so it's easy to configure with minimal effort.

---
Here are clear explanations on both requested topics:

---

## 📡 **Reserved IP Addresses in an AWS VPC Subnet**

When you create a subnet in an Amazon VPC, **AWS reserves 5 IP addresses** in each subnet for internal networking purposes.

Assume a subnet like `10.0.0.0/24`. Here's what the reserved addresses mean:

| **IP Address** | **Purpose**                                                                                  |
| -------------- | -------------------------------------------------------------------------------------------- |
| `10.0.0.0`     | **Network address** – Identifies the subnet; not assignable to instances.                    |
| `10.0.0.1`     | **VPC router** – Used by AWS as the default gateway for the subnet.                          |
| `10.0.0.2`     | **DNS server** – AWS DNS service IP (base + 2); used by instances for DNS.                   |
| `10.0.0.3`     | **Reserved for future use** – Not currently used by AWS.                                     |
| `10.0.0.255`   | **Broadcast address** – Not used in AWS (VPC doesn't support broadcast), but still reserved. |

> 📝 So, for a `/24` subnet (256 addresses), only **251 IPs are usable** by EC2 or other resources.

---

## 🔐 **Explicit Allow and Deny in IAM Policies**

### 🔹 **Explicit Allow**

* Grants a **specific permission** to a resource or action.
* Default behavior in IAM: **No access unless explicitly allowed**.

**Example:**

```json
{
  "Effect": "Allow",
  "Action": "s3:ListBucket",
  "Resource": "arn:aws:s3:::example-bucket"
}
```

### 🔸 **Explicit Deny**

* **Overrides any Allow**, even if permissions are granted elsewhere (like in another policy or group).
* Used to **restrict access under certain conditions**, or for security hardening.

**Example:**

```json
{
  "Effect": "Deny",
  "Action": "s3:*",
  "Resource": "arn:aws:s3:::example-bucket/private/*"
}
```

### 🧠 **Key Points**

* If a **Deny** and **Allow** both apply to a user, **Deny wins**.
* IAM policies are **deny-by-default**. Access is only granted if **explicitly allowed**.
* **Use Deny** to block sensitive actions or define **exceptions** to broader permissions.

---
Here’s a simple explanation of **Simple** and **Target Tracking** scaling policies in **Auto Scaling**:

---

### 🔹 **1. Simple Scaling Policy**

**What it does:**
Adds or removes a fixed number of instances when a CloudWatch alarm is triggered.

**How it works:**

* Based on **thresholds** (e.g., CPU > 70%)
* Waits for the cooldown period before any other action

**Example:**
If CPU > 70%, **add 2 instances**
If CPU < 30%, **remove 1 instance**

**Use when:**
You want **manual control** over how many instances are added or removed.

---

### 🔸 **2. Target Tracking Scaling Policy**

**What it does:**
Automatically adjusts capacity to keep a specific metric (e.g., CPU) at a target value.

**How it works:**

* You set a **target value** (e.g., keep CPU at 50%)
* Auto Scaling **automatically calculates** how many instances to add/remove
* No need to define alarms manually

**Example:**
Keep average **CPU utilization at 50%**
→ It may add/remove instances automatically to maintain that.

**Use when:**
You want **automatic and smart scaling** based on a target (like CPU, request count, etc.)

---

### ✅ Quick Comparison

| Feature   | Simple Scaling        | Target Tracking           |
| --------- | --------------------- | ------------------------- |
| Trigger   | CloudWatch Alarm      | Target metric (like CPU%) |
| Control   | Manual (fixed number) | Automatic (calculated)    |
| Cooldown  | Required              | Managed automatically     |
| Ideal for | Basic setups          | Most common use cases     |
---

### 📨 SQS Extended Client Library — Overview

The **Amazon SQS Extended Client Library** is an extension of the standard AWS SDK for SQS that supports sending **large message payloads** via **Amazon S3** instead of directly through SQS.

---

### 🔧 Why It’s Needed

* **SQS max message size** = 256 KB.
* When messages exceed this size (e.g., large JSON, images, reports), the extended client:

  * **Stores payload in S3**.
  * Sends a **pointer (reference)** to the S3 object in the actual SQS message.

---

### ✅ Key Features

| Feature                         | Description                                                                 |
| ------------------------------- | --------------------------------------------------------------------------- |
| **Large message support**       | Send/receive messages > 256 KB by storing payload in S3.                    |
| **Seamless integration**        | Wraps the standard `AmazonSQS` client.                                      |
| **Automatic storage/retrieval** | Handles upload/download from S3 internally.                                 |
| **S3-managed lifecycle**        | Can be configured to automatically delete payload from S3 after processing. |

---

### 🛠️ How It Works (Flow)

1. **Producer**:

   * Uploads the large payload to S3.
   * Sends a pointer (e.g., S3 bucket/key) in the SQS message.

2. **Consumer**:

   * Receives the pointer from SQS.
   * Downloads the full payload from S3 automatically.

---

### 🚀 Example Use Case

* Sending **large event data**, **logs**, or **files** between distributed systems or microservices that rely on SQS queues.

---

### ⚠️ Things to Consider

* **S3 costs** apply (storage, PUT/GET).
* **IAM permissions** are required for both SQS and S3 access.
* **Reliability**: Ensure cleanup if a message is not successfully processed (to avoid S3 orphaned objects).

---

### 🧪 Sample Code (Java)

```java
AmazonS3 s3 = AmazonS3ClientBuilder.defaultClient();
AmazonSQS sqs = AmazonSQSClientBuilder.defaultClient();

ExtendedClientConfiguration extendedConfig = new ExtendedClientConfiguration()
    .withLargePayloadSupportEnabled(s3, "my-bucket")
    .withAlwaysThroughS3(true);

AmazonSQSExtendedClient sqsExtendedClient = new AmazonSQSExtendedClient(sqs, extendedConfig);

SendMessageRequest request = new SendMessageRequest()
    .withQueueUrl(queueUrl)
    .withMessageBody(largePayloadString);

sqsExtendedClient.sendMessage(request);
```

---
Here are **concise study notes** on **Provisioned Concurrency in AWS Lambda**:

---

### ⚙️ **Provisioned Concurrency in AWS Lambda – Notes**

#### 🚀 What Is It?

* **Provisioned Concurrency** ensures that a pre-defined number of Lambda instances are **initialized and ready to serve requests instantly**.
* It eliminates **cold starts** — the latency that occurs during the first invocation or after a period of inactivity.

---

### 🔄 How It Works

* You **configure** a set number of concurrent instances to stay warm.
* AWS **keeps them initialized** with your function’s code and dependencies loaded.
* These instances are used **before any new ones are initialized** during traffic spikes.

---

### 🧩 Use Cases

* **Performance-critical applications** like:

  * APIs with **predictable traffic** (e.g., morning spike)
  * **Low-latency applications**
  * **Gaming backends**
  * **Real-time processing systems**

---

### 💵 Cost Consideration

* You are **billed separately** for:

  * **Provisioned concurrency** (per minute)
  * **Invocation time** (standard Lambda pricing)

---

### ⚙️ How to Configure

* Via:

  * AWS Console (under Lambda → Versions → Aliases → Provisioned Concurrency)
  * AWS CLI:

    ```bash
    aws lambda put-provisioned-concurrency-config \
      --function-name my-function \
      --qualifier prod \
      --provisioned-concurrent-executions 10
    ```
  * Infrastructure as Code tools (e.g., CloudFormation, Terraform)

---

### 🔄 Auto Scaling Option

* You can use **Application Auto Scaling** to adjust provisioned concurrency based on:

  * **Schedule**
  * **Utilization metrics**

---

### ✅ Benefits

* **Eliminates cold start latency**
* **Predictable performance**
* **Reduce Latency**
* Works well with **API Gateway**, **AppSync**, and **ALB**

---

---

### 📝 Notes: Automating EC2 & RDS Start/Stop with Minimal Cost

#### ✅ **Best Solution:**

**Use AWS Lambda with Amazon EventBridge**

* **Lambda**: Serverless function to start/stop EC2 & RDS instances.
* **EventBridge**: Triggers Lambda on a cron schedule (e.g., outside business hours).
* **No infrastructure maintenance**.
* **Cost-effective** (only pay for Lambda execution time).

---

#### 🛠️ **Why It Works:**

* Automates **resource scheduling** (e.g., stop at 7 PM, start at 7 AM).
* Can scale to multiple EC2/RDS instances.
* Uses **AWS SDK (Boto3 / SDK for .NET, Java, etc.)** to manage resources.
* Fully managed and secure.

---

#### 🚫 Alternatives & Drawbacks:

* **Crontab on EC2 (Option C)**: Adds EC2 cost and maintenance.
* **Marketplace Tools (Option B)**: May add extra cost and complexity.
* **Elastic Resize / Scale to Zero (Option A)**:

  * EC2 doesn’t support "scale to zero."
  * RDS can't be scaled to zero (except Aurora Serverless v2).

---
### 📝 POSIX-Compliant Storage 
---

### 📌 What is POSIX?

**POSIX** (Portable Operating System Interface) is a set of IEEE standards that ensures compatibility and interoperability between different Unix-like operating systems. For file systems, **POSIX compliance** means the storage supports standard file and directory operations like:

* `open()`, `read()`, `write()`, `close()`
* File permissions (`chmod`, `chown`)
* Symbolic links, hard links
* Locking, metadata (e.g., timestamps, ownership)
* Directory traversal with `ls`, `cd`, etc.

---

### ✅ Key Characteristics of POSIX-Compliant Storage:

* **Hierarchical file system structure**
* **User/Group/Other permission model**
* **Concurrent access with file locking**
* **Strong consistency** (immediate visibility of file changes)
* **Standard I/O system calls**

---

### 📦 AWS Services Offering POSIX-Compliant Storage:

| AWS Service                          | POSIX Compliance | Description                                                            |
| ------------------------------------ | ---------------- | ---------------------------------------------------------------------- |
| **Amazon EFS** (Elastic File System) | ✅ Yes            | Fully managed NFS (v4.1, v4.0) file system for Linux-based workloads.  |
| **Amazon FSx for Lustre**            | ✅ Yes            | High-performance file system used for HPC, ML – supports POSIX API.    |
| **Amazon FSx for OpenZFS**           | ✅ Yes            | OpenZFS-based POSIX-compliant storage. Suitable for Unix workloads.    |
| **Amazon FSx for NetApp ONTAP**      | ✅ Yes            | Fully managed ONTAP storage – supports NFS, SMB with POSIX compliance. |

---

### 🚫 Not POSIX-Compliant:

| Service            | Reason                                                                                       |
| ------------------ | -------------------------------------------------------------------------------------------- |
| **Amazon S3**      | Object storage – lacks file system semantics like directories, permissions, or file locking. |
| **Amazon Glacier** | Archival object storage – not designed for POSIX file access.                                |

---

### 🛠 Use Cases for POSIX-Compliant Storage:

* Traditional Linux/Unix applications
* Legacy enterprise apps that expect local or NFS-style file access
* HPC workloads (e.g., genomics, financial simulations)
* Shared home directories and configuration storage

---
### 📝 Amazon EFS (Elastic File System) Storage Tiers – Quick Notes

---

### 📦 EFS Storage Classes:

Amazon EFS offers **two main storage classes**, each with **two access tiers**, optimized for different performance and cost needs.

---

### 1. **Standard Storage Class**

* Designed for **frequently accessed files**.
* Higher performance, **higher cost**.
* Ideal for active workloads like:

  * Web servers
  * Application home directories
  * Dev/test environments

---

### 2. **Infrequent Access (IA) Storage Class**

* For **files not accessed every day**.
* **Up to 92% lower cost** than Standard.
* **Retrieval fee** applies per GB when accessed.
* Great for:

  * Backups
  * Logs
  * Archive-style data

---

### 🎛️ EFS Access Tiers

| Access Tier         | Description                                                                          |
| ------------------- | ------------------------------------------------------------------------------------ |
| **EFS Standard**    | Default for frequent access, low latency.                                            |
| **EFS Standard-IA** | Infrequent access, lower cost.                                                       |
| **EFS One Zone**    | Like Standard but within a single AZ. Lower cost, **less resilient**.                |
| **EFS One Zone-IA** | Single AZ + Infrequent Access. **Lowest cost**, use with backups or replicated data. |

---

### 🔄 Lifecycle Management

* **Automatic tiering** via lifecycle policies.
* Files can move from **Standard → IA** after a set number of days (e.g., 30).
* Only applies to files **not accessed** during that period.

---

### ⚠️ Notes

* **Standard and Standard-IA** are **multi-AZ and highly available**.
* **One Zone tiers** are **single-AZ** and suitable for redundant or temporary data.

---
Here are the **notes** for the solution:

---

### 🛒 **Ecommerce App: Session Management Requirements**

* **Running on**: EC2 instances in Auto Scaling group behind **ALB**
* **DB**: Amazon RDS for MariaDB (Multi-AZ)
* **Need**: **Durable session management** during transactions

---

### ✅ **Correct Answers**:

**B. Use an Amazon DynamoDB table to store customer session information**

* ✅ Fully managed NoSQL DB
* ✅ Durable and highly available
* ✅ Scales well with high traffic
* ✅ Ideal for storing session state

**D. Deploy an Amazon ElastiCache for Redis cluster to store customer session information**

* ✅ In-memory store, very low latency
* ✅ Supports TTL for session expiration
* ✅ Widely used for fast session storage
* ✅ Can be configured for durability with Redis replication and snapshots

---

### ❌ **Incorrect Options**:

**A. Turn on the sticky sessions feature (session affinity) on the ALB**

* ❌ Helps route users to the same instance
* ❌ **Not durable** — session data is lost if instance terminates (due to scaling)

**C. Deploy an Amazon Cognito user pool**

* ❌ Used for **authentication/authorization**, not for storing session state
* ❌ Not designed for transactional session data

**E. Use AWS Systems Manager Application Manager**

* ❌ Tool for **monitoring and managing** apps
* ❌ Not used for storing user sessions

---

Here's a **concise comparison between AWS CloudTrail and AWS Config** presented as notes:

---

### ☁️ **AWS CloudTrail vs AWS Config**

| Feature                 | **AWS CloudTrail**                                | **AWS Config**                                              |
| ----------------------- | ------------------------------------------------- | ----------------------------------------------------------- |
| 🔍 **Purpose**          | Tracks **API calls and user activity**            | Tracks **resource configurations and compliance**           |
| 📅 **Focus**            | **Who did what, when, and from where**            | **What does the resource look like and how has it changed** |
| 📜 **Logs**             | Captures **event history of AWS API calls**       | Captures **point-in-time configuration snapshots**          |
| 🛠️ **Use Case**        | Security analysis, auditing, troubleshooting      | Compliance, change tracking, auditing                       |
| ⌛ **Granularity**       | Event-based (per action)                          | Resource state over time (config history)                   |
| 🔁 **Historical View**  | API activity trail                                | Timeline of resource configuration changes                  |
| ✅ **Compliance Checks** | ❌ Not designed for compliance rules               | ✅ Supports **custom rules** and **managed rules**           |
| 🔐 **Security**         | Helps identify **unauthorized access or actions** | Helps detect **drift from desired configuration**           |
| 📤 **Integration**      | Integrates with CloudWatch, Lambda                | Integrates with AWS Config Rules, SNS, Lambda               |

---

### 🧠 **Summary**:

* **Use CloudTrail** to **audit API activity** across AWS accounts.
* **Use AWS Config** to **monitor resource configurations** and ensure they comply with organizational standards.
---
---

### 📝 **Kinesis Data Streams default settings problem:  Notes:**

#### 🟡 Problem Summary:

* Application sends data to **Amazon Kinesis Data Streams**.
* Every **other day**, the app **consumes** and writes data to **Amazon S3**.
* **Some data is missing** in S3.

---

### 🔍 **Analysis:**

| Option | Description                                                                         | Analysis                                                                                                                                                                                 |
| ------ | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A**  | Update the **data retention period** in Kinesis Data Streams (default is 24 hours). | ✅ **Correct**. Since the application reads the data **every other day**, the **default 24-hour retention** period would cause Kinesis to **discard** older records before they are read. |
| **B**  | Use Kinesis Producer Library (KPL).                                                 | ❌ Irrelevant to retention issue. KPL helps with performance while sending data, not with data loss due to retention settings.                                                            |
| **C**  | Increase number of shards.                                                          | ❌ Throughput might be fine. The problem is not about throttling, but **missing data** due to expired retention.                                                                          |
| **D**  | Enable S3 Versioning.                                                               | ❌ Irrelevant. S3 versioning doesn’t solve **missing** data from Kinesis. The issue is **before** data reaches S3.                                                                        |

---

### 📌 **Key Point**:

> **Kinesis Data Streams default retention period is 24 hours**. If data is not consumed within that period, it is **lost**.

---
---

### 📝 Notes:

#### 📌 Use Case Summary:

* Application receives **UDP** traffic from **remote devices**.
* Must process data **immediately** and respond if needed.
* No storage needed, but **low latency** and **rapid failover** across **Regions** are required.

---

### 🔍 Option Analysis:

| Option | Description                                  | Evaluation                                                                                                                                                                              |
| ------ | -------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **A**  | Route 53 failover + NLB + Lambda             | ❌ **NLB supports UDP**, but **Lambda does not** support **UDP triggers** directly. This setup is not feasible.                                                                          |
| **B**  | **Global Accelerator + NLB + ECS (Fargate)** | ✅ **Best Fit**: NLB supports **UDP**, ECS with Fargate handles stateless compute, and **Global Accelerator ensures low-latency global routing with automatic failover across regions**. |
| **C**  | Global Accelerator + **ALB** + ECS           | ❌ **ALB does not support UDP** — only **HTTP/HTTPS**.                                                                                                                                   |
| **D**  | Route 53 failover + **ALB** + ECS            | ❌ Same issue — **ALB doesn't support UDP** and **Route 53 failover is slower** compared to **Global Accelerator**.                                                                      |

---

### ⚡ Key Concepts:

* **UDP Support**: Only **NLB** supports **UDP** (not ALB).
* **Global Accelerator**: Offers **low-latency global routing** and **automatic regional failover** (much faster than DNS-based failover).
* **Fargate + ECS**: Perfect for handling stateless processing without managing infrastructure.

---
