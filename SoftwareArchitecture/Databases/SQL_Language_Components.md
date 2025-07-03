# 🗃️ SQL Language Components: DDL, DQL, DML, DCL, and TCL

Structured Query Language (SQL) is organized into distinct language categories, each serving a specific purpose in interacting with databases. This guide explores five major groups:

- **DDL**: Data Definition Language  
- **DQL**: Data Query Language  
- **DML**: Data Manipulation Language  
- **DCL**: Data Control Language  
- **TCL**: Transaction Control Language

---

## 🧱 1. DDL – Data Definition Language

**Purpose**: Define and modify the structure of database objects such as tables, schemas, indexes, and views.

### 🔹 `CREATE`
Creates a new database object.

```
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 🔹 `ALTER`
Modifies an existing object.

```
ALTER TABLE users ADD COLUMN last_login TIMESTAMP;
ALTER TABLE users RENAME TO customers;
```

### 🔹 `DROP`
Deletes an object permanently.

```
DROP TABLE users;
DROP INDEX idx_users_email;
```

### 🔹 `TRUNCATE`
Deletes all records from a table **without logging individual row deletions**.

```
TRUNCATE TABLE users;
```

> DDL changes are auto-committed and **cannot be rolled back** in many databases.

---

## 🔍 2. DQL – Data Query Language

**Purpose**: Retrieve data from database tables.

### 🔹 `SELECT`
Used to query data with filtering, ordering, grouping, and joining.

```
-- Simple select
SELECT name, email FROM users;

-- Filtering with WHERE
SELECT * FROM users WHERE active = true;

-- Ordering
SELECT * FROM users ORDER BY created_at DESC;

-- Aggregation
SELECT COUNT(*) FROM users WHERE active = true;

-- Grouping
SELECT country, COUNT(*) FROM users GROUP BY country;

-- Join
SELECT o.id, u.name 
FROM orders o
JOIN users u ON o.user_id = u.id;
```

> DQL is **read-only** and does not modify the database state.

---

## ✏️ 3. DML – Data Manipulation Language

**Purpose**: Insert, update, and delete records in tables.

### 🔹 `INSERT`
Adds new rows.

```
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
```

### 🔹 `UPDATE`
Modifies existing rows.

```
UPDATE users SET active = false WHERE last_login < NOW() - INTERVAL '30 days';
```

### 🔹 `DELETE`
Removes rows.

```
DELETE FROM users WHERE active = false;
```

> Unlike DDL, **DML operations can be rolled back** if wrapped in a transaction.

---

## 🛡️ 4. DCL – Data Control Language

**Purpose**: Manage permissions and access control.

### 🔹 `GRANT`
Gives privileges to users or roles.

```
GRANT SELECT, INSERT ON users TO analyst;
GRANT ALL PRIVILEGES ON DATABASE mydb TO devuser;
```

### 🔹 `REVOKE`
Removes privileges.

```
REVOKE INSERT ON users FROM analyst;
```

> Permissions apply to tables, views, databases, schemas, or functions.

---

## 🔁 5. TCL – Transaction Control Language

**Purpose**: Control the execution of SQL statements as a single unit of work.

### 🔹 `BEGIN` / `START TRANSACTION`
Starts a transaction.

```
BEGIN;
-- or
START TRANSACTION;
```

### 🔹 `COMMIT`
Makes all changes made in the transaction permanent.

```
COMMIT;
```

### 🔹 `ROLLBACK`
Reverts all changes since the last `BEGIN`.

```
ROLLBACK;
```

### 🔹 `SAVEPOINT`
Sets a named point in a transaction.

```
SAVEPOINT update_point;
UPDATE users SET active = false;
ROLLBACK TO update_point;
```

> Use TCL to ensure **data integrity** and support atomic, consistent operations.

---

## ✅ Summary Table

| Category | Commands                 | Purpose                          |
|----------|--------------------------|----------------------------------|
| **DDL**  | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` | Define DB schema and structure   |
| **DQL**  | `SELECT`                 | Query data                       |
| **DML**  | `INSERT`, `UPDATE`, `DELETE` | Manipulate table data            |
| **DCL**  | `GRANT`, `REVOKE`        | Control access and privileges    |
| **TCL**  | `BEGIN`, `COMMIT`, `ROLLBACK`, `SAVEPOINT` | Manage transactions          |

---

## 📚 Resources

- [PostgreSQL SQL Commands Reference](https://www.postgresql.org/docs/current/sql-commands.html)
- [MySQL SQL Syntax Guide](https://dev.mysql.com/doc/refman/8.0/en/sql-statements.html)
- [SQLBolt](https://sqlbolt.com/) – Interactive SQL lessons
- [LeetCode Database Practice](https://leetcode.com/problemset/database/)

---
