# 🚨 Caching Failures in Production: SRE Guide to Problems and Mitigations

Caching is a powerful tool to reduce latency and load on backend systems — but poorly implemented caching can **fail catastrophically** under pressure. This guide walks through common caching problems encountered in real-world production systems and how to mitigate them from an SRE perspective.

---

## 1. 🌩️ **Thundering Herd Problem**

### 🔍 What Happens:
- Many **cached keys expire at the same time**
- All incoming requests **bypass the cache and hit the database**
- The database gets overwhelmed by a flood of identical queries

### 🧠 Example Scenario:
Imagine thousands of requests trying to retrieve a product list — all keys expire at once → cache miss → DB overload.

### ✅ Mitigation Strategies:
- **Stagger expiration times**: Add random jitter to TTL values
```
// Add 0–60s random TTL jitter
TTL = 300 + rand(0, 60)
```

- **Request coalescing**: Let **only one request** trigger a DB call, others wait for result (e.g., Redis + locks, Go singleflight)
- **Protect non-core services**: Block requests for non-essential data until cache repopulates

---

## 2. 🕳️ **Cache Penetration**

### 🔍 What Happens:
- Requests are made for **keys that don’t exist in the cache or DB**
- Cache keeps missing → **DB gets queried repeatedly**
- Leads to performance degradation, wasted DB cycles

### ✅ Mitigation Strategies:
- **Cache negative responses**: Store `"null"` or `"not_found"` values with a short TTL
```
GET /user/unknown_id  
→ cache → miss  
→ DB → miss  
→ store `null` in cache with 30s TTL
```

- **Use a Bloom filter**:
  - Tracks known keys efficiently
  - Rejects clearly invalid queries early
  - Especially useful for **read-heavy systems**

---

## 3. 🔥 **Cache Breakdown (Hot Key Expiry)**

### 🔍 What Happens:
- A **popular key (hot key)** expires
- All users simultaneously trigger a cache miss
- Causes a spike in DB load for that key

### ✅ Mitigation Strategies:
- **Avoid expiring hot keys**: Set long or no TTL for keys accessed very frequently
- **Pre-warm** hot keys after restart or cache flush
- **Lazy expiration**: Extend TTL based on access patterns

---

## 4. 💥 **Cache Crash**

### 🔍 What Happens:
- Entire cache service (e.g., Redis, Memcached) goes down
- All traffic reroutes directly to the **origin system** (e.g., DB, API)
- Sudden load causes cascading failures

### ✅ Mitigation Strategies:
- **Circuit breakers**: Block traffic to DB when cache is down for non-critical data
- **Cache clustering / replication**:
  - Run Redis Sentinel, Redis Cluster, or ElastiCache Multi-AZ setups
- **Graceful degradation**:
  - Return defaults or reduced data if cache fails

```
// Circuit breaker for cache dependency
if (cacheDown && dataType == "nonCritical") {
  return fallbackValue;
}
```

---

## 5. 🧟 **Stale Data / Inconsistent Cache**

### 🔍 What Happens:
- Cache contains **outdated data** (due to race conditions or sync delay)
- System reflects incorrect state or introduces bugs

### ✅ Mitigation Strategies:
- Use **write-through cache**: Cache is updated on write, not read
- Use **event-driven cache invalidation** (Kafka, pub-sub, etc.)
- Prefer **short TTL + versioning** for consistency-sensitive data

---

## 6. 🧠 **Over-Caching / Memory Pressure**

### 🔍 What Happens:
- Cache stores too much irrelevant or stale data
- Eviction policies (e.g., LRU) remove useful data prematurely

### ✅ Mitigation Strategies:
- Set **appropriate TTLs**
- Use **namespaced keys** and selectively expire
- Monitor **hit ratio** and **memory usage**
- Tune eviction strategies (`allkeys-lru`, `volatile-ttl`, etc.)

---

## 🧰 Summary Table of Problems & Solutions

| Problem               | Root Cause                       | Solution Highlights                            |
|-----------------------|----------------------------------|------------------------------------------------|
| Thundering Herd       | Mass expiration                  | TTL jitter, request coalescing                 |
| Cache Penetration     | Nonexistent keys                 | Cache nulls, Bloom filter                      |
| Cache Breakdown       | Hot key expires                  | No expiry for hot keys, lazy revalidation      |
| Cache Crash           | Cache service down               | Circuit breaker, replication, failover cache   |
| Inconsistent Cache    | Race conditions, stale writes    | Write-through, pub-sub invalidation            |
| Over-Caching          | Irrelevant/expired data retained | TTL tuning, LRU config, metrics                |

---

## 🧪 Monitoring Tips

- Track cache **hit/miss ratio**
- Monitor cache node **CPU/memory/network usage**
- Alert on **keyspace evictions**, **latency spikes**, or **connection errors**
- Use **dashboards**: Redis Insight, Grafana, CloudWatch, etc.

```
// Redis CLI monitoring
redis-cli INFO stats | grep hit
```

---

## 📚 Further Reading

- [Redis Caching Patterns](https://redis.io/docs/)
- [Cloudflare’s Guide to Cache Invalidation](https://developers.cloudflare.com/cache/)
- [Netflix Engineering – Caching at Scale](https://netflixtechblog.com/)

---
