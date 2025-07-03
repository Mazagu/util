# 🔐 Deep Dive into Database Locks

**Database locking** is a concurrency control mechanism that ensures data consistency and integrity when multiple transactions access the same data simultaneously.

Locks prevent conflicts such as dirty reads, lost updates, and uncommitted data access.

---

## 📌 Why Are Locks Important?

- ✅ Prevent data corruption from simultaneous writes
- ✅ Ensure **ACID** (Atomicity, Consistency, Isolation, Durability)
- ✅ Allow safe concurrent access
- ✅ Enable isolation levels (READ COMMITTED, SERIALIZABLE, etc.)

---

## 🧠 Core Lock Types

### 🔄 Shared Lock (S Lock)

- Acquired when **reading** data
- Allows **other shared locks** (multiple readers)
- **Blocks exclusive locks**

```
// SQL Server
SELECT * FROM orders WITH (HOLDLOCK);
```

✅ Good for consistency  
⚠️ Blocks writers

---

### 🔒 Exclusive Lock (X Lock)

- Acquired when **writing** data (INSERT, UPDATE, DELETE)
- **Blocks all other locks**, including shared
- Only **one exclusive lock** allowed on a resource

```
// MySQL (InnoDB)
START TRANSACTION;
UPDATE products SET price = price * 1.1 WHERE category = 'Electronics';
```

✅ Ensures safe writes  
⚠️ Can lead to contention

---

### 🔁 Update Lock (U Lock)

- Used **before acquiring exclusive locks** to prevent deadlocks
- Only one update lock is allowed until it's escalated to exclusive
- Common in SQL Server

```
-- SQL Server hint:
SELECT * FROM accounts WITH (UPDLOCK);
```

✅ Prevents deadlocks in read-then-write patterns  
⚠️ Confusing to debug

---

### 🧱 Schema Locks

- Lock **metadata structures** like tables, indexes, constraints
- Acquired during `ALTER TABLE`, `CREATE`, or `DROP`
- Prevents reads/writes while schema changes

✅ Maintains metadata consistency  
⚠️ Can block regular queries if schema change is long

---

### 📦 Bulk Update Lock (BU Lock)

- Used for **bulk insert/update operations**
- Allows high throughput by **restricting concurrency**
- Mostly seen in SQL Server and during `BULK INSERT`

✅ Fast bulk operations  
⚠️ Prevents other access

---

## 🔍 Granularity Levels

### 1. Row-level Lock

- Locks a **single row**
- Supported by InnoDB, PostgreSQL, SQL Server

✅ High concurrency  
⚠️ Overhead managing many locks

---

### 2. Page-level Lock

- Locks a **data page** (typically 8KB)
- Used in some SQL Server/older engines

✅ Balance between row/table  
⚠️ Can cause contention if multiple rows per page accessed

---

### 3. Table-level Lock

- Locks **entire table**
- Used by MyISAM and optionally by InnoDB or during DDL

```
// MySQL
LOCK TABLES orders WRITE;
```

✅ Simple to manage  
⚠️ Low concurrency

---

## 🔎 Other Locks and Concepts

### 🔐 Intention Locks

- Metadata locks that **signal intent** to acquire a row/table lock
- Help coordinate between lock levels
- Common in InnoDB

| Lock | Meaning |
|------|---------|
| IS   | Intention to acquire shared lock at lower level |
| IX   | Intention to acquire exclusive lock at lower level |

---

### 🔑 Key Range Locks

- Lock a **range of keys** to prevent phantom reads
- Used in SERIALIZABLE isolation (e.g. `SELECT … FOR UPDATE` with range)

✅ Needed for correct SERIALIZABLE behavior  
⚠️ May block inserts even outside exact keys

---

## 🧪 Locking Modes Summary Table

| Lock Type         | Read Allowed | Write Allowed | Use Case                  |
|-------------------|--------------|----------------|---------------------------|
| Shared (S)        | ✅            | ❌             | Safe reads                |
| Exclusive (X)     | ❌            | ✅             | Safe writes               |
| Update (U)        | ❌            | Later → ✅     | Deadlock prevention       |
| Schema            | ❌            | ❌             | DDL operations            |
| Bulk Update (BU)  | ❌            | ✅             | Mass ingestion            |
| Row-level         | ✅            | ✅ (isolated)  | High concurrency systems  |
| Page-level        | ✅            | ✅             | Medium granularity        |
| Table-level       | ❌            | ❌             | Full-table updates        |

---

## 🔁 Lock Escalation

Some engines (e.g. SQL Server) escalate from **row/page → table-level** locks automatically when:

- Too many row locks
- Resource pressure
- Lock threshold exceeded

✅ Prevents memory exhaustion  
⚠️ May cause unexpected contention

---

## 🧯 Deadlocks and Prevention

Two transactions holding locks on resources the other needs → deadlock.

### Coffman Conditions for Deadlock:

1. Mutual exclusion  
2. Hold and wait  
3. No preemption  
4. Circular wait

🛠️ Strategies:

- **Set lock timeout**
- **Retry with backoff**
- **Always acquire locks in same order**
- **Use optimistic locking where applicable**

---

## 🧪 Monitoring Locks

- **PostgreSQL**: `pg_locks`
- **MySQL**: `INFORMATION_SCHEMA.INNODB_LOCKS`
- **SQL Server**: `sys.dm_tran_locks`

---

## ✅ Summary

| Category        | Examples                         | When to Use                        |
|----------------|----------------------------------|-------------------------------------|
| **Granular Locks** | Row, Page, Table               | Based on concurrency/performance    |
| **Semantic Locks** | Shared, Exclusive, Update      | Based on read/write behavior        |
| **Metadata Locks** | Schema, Intention              | Structural or hierarchical locking  |
| **Optimizations**  | Key Range, Bulk Update         | Specific high-throughput needs      |

---

## 📚 Further Reading

- [PostgreSQL Concurrency Control](https://www.postgresql.org/docs/current/mvcc.html)
- [SQL Server Locking Overview](https://learn.microsoft.com/en-us/sql/relational-databases/sql-server-locks)
- [MySQL InnoDB Locking](https://dev.mysql.com/doc/refman/8.0/en/innodb-locking.html)
- [High Performance MySQL (O'Reilly)](https://www.oreilly.com/library/view/high-performance-mysql/9781449332471/)

---
