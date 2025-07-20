# 🛡️ System Design: Reliability & Resiliency Guide

Designing for **reliability** and **resiliency** ensures your system stays available, consistent, and correct—even when components fail.

---

## 📘 1. Definitions

- **Reliability**: The ability of a system to function correctly and consistently over time.
- **Resiliency**: The ability of a system to recover quickly from failures and continue operating.

> 💡 A *reliable* system minimizes downtime. A *resilient* system recovers gracefully.

---

## 🧰 2. Core Strategies

### ✅ Redundancy
- Duplicate components to avoid single points of failure
- Examples:
  - Multiple app instances
  - DB replication
  - Multi-region deployment

### ⚖️ Load Balancing
- Distribute traffic to healthy services
- Automatically redirect if a node goes down

### 💬 Graceful Degradation
- Prioritize core functionality during partial failures
- Example: Show cached data if DB is slow

### 🔁 Retries with Backoff
- Retry transient failures with exponential backoff
- Add jitter to avoid thundering herd

```
retryCount = 0
while retryCount < maxRetries:
    wait = 2 ** retryCount + random_jitter()
    sleep(wait)
    retryCount += 1
```

### 🔒 Circuit Breaker
- Avoid repeated calls to failing services
- Example tools: Netflix Hystrix, Resilience4j

### 🪝 Health Checks
- Liveness and readiness probes
- Useful for auto-recovery and autoscaling

---

## 🔄 3. Data Resiliency Techniques

### 🧬 Replication
- Real-time copies of data across nodes or regions
- Synchronous (strong consistency) vs. Asynchronous (eventual)

### ⛓️ Event Sourcing
- Record all state changes as events
- Enables replaying to restore state

### 💾 Backups
- Automate frequent backups
- Verify recovery process regularly

---

## 📉 4. Failure Handling Patterns

### 🌐 Bulkheads
- Isolate failures to prevent cascading
- Example: Separate thread pools or service instances per feature

### 📦 Queue-Based Decoupling
- Use queues to absorb load spikes and smooth traffic
- Helps when downstream services are slow or fail

### ⏳ Timeouts
- Fail fast instead of hanging indefinitely
- Set appropriate timeouts per service

---

## 📊 5. Monitoring & Alerting

- Track availability, errors, latency, and saturation (Google's Four Golden Signals)
- Use observability tools: Prometheus, Grafana, Datadog, Sentry
- Alert on symptoms, not causes

---

## 🧪 6. Chaos Engineering

- Introduce failures in controlled ways to test resilience
- Tools: Gremlin, Chaos Monkey, LitmusChaos

---

## 🧩 7. Common Tools & Libraries

| Area                | Tools / Frameworks                  |
|---------------------|-------------------------------------|
| Circuit Breakers    | Hystrix, Resilience4j               |
| Retries & Backoff   | Polly (.NET), Tenacity (Python)     |
| Load Balancing      | HAProxy, Envoy, NGINX, Istio        |
| Chaos Engineering   | Gremlin, Chaos Monkey, LitmusChaos  |
| Observability       | Prometheus, Grafana, ELK, Sentry    |

---

## 📋 8. Reliability/Resilience Design Checklist

- [ ] Is every component redundant or recoverable?
- [ ] Are retries implemented with backoff and limits?
- [ ] Are circuit breakers and timeouts in place?
- [ ] Do we gracefully degrade during failures?
- [ ] Is observability set up for all critical paths?
- [ ] Are backups regularly tested for restoration?
- [ ] Have we performed chaos testing?
