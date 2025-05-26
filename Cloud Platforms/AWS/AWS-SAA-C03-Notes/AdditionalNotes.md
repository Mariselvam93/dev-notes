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

---

### 📝 **Amazon CloudWatch EC2 Metrics - Notes**

#### ✅ **Available by Default (Basic EC2 Metrics):**

CloudWatch provides these metrics out of the box for Amazon EC2 instances:

* **CPU Utilization**
* **Network Utilization**
* **Disk Performance**
* **Disk Reads/Writes**

---

#### ⚠️ **Metrics That Require Custom Configuration:**

The following metrics are **not available by default** and require **custom CloudWatch metrics** using scripts (e.g., Perl, shell script, or CloudWatch agent):

* **Memory Utilization**
* **Disk Swap Utilization**
* **Disk Space Utilization**
* **Page File Utilization (Windows)**
* **Log Collection**

---

#### 🛠️ **How to Enable Custom Metrics:**

* Install and configure the **CloudWatch Agent** on the EC2 instance.
* Use custom scripts to gather and push these metrics to CloudWatch.
* CloudWatch Agent supports **Linux and Windows** and can collect both metrics and logs.

---
Here are the notes based on the information you provided:

---

### 📝 **AWS Lambda Environment Variables & Encryption with KMS**

#### 🔐 **Default Encryption Behavior:**

* When you **create or update** Lambda functions that use **environment variables**, AWS Lambda:

  * **Encrypts** them using **AWS Key Management Service (AWS KMS)**.
  * On invocation, the variables are **decrypted** and made available to the Lambda code.

#### 🗝️ **Default KMS Key (Automatically Managed):**

* On first use in a Region, AWS Lambda **automatically creates a default service key** in AWS KMS.
* This **default key is used** to encrypt environment variables by default.

#### ⚠️ **Limitations of the Default KMS Key:**

* You **cannot use** the default key with **encryption helpers** after the function is created.
* Selecting the **default key manually** may result in **errors**.

---

#### ✅ **Using a Customer Managed KMS Key (Recommended for More Control):**

* You can **create your own KMS key** and configure the Lambda function to use it instead of the default.
* Benefits of using a **Customer Managed Key (CMK):**

  * **Key rotation** capability.
  * **Enable/disable** the key.
  * **Define granular access control** (IAM policies and key policies).
  * **Auditing** using CloudTrail logs for key usage.

---
Here are the notes based on your provided information:

---

### 📝 **Amazon RDS Enhanced Monitoring vs. CloudWatch Metrics**

#### 📊 **Monitoring Options for RDS DB Instances:**

1. **Amazon CloudWatch (Default):**

   * Collects **OS metrics** (e.g., CPU, memory, disk I/O) **from the hypervisor layer**.
   * Less granular view of resource usage.
   * CPU utilization values might differ from actual usage by processes.

2. **Enhanced Monitoring (Optional – More Detailed):**

   * Collects **real-time OS metrics** **from an agent on the RDS instance itself**.
   * Offers **per-process/thread metrics** and **granular visibility** into DB performance.
   * **JSON output** is available in **CloudWatch Logs** under the `RDSOSMetrics` log group.
   * **Retention**: By default, metrics are stored for **30 days**, but this can be changed in CloudWatch settings.

---

#### 🔍 **Differences Between CloudWatch and Enhanced Monitoring:**

| Feature                  | CloudWatch                                 | Enhanced Monitoring              |
| ------------------------ | ------------------------------------------ | -------------------------------- |
| **Metric Source**        | Hypervisor                                 | RDS agent inside the DB instance |
| **Granularity**          | Instance-level                             | Process/thread-level             |
| **Accuracy (CPU, etc.)** | Approximate (includes hypervisor overhead) | More accurate and detailed       |
| **Latency**              | \~1 minute                                 | \~1–5 seconds                    |
| **Customization**        | Limited                                    | High                             |

---

#### ⚠️ **Additional Considerations:**

* Differences are **more pronounced in smaller instance classes**, where **hypervisor overhead** impacts metrics more.
* For **in-depth diagnostics** (e.g., CPU usage per thread), **Enhanced Monitoring is essential**.
* To **change retention**, update the `RDSOSMetrics` **log group settings in CloudWatch**.

---
Here are your notes in a clear and structured format:

---

### 📝 **Default Termination Policy in Amazon EC2 Auto Scaling**

The **default termination policy** is designed to **maintain high availability and optimize cost** by distributing instances **evenly across Availability Zones (AZs)** and **terminating instances wisely** when scaling in.

---

### 🔁 **Default Termination Policy – Step-by-Step Behavior:**

1. **Choose an Availability Zone (AZ):**

   * Identify AZs with **the most instances**.
   * **Exclude** instances protected from scale-in.
   * If multiple AZs qualify:

     * Choose the one with instances using the **oldest launch template or configuration**.

2. **Filter Instances:**

   * From the selected AZ, **filter unprotected instances** using the **oldest launch template**.

3. **Billing Hour Optimization:**

   * Among the filtered instances, choose the one **closest to the next billing hour**.
   * This step helps **maximize usage** before incurring a new billing hour.

4. **Final Selection:**

   * If multiple instances are equally close to the next billing hour:

     * **Select one at random** for termination.

---

### 🎯 **Key Goals of the Default Termination Policy:**

* Maintain **even distribution** across Availability Zones.
* Favor **older launch templates** for termination (to phase out old configurations).
* **Avoid early termination** to **maximize EC2 billing efficiency**.
* Use **random selection** only as a last resort to avoid bias.

---

---

### 🛡️ **AWS Security Groups – Key Concepts**

#### 🔸 What is a Security Group?

* A **virtual firewall** for EC2 instances to **control inbound and outbound traffic**.
* Acts **at the instance level**, **not at the subnet level**.
* Each instance can be associated with **up to five** security groups.

---

### 🚪 **Inbound and Outbound Rules**

* You can **specify rules based on protocol, port number, and source/destination IP range**.
* **/32 CIDR** notation is used to **allow a single IP address** (e.g., `203.0.113.7/32`).
* **SSH** (Secure Shell) uses:

  * **Protocol:** TCP
  * **Port:** 22
  * **Usage:** Secure remote login.

---

### 🔁 **Stateful Behavior of Security Groups**

* **Security groups are stateful**, meaning:

  * If you allow **inbound traffic**, **return traffic is automatically allowed**, even without an explicit outbound rule.
  * This simplifies configuration for protocols like SSH.

---

### ✅ **Example Scenario: Allowing a Single IP for SSH**

* **Goal:** Allow only **one specific client IP** to SSH into the EC2 instance.
* **Action:**

  * Create a **security group** with an **inbound rule**:

    * **Protocol:** TCP
    * **Port:** 22
    * **Source:** `X.X.X.X/32` (client’s IP)

---
---

### 🚀 **Canary Release Deployment – Overview**

* **Definition:** Releases the new version to a **small percentage of users** initially.
* **Purpose:** Monitor and verify functionality with real traffic.
* **Traffic Control:** Gradually **increase traffic** to the new version.
* **Rollback:** Quick and targeted (just shift traffic back to old version).
* **Usage in API Gateway:** Supported via **stage variables and deployment weights**.
* **Best For:**

  * Incremental, risk-controlled rollouts.
  * Gathering feedback on new features.
  * Critical production APIs.

---

### 🟢 **Blue-Green Deployment – Overview**

* **Definition:** You run two environments: **Blue (current)** and **Green (new)**.
* **Purpose:** Switch all traffic from Blue to Green at once (after validation).
* **Traffic Control:** **All-or-nothing** switch (though traffic shifting tools can add gradual shift).
* **Rollback:** Instant – revert DNS or load balancer to old environment.
* **Usage in API Gateway:** Achieved by creating a new stage or deployment and swapping them.
* **Best For:**

  * Non-critical updates with clear success criteria.
  * Full environment validation/testing before going live.
  * Environments with heavy infrastructure or database changes.

---

### ⚖️ **Canary vs Blue-Green: Comparison Table**

| Feature                      | **Canary Release**                              | **Blue-Green Deployment**                 |
| ---------------------------- | ----------------------------------------------- | ----------------------------------------- |
| **Traffic Shift**            | Gradual                                         | Instant (usually)                         |
| **Risk Level**               | Low (small % of users impacted)                 | Medium (entire switch affects all users)  |
| **Rollback Strategy**        | Easy (shift traffic back)                       | Easy (redirect to old environment)        |
| **Monitoring**               | Continuous during release                       | Mostly pre-release testing                |
| **Cost**                     | Slightly lower (same infra with staged traffic) | Higher (requires two full environments)   |
| **Best Use Cases**           | APIs, microservices, user-facing apps           | Large deployments, infrastructure updates |
| **Supported in API Gateway** | Yes (native support for canary deployments)     | Yes (via stages & routes)                 |

---

### ✅ **Which Is Better?**

**Canary Deployment is better when:**

* You need **granular traffic control**.
* **Monitoring and metrics** are in place to measure performance.
* You want to **minimize impact** and test with real users.
* You're releasing a new version of an **API or microservice**.

**Blue-Green Deployment is better when:**

* You require **full validation before going live**.
* You want a **clean rollback mechanism**.
* You can afford the cost of maintaining **two environments**.
* You're making **database or infrastructure-level** changes.

---

### 📝 Conclusion:

Use **Canary** for **progressive delivery and user feedback** in real-time environments like API Gateway.
Use **Blue-Green** for **safer, environment-isolated deployments** where testing everything ahead of time is a priority.

---

---

### 🔐 **Secret Encryption in Amazon EKS with AWS KMS**

* **Purpose:**
  To **secure sensitive data** (e.g., secrets, configuration) stored in the **etcd key-value store** of an EKS cluster.

* **Default Behavior:**
  Secrets in etcd are **not encrypted** by default, which poses a **security risk**.

* **Solution:**
  **Integrate AWS Key Management Service (KMS)** with your EKS cluster.

* **Benefits of Using AWS KMS:**

  * Manages **cryptographic keys** securely.
  * Provides **fine-grained access control** over key usage.
  * Supports **auditing** via CloudTrail.
  * Enables **key rotation, disabling, and lifecycle policies**.

* **Implementation Steps:**

  1. **Create a new KMS key** for encryption.
  2. **Enable secret encryption** on the EKS cluster using this KMS key.
  3. Configure the cluster to **encrypt secrets before saving them to etcd**.

* **Security Outcome:**

  * Secrets are encrypted **at rest** in etcd.
  * Ensures **data confidentiality and integrity**.
  * Helps meet **industry standards and compliance** (e.g., HIPAA, GDPR).

* **Key Insight:**
  Enabling secret encryption on an existing EKS cluster with a new AWS KMS key is essential for securing **sensitive Kubernetes data**.

---

✅ **Correct Answer Summary:**
Enable secret encryption with a new AWS KMS key on an existing Amazon EKS cluster to encrypt sensitive data stored in the EKS cluster’s etcd key-value store.

---

---

### 📚 **AWS Lake Formation – Key Concepts & Benefits**

* **Purpose:**
  AWS Lake Formation simplifies the process of **building, securing, and managing a data lake** quickly and at scale.

* **What is a Data Lake?**
  A centralized, curated, and secure **repository** that stores structured and unstructured data in its raw and prepared formats for analytics.

---

### 🧊 **Storage Layer: Amazon S3**

* Lake Formation uses **Amazon S3** as its **data lake storage layer**.
* You can:

  * **Register existing S3 buckets** with Lake Formation.
  * Or let Lake Formation **create new S3 buckets** for storing imported data.
* Data **remains in your AWS account**, and **you retain direct access**.

---

### 🔍 **Integration with AWS Glue**

* **AWS Glue Data Catalog** is used to:

  * Describe **available datasets**.
  * Define **schemas** and **business metadata**.
* Helps organize data for **querying, crawling, and transformation**.

---

### 🔐 **Security & Access Control**

* **Fine-grained access** to:

  * **Tables**, **columns**, **databases**.
* Uses **simple grant/revoke permissions** on **catalog objects** (not directly on S3).
* Supports access for:

  * IAM users, roles, and groups.
  * **Federated users** via **Active Directory integration**.

---

### ✅ **Correct Use Case / Answer**

> Use **AWS Lake Formation** to **consolidate data from multiple accounts** into a **single account** with **centralized access control**.

* Allows secure, cross-account access to data.
* Facilitates **centralized governance and auditing**.

---

---

### 🔐 **Redis AUTH – Security Enhancement**

* The `AUTH` command in Redis **requires a password** before executing any other Redis commands.
* It is a security feature to **restrict unauthorized access** to the Redis instance or cluster.

---

### 🚀 **How to Enable AUTH in AWS ElastiCache for Redis**

* When creating a Redis cluster or replication group, use:

  * `--auth-token` – Specifies the **password/token** that clients must use.
  * `--transit-encryption-enabled` – Ensures **TLS encryption in transit**, protecting data as it travels over the network.

> This combination enforces both **authentication** and **encryption**, providing a robust security posture.

---

### 📌 **Best Practices**

* Always **store the auth token securely** (e.g., in AWS Secrets Manager).
* Use **TLS encryption** to prevent data sniffing and man-in-the-middle attacks.
* Ensure all clients connecting to Redis are configured with:

  * The correct **auth token**.
  * Support for **TLS** if enabled.

---

### ✅ **Correct Answer Summary**

> Authenticate users using Redis AUTH by creating a new Redis Cluster with both the `--transit-encryption-enabled` and `--auth-token` parameters enabled.

This ensures:

* Password protection using `AUTH`.
* Encrypted communication via TLS.

---


---

### 📄 **AWS Artifact – Centralized Compliance Resource**

* **AWS Artifact** provides **on-demand access** to:

  * **Security and compliance reports**
  * **Online agreements** (e.g., BAA, NDA)

---

### 📊 **Common Reports Available in AWS Artifact**

* **SOC Reports** (Service Organization Control)
* **PCI Reports** (Payment Card Industry)
* **Certifications** from global compliance bodies

---

### 📜 **Available Agreements**

* **Business Associate Addendum (BAA)** – for HIPAA compliance
* **Nondisclosure Agreement (NDA)** – for confidential engagements

---

### 👥 **Access Control**

* **Root users** and **IAM users with admin permissions**:
  ✅ Full access to download reports and agreements
* **Non-admin IAM users**:
  🔒 Need explicit IAM permissions to access AWS Artifact
  🔐 Can restrict access to Artifact only without broader service access

---

### ✅ **Correct Answer Summary**

> Use **AWS Artifact** to view security reports and AWS compliance-related information.

This is ideal for audits, legal, security, and compliance teams needing validated documents for due diligence and regulatory requirements.

---
---

### Amazon FSx for NetApp ONTAP – Key Notes

* Fully managed AWS service with **NetApp ONTAP file system**.
* Supports **file protocols**: NFS, SMB.
* Supports **block protocol**: iSCSI.
* Compatible with **Windows, Linux, macOS**.
* Offers **Multi-AZ deployment** for high availability across Availability Zones.
* Provides **consistent sub-millisecond latency** with SSD storage.
* Suitable for **Windows Server workloads** requiring **low-latency block storage** via iSCSI.
* Simplifies **migration from on-premises NetApp systems**.
* Scales to **petabyte-scale datasets**.

---

### Why Use FSx for NetApp ONTAP for Trading Application?

* Multi-AZ file system ensures **continuous availability**.
* iSCSI protocol support enables **low-latency block storage access**.
* Perfect for Windows Server applications needing **shared block storage**.

---

### Why Other Options Are Not Suitable

* **FSx for Windows File Server:**

  * Supports SMB file shares.
  * Does **not support block storage or iSCSI**.
  * Cannot provide low-latency block storage required.

* **Amazon EFS:**

  * Designed for Linux workloads.
  * File storage only; no block storage support.
  * Not optimized for Windows or low-latency block access.

---

### Correct Solution Summary

* Deploy trading app on **EC2 Windows Server instances across two AZs**.
* Use **Amazon FSx for NetApp ONTAP Multi-AZ file system**.
* Access storage via **iSCSI protocol** for low-latency block storage.

---
---

### IAM Database Authentication – Key Notes

* Supports **MySQL** and **PostgreSQL** DB instances.
* Allows authentication **without using a database password**.
* Uses **authentication tokens** generated by Amazon RDS on request.
* Tokens are created with **AWS Signature Version 4**.
* Each authentication token is **valid for 15 minutes**.
* **No need to store database user passwords** in the DB; authentication managed externally by IAM.
* You can **still use standard database authentication** alongside IAM authentication.

---

### Benefits of IAM Database Authentication

* **Network traffic is encrypted** via SSL when connecting to the DB.
* Centralized access control using **IAM policies** instead of managing DB credentials on each instance.
* For applications on **Amazon EC2**, you can use the **EC2 instance profile credentials** (IAM role attached to EC2) to authenticate to the database securely without passwords.

---
---

### Temporary Credentials in AWS – Key Points

* **Use cases**: identity federation, delegation, cross-account access, IAM roles.
* **Enterprise identity federation** involves integrating corporate identities with AWS and enabling **Single Sign-On (SSO)**.

---

### Correct Solution Approach

1. **Set up a federation proxy or an identity provider (IdP)**

   * Integrate your enterprise identity system (e.g., Active Directory, SAML IdP) with AWS.
   * Use **AWS Security Token Service (STS)** to generate **temporary security tokens** for authenticated users.

2. **Configure IAM Role and IAM Policy**

   * Create an **IAM role** with permissions to access required AWS resources (e.g., S3 bucket).
   * Attach appropriate **IAM policies** to the role to define access scope.
   * Users assume this role temporarily using the temporary credentials provided by STS.

---

This setup allows seamless, secure access without managing long-term AWS credentials for users, enabling centralized access control and SSO.

---
Here are concise notes about **Partition Key Design and Provisioned Throughput in DynamoDB**:

---

### Partition Key and Throughput in DynamoDB – Key Points

* **Partition key** determines the **logical partitions** of a table’s data.
* Logical partitions map to **physical partitions**.
* **Provisioned I/O capacity** is **evenly divided among physical partitions**.
* Poor partition key design causing **uneven I/O distribution** leads to **"hot" partitions**.
* Hot partitions cause **throttling** and inefficient use of provisioned throughput.
* Optimal throughput usage depends on:

  * **Workload patterns** (access frequency of items).
  * **Partition key design** (distribution of accessed partition key values).
* You **do not need to access all partition keys** for efficiency.
* Efficiency improves as the **ratio of distinct partition key values accessed** to the **total partition keys increases**.
* More distinct partition key values accessed → more even spread of requests → better throughput utilization.

---
Here are the notes summarizing the key points about **Amazon Aurora endpoints and load balancing**:

---

### Amazon Aurora Endpoints – Key Points

* Aurora clusters consist of multiple DB instances, not a single instance.
* Connections use **endpoints** that abstract underlying instance hostnames.
* Endpoints handle **load balancing** and **failover** automatically.
* Types of endpoints:

  * **Primary endpoint**: Connects to the primary instance for DDL and DML operations (read-write).
  * **Reader endpoint**: Connects to all Aurora Replicas for read-only queries, with automatic load balancing.
  * **Custom endpoints**: Connect to a subset of instances based on specific criteria (e.g., instance class, parameter group).

---

### Use Case for Custom Endpoints

* Separate production traffic and reporting queries by:

  * Creating a **custom endpoint** for production workloads targeting high-capacity instances.
  * Creating another **custom endpoint** for reporting queries directed at lower-capacity instances.
* Custom endpoints allow **fine-grained control** over which instances serve which workloads.
* Enables **optimized resource usage** and improved performance isolation.

---

### Summary

* Use **custom endpoints** to route traffic based on instance capacity or configuration.
* This removes the need to hardcode hostnames or implement your own load balancing logic.

---
Here are the **notes** summarizing the key concepts and solution:

---

### 🔥 AWS Storage Types – Hot, Warm, and Cold

* **Hot Storage**:

  * Frequently accessed data
  * Requires **high performance** and **low latency**
  * Example: **Amazon FSx for Lustre**

* **Warm Storage**:

  * Moderately accessed data
  * Balanced between performance and cost
  * Example: **Amazon EFS (Elastic File System)**

* **Cold Storage**:

  * Rarely accessed data
  * **Low cost** to store, **higher cost** to retrieve
  * Example: **Amazon S3 with Glacier or Glacier Deep Archive**

---

### ✅ AWS Services Used

* **Amazon FSx for Lustre**

  * High-performance, parallel file system
  * Best for **hot storage** (frequent, fast data processing)
  * Ideal for **training datasets** and **concurrent processing**

* **Amazon S3**

  * Scalable object storage
  * Supports **cold storage** tiers:

    * S3 Standard-IA (Infrequent Access)
    * S3 Glacier
    * S3 Glacier Deep Archive
  * Best for **archived datasets** that are rarely accessed

---

### 🎯 Use Case & Solution

* **Requirement 1**: High-performance hot storage for training datasets
  → Use **Amazon FSx for Lustre**

* **Requirement 2**: Cost-effective cold storage for archived datasets
  → Use **Amazon S3** with appropriate cold storage tier

---

### ✅ Final Answer

> Use **Amazon FSx For Lustre** for hot storage and **Amazon S3** for cold storage.
---

Here are the **notes** for this use case:

---

### 🧠 **Invoking AWS Lambda from Amazon Aurora MySQL-Compatible Edition**

* **Purpose**: Integrate **Aurora MySQL** with other **AWS services** like **Lambda** and **SQS**
* **Use Case**: Trigger actions (e.g., notifications, data forwarding) when specific database operations occur

---

### 🛠️ **How It Works**

* **Aurora MySQL-Compatible Edition** supports invoking **AWS Lambda** directly using:

  * **Native functions**
  * **Stored procedures**

* This enables:

  * Capturing **data changes**
  * Triggering **event-driven workflows**

---

### 🧾 **Scenario Example**

* **Trigger**: A **listing** is deleted from the Aurora database
* **Action**:

  1. A **stored procedure/native function** calls a **Lambda function**
  2. The **Lambda function** sends the event to an **Amazon SQS queue**
  3. Downstream services consume the SQS messages for further processing

---

### ✅ **Correct Solution**

> **Create a native function or stored procedure** in Aurora MySQL that invokes an **AWS Lambda function**.
> Configure the **Lambda** function to **send event notifications** to an **Amazon SQS** queue.

---
Here are your **notes** for this scenario:

---

### 🧠 **Using AWS Lambda & API Gateway to Handle Bursts of Traffic**

---

### ✅ **Key Requirements**

1. **Users should access data via an API**
2. **Solution must handle sudden traffic bursts quickly**

---

### ⚙️ **Recommended Services**

#### ✅ **Amazon API Gateway**

* Front-door service to create, manage, and expose RESTful APIs
* Connects external users/apps to backend services (e.g., Lambda)

#### ✅ **AWS Lambda**

* **Serverless compute** service that scales automatically
* Handles **bursts of traffic** faster than EC2 Auto Scaling or Elastic Beanstalk
* Cold starts happen on first invocation, but **concurrent invocations** are handled by **provisioning more instances**
* **Initial burst concurrency**: 500–3000 per Region (depending on Region)

---

### 🚀 **Why Lambda for Burst Handling?**

* Scales **within seconds**, faster than launching EC2 instances
* Cost-efficient: **Pay only for execution time**
* AWS handles:

  * Provisioning
  * Scaling
  * High availability
* Supports **event-driven** and **on-demand** architectures

---

### 🏗️ **Architecture Flow**

```
Client → Amazon API Gateway → AWS Lambda → (Access data / DB / services)
```

---

### ✅ **Correct Approach**

> **Create an API using Amazon API Gateway** and **use AWS Lambda** to handle bursts of traffic efficiently and cost-effectively.

---
Great question! Let's break it down clearly:

---

## ✅ **When to Use Signed Cookies vs Signed URLs**

### 🧾 **Signed Cookies**

Use **Signed Cookies** when:

* You want to **restrict access to multiple files** (e.g., all video chunks in HLS/DASH streaming or a group of documents).
* You **don’t want to modify URLs** (e.g., existing pages or resources stay untouched).
* Your users **access content via browsers** (that support cookies).
* You want a **smoother user experience** — users get access once and all authorized resources are accessible without needing a new signed URL for each.

🟢 Example Use Cases:

* Video-on-demand platforms (accessing playlists and media segments)
* Member-only portals (subscribers’ area of a site)

---

### 🔗 **Signed URLs**

Use **Signed URLs** when:

* You want to **restrict access to individual files** (e.g., one-time software download).
* You’re using a **custom HTTP client or a client that doesn't support cookies**.
* You're distributing **RTMP content** (CloudFront RTMP distributions only support signed URLs).
* You want **more granular access control** — per-file, per-request, etc.

🟢 Example Use Cases:

* License-protected software downloads
* Temporary download links
* Secure access via mobile apps or IoT clients

---

## 🔐 **When to Use Field-Level Encryption**

Use **Field-Level Encryption** when:

* You want to **protect sensitive user data** in HTTP POST requests that go through CloudFront.
* The data includes things like **credit card numbers, personally identifiable information (PII)**, etc.
* You want **only your origin application to decrypt** certain fields, while CloudFront and intermediate services **cannot see** them.

🛡️ It uses **public-key encryption**, where:

* You configure CloudFront with your **public key** to encrypt specific fields.
* Only your origin (with the **private key**) can decrypt them.

🟢 Example Use Cases:

* Encrypting form submissions that contain sensitive fields like SSNs, passwords, or card numbers.
* Securing sensitive POST data in healthcare, finance, or compliance-heavy applications.

---

## 🔄 Summary

| Use Case                            | Signed URL | Signed Cookie | Field-Level Encryption |
| ----------------------------------- | ---------- | ------------- | ---------------------- |
| Single file access (e.g., PDF, ZIP) | ✅          | ❌             | ❌                      |
| Access multiple files at once       | ❌          | ✅             | ❌                      |
| Browser-based apps                  | ✅/✅        | ✅             | ✅                      |
| Custom client/no cookie support     | ✅          | ❌             | ❌                      |
| Protect sensitive form POST data    | ❌          | ❌             | ✅                      |
| Don't want to modify URLs           | ❌          | ✅             | ❌                      |
| Encrypt specific form fields (PII)  | ❌          | ❌             | ✅                      |

---
You're exactly right — your explanation concisely captures the **ideal multi-account strategy** on AWS. Here's a quick summary and reinforcement for clarity:

---

## ✅ **Correct Solution Breakdown**

### 1. **Use AWS Organizations**

* **Purpose**: Centralized account management.
* **Why**:

  * Consolidates billing.
  * Simplifies access control via **Service Control Policies (SCPs)**.
  * Enables governance at scale.
* **Key Features**:

  * Create/manage member accounts centrally.
  * Apply policies across groups of accounts (Organizational Units).

### 2. **Use AWS Resource Access Manager (RAM)**

* **Purpose**: Share AWS resources securely **across accounts** or within an organization.
* **Why**:

  * Avoids duplication of resources.
  * Reduces management overhead.
* **Supported Resources**:

  * Amazon VPC subnets
  * AWS Transit Gateway
  * Route 53 Resolver rules
  * License Manager configs
  * Resource shares can be scoped to:

    * Specific AWS accounts
    * Organizational Units
    * Entire Organization

---

## 🧠 **Example Use Case**

Imagine a **central networking team** managing VPC subnets and Transit Gateways:

* They set up these resources **in a centralized networking account**.
* Using **RAM**, they share the Transit Gateway and subnets with multiple **application accounts**.
* Using **Organizations**, the accounts are grouped into OUs like `Dev`, `Test`, and `Prod`, with policies to govern access.

This results in:

* **Better security and governance**
* **Reduced cost and duplication**
* **Simplified operations**

---

### ✅ Final Answer:

> **Consolidate all of the company's accounts using AWS Organizations. Use the AWS Resource Access Manager (RAM) service to easily and securely share your resources with your AWS accounts.**

---

Here are your notes based on the scenario:

---

### ✅ **Enforcing Encrypted EBS Volumes in AWS Organizations**

**Scenario:**
A company wants to ensure that **all new EC2 instances** launched in the **`ap-southeast-2`** Region use **encrypted EBS volumes**. The company uses **AWS Organizations** with **Service Control Policies (SCPs)** and wants **minimal disruption** to developers.

---

### ✅ **Recommended Steps:**

1. **Enable Default EBS Encryption in Each Account (Minimally Disruptive)**

   * ✅ **Action:**
     In the **Amazon EC2 Console**, enable the **"EBS encryption by default"** setting.
   * ✅ **Effect:**
     All newly created EBS volumes in the account will be encrypted automatically using the default KMS key.

2. **Enforce Encryption Using SCP**

   * ✅ **Action:**
     Create and attach an **SCP** to the **root OU** that **denies `ec2:CreateVolume`** if **`ec2:Encrypted` is false**.
   * ✅ **Example SCP Condition:**

     ```json
     {
       "Version": "2012-10-17",
       "Statement": [
         {
           "Sid": "DenyUnencryptedEBSVolumes",
           "Effect": "Deny",
           "Action": "ec2:CreateVolume",
           "Resource": "*",
           "Condition": {
             "BoolIfExists": {
               "ec2:Encrypted": "false"
             }
           }
         }
       ]
     }
     ```
   * ✅ **Effect:**
     Prevents creation of **unencrypted** EBS volumes, even if attempted manually.

---
