# 🧠 Data Management Patterns

Data management patterns help architects and engineers design systems that are **scalable**, **efficient**, and **maintainable** by defining proven strategies for handling data storage, retrieval, consistency, and performance.

This guide covers essential data patterns used across distributed systems, analytics platforms, and high-throughput applications.

---

## 🧊 1. Cache Aside

### 🧠 Concept

The application **checks the cache first**. On a **cache miss**, it **loads data from the database**, stores it in the cache, and returns it to the caller.

```
// Pseudocode
if (cache.has(key)) {
  return cache.get(key);
} else {
  data = db.query(key);
  cache.set(key, data);
  return data;
}
```

### ✅ Use Cases
- High-read, low-write scenarios
- APIs with frequently accessed data
- Microservices using Redis or Memcached

### ⚠️ Caveats
- Must handle cache invalidation carefully
- Risk of stale data if not synchronized

---

## 🪟 2. Materialized View

### 🧠 Concept

A **Materialized View** is a **precomputed result** of a SQL query stored on disk. It can be periodically refreshed to reflect changes.

```
// PostgreSQL example
CREATE MATERIALIZED VIEW active_users AS
SELECT id, name FROM users WHERE status = 'active';
```

### ✅ Use Cases
- BI/Analytics dashboards
- Aggregation-heavy workloads
- Read-optimized architectures

### ⚠️ Caveats
- Needs refresh logic (manual or scheduled)
- Storage overhead

---

## 🪓 3. CQRS (Command Query Responsibility Segregation)

### 🧠 Concept

**Separate models** for:
- **Commands**: create/update/delete (write)
- **Queries**: read and fetch data

```
// Commands write to DB
POST /users → write store

// Queries use denormalized read store
GET /users → read store
```

### ✅ Use Cases
- Systems with vastly different read/write loads
- Event-driven or eventual consistency systems
- Real-time dashboards vs transactional writes

### ⚠️ Caveats
- Added complexity in syncing data
- Often paired with Event Sourcing

---

## 🌀 4. Event Sourcing

### 🧠 Concept

Instead of storing the latest state, **store all changes (events)** as a log. Rebuild state by **replaying events**.

```
// Event log example
UserCreated {id: 1}
UserNameChanged {id: 1, name: "Bob"}
UserVerified {id: 1, verified: true}
```

### ✅ Use Cases
- Systems requiring audit trails
- Complex domains (financial, logistics)
- Rollbacks, replays, debugging

### ⚠️ Caveats
- High learning curve
- Querying current state requires rebuilding
- Event versioning and storage management

---

## 🧮 5. Index Table

### 🧠 Concept

A manually maintained table optimized for a specific query pattern. Acts as a **custom secondary index**.

```
// Example: speeding up user lookup by email
CREATE TABLE email_index (
  email TEXT PRIMARY KEY,
  user_id INT
);
```

### ✅ Use Cases
- NoSQL or denormalized systems
- Read-heavy queries on non-primary fields
- Avoiding full table scans

### ⚠️ Caveats
- Needs consistency logic (write-through, eventual)
- Duplicated data

---

## 🧩 6. Sharding

### 🧠 Concept

**Split large datasets** into smaller subsets (“shards”) distributed across multiple servers or databases.

```
// Shard users by user ID modulus
user_id % 4 → shard_0 to shard_3
```

### ✅ Use Cases
- Horizontal scaling
- Geo-based partitioning
- Large, multi-tenant systems

### ⚠️ Caveats
- Complex query routing
- Cross-shard joins are hard
- Rebalancing shards is non-trivial

---

## 📂 Other Patterns to Know

### 🔁 Read Replicas
Use replica nodes for **read traffic**, keeping the master for **writes**. Good for scaling reads and improving latency.

### 💾 Write-Behind Caching
Update the cache first, and write to the database asynchronously.

### 🕒 TTL/Expiry Caching
Automatically remove data after a defined lifetime, useful for temporary or fast-changing content.

---

## 🔁 Summary Comparison

| Pattern           | Optimized For          | Complexity | Notes                                       |
|------------------|------------------------|------------|---------------------------------------------|
| Cache Aside       | Read latency           | Low        | Simple, needs cache invalidation            |
| Materialized View | Aggregation queries    | Medium     | High speed reads, needs refresh strategy    |
| CQRS              | Scalability, separation| High       | Often paired with Event Sourcing            |
| Event Sourcing    | Auditability, replay   | High       | Rebuild state from events                   |
| Index Table       | Fast lookup            | Medium     | Denormalized data, requires sync logic      |
| Sharding          | Scalability            | High       | Partitioning for large-scale systems        |

---

## 📚 Further Reading

- [Martin Fowler: CQRS](https://martinfowler.com/bliki/CQRS.html)
- [Event Sourcing by Greg Young](https://cqrs.wordpress.com/)
- [Redis Caching Patterns](https://redis.io/docs/)
- [PostgreSQL Materialized Views](https://www.postgresql.org/docs/current/rules-materializedviews.html)
- [Designing Data-Intensive Applications by Martin Kleppmann](https://dataintensive.net/)

---
