# 🧠 Relational Database Design – In-Depth Guide

Relational databases are one of the most commonly used systems for storing structured data. Designing them well is crucial for scalability, maintainability, and query performance.

---

## 🧾 What is SQL?

**SQL (Structured Query Language)** is the standard language used to interact with relational databases. It supports operations like:

- **DDL** (Data Definition Language): `CREATE`, `ALTER`, `DROP`
- **DML** (Data Manipulation Language): `INSERT`, `UPDATE`, `DELETE`
- **DQL** (Data Query Language): `SELECT`
- **DCL** (Data Control Language): `GRANT`, `REVOKE`

---

## 🔑 Keys in Relational Databases

### 1. **Primary Key**
- Uniquely identifies each record in a table.
- Cannot be `NULL`.
- Each table should have **one primary key**.

```
CREATE TABLE users (
  id INT PRIMARY KEY,
  username VARCHAR(100) UNIQUE
);
```

### 2. **Natural Key**
- A key that comes from real-world data (e.g. email, SSN).
- Can be used as a primary key but may change over time or leak information.

### 3. **Surrogate Key**
- A system-generated key (e.g., auto-increment ID).
- Preferred when natural keys are large or changeable.

### 4. **Foreign Key**
- Links a record in one table to another.
- Maintains **referential integrity**.

```
CREATE TABLE orders (
  id INT PRIMARY KEY,
  user_id INT,
  FOREIGN KEY (user_id) REFERENCES users(id)
);
```

---

## 🔁 Relationship Types

| Type              | Description                                 | Example             |
|-------------------|---------------------------------------------|---------------------|
| One-to-One        | Each record links to one in the other table | User → Profile      |
| One-to-Many       | One record maps to multiple in another      | User → Orders       |
| Many-to-Many      | Requires a join table                       | Students ↔ Courses  |

```
// Join table example
CREATE TABLE student_courses (
  student_id INT,
  course_id INT,
  PRIMARY KEY (student_id, course_id),
  FOREIGN KEY (student_id) REFERENCES students(id),
  FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

---

## 🔗 JOIN Types

| Type         | Description                                 |
|--------------|---------------------------------------------|
| INNER JOIN   | Returns only matching records               |
| LEFT JOIN    | Returns all records from left + matches     |
| RIGHT JOIN   | Returns all records from right + matches    |
| FULL JOIN    | All records with matches or NULLs           |
| CROSS JOIN   | Cartesian product of two tables             |
| SELF JOIN    | A table joined to itself                    |

```
SELECT u.name, o.amount
FROM users u
INNER JOIN orders o ON u.id = o.user_id;
```

---

## 📐 Normalization

Normalization reduces data redundancy and improves data integrity by structuring the data into logical tables.

| Normal Form | Description                                           |
|-------------|-------------------------------------------------------|
| 1NF         | Atomic values, no repeating groups                    |
| 2NF         | 1NF + No partial dependency on composite keys         |
| 3NF         | 2NF + No transitive dependencies                      |

✅ Normalize for integrity  
❗ Denormalize for performance (carefully)

---

## ✅ Best Practices

### ✔ Use Meaningful Table and Column Names
- Use `snake_case` or `camelCase` consistently
- Avoid vague names like `data1`, `table2`

### ✔ Prefer Surrogate Keys for Simplicity
- Helps avoid changes from business logic impacting keys

### ✔ Enforce Constraints
- Use `NOT NULL`, `CHECK`, `UNIQUE`, `FOREIGN KEY`, etc.

### ✔ Index Important Columns
- Especially for **JOIN**, **WHERE**, and **ORDER BY**

```
CREATE INDEX idx_user_id ON orders(user_id);
```

### ✔ Avoid EAV and Over-Normalization
- Don't go too far with normalization; balance with performance.

### ✔ Back Your Schema With Migrations
- Use tools like Flyway, Liquibase, Prisma, Django ORM

---

## 🗃️ Example Schema

```
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE
);

CREATE TABLE posts (
  id SERIAL PRIMARY KEY,
  user_id INT REFERENCES users(id),
  content TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

## 📘 Learning Resources

- 📚 *SQL Antipatterns* by Bill Karwin
- 📚 *Database Design for Mere Mortals* by Michael J. Hernandez
- 🔗 [dbdiagram.io](https://dbdiagram.io) – Visual schema design
- 🔗 [SQLBolt](https://sqlbolt.com) – Interactive SQL learning

---
