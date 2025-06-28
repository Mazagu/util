# 🔁 Data Consistency Models

Data consistency defines **how and when** changes made to data become visible to users or systems. In distributed systems, ensuring the right **balance between performance, availability, and correctness** is critical.

This guide dives into **strong, eventual, causal**, and other consistency models, when to use them, trade-offs, and examples.

---

## 📌 Why Consistency Matters

In a distributed system, data is often **replicated across nodes** to improve availability and fault tolerance. Consistency models determine **what guarantees** a system provides when data is read and written across these nodes.

---

## 💪 Strong Consistency

### 🧠 Definition

A system is **strongly consistent** if **all reads reflect the most recent write**. As soon as a write is confirmed, all nodes and clients will see that new value.

```
// Write operation
PUT /users/123 { "email": "new@example.com" }

// Subsequent reads always return:
GET /users/123 → { "email": "new@example.com" }
```

### ✅ Use Cases
- Financial transactions (banking)
- Inventory systems
- Critical state synchronization

### ⚠️ Trade-offs
- Higher latency
- Lower availability during network partitions (CAP theorem)

---

## ⏳ Eventual Consistency

### 🧠 Definition

**Eventually consistent** systems allow replicas to be **temporarily inconsistent** after a write, but guarantee that all replicas will **converge to the same value over time**.

```
// Client A writes new value
PUT /profile → name: "Alice"

// Client B might see the old value briefly
GET /profile → name: "Alicia"

// Later...
GET /profile → name: "Alice"
```

### ✅ Use Cases
- Social media feeds
- Shopping cart updates
- IoT and telemetry data

### ⚠️ Trade-offs
- Temporary inconsistencies
- Complex conflict resolution in concurrent updates

---

## 🔗 Causal Consistency

### 🧠 Definition

**Causal consistency** preserves the **order of related operations**. If operation A causally affects operation B, then any node that sees B must also see A.

```
// Example causal sequence
1. Alice posts a message
2. Bob replies to Alice's message

// Everyone who sees (2) will have seen (1)
```

### ✅ Use Cases
- Collaborative tools (Google Docs, chat)
- Real-time apps needing logical ordering

### ⚠️ Trade-offs
- More complex implementation
- Slightly higher latency than eventual consistency

---

## 🧊 Read-Your-Writes Consistency

### 🧠 Definition

After a client performs a **write**, it is **guaranteed to see that change** in subsequent reads.

```
// Alice updates her profile picture
PUT /user/123/avatar → "new.png"

// Alice immediately sees the new avatar
GET /user/123/avatar → "new.png"
```

### ✅ Use Cases
- User dashboards
- Session-aware applications

---

## 🎛️ Monotonic Reads & Writes

- **Monotonic Reads**: Once a client reads a value, it will **never read an older version** afterward.
- **Monotonic Writes**: Writes from a client are **executed in order** on all replicas.

---

## 🔍 CAP Theorem and Consistency

**CAP Theorem** states that in the presence of a **Partition (network failure)**, a system can choose only **Consistency** or **Availability**, but not both.

| Model                 | Guarantees     | Availability | Latency  |
|-----------------------|----------------|--------------|----------|
| Strong Consistency    | All replicas are in sync immediately | ❌ Lower | ❌ Higher |
| Eventual Consistency  | Syncs over time | ✅ High | ✅ Lower |
| Causal Consistency    | Preserves logical ordering | ⚠️ Medium | ⚠️ Medium |

---

## 🗂️ Examples from Real Systems

| System           | Consistency Model     |
|------------------|------------------------|
| **Relational DBs (PostgreSQL, MySQL)** | Strong (ACID)             |
| **DynamoDB**      | Tunable (Strong or Eventual) |
| **Cassandra**     | Eventual, Tunable consistency |
| **MongoDB**       | Tunable (read/write concerns) |
| **Redis Cluster** | Eventual (replication lag)    |
| **CockroachDB**   | Serializable (Strong)         |

---

## ⚙️ Practical Considerations

- **User experience matters**: A "wrong" value for a few milliseconds might be acceptable in most apps.
- **Tunable consistency**: Some databases allow you to choose per operation (`read consistency level`, `write concern`).
- **Conflict resolution**: CRDTs (conflict-free replicated data types) and operational transforms help preserve intent in eventual models.

---

## 📚 Further Reading

- [Amazon Dynamo Paper (2007)](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
- [Consistency Models in Distributed Systems – Jepsen](https://jepsen.io/)
- [Martin Kleppmann – DDIA](https://dataintensive.net/)
- [Cassandra Consistency Explained](https://cassandra.apache.org/doc/latest/architecture/dynamo.html)

---
