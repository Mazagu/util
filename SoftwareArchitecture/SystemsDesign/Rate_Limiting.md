# 🚦 Guide: Rate Limiting — A Systems Design Deep Dive

**Rate limiting** is a mechanism to **control the rate of requests** clients can make to a system. It’s essential in distributed systems for **fairness**, **protection**, and **resource management**.

---

## 🧠 1. What Is Rate Limiting?

Rate limiting restricts how many **requests** a client (IP, user, API key, etc.) can make in a **given time window**.

### Why?
- Prevent abuse and DoS attacks
- Protect backend resources
- Ensure fair usage (multi-tenant systems)
- Enforce SLAs or pricing tiers
- Throttle traffic during peak load

---

## 📏 2. Key Dimensions
```
| Dimension       | Examples                                |
|------------------|------------------------------------------|
| **Identity**     | IP address, user ID, access token         |
| **Scope**        | Per-endpoint, per-service, per-tenant     |
| **Granularity**  | Per second, minute, hour, day             |
| **Limit Type**   | Fixed, dynamic, bursty                   |
```
---

## ⚙️ 3. Rate Limiting Algorithms

### ⏱️ 1. **Fixed Window Counter**
- Track count of requests in a fixed time window

```
# Key: user:123|minute:202506192103
incr_counter(key)
if counter > limit: block
```

✅ Simple  
❌ Bursts near window boundaries (race condition)

---

### ⌛ 2. **Sliding Window Log**
- Store timestamps of each request and expire old ones

```
timestamps = get_requests(user)
timestamps = [t for t in timestamps if t > now - 60]
if len(timestamps) >= limit: block
```

✅ Precise  
❌ Memory grows with traffic

---

### 📉 3. **Sliding Window Counter (Leaky Approximation)**
- Smoothen the fixed window using interpolation

✅ Balanced burst tolerance and performance  
❌ Slightly complex to implement

---

### 🔄 4. **Token Bucket**
- Tokens are added at a fixed rate; requests consume tokens

```
bucket = get_bucket(user)
if bucket.tokens >= 1:
  bucket.tokens -= 1
  allow
else:
  reject
```

✅ Allows short bursts  
✅ Ideal for APIs and microservices

---

### 🪣 5. **Leaky Bucket**
- Queue-like system where requests drip out at constant rate

✅ Controls average rate  
❌ Harder to burst

---

## 🧰 4. Where to Implement Rate Limiting?
```
| Layer         | Use Case                               | Tools / Examples             |
|---------------|------------------------------------------|------------------------------|
| **Client SDK** | Preemptive throttling                   | Retry logic, backoff         |
| **API Gateway**| Global protection                       | Kong, Envoy, NGINX, AWS API Gateway |
| **Backend**    | Per-service business limits             | Redis, Memcached             |
| **Middleware** | Shared logic across endpoints           | Express/Flask middleware     |
```
---

## 📊 5. Rate Limiting in Redis

Redis is commonly used due to its atomic operations and expiry features:

```
INCR user:123
EXPIRE user:123 60
IF GET user:123 > 100 → block
```

Atomic Lua scripts or `SETNX` can improve precision.

---

## ⚠️ 6. Distributed Rate Limiting

Challenges:
- **Shared state** across nodes
- **Atomic counters** under high load
- **Replication lag** and **clock drift**

Solutions:
- Use Redis with TTL and Lua
- Use distributed coordination (etcd, Consul)
- External rate limiting services (Cloudflare, AWS)

---

## 🔄 7. Retry-After and Headers

Always provide rate-limit headers:
```
| Header                  | Description                                |
|--------------------------|--------------------------------------------|
| `X-RateLimit-Limit`      | Total allowed requests                     |
| `X-RateLimit-Remaining`  | Remaining requests in window               |
| `Retry-After`            | Time to wait before retrying               |
```
This helps clients throttle themselves gracefully.

---

## 🧠 8. Best Practices

- Separate limits for read vs write
- Apply stricter limits to anonymous users
- Use exponential backoff on retry
- Log rate-limited requests for observability
- Allow whitelisting for trusted services

---

## 🧱 9. Real-World Examples
```
| Service          | Strategy                                      |
|------------------|-----------------------------------------------|
| **GitHub API**    | 5,000 requests per hour per authenticated user |
| **Twitter API**   | Varies by endpoint and access tier            |
| **Cloudflare**    | Custom global rate limits at the edge         |
| **Stripe**        | Burst + steady limits, HTTP 429 with Retry-After |
| **AWS API Gateway** | Token bucket model per stage + method         |
```
---

## ✅ Summary
```
| Aspect         | Key Points                                    |
|----------------|-----------------------------------------------|
| Purpose        | Protect systems from abuse and overload       |
| Algorithms     | Fixed window, sliding window, token/leaky bucket |
| Deployment     | Gateway, backend, middleware                  |
| Tools          | Redis, Envoy, Kong, NGINX, AWS, Cloudflare    |
```
---

## 📚 Further Reading

- [Rate Limiting Algorithms Demystified (Cloudflare)](https://blog.cloudflare.com/counting-things-a-lot-of-different-things/)
- [AWS Rate Limiting Docs](https://docs.aws.amazon.com/apigateway/latest/developerguide/api-gateway-request-throttling.html)
- [Envoy Rate Limiting Filter](https://www.envoyproxy.io/docs/envoy/latest/configuration/http/http_filters/rate_limit_filter)

---
