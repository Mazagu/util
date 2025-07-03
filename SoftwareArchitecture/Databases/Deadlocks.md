# 🔄 Deadlocks: What They Are and How to Prevent Them

A **deadlock** occurs when two or more transactions are waiting for each other to release resources, causing all of them to be stuck indefinitely. It’s a critical issue in **concurrent systems**, especially databases and multithreaded applications.

---

## 🧩 What is a Deadlock?

A **deadlock** happens when:
- Transaction A holds **resource 1** and requests **resource 2**
- Transaction B holds **resource 2** and requests **resource 1**
- Both are blocked, waiting for each other

This leads to a **circular wait** where none can proceed.

### 🔁 Simple Example

```
// Pseudo SQL
Transaction 1:
LOCK table_a;
WAIT for table_b;

Transaction 2:
LOCK table_b;
WAIT for table_a;
```

---

## 🧱 Coffman Conditions (Necessary for Deadlock)

A deadlock can occur only if **all** four Coffman conditions hold:

1. **Mutual Exclusion**: At least one resource must be non-shareable.
2. **Hold and Wait**: A process is holding one resource and waiting for others.
3. **No Preemption**: Resources cannot be forcibly taken.
4. **Circular Wait**: A cycle of processes waiting on each other.

---

## 🛡 Deadlock Prevention Strategies

### 1. **Lock Ordering**
Acquire locks in a **predefined global order** to avoid circular waits.

```
// Ensure all transactions lock table_a before table_b
```

### 2. **Timeouts**
If a transaction waits too long, abort it.

### 3. **Try-Lock Pattern**
Attempt to acquire all required locks. If not successful, release and retry.

### 4. **Avoid Long Transactions**
Keep transactions short to minimize lock time.

### 5. **Use Higher Isolation Levels Carefully**
Higher isolation (like SERIALIZABLE) increases the chance of deadlocks.

---

## 🧯 Deadlock Detection & Recovery

Even with prevention, deadlocks may occur. That’s where **deadlock detection** comes in.

### How it Works:
- The system builds a **wait-for graph**.
- If it finds a **cycle**, it chooses a victim transaction to roll back.

**Databases like MySQL, Postgres, and SQL Server do this automatically.**

```
// PostgreSQL error
ERROR: deadlock detected
DETAIL: Process 123 waits for ShareLock on transaction 456
```

---

## 📦 Deadlocks in Multithreading

Deadlocks can also occur in code with multiple threads accessing shared resources.

### Prevention Techniques:
- Use reentrant locks (e.g., `std::recursive_mutex`)
- Apply lock hierarchy or ordering
- Prefer lock-free or async patterns if suitable

---

## ✅ Best Practices to Avoid Deadlocks

| Practice                        | Benefit                                  |
|---------------------------------|-------------------------------------------|
| Lock in consistent order        | Prevents circular wait                    |
| Keep transactions short         | Reduces lock duration                     |
| Use retry logic on failure      | Helps in recovery                         |
| Monitor and log long waits      | Helps detect patterns early               |
| Use tools like pg_stat_activity| Identify blocking processes in Postgres   |

---

## 📚 Further Reading

- [MySQL Deadlocks](https://dev.mysql.com/doc/refman/8.0/en/innodb-deadlocks.html)
- ["Dining Philosophers Problem"](https://en.wikipedia.org/wiki/Dining_philosophers_problem)

---
