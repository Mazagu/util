# 🧠 Deep Dive into Memcached

Memcached is a high-performance, distributed memory object caching system. It’s commonly used to reduce database load, speed up dynamic applications, and cache arbitrary data such as results of database calls, API calls, or page rendering.

---

## 📌 What Is Memcached?

- **In-memory key-value store**: Stores small chunks of arbitrary data (strings, objects) in RAM.
- **Volatile storage**: Data is stored temporarily and **evicted** when memory is full.
- **No persistence**: Memcached is designed purely for caching, not as a durable database.
- **No query language or rich data types**: It’s simple, fast, and highly optimized for read-heavy use cases.

---

## 🚀 Use Cases

- **Caching DB queries or HTML pages** to reduce latency
- **Session storage** in web applications
- **Rate limiting counters** or temporary metadata
- **DNS lookup caching**

---

## 🛠 Architecture Overview

- **Client-Server Model**: Clients interact with one or more Memcached servers via TCP or UDP.
- **Distributed (sharding at client level)**: Clients hash keys to distribute data across multiple nodes.
- **Eviction strategy**: Least Recently Used (LRU)
- **Multithreaded server**: Scales across cores, especially useful on large-memory machines

---

## ⚙️ Core Operations

| Command   | Description                          |
|-----------|--------------------------------------|
| `set`     | Add a key-value pair                 |
| `get`     | Retrieve a value by key              |
| `delete`  | Remove a key                         |
| `add`     | Only sets the key if it doesn’t exist|
| `replace` | Replaces existing value              |
| `incr` / `decr` | For numeric counters         |

```
// Python Example with `pymemcache`
from pymemcache.client import base
client = base.Client(('localhost', 11211))
client.set('username_123', 'john')
client.get('username_123')  # returns b'john'
```

---

## 🧱 Data Storage Characteristics

- **Keys**: Max 250 bytes
- **Values**: Max 1 MB (default, can be tuned)
- **No hierarchical namespace**, no data expiration guarantees
- **TTL**: Each key can have a time-to-live (default is unlimited until LRU kicks in)

```
// Set a key with 60s expiration
client.set('page_home', '<html>...</html>', expire=60)
```

---

## ⚡ Performance and Scaling

### Horizontal Scaling
- Clients handle sharding: use consistent hashing (Ketama) to distribute keys across nodes.
- Add or remove nodes without downtime (with minimal key movement if using consistent hashing).

### Memory Management
- Fixed-size memory allocation
- Uses slab allocation to minimize fragmentation
- Eviction via LRU when full

---

## 🔐 Security Considerations

- **No authentication by default**: Place Memcached behind firewalls or VPC
- Use **SASL** for authentication in newer versions
- Always disable UDP if not used (vulnerable to amplification attacks)

```
// Start Memcached with TCP only and limited IP binding
memcached -m 512 -p 11211 -U 0 -l 127.0.0.1
```

---

## 🧪 Monitoring & Metrics

Monitor via:
- `stats` command
- Tools like `memcached-top`, `munin`, `Prometheus exporters`

Important metrics:
- `get_hits`, `get_misses`
- `bytes`, `curr_connections`
- `evictions`, `cmd_get`, `cmd_set`

---

## 🆚 Memcached vs Redis

| Feature                | Memcached              | Redis                    |
|------------------------|------------------------|--------------------------|
| Data Persistence       | ❌ No                  | ✅ Yes                   |
| Advanced Data Types    | ❌ No                  | ✅ Lists, Sets, Hashes   |
| Pub/Sub, Streams       | ❌ No                  | ✅ Yes                   |
| TTL Granularity        | Basic per-key TTL      | Fine-grained per key     |
| Max Value Size         | ~1MB                   | ~512MB (configurable)    |
| Use Case Fit           | Simple, volatile cache | Richer caching + logic   |

---

## 🧠 Best Practices

- Use **consistent hashing** to minimize cache invalidation on node changes.
- Do **not use Memcached for persistent or sensitive data**.
- Apply **expiration jitter** to prevent cache stampedes.
- For structured objects, use **serialization (e.g., JSON, pickle)** carefully to avoid compatibility issues.

---

## 🧩 Alternatives & When to Use Memcached

| Alternative | When to Prefer                                     |
|-------------|----------------------------------------------------|
| Redis       | You need persistence, pub/sub, complex structures |
| CDN         | For caching static web assets at global scale     |
| Local cache | For small, fast access within the same process     |

**Use Memcached when**:
- You need ultra-fast, volatile caching
- Simplicity and low overhead matter
- You’re caching flat or string-based data

---

## 📚 Further Reading

- [Official Memcached Documentation](https://memcached.org/)
- [Memcached GitHub Repo](https://github.com/memcached/memcached)

---
