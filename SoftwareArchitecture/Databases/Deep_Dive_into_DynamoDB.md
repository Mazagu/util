# ⚡ Deep Dive into DynamoDB: Design, Modeling, and Best Practices

**Amazon DynamoDB** is a fully managed NoSQL key-value and document database that delivers single-digit millisecond performance at any scale. It is serverless, supports automatic scaling, built-in security, and multi-region replication.

---

## 🧠 Core Concepts

### 🔹 Tables, Items, and Attributes

- **Table**: Collection of data
- **Item**: Single row of data (like a document)
- **Attribute**: Field within an item (like a column)

```
// Sample item in DynamoDB (JSON format)
{
  "UserId": "user-123",
  "Name": "Alice",
  "Age": 30,
  "Email": "alice@example.com"
}
```

---

### 🔹 Primary Key

Every item must have a **primary key**, which can be:

- **Partition key only** (simple key)
- **Partition key + Sort key** (composite key)

The partition key determines the **data distribution** across storage partitions.

```
// Example composite key
Partition Key: "UserId" = "user-123"
Sort Key: "OrderDate" = "2024-10-10"
```

---

### 🔹 Secondary Indexes

- **Global Secondary Index (GSI)**: Query using non-primary key attributes
- **Local Secondary Index (LSI)**: Adds sort key to the existing partition key

Indexes must be **defined at table creation (LSI)** or can be **added later (GSI)**.

---

### 🔹 Read and Write Capacity Modes

1. **On-Demand**: Auto-scales based on usage (pay per request)
2. **Provisioned**: Manually set read/write units (cheaper with predictable load)

```
// Provisioned example
Read Capacity Units (RCU) = 5
Write Capacity Units (WCU) = 10
```

Use **DAX (DynamoDB Accelerator)** to cache reads and improve performance.

---

## 📊 Data Modeling in DynamoDB

DynamoDB is **not relational** — it's designed for **access patterns**.

### ✅ Design Principles

- **One table per application** (single-table design)
- Predefine your query patterns
- Use compound keys and GSIs to shape access
- Store denormalized or nested data when appropriate

```
// Example item with embedded data
{
  "UserId": "user-123",
  "Type": "Profile",
  "Profile": {
    "Name": "Alice",
    "Email": "alice@example.com"
  }
}
```

> ⚠️ Avoid joins. You query based on access patterns, not relations.

---

## 🧪 Querying & Scanning

### 📌 Query

Efficient — uses partition key and optional sort key conditions.

```
aws dynamodb query \
  --table-name Users \
  --key-condition-expression "UserId = :uid" \
  --expression-attribute-values '{":uid":{"S":"user-123"}}'
```

### 📌 Scan

Inefficient — reads all items. Use only for small datasets or batch jobs.

---

## 🔐 Security & Access Control

- **IAM policies**: Grant fine-grained access control
- **Encryption at rest**: AWS KMS integrated
- **Encryption in transit**: Enabled by default
- **VPC endpoints**: Secure internal traffic

```
// IAM example: Allow read-only access
{
  "Effect": "Allow",
  "Action": [
    "dynamodb:GetItem",
    "dynamodb:Query",
    "dynamodb:Scan"
  ],
  "Resource": "arn:aws:dynamodb:region:acct-id:table/Users"
}
```

---

## ♻️ Streams and Triggers

Enable **DynamoDB Streams** to capture item changes (insert, update, delete).

- Stream events can trigger **AWS Lambda**
- Used for audit logging, replication, and downstream processing

```
// Event structure (example)
{
  "eventName": "INSERT",
  "dynamodb": {
    "Keys": { "UserId": { "S": "user-123" } },
    "NewImage": { "Name": { "S": "Alice" } }
  }
}
```

---

## 🚦 Best Practices

| Practice                          | Reason                                         |
|----------------------------------|------------------------------------------------|
| Design access patterns first     | Helps with schema design in NoSQL             |
| Prefer `Query` over `Scan`       | Better performance and cost                   |
| Avoid hot partitions              | Use high-cardinality partition keys           |
| Use DAX for heavy-read workloads | In-memory caching layer                       |
| Use TTL                          | Automatically delete expired data             |
| Monitor with CloudWatch          | Track latency, throttles, WCU/RCU usage       |

---

## 🔥 When to Use DynamoDB

- Applications with **predictable access patterns**
- **Serverless apps** using AWS Lambda
- Workloads needing **massive scale with low latency**
- Use cases: IoT, gaming, real-time analytics, chat apps

---

## 🛑 When Not to Use

- Complex relational joins or multi-table transactions
- Heavy ad-hoc queries
- High write throughput without thoughtful partitioning

---

## 📚 Resources

- [DynamoDB Docs](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/Introduction.html)
- [DynamoDB Data Modeling](https://www.alexdebrie.com/)
- [AWS DynamoDB CLI Reference](https://docs.aws.amazon.com/cli/latest/reference/dynamodb/)
- [NoSQL Design Patterns](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/bp-general-nosql-design.html)

---
