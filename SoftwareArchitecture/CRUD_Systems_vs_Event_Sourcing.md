# 🔄 Guide: CRUD Systems vs Event Sourcing

Modern applications can manage state using **CRUD (Create, Read, Update, Delete)** operations or via **Event Sourcing**, where changes are captured as a sequence of immutable events. This guide compares both approaches, their strengths, weaknesses, and when to use them.

---

## 1. 📋 CRUD: The Traditional Model

In a **CRUD** system, you interact directly with **current state** stored in a database. It’s the default pattern in relational systems.

### 🔧 Typical CRUD Operations
| Action   | SQL Example                          |
|----------|--------------------------------------|
| Create   | `INSERT INTO orders (...)`           |
| Read     | `SELECT * FROM orders WHERE id = 1`  |
| Update   | `UPDATE orders SET status='shipped'` |
| Delete   | `DELETE FROM orders WHERE id = 1`    |

```
-- Example: CRUD update
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
```

### ✅ Pros

- Simple to understand and implement
- Matches relational DB semantics
- Easy to query current state

### ❌ Cons

- No built-in history of changes
- Hard to audit or debug past state
- Schema changes affect data directly

---

## 2. 📦 Event Sourcing: State as a Log of Changes

In **Event Sourcing**, you never store the current state directly. Instead, you store **immutable events** that describe what happened, and **derive state** by replaying them.

### 🧾 Example Events

```json
[
  { "type": "AccountCreated", "data": { "id": 1, "balance": 0 } },
  { "type": "MoneyDeposited", "data": { "id": 1, "amount": 100 } },
  { "type": "MoneyWithdrawn", "data": { "id": 1, "amount": 40 } }
]
```

### 🔄 Derived State

At any time, current balance is calculated by replaying events:

```
// Simplified balance projection
let balance = 0;
for (event of eventStream) {
  if (event.type === 'MoneyDeposited') balance += event.data.amount;
  if (event.type === 'MoneyWithdrawn') balance -= event.data.amount;
}
```

---

## 3. 🔁 CRUD vs Event Sourcing: Side-by-Side
| Feature               | CRUD                           | Event Sourcing                        |
|-----------------------|---------------------------------|----------------------------------------|
| Stores current state  | ✅ Yes                          | ❌ No — state is derived               |
| Stores history        | ❌ No (unless added manually)   | ✅ Yes — full history by default       |
| Schema flexibility    | 🟡 Medium (migration required)  | 🟢 High (event evolution possible)     |
| Read performance      | 🟢 Fast                         | 🟡 Depends on snapshotting             |
| Write logic           | 🟢 Simple                       | 🔴 More complex                        |
| Debugging             | ❌ Hard to reconstruct state    | ✅ Easy to replay and audit            |
| CQRS support          | ❌ Limited                      | ✅ Common pairing                      |
| Transactions          | ✅ Native (SQL)                 | 🟡 Requires careful coordination        |

---

## 4. 📚 Use Cases for Each

### ✅ Use CRUD When:
- You need simple, state-based data management
- Your application has no complex domain logic
- You prioritize read/write performance over traceability
- Example: Blog CMS, admin tools, internal dashboards

### ✅ Use Event Sourcing When:
- You need **auditable state history**
- State changes are complex or domain-driven
- You're using **CQRS** or **DDD** patterns
- Example: Financial systems, logistics, real-time games

---

## 5. 🏗️ Event Sourcing Architecture Basics

1. **Command**: User requests a change (`WithdrawMoney`)
2. **Domain logic**: Validates and emits events (`MoneyWithdrawn`)
3. **Event Store**: Appends event to log
4. **Projections**: Build views (e.g., current balance)
5. **Read Models**: Optimized views for UI or APIs

```
POST /withdraw
{
  "accountId": 1,
  "amount": 100
}

=> Emit: { "type": "MoneyWithdrawn", "data": { ... } }
```

---

## 6. 🔐 Versioning & Event Evolution

Events are immutable, but your **schema and logic will change**. Strategies:

- Use **versioned event types** (e.g. `OrderCreatedV2`)
- Use **schema migration layers** (e.g. adapter functions)
- Keep projections backward-compatible

---

## 7. 🧪 Hybrid: CRUD + Events (Change Data Capture)

Sometimes you can **get the best of both**:

- Use CRUD for simplicity
- Use **CDC (Change Data Capture)** tools like Debezium to generate events from DB changes
- Store those in an event store or stream (Kafka, Pulsar)

---

## 8. 🧠 Summary Comparison Table
| Dimension           | CRUD                        | Event Sourcing             |
|---------------------|-----------------------------|----------------------------|
| Simplicity          | ✅ Simple                   | ❌ Complex initial setup   |
| Flexibility         | 🟡 Limited schema evolution | ✅ Evolving event model    |
| Auditability        | ❌ Manual audit logs        | ✅ Built-in                |
| Rollback            | ❌ Hard                     | ✅ Replay past events      |
| Storage             | ✅ Compact (current only)   | ❌ Grows over time         |
| Read Model Custom   | ❌ Shared schema            | ✅ Any projection logic    |

---

## 📚 Further Reading

- [Martin Fowler: Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Debezium for CDC](https://debezium.io/)
- [CQRS and Event Sourcing Patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)

---

## ❓ FAQ: Event Sourcing vs CRUD

---

### 🟡 When the current state is derived from the latest event (e.g., user is verified), do we still need to compute the full event history?

**Not necessarily.** Many systems only need the **current state**, which can be stored as a **projection** or **materialized view**.

#### 🔹 Typical approach:
- Store events **immutably** in an **event store**
- Maintain **read models** (aka projections) that are **updated incrementally** as new events arrive
- This allows you to **read the current state without replaying** all events every time

```
// Example: User status projection
user_id: 123
status: "verified"
updated_at: "2025-06-10"
```

This works well for **low-complexity state** (e.g., "verified", "active", "locked").

---

### 🟡 But what about use cases like computing account balances from all financial transactions?

In these cases, **replaying all events** (e.g., `deposit`, `withdrawal`) **may be required** to compute the current state (e.g., balance).

#### 🔹 Strategies to optimize:
1. **Snapshots**: Save periodic state snapshots (e.g., every 1,000 events)
   - Replay only events **after** the last snapshot
   - Tradeoff: storage overhead, snapshot versioning

```
// Snapshot example:
snapshot: {
  user_id: 123,
  balance: 3500,
  event_offset: 10593
}
```

2. **Materialized Views**: Maintain balance in real-time using a read model
   - Event handler updates balance as events arrive
   - Fast reads, consistent as of last event processed

3. **Delta Events**: Emit derived events like `daily_balance_computed` or `interest_applied`
   - Reduces need to compute complex history on-the-fly

---

### 🟢 Which approach should I use?

| Case                          | Recommended Strategy                    |
|-------------------------------|-----------------------------------------|
| Simple status (e.g. verified) | Projection or read model                |
| Financial balance             | Snapshots + projection                  |
| Compliance/auditing required  | Full event history + periodic snapshot  |
| Complex aggregations          | Event replay + cached materializations  |

---

### ⚙️ How do you scale event sourcing systems?

#### 1. **Sharding the Event Store**
- Partition events by aggregate ID (e.g. per user, per account)
- Use Kafka, DynamoDB streams, or purpose-built stores (EventStoreDB, Axon)

#### 2. **Asynchronous Projections**
- Keep projections separate from the write model
- Use background consumers to update materialized views
- Enables horizontal scaling of read performance

#### 3. **Append-Only Optimization**
- Use log-structured storage (e.g. Kafka, Apache Pulsar)
- Writes are fast and storage is immutable

#### 4. **Snapshotting & Compaction**
- Store snapshots for fast recovery
- Apply compaction policies to trim historical events if legally allowed

---

### 📦 How does event sourcing compare to CRUD in terms of storage?

| Aspect        | CRUD                          | Event Sourcing                          |
|---------------|-------------------------------|------------------------------------------|
| Storage Use   | Stores only current state     | Stores full event history + projections |
| Mutation Type | Overwrites previous state     | Appends immutable events                |
| Auditing      | Needs extra logging layer     | Built-in audit trail                    |
| Storage Growth| Controlled                    | Grows over time, needs pruning/snapshot |

---

### 🧠 Final Tips

- Keep **event granularity meaningful** — not too coarse or too fine
- Maintain **projection rebuild tooling** (in case schema evolves)
- Use **event versioning** practices (additive changes, upcasters)
- Monitor **event lag** between the event store and projections

---
