# ⚡ Guide: Caching — A Systems Design Deep Dive

**Caching** is a critical system design pattern used to improve **performance**, **reduce latency**, and **scale** applications by avoiding repeated computation or I/O.

This guide covers:
- What caching is and why it matters
- Caching placement strategies (client, CDN, edge, app, DB)
- Cache invalidation and consistency
- Eviction policies
- Common caching architectures and tools

---

## 🧠 1. What Is Caching?

**Caching** stores precomputed or previously retrieved data in a faster-access storage layer, allowing future requests to bypass more expensive operations.

### 🚀 Why Cache?
- Reduce **database or API load**
- Lower **latency** for frequently accessed data
- Smooth **traffic spikes**
- Enable **offline access** (client-side)

```
// Without cache
user = db.query("SELECT * FROM users WHERE id = 42")

// With cache
user = cache.get("user:42")
if (!user) {
  user = db.query(...)
  cache.set("user:42", user)
}
```

---

## 📦 2. Caching Layers & Placement

| Layer         | Description                              | Example                         |
|---------------|------------------------------------------|----------------------------------|
| Client-side   | In browser/app                           | LocalStorage, IndexedDB         |
| CDN           | Caches static assets near users          | Cloudflare, Akamai              |
| Edge caching  | Dynamic content at PoPs                  | Fastly, CloudFront Lambda@Edge  |
| App-level     | Cache inside the app                     | In-memory (e.g. `Map`, `Guava`) |
| Server-side   | Central cache layer                      | Redis, Memcached                |
| DB/Storage    | Internal query caching                   | PostgreSQL buffer cache         |

---

## 🔁 3. Caching Strategies

### 🟢 Read-through Cache
App asks cache first; if not found, fetches from source and stores it.

```
function getUser(id) {
  let user = cache.get(`user:${id}`);
  if (!user) {
    user = db.query(...);
    cache.set(`user:${id}`, user);
  }
  return user;
}
```

### 🟠 Write-through Cache
Write to cache **and** the DB at the same time.

### 🔴 Cache-aside (Lazy loading)
App controls cache population manually.

### 🟣 Write-back (Write-behind)
Write only to cache and flush to DB asynchronously (dangerous on crashes).

---

## ⛔ 4. Cache Invalidation

> “There are only two hard things in computer science: cache invalidation and naming things.”  
> — Phil Karlton

### 💣 Why It’s Hard:
- When data changes, how do we know which cache keys to evict?
- Cached data might become **stale** or **inconsistent**

### 🔧 Techniques:
- **Time-to-live (TTL)**: Expire keys automatically
- **Explicit Invalidation**: App deletes/updates cache when DB changes
- **Versioning**: Store with key version or hash (`user:42:v3`)
- **Event-driven**: Use pub/sub (e.g. Redis Streams, Kafka) to evict across systems

---

## 🧹 5. Eviction Policies

| Policy        | Description                          | Use Case                         |
|---------------|--------------------------------------|----------------------------------|
| LRU (Least Recently Used) | Remove least recently accessed      | Most popular                    |
| LFU (Least Frequently Used)| Remove least accessed frequently   | Hot/cold data separation        |
| FIFO          | Remove oldest added                  | Simple but naive                 |
| TTL-based     | Remove after N seconds               | Time-bound data like sessions    |
| Manual        | Application deletes keys explicitly  | Fine-grained control             |

```
// Redis: set TTL
SET user:42 "data" EX 60
```

---

## ⚠️ 6. Consistency Models

| Consistency Model  | Description                        | Trade-offs                      |
|--------------------|------------------------------------|----------------------------------|
| **Strong**         | Cache and source always in sync    | Hard to scale                   |
| **Eventual**       | Cache updated after source writes  | Simpler, may serve stale reads  |
| **Write-through**  | Write to cache + DB together       | Safer, slightly slower          |
| **Write-back**     | Write to cache only, sync later    | Fast, but risk of data loss     |

---

## 📚 7. Common Use Cases

| Use Case               | Caching Strategy                | Tools                    |
|------------------------|----------------------------------|--------------------------|
| API responses          | TTL or CDN-based caching        | Fastly, Varnish          |
| User profiles          | Cache-aside or read-through     | Redis                    |
| Search suggestions     | In-memory with LRU              | Guava, Caffeine (Java)   |
| Product catalog        | Versioned cache + TTL           | Redis + async updates    |
| ML features or scores  | Write-through, time-bounded     | Redis, feature stores    |

---

## 🛠️ 8. Tools and Technologies

| Tool        | Type           | Notes                              |
|-------------|----------------|------------------------------------|
| **Redis**   | In-memory, LRU, TTL, pub/sub | Versatile, widely used           |
| **Memcached** | In-memory, LRU only      | Lightweight, simpler than Redis  |
| **Caffeine** (Java) | Local, LRU, async loading | High-performance in-JVM caching  |
| **Varnish** | HTTP reverse proxy         | Edge and CDN caching             |
| **Cloudflare / Fastly** | CDN           | Global static and dynamic caching|

---

## 🔥 9. Pitfalls to Avoid

| Pitfall                         | Recommendation                              |
|----------------------------------|---------------------------------------------|
| Serving stale data               | Use TTLs, versioned keys, or event triggers |
| Inconsistent multi-node caches   | Use central Redis or distributed cache      |
| Cache stampede (thundering herd)| Use locking or request coalescing           |
| Large unbounded keys             | Use size limits and LRU eviction            |
| Over-caching                     | Don’t cache low-traffic or fast queries     |

```
// Prevent cache stampede
if (!cache.get(key)) {
  if (acquireLock(key)) {
    let val = db.query(...)
    cache.set(key, val);
    releaseLock(key);
  } else {
    waitAndRetry(); // someone else is loading
  }
}
```

---

## 🧰 10. Advanced Patterns

### 🧩 Sharded Caches
- Split cache across multiple nodes
- Redis Cluster, consistent hashing

### 🔁 Cache Invalidation by Events
- Invalidate based on pub/sub changes
- Kafka, Redis Streams, Debezium

### 📊 Cache Observability
- Monitor hit/miss rates, TTL behavior, evictions
- Use Prometheus exporters, Redis Insights, Datadog

---

## ✅ Summary

| Topic            | Key Point                             |
|------------------|----------------------------------------|
| Strategy         | Cache-aside is most common             |
| Placement        | Choose based on latency + scale        |
| Invalidation     | TTL + versioning often best combo      |
| Eviction         | LRU is best default                    |
| Consistency      | Choose based on criticality & latency  |

---

## 📚 Further Reading

- [Caching at Scale (Cloudflare)](https://blog.cloudflare.com/what-is-caching/)
- [Caching Strategies — AWS Docs](https://docs.aws.amazon.com/whitepapers/latest/caching-strategies/caching-strategies.pdf)
- [Redis Caching Patterns](https://redis.io/docs/latest/use-cases/caching/)
- [Cache-Aside vs Write-Through vs Write-Behind](https://martinfowler.com/bliki/CacheAside.html)
- [Avoiding Cache Stampede – Netflix Engineering](https://netflixtechblog.com/preventing-cache-stampede-using-singleflight-61b0e6f3512e)

---
