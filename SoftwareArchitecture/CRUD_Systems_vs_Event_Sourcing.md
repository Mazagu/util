# 🔄 Guide: CRUD Systems vs Event Sourcing

Modern applications can manage state using **CRUD (Create, Read, Update, Delete)** operations or via **Event Sourcing**, where changes are captured as a sequence of immutable events. This guide compares both approaches, their strengths, weaknesses, and when to use them.

---

## 1. 📋 CRUD: The Traditional Model

In a **CRUD** system, you interact directly with **current state** stored in a database. It’s the default pattern in relational systems.

### 🔧 Typical CRUD Operations
```
| Action   | SQL Example                          |
|----------|--------------------------------------|
| Create   | `INSERT INTO orders (...)`           |
| Read     | `SELECT * FROM orders WHERE id = 1`  |
| Update   | `UPDATE orders SET status='shipped'` |
| Delete   | `DELETE FROM orders WHERE id = 1`    |
```
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
```
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
```
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
```
| Dimension           | CRUD                        | Event Sourcing             |
|---------------------|-----------------------------|----------------------------|
| Simplicity          | ✅ Simple                   | ❌ Complex initial setup   |
| Flexibility         | 🟡 Limited schema evolution | ✅ Evolving event model    |
| Auditability        | ❌ Manual audit logs        | ✅ Built-in                |
| Rollback            | ❌ Hard                     | ✅ Replay past events      |
| Storage             | ✅ Compact (current only)   | ❌ Grows over time         |
| Read Model Custom   | ❌ Shared schema            | ✅ Any projection logic    |
```
---

## 📚 Further Reading

- [Martin Fowler: Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
- [Debezium for CDC](https://debezium.io/)
- [CQRS and Event Sourcing Patterns](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs)

---
