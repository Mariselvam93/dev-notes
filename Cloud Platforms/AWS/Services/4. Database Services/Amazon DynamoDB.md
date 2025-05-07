## 🌐 **Amazon DynamoDB Overview**

Amazon **DynamoDB** is a **fully managed NoSQL database** service that provides **fast and predictable performance** with **seamless scalability**. It supports **key-value** and **document-based** data models and is commonly used in serverless and high-performance applications.

---

## 🔧 **1. Architecture**

DynamoDB is designed as a **distributed system** for **high availability and fault tolerance**:

* **Decentralized architecture** spread across **multiple Availability Zones (AZs)**.
* **Data partitioning** and **replication** are automatic.
* Uses **SSD storage**, **in-memory caching (DAX)**, and **stream processing (DynamoDB Streams)**.
* **Write-ahead logging** for durability and strong consistency options.

---

## 🗃️ **2. Data Model**

### 📋 Tables, Items, and Attributes

* **Table**: Container for data.
* **Item**: A single record in a table (analogous to a row).
* **Attribute**: A field in the item (analogous to a column).

### 📌 Primary Keys

There are two types of primary keys:

* **Partition key (hash key)**: Single attribute used to partition data.
* **Composite key (partition key + sort key)**: Enables multiple items with the same partition key.

Example:

```json
{
  "UserId": "123",
  "Timestamp": "2025-05-07T10:00:00Z",
  "Activity": "Login"
}
```

Primary Key: `UserId` (partition) + `Timestamp` (sort)

---

## 🔍 **3. Indexing**

### 📌 Local Secondary Index (LSI)

* Uses the same **partition key** as the base table but a **different sort key**.
* Must be defined at table creation time.
* Shares throughput with base table.

### 🌍 Global Secondary Index (GSI)

* Can have **different partition and sort keys**.
* Can be added **after table creation**.
* Has its own **provisioned throughput settings**.

Both allow **eventual or strong consistency** (LSIs allow strong reads; GSIs support only eventual consistency).

---

## 🧩 **4. Partitions**

DynamoDB **automatically splits data** into **partitions**:

* Each partition can hold up to **10 GB** of data.
* Read capacity: up to **3,000 RCU**; write capacity: up to **1,000 WCU** per partition.
* Based on **partition key's hash value**, so uneven key distribution causes **hot partitions** (performance bottlenecks).

---

## ⚖️ **5. Throughput Capacity Modes**

### 📐 Provisioned Mode

* Specify **read and write capacity units (RCU/WCU)**.
* Can **enable Auto Scaling**.
* Use **reserved capacity** for predictable workloads.

### ⚡ On-Demand Mode

* Pay-per-request model.
* Automatically scales.
* Ideal for **unpredictable** or **new workloads**.

---

## 🔁 **6. DynamoDB Streams**

* **Time-ordered sequence** of item-level modifications (INSERT, MODIFY, REMOVE).
* Retains records for **24 hours**.
* Integrated with **AWS Lambda** for **event-driven architectures**.

Use cases:

* Change data capture (CDC)
* Real-time analytics
* Cross-table synchronization

---

## 🌍 **7. Global Tables**

* **Multi-Region, fully active** replication.
* Supports low-latency reads/writes in multiple regions.
* Useful for **disaster recovery**, **global apps**, and **resilient architectures**.

---

## 🔐 **8. Security**

### ✅ Encryption

* At rest: **AES-256**, using **KMS-managed keys**.
* In transit: **TLS** encryption.

### ✅ IAM (Identity and Access Management)

* Fine-grained permissions with **IAM policies**.
* Table-level, item-level, and attribute-level access control.

### ✅ VPC Endpoints

* Use **interface endpoints** to securely access DynamoDB without traversing the public internet.

---

## 🧰 **9. Backup and Restore**

* **On-Demand Backup**: Manual backups with no impact on performance.
* **Point-in-Time Recovery (PITR)**: Restores to any second in the last 35 days.

---

## 💰 **10. Pricing**

Depends on:

* **Read/Write capacity mode** (provisioned vs. on-demand)
* **Data storage**
* **Indexes** (GSI adds cost)
* **Streams**, **backup**, **restore**
* **Data transfer**, especially with **Global Tables**

---

## 🆚 **11. DynamoDB vs. Other AWS Databases**

| Feature      | DynamoDB                        | Amazon RDS                    | Amazon Aurora                         |
| ------------ | ------------------------------- | ----------------------------- | ------------------------------------- |
| Type         | NoSQL                           | Relational (SQL)              | Relational, MySQL/Postgres-compatible |
| Schema       | Schema-less                     | Schema-based                  | Schema-based                          |
| Scalability  | Auto scaling, highly scalable   | Manual or Aurora Auto Scaling | Aurora Serverless or manual           |
| Transactions | Supported (ACID, limited scope) | Full ACID transactions        | Full ACID transactions                |
| Use Cases    | IoT, gaming, mobile apps        | ERP, CRM, legacy apps         | SaaS, enterprise apps                 |
| Pricing      | Pay-per-request or provisioned  | Instance-based                | Instance-based                        |

---

## ✅ **12. Best Practices**

* Use **uniform partition keys** to avoid hot partitions.
* Choose **On-Demand** for unpredictable workloads; **Provisioned** for stable ones.
* Use **DAX** for caching frequently read items.
* Enable **PITR** and **on-demand backups**.
* Use **GSIs** wisely—avoid excessive write amplification.
* Implement **fine-grained IAM policies**.

---

## 🚀 **13. Performance Optimization**

* Use **parallel scans** for faster full-table scans.
* Use **BatchGetItem** instead of multiple GetItem calls.
* Keep item size small (max 400 KB).
* Compress large blobs or store in **S3**, keep metadata in DynamoDB.
* Use **DAX** (DynamoDB Accelerator) to reduce latency.
* Avoid **sequential partition keys** (e.g., timestamps).

---

## 💼 **14. Real-World Use Cases**

* **Netflix**: Real-time recommendation engine metadata.
* **Airbnb**: Configuration and metadata storage.
* **Lyft**: User sessions and dynamic configuration.
* **Gaming apps**: Leaderboards, session state.
* **IoT**: Device telemetry, configuration.

---

## 🛠️ **15. Troubleshooting Techniques**

| Issue                   | Possible Causes                              | Resolution                                    |
| ----------------------- | -------------------------------------------- | --------------------------------------------- |
| **Throttling**          | Exceeded RCU/WCU or hot partition            | Use Auto Scaling, distribute partition keys   |
| **Slow response times** | Missing indexes, cold starts, large payloads | Optimize queries, add GSI/LSI, enable DAX     |
| **Provisioned limits**  | Hitting max throughput or partitions         | Switch to On-Demand or request limit increase |
| **Data inconsistency**  | Using eventual consistency                   | Use strongly consistent reads if needed       |

---

## 📝 **Exam Tips (AWS SAA-C03)**

* Understand **primary key types**, **indexing**, and **partition behavior**.
* Be able to choose between **On-Demand** and **Provisioned** throughput.
* Know **integration with Lambda** via **DynamoDB Streams**.
* Distinguish use cases for **DynamoDB vs. RDS/Aurora**.
* Understand **encryption**, **IAM policies**, and **VPC endpoints**.
* Recognize when to use **Global Tables** and **PITR**.

---
