# 🧪 Deep Dive into ACID: Reliable Transactions in Databases

**ACID** is a set of properties that guarantee reliable processing of **transactions** in a database system. It stands for:

- **A**tomicity  
- **C**onsistency  
- **I**solation  
- **D**urability

These properties are the **foundation of relational databases** like MySQL, PostgreSQL, and also apply (with variations) to some NoSQL systems.

---

## 🧩 What is a Transaction?

A **transaction** is a sequence of operations performed as a single logical unit of work. All operations within a transaction must complete **successfully**, or **none** should take effect.

---

## 🔹 A — Atomicity

> All or nothing.

If a transaction involves multiple steps, **either all succeed**, or **the whole transaction is rolled back**.

### Example

```
START TRANSACTION;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

If the second update fails, the first is **rolled back** automatically.

---

## 🔹 C — Consistency

> Transactions must move the database from one valid state to another, preserving **integrity constraints**.

Examples of constraints:
- Primary keys
- Foreign keys
- Domain rules (e.g., balance ≥ 0)

### Example

```
-- Withdraw that causes balance to go negative (invalid state)
UPDATE accounts SET balance = balance - 1000 WHERE id = 1;
```

If there's a rule preventing negative balances, the DBMS will **reject the transaction**, preserving **consistency**.

---

## 🔹 I — Isolation

> Concurrent transactions do not interfere with each other.

### Why Isolation Matters

Imagine two users trying to transfer money at the same time. Isolation ensures **data remains correct and conflict-free**.

### Isolation Levels (SQL Standard)

| Level            | Dirty Read | Non-repeatable Read | Phantom Read |
|------------------|------------|----------------------|---------------|
| Read Uncommitted | ✅         | ✅                   | ✅            |
| Read Committed   | ❌         | ✅                   | ✅            |
| Repeatable Read  | ❌         | ❌                   | ✅            |
| Serializable     | ❌         | ❌                   | ❌            |

Each level trades **performance vs correctness**.

### Example in PostgreSQL

```
BEGIN TRANSACTION ISOLATION LEVEL SERIALIZABLE;
```

> Note: Some databases (like MySQL with InnoDB) default to **Repeatable Read**.

---

## 🔹 D — Durability

> Once a transaction is committed, it is **permanently stored**, even in the event of power loss or crash.

### How Databases Achieve Durability

- Write-ahead logs (WAL)
- Disk flushing on commit
- Replication & backups

### Example

If your app crashes **after a commit**, the data is **still saved**.

```
COMMIT;
```

---

## 🧪 Practical Example

```
START TRANSACTION;

UPDATE products SET stock = stock - 1 WHERE id = 42;
INSERT INTO orders (product_id, user_id) VALUES (42, 5);

COMMIT;
```

If any part fails, the transaction **won't apply**—ensuring atomicity and consistency.

---

## 🛠️ ACID in Distributed Systems

While traditional RDBMSs implement ACID on a **single node**, distributed systems often face **trade-offs** due to:

- Network partitions (see: **CAP Theorem**)
- Latency
- Availability

### Examples:

| DBMS           | ACID Support          |
|----------------|------------------------|
| PostgreSQL     | Full ACID              |
| MySQL (InnoDB) | Full ACID              |
| MongoDB        | ACID (since 4.0, multi-doc) |
| Cassandra      | Tunable Consistency, not full ACID |
| CockroachDB    | Distributed ACID       |

---

## ❌ Common Pitfalls

- Not using transactions in code (e.g., multiple `UPDATE`s outside `BEGIN`)
- Misunderstanding isolation levels, leading to race conditions
- Assuming NoSQL systems are fully ACID by default

---

## ✅ Summary Table

| Property   | What It Ensures                        | Failure Without It                     |
|------------|----------------------------------------|----------------------------------------|
| Atomicity  | All steps succeed or none do           | Partial updates, inconsistent state    |
| Consistency| Integrity constraints remain enforced  | Invalid data (e.g., foreign key errors)|
| Isolation  | No conflict between concurrent txs     | Race conditions, dirty reads           |
| Durability | Data is safe after commit              | Lost transactions on crash             |

---

## 📚 Resources

- [PostgreSQL Transactions](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [MySQL InnoDB & ACID](https://dev.mysql.com/doc/refman/8.0/en/innodb-transaction-model.html)
- [MongoDB Transactions](https://www.mongodb.com/docs/manual/core/transactions/)

---
