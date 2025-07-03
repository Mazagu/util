# 🚀 Redis Essentials: CLI, Configuration, Persistence, and Usage

Redis is a fast, in-memory key-value store widely used for **caching**, **session storage**, **pub/sub**, and **real-time analytics**. This guide covers how to use Redis from the CLI, configure it for production, manage persistence, and apply practical usage patterns.

---

## ⚡ 1. Redis CLI Basics

### 🔹 Connecting to Redis

```
redis-cli
redis-cli -h 127.0.0.1 -p 6379 -a yourpassword
```

### 🔹 Common Commands

```
SET user:1 "John"
GET user:1

DEL user:1
EXISTS user:1
TTL user:1

KEYS user:*        -- (⚠️ avoid in production)
SCAN 0 MATCH user:* COUNT 100

FLUSHDB           -- flush current DB
FLUSHALL          -- flush all DBs
```

> Use `SCAN` instead of `KEYS` in production to avoid blocking.

---

## ⚙️ 2. Configuration for Production

### 🔹 Config File

Default path: `/etc/redis/redis.conf` (Debian) or `/etc/redis.conf` (Alpine/Docker)

Important entries:

```conf
bind 127.0.0.1
port 6379
requirepass yourpassword
timeout 300
maxmemory 512mb
maxmemory-policy allkeys-lru
appendonly yes
save 900 1
save 300 10
```

```
# Test config
redis-server /etc/redis/redis.conf --test-memory 512

# Restart service (Debian)
sudo systemctl restart redis
```

---

## 🔐 3. Access Control & Security

### 🔹 Authentication

Enable in config:

```conf
requirepass s3cret
```

Or at runtime:

```
AUTH s3cret
```

> Use Redis ACLs (`redis >= 6.0`) for fine-grained control:

```
ACL SETUSER readonly on >password ~* +@read
ACL SETUSER writer on >password ~* +@write
```

---

## 💾 4. Persistence Modes

Redis supports two main persistence strategies:

### 🔹 AOF (Append-Only File)

- Safer, logs each write operation
- Config:
```conf
appendonly yes
appendfsync everysec
```

### 🔹 RDB Snapshots

- Periodic full memory snapshots
- Config:
```conf
save 900 1
save 300 10
```

Backups are written to:
- `/var/lib/redis/dump.rdb`
- `/var/lib/redis/appendonly.aof`

```
# Trigger save manually
SAVE         # blocking
BGSAVE       # non-blocking
```

---

## 🧠 5. Redis Data Structures

| Type      | Example Key Pattern | Commands                        | Use Case                        |
|-----------|---------------------|----------------------------------|----------------------------------|
| Strings   | `user:1:name`       | `GET`, `SET`, `INCR`             | Counters, simple values          |
| Hashes    | `user:1`            | `HGET`, `HSET`, `HGETALL`        | Objects (user profile, etc.)     |
| Lists     | `queue:jobs`        | `LPUSH`, `RPOP`, `LRANGE`        | Queues, logs                     |
| Sets      | `tags:post:1`       | `SADD`, `SMEMBERS`, `SINTER`     | Tags, unique values              |
| Sorted Sets | `leaderboard`     | `ZADD`, `ZRANGE`, `ZREVRANK`     | Rankings, scores                 |
| Bitmaps   | `feature:flags`     | `SETBIT`, `GETBIT`, `BITCOUNT`   | Flags, tracking on/off bits      |
| HyperLogLog | `users:seen`      | `PFADD`, `PFCOUNT`               | Approximate cardinality          |

---

## 🛰️ 6. Redis Pub/Sub

For **event messaging** and notifications between services:

```
# Terminal A
SUBSCRIBE events:signup

# Terminal B
PUBLISH events:signup "new_user_id:123"
```

> Redis Pub/Sub is fast but **not durable** — not guaranteed to deliver if subscriber is offline.

---

## 🔄 7. Use as a Cache (with TTLs)

```
# Set with TTL
SET session:abc123 "user data" EX 3600

# Get TTL
TTL session:abc123

# Atomic get+set+expire
SET mykey "value" EX 10 NX
```

Common eviction policies (set in config):

- `allkeys-lru` (least recently used)
- `volatile-ttl` (shortest time to live)
- `noeviction` (reject writes when full)

Use `INFO memory` to track memory usage.

---

## 🔍 8. Monitoring Redis

### 🔹 Stats and Info

```
INFO          -- All stats
INFO memory   -- Memory-specific
MONITOR       -- Live command feed (verbose)
CLIENT LIST   -- Connected clients
```

### 🔹 Logging

- Logs found in `/var/log/redis/redis-server.log`
- Verbosity set in config: `loglevel notice`

---

## 🧪 9. Useful Tools

- **redis-cli** – default CLI tool
- **RedisInsight** – official GUI dashboard
- **redis-benchmark** – test performance
```
redis-benchmark -n 100000 -q -P 10
```

- **RDB Tools** – inspect `.rdb` snapshots
- **redis-commander** – Node-based GUI for live keys

---

## 🛡️ 10. Reliability Tips

- Always set **maxmemory** and **eviction policy** in production
- Use AOF for safer persistence
- Use **replication** (`slaveof`, `replicaof`) for HA
- Combine with **Sentinel** or **Redis Cluster** for fault tolerance
- Protect Redis from the internet (`bind`, `password`, firewalls)

---

## 📚 Resources

- [Redis Official Docs](https://redis.io/docs/)
- [Redis CLI Reference](https://redis.io/commands/)
- [RedisInsight Dashboard](https://www.redis.com/redis-enterprise/redis-insight/)

---

## ✅ Summary

| Topic          | Tool/Command/Config                         |
|----------------|---------------------------------------------|
| Connect        | `redis-cli -a password`                     |
| Set/Query Keys | `SET`, `GET`, `EXPIRE`, `TTL`               |
| Persistence    | `appendonly yes`, `BGSAVE`                  |
| Monitor        | `INFO`, `MONITOR`, `CLIENT LIST`            |
| Auth & ACLs    | `requirepass`, `ACL SETUSER`                |
| Eviction       | `maxmemory-policy allkeys-lru`              |
| Use as Cache   | `SET key val EX 60 NX`                      |
