# 🗄️ System Design: Database Design Deep Dive

Database design is crucial for system scalability, performance, and maintainability. This guide walks through the key principles and decisions you’ll face when designing the database layer in a system.

---

## 🧱 1. Choose the Right Type of Database

### 📊 Relational Databases (SQL)
- **Examples**: PostgreSQL, MySQL, SQL Server
- **Use When**:
  - Strong data integrity is required (ACID)
  - Complex joins and transactions are needed
  - Data model is well-structured and stable

### 📂 Non-Relational Databases (NoSQL)
- **Examples**: MongoDB, Cassandra, DynamoDB, Redis
- **Types**:
  - **Document** (e.g., MongoDB)
  - **Key-Value** (e.g., Redis)
  - **Columnar** (e.g., Cassandra)
  - **Graph** (e.g., Neo4j)
- **Use When**:
  - Flexible or evolving schemas
  - High write throughput
  - Denormalized data fits use case

---

## 🧩 2. Data Modeling

### 🔄 Normalization
- Avoids data duplication
- Promotes consistency
- Better for write-heavy systems

### ⚡ Denormalization
- Improves read performance
- Avoids complex joins
- Used in analytics or read-heavy apps

### ✅ Best Practices
- Use clear, consistent naming conventions
- Design around access patterns
- Choose appropriate data types
- Index frequently queried fields

---

## 🗂️ 3. Primary Keys and Indexes

- Use **surrogate keys** (e.g., UUIDs, auto-increment) vs **natural keys** based on needs
- Add **indexes** for:
  - Foreign keys
  - Frequently filtered/sorted fields
  - Full-text search (where needed)
- Monitor index bloat and write penalties

---

## ⚙️ 4. Relationships

- **1:1** – use foreign keys
- **1:N** – foreign key on the "many" side
- **M:N** – use a join table
- In NoSQL: embed vs reference depends on read/write patterns

---

## 🔐 5. Data Integrity

- Use **constraints**: NOT NULL, UNIQUE, FOREIGN KEY, CHECK
- Enforce **referential integrity** where applicable
- Consider **soft deletes** vs hard deletes

---

## 🔁 6. Transactions

- Required for operations that affect multiple tables
- ACID compliance is a must for banking, inventory, etc.
- NoSQL: support varies (MongoDB supports multi-doc transactions since v4.0)

---

## 🔍 7. Query Optimization

- Use `EXPLAIN` to understand query plans
- Avoid N+1 queries (especially in ORMs)
- Use pagination for large results (`LIMIT`, `OFFSET`, or cursors)
- Consider materialized views or caching for expensive reads

---

## 🌍 8. Scaling Strategy

### 🧭 Vertical Scaling
- Add more CPU, RAM, storage
- Limited by hardware

### 🌐 Horizontal Scaling
- **Read Replicas** – offload reads
- **Sharding** – split data across nodes (by user ID, region, etc.)
- **Multi-region** – reduce latency, increase availability

---

## 📦 9. Caching

- Use Redis or Memcached for:
  - Frequently read data
  - Expensive joins or aggregates
  - Session storage

---

## 🛡️ 10. Security & Compliance

- Protect PII, PCI, HIPAA data
- Enable TLS for in-transit encryption
- Use row-level security or RBAC
- Regular backups and audits

---

## 📅 11. Migrations and Schema Evolution

- Use versioned migration tools:
  - SQL: Flyway, Liquibase
  - ORMs: Eloquent (Laravel), Hibernate (Java), Prisma (Node)
- Avoid destructive migrations during peak hours
- Maintain backward compatibility across deployments

---

## 📓 12. Documentation

- Document:
  - ERDs (Entity-Relationship Diagrams)
  - Table purpose and key fields
  - Relationships and constraints

---

## 📌 Summary Checklist

- [ ] Chose the right DB for use case (SQL vs NoSQL)
- [ ] Modeled data for access patterns
- [ ] Enforced integrity with constraints
- [ ] Optimized queries and indexing
- [ ] Planned for scaling, backup, and migrations
