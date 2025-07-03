# 🧩 Guide to Database Models: From Flat Files to Star Schemas

A **Database Model** defines how data is structured, stored, and related within a database system. Choosing the right model depends on the system’s complexity, access patterns, performance, and consistency needs.

This guide covers the most widely used database models with examples and trade-offs.

---

## 📄 1. Flat File Model

### 🧠 Concept

The simplest model — all data is stored in a **single table or file**, often in CSV or TSV format.

### ✅ Use Cases

- Simple configurations or exports
- Logging systems
- Legacy systems

```
// Example CSV
UserId,Name,Age
1,Alice,30
2,Bob,25
```

### ⚠️ Limitations

- No relationships between records
- Not scalable or efficient for large, complex datasets

---

## 🌲 2. Hierarchical Model

### 🧠 Concept

Data is organized in a **tree-like structure**, with parent-child relationships (like XML).

```
// Pseudocode structure
Company
 ├── Department
 │    ├── Employee
 │    └── Employee
 └── Department
      └── Employee
```

### ✅ Use Cases

- XML/JSON storage
- Mainframes (IBM IMS)
- File systems

### ⚠️ Limitations

- Rigid hierarchy
- Hard to model many-to-many relationships

---

## 🔗 3. Network Model

### 🧠 Concept

Like the hierarchical model, but supports **many-to-many relationships** via graph-like links.

```
// Simplified example
Employee {
  ID: 1
  WorksOn: [Project1, Project2]
}
```

### ✅ Use Cases

- Telecommunications
- Complex data structures in legacy systems

### ⚠️ Limitations

- Complex to design and query
- Largely replaced by relational and graph models

---

## 🧱 4. Relational Model

### 🧠 Concept

Data is stored in **tables (relations)**. Relationships are managed via **foreign keys**.

```
// SQL table example
CREATE TABLE Orders (
  OrderID INT PRIMARY KEY,
  UserID INT,
  Amount DECIMAL,
  FOREIGN KEY (UserID) REFERENCES Users(UserID)
);
```

### ✅ Use Cases

- Transactional systems
- Enterprise business apps
- Any domain requiring **ACID compliance**

### ⚖️ Trade-offs

| Pros                  | Cons                       |
|-----------------------|----------------------------|
| Strong data integrity | Harder to scale horizontally |
| SQL is powerful       | Complex joins impact performance |

---

## ⭐ 5. Star Schema (Dimensional Model)

### 🧠 Concept

Used in **data warehousing**, a **central fact table** connects to **denormalized dimension tables**.

```
// Star schema layout
FactSales
 ├── DateID
 ├── ProductID
 ├── StoreID

DimensionDate
DimensionProduct
DimensionStore
```

### ✅ Use Cases

- OLAP systems
- BI dashboards
- High-speed analytical queries

### ⚖️ Trade-offs

| Pros                    | Cons                          |
|-------------------------|-------------------------------|
| Fast read performance   | Redundant data (denormalized) |
| Simpler to query        | Less normalized = update cost |

---

## ❄️ 6. Snowflake Schema

### 🧠 Concept

Like a star schema, but **dimension tables are normalized** (split into sub-tables).

```
// Snowflake schema layout
FactSales
 ├── ProductID

DimensionProduct
 └── ProductCategory
      └── ProductDepartment
```

### ✅ Use Cases

- Data warehouses with complex hierarchies
- When storage efficiency is prioritized

### ⚖️ Trade-offs

| Pros                        | Cons                       |
|-----------------------------|----------------------------|
| More normalized             | More complex queries       |
| Space efficient             | Requires joins             |

---

## 🧮 7. Object-Oriented Model

### 🧠 Concept

Combines database concepts with **object-oriented programming**. Objects contain data and methods.

```
// Example object
class User {
  int id;
  String name;
  Address address;
}
```

### ✅ Use Cases

- Applications with complex data types
- Object databases (ObjectDB, db4o)

---

## 🧠 8. Document Model (NoSQL)

### 🧠 Concept

Stores data as **JSON-like documents**, which can be deeply nested.

```
// MongoDB document
{
  "userId": "123",
  "name": "Alice",
  "orders": [
    { "orderId": "a1", "amount": 50 },
    { "orderId": "a2", "amount": 100 }
  ]
}
```

### ✅ Use Cases

- Flexible schema
- Content management systems
- Real-time apps

---

## 🕸️ 9. Graph Model

### 🧠 Concept

Data is stored as **nodes and edges**, ideal for traversing relationships.

```
// Graph elements
(User)-[FRIENDS_WITH]->(AnotherUser)
```

### ✅ Use Cases

- Social networks
- Fraud detection
- Recommendation systems

Examples: Neo4j, Amazon Neptune

---

## 📦 Comparison Summary

| Model             | Use Case                       | Strengths                        | Weaknesses                     |
|-------------------|--------------------------------|----------------------------------|-------------------------------|
| Flat              | Logs, configs                  | Simplicity                       | No relationships              |
| Hierarchical      | XML data, filesystems          | Tree-like queries                | Rigid                         |
| Network           | Legacy complex systems         | Many-to-many                    | Complex navigation            |
| Relational        | Transactions, business apps    | ACID, mature SQL support        | Scaling joins is difficult    |
| Star              | Data warehousing               | Fast reads, easy queries        | Denormalized                  |
| Snowflake         | Data warehouses (normalized)   | Storage-efficient               | Complex joins                 |
| Object-Oriented   | OOP apps                       | Aligns with code models         | Less adoption                 |
| Document          | CMS, NoSQL apps                | Flexible, nested structures     | Limited query capabilities    |
| Graph             | Networked data                 | Relationship traversal is fast  | Less suited for tabular data  |

---

## 📘 Resources

- [Amazon DynamoDB (Document & Key-Value)](https://aws.amazon.com/dynamodb/)
- [Graph Theory and Databases – Neo4j Docs](https://neo4j.com/docs/)
- [MongoDB Data Models](https://www.mongodb.com/docs/manual/core/data-models/)

---
