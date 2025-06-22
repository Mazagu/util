# 🔄 Guide: Consistent Hashing — A Systems Design Deep Dive

**Consistent hashing** is a key technique in system design for **evenly distributing load across dynamic server nodes** without needing to reassign all keys when the cluster topology changes.

It is widely used in:
- **Distributed caches** (e.g., Memcached, Redis)
- **Sharded databases** (e.g., Cassandra, DynamoDB)
- **Load balancers**
- **Distributed file systems**

---

## 🎯 1. The Problem with Naive Hashing

Let’s say you distribute keys using:
```
node = hash(key) % N
```

❌ When `N` (the number of servers) changes, **almost all keys are remapped**, which causes:
- Cache misses
- Massive data movement
- Load imbalance

---

## 🧠 2. What Is Consistent Hashing?

Consistent hashing solves this by mapping **both nodes and keys onto the same circular hash space** (a ring). 

- Each **node** is assigned a hash position on the ring.
- Each **key** is assigned a hash and placed on the ring.
- A key is stored in the **first node clockwise** from its position.

```
// Example: simplified concept
nodes = ["A", "B", "C"]
hash_ring = sorted([hash(n) for n in nodes])
key_hash = hash("user42")
target_node = find_first_node_clockwise(hash_ring, key_hash)
```

✅ When a node is added/removed, **only a small subset of keys** move  
✅ Maintains **balanced distribution** and **scales smoothly**

---

## 🌀 3. The Hash Ring

Visualize the hash space as a circle:

```
   0 ----------------> 2^32-1
   ^                  |
   |     Node B       |
   |     ↑            ↓
   |  Node A      Node C
   --------------------
```

- A key hashed to a point between A and B is routed to B.
- If B is removed, A or C will pick up B's segment.

---

## 🧩 4. Virtual Nodes (VNodes)

**Virtual nodes** improve balance and fault tolerance:

- Each physical node is assigned multiple **virtual positions** on the ring
- This avoids uneven load if one node lands in a sparse region

```
# Node A gets 100 virtual nodes
for i in range(100):
    ring.add(hash("A#" + str(i)))
```

✅ Better load distribution  
✅ Easier rebalance during scale-up/down  
✅ Faster recovery after failure

---

## 🔄 5. When Nodes Join or Leave

### ✅ Node Addition
- Only keys falling into the new node’s range are reassigned
- Minimal key movement

### ❌ Node Removal
- Keys that were on the removed node get reassigned to next clockwise node
- Other keys stay untouched

This **locality** and **stability** make consistent hashing ideal for dynamic systems.

---

## 📦 6. Use Cases
| Use Case                | Why Consistent Hashing Helps                |
|-------------------------|---------------------------------------------|
| Distributed Caching     | Prevents cache misses on node changes       |
| Database Sharding       | Smooth resharding with minimal movement     |
| Load Balancing          | Assigns client requests to backend servers  |
| Partitioned Logs        | Distributes partitions across brokers       |
| Distributed File Stores | Maps files to storage servers efficiently   |

---

## ⚙️ 7. Real-World Systems That Use It
| System         | Use of Consistent Hashing             |
|----------------|----------------------------------------|
| **Amazon Dynamo** | Ring-based partitioning, VNodes         |
| **Cassandra**     | VNodes + token ring for partitioning   |
| **Riak**          | Core to key distribution and fault tolerance |
| **Memcached**     | Client-side hashing for cache routing  |
| **Envoy Proxy**   | Uses it in load balancing algorithms   |
| **Kafka**         | Custom partitioners based on hash ring |

---

## ⚠️ 8. Pitfalls and Considerations
| Issue                  | Solution                           |
|------------------------|------------------------------------|
| Hash imbalance         | Use **VNodes** to smooth the ring  |
| Hot keys (skew)        | Combine hashing + replication      |
| Hash collisions        | Use strong, uniform hash function  |
| Rebalancing complexity | Use libraries/tools (e.g. Ketama)  |

---

## 🛠️ 9. Sample Implementation (Python-like Pseudocode)

```
class ConsistentHashRing:
    def __init__(self, nodes, vnodes=100):
        self.ring = {}
        self.sorted_keys = []
        for node in nodes:
            for i in range(vnodes):
                key = hash(f"{node}#{i}")
                self.ring[key] = node
                self.sorted_keys.append(key)
        self.sorted_keys.sort()

    def get_node(self, item_key):
        key = hash(item_key)
        for k in self.sorted_keys:
            if key <= k:
                return self.ring[k]
        return self.ring[self.sorted_keys[0]]  # Wrap around
```

---

## ✅ Summary
| Concept           | Description                                     |
|-------------------|-------------------------------------------------|
| Problem           | % N hashing remaps all keys on change           |
| Solution          | Consistent hashing maps nodes + keys to ring   |
| Benefit           | Only small subset of keys need to move          |
| Advanced Feature  | Virtual nodes improve balance                   |
| Real-World Use    | Dynamo, Cassandra, Kafka, Envoy, Memcached     |

---

## 📚 Further Reading

- [Amazon Dynamo Paper (2007)](https://www.allthingsdistributed.com/2007/10/amazons_dynamo.html)
- [Cassandra Architecture](https://cassandra.apache.org/doc/latest/cassandra/architecture/index.html)
- [Consistent Hashing Explained](https://tom-e-white.com/2007/11/consistent-hashing.html)
- [Ketama: Consistent Hashing in Memcached](https://github.com/RJ/ketama)

---
