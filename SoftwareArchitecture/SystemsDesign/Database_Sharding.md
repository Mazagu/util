# 🧩 Guide: Database Sharding — A Systems Design Deep Dive

**Database sharding** is a horizontal partitioning technique where large databases are split into smaller, faster, more manageable **"shards"** that are spread across multiple servers or nodes.

This is a foundational strategy for scaling **read/write-heavy systems**, used by platforms like Twitter, Amazon, and Facebook.

---

## 🧠 1. What Is Sharding?

Sharding = **splitting a large dataset** across **multiple databases or storage systems**, each handling only a subset of the total data.

Each shard is a **self-contained unit** responsible for a **portion** of the data.

### Sharding vs Partitioning
```
| Aspect         | Sharding                            | Partitioning (in one DB)     |
|----------------|--------------------------------------|------------------------------|
| Level          | Across nodes                         | Inside a single DB server    |
| Scope          | Horizontal scale-out                 | Logical organization         |
| Used for       | Scaling and distribution             | Query performance, archival  |
```
---

## ⚙️ 2. Why Shard?

- **Horizontal scalability**: Handle large volumes of data and traffic
- **Parallelism**: Each shard handles its own read/write load
- **Fault isolation**: Failure in one shard ≠ total failure
- **Data locality**: Can co-locate data near users (geo-sharding)

---

## 🧭 3. Sharding Strategies

### 1️⃣ **Hash-Based Sharding**
Use a **hash of a key** (e.g. user ID) to assign to a shard.

```
shard_id = hash(user_id) % num_shards
```

✅ Simple and uniform  
❌ Hard to rebalance when adding/removing shards

---

### 2️⃣ **Range-Based Sharding**
Assign data based on a value **range**.
```
| Shard | User ID Range |
|-------|---------------|
| 1     | 1–1M          |
| 2     | 1M–2M         |
| 3     | 2M–3M         |
```
✅ Good for queries by range  
❌ Risk of **hotspots** (e.g., last shard gets all new users)

---

### 3️⃣ **Directory-Based (Lookup Table)**
Use a **central directory or metadata service** to track where each record lives.

```
getShard(user_id) → lookup in user_shard_map
```

✅ Dynamic and flexible  
❌ Extra network hop; central point of failure unless replicated

---

### 4️⃣ **Geo-Sharding**
Shard based on **location or geography** (e.g. US, EU, Asia).

✅ Complies with regulations (e.g. GDPR), improves latency  
❌ Hard to rebalance globally, cross-region queries are expensive

---

## 🗺️ 4. Data Routing

How does your application know **which shard to query**?

- Client-based routing: Client calculates shard (`hash(id) % n`)
- Proxy-based routing: Intermediate layer routes based on metadata
- Middleware/router (e.g. Vitess, Citus) intercepts and distributes queries

---

## 🧹 5. Rebalancing & Resharding

When you add or remove shards, you may need to **reshuffle data**:

- **With hash sharding**: Change in `num_shards` means **all keys must be remapped**
- **Solution**: Use **consistent hashing**

```
ring = HashRing(shards)
shard = ring.get_node(user_id)
```

- **Online Resharding Tools**: Vitess, Facebook's Hydra, Instagram’s Shard Manager

---

## 🏗️ 6. Sharding Architectures

### Shared Nothing
Each shard has its own DB server, schema, and hardware.

✅ Fully independent  
❌ More operational complexity

### Shared Schema
All shards use the same schema; each handles only part of the data.

✅ Easy to replicate/scale  
❌ Requires careful query scoping

---

## 🧪 7. Querying Sharded Databases
```
| Query Type         | Strategy                              |
|--------------------|----------------------------------------|
| **Point lookup**   | Route directly to correct shard        |
| **Range query**    | Might hit multiple shards              |
| **Cross-shard join** | Avoid! Or pre-join in application    |
| **Aggregation**    | Perform in parallel, merge in app layer|
```
```
// Application-side fan-out
for (shard in shards) {
  results += query(shard, "SELECT COUNT(*) FROM orders")
}
return sum(results)
```

---

## 🔒 8. Consistency, Failover & Replication

- Each shard should have **replication** (primary + replicas)
- May use **eventual consistency** for performance
- Failover and monitoring per shard

Tools like **Vitess**, **Citus**, and **CockroachDB** handle this automatically.

---

## ⚠️ 9. Sharding Gotchas
```
| Problem                      | Notes                                     |
|-----------------------------|--------------------------------------------|
| Cross-shard joins           | Expensive, often need application logic    |
| Rebalancing difficulty      | Hard to add shards without downtime        |
| Operational complexity      | Monitoring, backup, failover per shard     |
| Hotspots                    | Uneven traffic on certain shards           |
| Global uniqueness           | Must generate unique IDs across shards     |
```
### 🔑 Solutions

- Use **UUIDs or Snowflake IDs** to ensure global uniqueness
- Implement **query routers or gateways**
- Use **application-level join** if needed

---

## 🧰 10. Tools & Technologies
```
| Tool           | Description                        |
|----------------|------------------------------------|
| **Vitess**     | MySQL sharding middleware, used by YouTube |
| **Citus**      | PostgreSQL extension for sharding  |
| **CockroachDB**| Globally distributed, auto-sharding |
| **MongoDB**    | Native sharding via config servers |
| **Cassandra**  | Peer-to-peer, partitioned by key   |
```
---

## 🧠 11. When to Use Sharding

✅ Use sharding when:
- Your DB size or write traffic exceeds what a single machine can handle
- You’ve already optimized indexing, caching, and vertical scaling
- You need high availability across regions or fault domains

🚫 Avoid premature sharding — it's **complex** and **operationally expensive**

---

## 📚 Further Reading

- [Sharding at Instagram (Engineering Blog)](https://instagram-engineering.com/sharding-ids-at-instagram-1cf5a71e5a5c)
- [Vitess Overview](https://vitess.io/)
- [Citus Sharding for Postgres](https://docs.citusdata.com/)
- [High Scalability: YouTube's Architecture](http://highscalability.com/youtube-architecture/)
- [Google Spanner vs Sharded DBs](https://cloud.google.com/spanner/docs)

---
