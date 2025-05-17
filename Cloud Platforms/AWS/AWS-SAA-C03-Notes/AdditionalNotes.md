Here's your AWS SAA-C03 study notes properly formatted in **Markdown**, with enhancements, missing keywords, and additional context added for each item.

---

# 🧠 AWS SAA-C03 Study Notes

---

## 🔧 DHCP Options Set

* **Key Terms**: Domain Name Servers (DNS), Domain Name, NTP Servers, NetBIOS name server.
* **Use Case**: Used to define custom DNS settings for instances in a VPC.
* **Association**: Can be associated with one or more VPCs.
* **Example**: Assigning internal domain names (e.g., `corp.internal`) to instances.

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
