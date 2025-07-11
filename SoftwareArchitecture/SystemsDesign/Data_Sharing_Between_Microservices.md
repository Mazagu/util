# 🔄 Data Sharing Between Microservices

Microservices promote loose coupling and independent data ownership, but real-world systems often require some level of **data sharing** across services. The challenge is to maintain service boundaries without creating tight coupling.

---

## 🧱 1. **Database per Service**

Each service owns and manages its own database.

**Pros**:
- Clear boundaries and autonomy
- Better encapsulation and scalability

**Cons**:
- Makes sharing data more complex
- Requires communication between services

---

## 🔁 2. **Synchronous Data Sharing (via APIs)**

One service queries another via HTTP/gRPC to fetch needed data.

```
Service A → (HTTP/REST) → Service B: GET /users/123
```

**Use when**:
- Real-time data is required
- Data ownership must remain within a service

**Challenges**:
- Coupling through network dependency
- Adds latency and risk of cascading failures

**Tips**:
- Use retries, timeouts, circuit breakers (e.g., Hystrix)
- Consider API Gateways or BFFs to simplify API access

---

## 📩 3. **Asynchronous Data Sharing (via Events)**

Services publish events when data changes (event-driven architecture).

```
User Service emits: UserCreated { user_id: 123, name: "Alice" }
Order Service subscribes and updates its local copy
```

**Pros**:
- Loose coupling
- High performance and resilience

**Cons**:
- Eventual consistency
- Debugging is more complex

**Tools**:
- Kafka, RabbitMQ, NATS, Amazon SNS/SQS

---

## 🗃️ 4. **Data Replication (Read Models or Caches)**

One service maintains a **read-optimized copy** of another service’s data, often updated via events or batch jobs.

```
Order Service maintains a local cache of User data for display purposes.
```

**Use when**:
- Fast read access is required
- You want to avoid cross-service calls

**Best practices**:
- Make it read-only
- Keep it eventually consistent

---

## 🔍 5. **Shared Libraries or SDKs**

For internal APIs, shared SDKs can encapsulate access logic.

```
user_sdk.get_user_profile(user_id)
```

**Pros**:
- Standardizes access
- Easier updates to logic

**Cons**:
- Versioning and deployment overhead
- Should not contain business logic

---

## 🚧 Anti-Pattern: Shared Database

Multiple services writing to the same database is a common **anti-pattern**.

**Problems**:
- Tight coupling of schema and deployment
- Violates the microservices principle of autonomy

**Only acceptable**:
- For legacy systems or during transitional phases
- If all services are tightly integrated and owned by a single team

---

## ✅ Best Practices

- Apply **event-driven architecture** for real-time async updates
- Use **read replicas** or materialized views for high-read access
- Respect **bounded contexts** from Domain-Driven Design
- Implement **schema versioning** and backward compatibility for events
- Use **idempotent consumers** and retry logic in async flows

---

## 🧭 Choosing a Strategy

| Use Case                         | Best Option                     |
|----------------------------------|----------------------------------|
| Real-time, latest data needed    | Synchronous API (REST/gRPC)      |
| Loose coupling + eventual sync  | Event-driven architecture        |
| High-volume reads, low writes   | Local cache or read replica      |
| Internal tooling                | Shared SDK or internal proxy     |

---
