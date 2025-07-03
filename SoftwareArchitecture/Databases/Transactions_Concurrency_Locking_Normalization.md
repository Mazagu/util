# 📚 Guide: Database Transactions, Concurrency, Locking, and Normalization

This guide covers foundational concepts that ensure data integrity, consistency, and performance in transactional systems — including **ACID properties**, **concurrency control**, **locking**, and **data modeling trade-offs** between **normalization** and **denormalization**.

---

## ✅ 1. Database Transactions & ACID

A **transaction** is a unit of work that must be **atomic**, **consistent**, **isolated**, and **durable**.

### 🧪 ACID Properties

| Property       | Description                                          |
|----------------|------------------------------------------------------|
| **Atomicity**  | All steps succeed or none do                         |
| **Consistency**| Must leave DB in valid state (constraints respected) |
| **Isolation**  | Transactions don't interfere with each other         |
| **Durability** | Once committed, it remains — even after crash        |

### Example: Atomic transfer between accounts

```
BEGIN;

UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;

COMMIT;
```

---

## 🔀 2. Concurrency & Isolation Levels

Concurrency allows multiple transactions to run in parallel. To prevent race conditions and anomalies, databases use **isolation levels**:

| Isolation Level        | Prevents...                       | Notes                  |
|------------------------|-----------------------------------|------------------------|
| Read Uncommitted       | ❌ Nothing                        | Fast, unsafe          |
| Read Committed         | ✅ Dirty Reads                    | Default in many DBs   |
| Repeatable Read        | ✅ Non-repeatable Reads           | Good for reads        |
| Serializable           | ✅ Phantom Reads, full isolation  | Strict, slowest       |

### Common concurrency issues:

| Issue                  | Description                                        |
|------------------------|----------------------------------------------------|
| Dirty read             | Read uncommitted data                              |
| Non-repeatable read    | Same query returns different results mid-tx        |
| Phantom read           | New rows appear that match query mid-tx            |
| Lost update            | Overwrite data written by another tx               |

---

## 🔒 3. Locking Strategies

Locks control access to rows, tables, or ranges.

### 🔵 Pessimistic Locking

- Locks data before using it
- Prevents conflicts by blocking access
- Slower, may cause contention

```
-- Example: SELECT ... FOR UPDATE (pessimistic)
BEGIN;
SELECT * FROM accounts WHERE id = 1 FOR UPDATE;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
COMMIT;
```

### 🟢 Optimistic Locking

- Assumes no conflict — verifies before write
- Uses **version/timestamp** fields
- Great for low-contention scenarios

```
-- Example: optimistic version check
UPDATE products
SET stock = stock - 1, version = version + 1
WHERE id = 123 AND version = 4;
```

If 0 rows affected → retry, conflict detected.

---

## 🔁 4. Normalization vs Denormalization

These are two competing strategies for modeling data in relational databases.

### 🧪 Normalization

- Splits data into multiple related tables
- Reduces **redundancy**, improves **integrity**

#### Example:

| Table: Orders        | Table: Customers         |
|----------------------|--------------------------|
| id, customer_id, ... | id, name, email, ...     |

### Pros
✅ Smaller storage  
✅ Fewer anomalies  
✅ Easier to update single source of truth

### Cons  
❌ More joins = slower queries  
❌ Complex schema evolution  
❌ Tougher for analytics

---

### 📦 Denormalization

- Combines data to reduce joins
- Prioritizes **read performance** and **query simplicity**

#### Example:

| Table: Orders                          |
|----------------------------------------|
| id, customer_name, customer_email, ... |

### Pros
✅ Fewer joins  
✅ Faster reads  
✅ Simple queries for dashboards

### Cons  
❌ Data duplication  
❌ Risk of inconsistencies  
❌ Update anomalies (requires syncing)

---

## 🆚 Comparison Table

| Feature               | Normalized               | Denormalized               |
|-----------------------|--------------------------|----------------------------|
| Write Performance     | Better                   | Worse (updates touch more) |
| Read Performance      | Worse (joins)            | Better (fewer joins)       |
| Storage Usage         | Efficient                | Redundant                  |
| Data Consistency      | Strong                   | Weaker                     |
| Analytics Use         | Harder                   | Easier                     |

---

## 🧠 Bonus: When to Choose What?

| Use Case                       | Recommendation                         |
|--------------------------------|-----------------------------------------|
| OLTP (banking, inventory)      | ✅ Normalize                           |
| OLAP (dashboards, BI)          | ✅ Denormalize                         |
| Mixed workloads                | 🤝 Hybrid (views, materialized tables) |

---

## 📚 Further Reading

- [PostgreSQL Transaction Docs](https://www.postgresql.org/docs/current/tutorial-transactions.html)
- [SQL Isolation Levels Explained](https://www.cockroachlabs.com/blog/sql-isolation-levels-explained/)
- [Optimistic vs Pessimistic Locking (Baeldung)](https://www.baeldung.com/jpa-optimistic-locking)
- [Normalization Forms (1NF to 5NF)](https://www.studytonight.com/dbms/database-normalization.php)
- [Martin Kleppmann - CRDTs, Transactions, and Concurrency](https://martin.kleppmann.com/2020/07/06/crdt-hard-parts-hydra.html)

---
