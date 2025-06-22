# ⚖️ Guide: CAP Theorem — A Systems Design Deep Dive

**CAP Theorem** describes the fundamental trade-offs in **distributed systems**. It states that a system can guarantee at most **two out of three** properties:

- **Consistency (C)**  
- **Availability (A)**  
- **Partition Tolerance (P)**

This guide explains what each term means, how real systems handle the trade-offs, and how to make architecture decisions based on CAP constraints.

---

## 🧠 1. What Is the CAP Theorem?

Formulated by **Eric Brewer** and proven in 2002, CAP says:

> **In the presence of a network partition, you must choose between consistency and availability.**

Meaning:  
You **cannot simultaneously guarantee** all three of:
- **Consistency**: All nodes see the same data at the same time.
- **Availability**: Every request receives a (non-error) response.
- **Partition Tolerance**: The system continues to function despite dropped or delayed messages between nodes.

---

## 🔍 2. Definitions
```
| Property         | Description                                                         |
|------------------|---------------------------------------------------------------------|
| **Consistency (C)** | Every read gets the most recent write or an error                 |
| **Availability (A)**| Every request receives a response (may be stale)                  |
| **Partition Tolerance (P)** | System continues to operate despite communication failure |
```
---

## 🔺 3. CAP Triangle

You can only pick **2 out of 3**:

```
         Consistency
          /       \
         /         \
    CP  /           \  CA
       /             \
      /               \
Availability ------- Partition Tolerance
               AP
```

- **CA**: Single-node systems or non-distributed databases
- **CP**: Prioritize consistency over uptime (e.g., databases with quorum reads/writes)
- **AP**: Prioritize availability, may return stale data

---

## ⚙️ 4. Real-World Interpretations
| Scenario                 | C | A | P | Example Systems                     |
|--------------------------|---|---|---|-------------------------------------|
| Relational DB (no partitions) | ✅ | ✅ | ❌ | MySQL on one node, SQLite          |
| MongoDB (eventual consistency) | ❌ | ✅ | ✅ | AP by default                      |
| HBase / Spanner (quorum) | ✅ | ❌ | ✅ | CP                                  |
| Cassandra / DynamoDB     | ❌ | ✅ | ✅ | Tunable AP                          |
| Zookeeper / Etcd         | ✅ | ❌ | ✅ | CP (election-based)                |

---

## 🧪 5. Network Partitions in Practice

A **partition** is a network failure between parts of the system — one server can't talk to another, but they're both still running.

- Can be caused by: switch failures, overloaded nodes, slow network, cloud zone outages
- When a partition happens, systems must **choose**:

  - Serve stale/partial data (Availability)
  - Reject requests until consensus (Consistency)

---

## 🔄 6. CP vs AP: What’s the Difference?

### ✅ CP (Consistency + Partition Tolerance)
- Every read reflects the latest write
- Writes blocked if quorum is not available

```
// Example: quorum-based write in CP
write(data) {
  sendToReplicas(data)
  wait for majority ack
  return success
}
```

✅ Predictable behavior  
❌ Less tolerant to latency or downtime

---

### ✅ AP (Availability + Partition Tolerance)
- Always responds (even if data is outdated)
- Consistency is eventually restored

```
// Example: AP behavior
write(data) {
  storeLocally(data)
  sync with peers later
}
```

✅ Highly available  
❌ May serve stale or conflicting data

---

## 🧱 7. Mitigating CAP Limitations

- Use **quorum reads/writes**: Wait for N/2+1 nodes to agree (tunable consistency)
- Combine **caching + retry logic** for better user experience
- Use **CRDTs** (Conflict-free Replicated Data Types) to merge state safely
- Design **bounded staleness**: allow limited delay in consistency

---

## 🔧 8. Tools and How They Handle CAP
| Tool / DB       | CAP Preference | Notes                                           |
|------------------|----------------|--------------------------------------------------|
| **Cassandra**    | AP (Tunable)   | Can configure consistency level per request     |
| **DynamoDB**     | AP             | Eventually consistent by default, strongly consistent option available |
| **Spanner**      | CP             | Global consistency using TrueTime API           |
| **Etcd / Zookeeper** | CP        | Used for coordination, must avoid split-brain   |
| **MongoDB**      | AP             | Defaults to eventual, can be made strongly consistent |
| **Redis Sentinel** | AP          | Asynchronous replication, brief inconsistency on failover |

---

## ❌ 9. Common Misconceptions
| Myth                                | Reality                                      |
|-------------------------------------|----------------------------------------------|
| "You can’t ever get all 3"          | You can get all 3 in **absence of partitions** |
| "Availability = uptime"             | CAP availability = always returns a response |
| "CAP explains everything"           | Other models (e.g. PACELC, BASE) offer better nuance |

---

## 🧠 10. Related Concepts

- **PACELC Theorem**: If Partitioned, choose A or C; Else, choose Latency or Consistency.
- **BASE systems**: Basically Available, Soft state, Eventually consistent
- **Consistency models**: Strong, Eventual, Causal, Monotonic

---

## ✅ Summary
| Property      | Description                                 |
|---------------|---------------------------------------------|
| Consistency   | All nodes return same, latest data          |
| Availability  | Always respond, even if stale               |
| Partition Tol.| Works during network failures               |
| Trade-off     | Pick **2 out of 3** when network partitions |

---

## 📚 Further Reading

- [CAP Theorem](https://www.bmc.com/blogs/cap-theorem/)
- [Designing Data-Intensive Applications (Book)](https://dataintensive.net)
- [PACELC Explained](https://en.wikipedia.org/wiki/PACELC_design_principle)
- [Dynamo: Amazon’s Highly Available Key-value Store (Paper)](https://www.allthingsdistributed.com/files/amazon-dynamo-sosp2007.pdf)
- [Jepsen Tests for Distributed Systems](https://jepsen.io/)

---
