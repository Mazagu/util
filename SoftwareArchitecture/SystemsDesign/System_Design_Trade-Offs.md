# System Design Trade-Offs

Designing systems often involves choosing between trade-offs that optimize for scalability, reliability, performance, cost, or developer productivity. Below is a comprehensive list of common trade-offs in system design.

---

## 🆙 Vertical vs Horizontal Scaling

- **Vertical Scaling**: Add more resources (CPU, RAM) to a single server.
  - ✅ Easier to implement.
  - ❌ Hardware limits and single point of failure.

- **Horizontal Scaling**: Add more servers to handle load.
  - ✅ High availability and redundancy.
  - ❌ Complexity in load balancing and state management.

---

## 🗃️ SQL vs NoSQL

- **SQL**: Relational, structured, ACID-compliant.
  - ✅ Consistency and complex querying.
  - ❌ Rigid schema.

- **NoSQL**: Document, key-value, column, or graph databases.
  - ✅ Flexible schema, horizontal scalability.
  - ❌ Weaker consistency guarantees.

---

## 🕒 Batch vs Stream Processing

- **Batch**: Processes large volumes at once.
  - ✅ Efficient for large, periodic jobs.
  - ❌ Latency.

- **Stream**: Processes data in real-time.
  - ✅ Low-latency insights.
  - ❌ Higher complexity and cost.

---

## 📊 Normalization vs Denormalization

- **Normalization**: Organize data to avoid redundancy.
  - ✅ Integrity and smaller storage.
  - ❌ More joins and slower reads.

- **Denormalization**: Combine data to reduce joins.
  - ✅ Faster reads.
  - ❌ Redundant data, risk of inconsistency.

---

## 🧭 Consistency vs Availability

As per the **CAP Theorem**:

- **Consistency**: All nodes see the same data.
  - ✅ Predictable results.
  - ❌ Might reduce availability.

- **Availability**: Always respond, even if some nodes are outdated.
  - ✅ Uptime.
  - ❌ Risk of stale data.

---

## 📥 Strong vs Eventual Consistency

- **Strong**: All reads reflect the latest write.
  - ✅ Accuracy.
  - ❌ Latency and performance cost.

- **Eventual**: Data will eventually sync across nodes.
  - ✅ Performance and availability.
  - ❌ Possible stale reads.

---

## 🌐 REST vs GraphQL

- **REST**: Predefined endpoints for fixed resources.
  - ✅ Simpler and widely supported.
  - ❌ Over/under-fetching data.

- **GraphQL**: Query exactly what you need.
  - ✅ Efficient data retrieval.
  - ❌ Complexity in schema and tooling.

---

## 🧠 Stateful vs Stateless

- **Stateful**: Maintains session info across requests.
  - ✅ Personalized experiences.
  - ❌ Scaling challenges.

- **Stateless**: Each request is independent.
  - ✅ Easier scaling and caching.
  - ❌ Redundant data transmission.

---

## 🧊 Read-Through vs Write-Through Caching

- **Read-Through**: Cache is populated on cache miss.
  - ✅ Simpler logic.
  - ❌ Initial latency on cache miss.

- **Write-Through**: Cache is updated when data is written.
  - ✅ Always up-to-date cache.
  - ❌ Higher write latency.

---

## 🔄 Sync vs Async Processing

- **Synchronous**: Waits for task to finish before proceeding.
  - ✅ Simpler flow and immediate feedback.
  - ❌ Slower for long-running tasks.

- **Asynchronous**: Tasks run in background.
  - ✅ Higher responsiveness.
  - ❌ Complexity in coordination.

---

## 🌍 Monolith vs Microservices

- **Monolith**: All logic in one codebase.
  - ✅ Easier to develop early on.
  - ❌ Hard to scale or update independently.

- **Microservices**: Independent components.
  - ✅ Scalable and resilient.
  - ❌ DevOps complexity.

---

## 🔁 Push vs Pull Model

- **Push**: Data is sent proactively.
  - ✅ Low latency.
  - ❌ Risk of overwhelming consumers.

- **Pull**: Consumers request data when needed.
  - ✅ More control.
  - ❌ Higher latency.

---

## 📦 Containerization vs Serverless

- **Containers**: Deploy packaged code in isolated environments (e.g. Docker).
  - ✅ Portability and control.
  - ❌ Needs orchestration (e.g. Kubernetes).

- **Serverless**: Run code on demand (e.g. AWS Lambda).
  - ✅ Scalability and no infra management.
  - ❌ Cold starts and vendor lock-in.

---

## 🛡️ Custom Auth vs Federated Identity

- **Custom Auth**: Build and manage your own user authentication system.
  - ✅ Full control.
  - ❌ Hard to secure and scale.

- **Federated Identity**: Use external providers (e.g., OAuth2, SAML).
  - ✅ Faster implementation, better security.
  - ❌ Less flexibility.

---

## 📈 Proactive vs Reactive Scaling

- **Proactive**: Anticipate load and scale in advance.
  - ✅ Better preparation.
  - ❌ Resource waste.

- **Reactive**: Scale based on real-time metrics.
  - ✅ Efficient resource usage.
  - ❌ Lag in scaling can hurt performance.

---

## Summary Table

| Trade-off                     | Option A                 | Option B                |
|------------------------------|--------------------------|-------------------------|
| Scaling                      | Vertical                 | Horizontal              |
| Data Model                   | SQL                      | NoSQL                   |
| Processing                   | Batch                    | Stream                  |
| Schema Design                | Normalized               | Denormalized            |
| CAP Focus                    | Consistency              | Availability            |
| Consistency                  | Strong                   | Eventual                |
| API Paradigm                 | REST                     | GraphQL                 |
| App Architecture             | Monolith                 | Microservices           |
| Communication                | Push                     | Pull                    |
| Deployment                   | Containerized            | Serverless              |
| Auth                         | Custom                   | Federated               |
| Scaling Trigger              | Proactive                | Reactive                |

---

Design choices should be guided by the specific needs of your product, your users, and your infrastructure. There's rarely a universally "right" option — only the most **appropriate** one for your constraints.
