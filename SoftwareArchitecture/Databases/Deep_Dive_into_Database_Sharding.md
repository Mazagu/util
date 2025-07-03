# 🧩 Deep Dive into Database Sharding

**Database Sharding** is a technique for **partitioning large databases** into smaller, faster, and more manageable pieces called **shards**. It is a critical concept in **scaling databases horizontally**, especially for high-traffic or large-volume systems.

---

## 📌 What Is Sharding?

Sharding is a form of **horizontal partitioning** where data is distributed across multiple databases or nodes, each of which holds a portion of the dataset. Each **shard** operates as an independent database instance but collectively makes up the full system.

> Think of sharding as breaking up a huge table into smaller tables, each hosted on a different server.

---

## 🧠 Why Shard?

- 🚀 **Performance**: Reduces query latency by reducing dataset size per node
- ⚖️ **Scalability**: Allows linear scaling by adding more shards
- 🧱 **Fault Isolation**: Failure in one shard doesn’t bring down the entire system
- 💰 **Cost Efficiency**: Enables use of commodity hardware over vertically scaling a single machine

---

## 🧬 Types of Sharding

### 1. **Horizontal Sharding (Row-based)**

Data is divided by rows. For example, users A–F go to one shard, G–M to another.

✅ Most common  
✅ Balances load across nodes

### 2. **Vertical Sharding (Functional/Service-based)**

Each shard contains different tables (or microservice ownership). Example:

- Shard 1: user, profile  
- Shard 2: payments, invoices

✅ Matches well with service boundaries  
⚠️ Can lead to cross-shard joins

### 3. **Directory-based Sharding**

A lookup table is used to map keys to shards.

```
// Directory table example
user_shard_map = {
  'user_1': 'shard_1',
  'user_2': 'shard_2'
}
```

✅ Maximum flexibility  
⚠️ Adds metadata overhead

---

## 🔁 How Are Requests Routed?

Routing is the process of determining which shard a request belongs to.

| Method               | Description |
|----------------------|-------------|
| **Middleware Routing** | App queries a proxy/middleware that knows the sharding rules |
| **Client-side Routing** | Application code uses a sharding function to select the shard |
| **Database Proxy**    | Tools like Citus or Vitess act as distributed routers |

---

## 🧩 Choosing the Shard Key

The **shard key** determines how data is split across shards.

| Strategy            | Example        | Pros | Cons |
|---------------------|----------------|------|------|
| **Hash-based**      | `hash(user_id) % N` | Good distribution | Hard to range query |
| **Range-based**     | `user_id < 1000` | Great for sequential access | Risk of uneven distribution |
| **Geo-based**       | Region-specific sharding | Localized access | Skewed traffic risk |
| **Entity-based**    | User ID, Tenant ID | Natural separation | Must avoid hotspot keys |

🧠 **Good Shard Keys Should:**

- Evenly distribute data
- Keep data locality (minimize cross-shard joins)
- Be immutable (avoid changing shard key)

---

## ⚙️ Technical Considerations

### 💥 Cross-Shard Joins

Querying across shards can be expensive and complex. Avoid joins across shards when possible, or use data denormalization.

### 🔄 Rebalancing and Resharding

Adding/removing shards often requires **resharding**, a complex operation that includes:

- Moving data between nodes
- Updating routing logic
- Ensuring zero downtime during migration

Some systems support **online resharding** (e.g., Vitess, MongoDB with resharding features).

### 💾 Backup and Restore

Backups must be coordinated across shards for consistency. Use snapshot mechanisms or write-ahead logging.

### 🔐 Transactions

Distributed transactions across shards are complex. Use:

- **Two-phase commit** (expensive and slow)
- **Application-level coordination**
- Or avoid cross-shard transactions entirely

---

## 🛠️ Real-World Sharding in Practice

### ✅ MongoDB

- Supports sharding out of the box  
- Uses **config servers** to track metadata  
- Shard key selection is critical to avoid performance problems

### ✅ Twitter (MySQL)

- Initially sharded MySQL by user ID  
- Had to re-architect due to poor shard key and uneven load

### ✅ Facebook

- Uses **logical sharding** based on user IDs  
- Writes are sharded, reads are heavily cached and replicated

### ✅ Shopify

- Sharded by shop ID (tenant-based model)  
- Uses PostgreSQL with custom routing logic

---

## 📊 When Should You Shard?

| ✅ Shard When…           | 🚫 Avoid Sharding If… |
|--------------------------|------------------------|
| Dataset doesn't fit one node | Dataset is small |
| Write/read throughput is high | Simpler replication suffices |
| Multi-tenant or region-based | Team lacks operational maturity |
| You need horizontal scale | Scaling vertically is cheaper |

---

## 🧠 Alternatives to Sharding

- **Read Replicas**: Scale reads, not writes
- **Partitioning (within DB)**: Native table partitions
- **CQRS + Event Sourcing**: Write-heavy systems

---

## 📚 Resources

- [MongoDB Sharding Docs](https://www.mongodb.com/docs/manual/sharding/)
- [Vitess: Scalable MySQL Sharding](https://vitess.io/)
- [Designing Data-Intensive Applications](https://dataintensive.net) – Chapter on sharding
- [Twitter’s Shard Manager](https://blog.twitter.com/engineering/en_us/topics/infrastructure/2019/shard-manager)

---

## ✅ Summary

| Feature             | Description |
|---------------------|-------------|
| Scaling Type         | Horizontal |
| Primary Benefit      | Performance + Scalability |
| Major Challenge      | Routing, rebalancing, cross-shard ops |
| Use Cases            | Multi-tenant SaaS, social networks, IoT, e-commerce |
| Key Risk             | Choosing a bad shard key |

---
