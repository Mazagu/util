# 🍃 MongoDB Essentials: CLI, Configuration, Users, and Data Modeling

MongoDB is a flexible NoSQL document-oriented database popular for rapid development and scalable systems. This guide covers the MongoDB shell (`mongosh`), server configuration, user access, data modeling, indexing, and performance insights.

---

## 🧑‍💻 1. Using the MongoDB Shell (`mongosh`)

### 🔹 Connecting to MongoDB

```
mongosh
mongosh "mongodb://localhost:27017"
mongosh "mongodb://user:pass@localhost:27017/admin"
```

### 🔹 Common Shell Commands

```
show dbs
use mydb
show collections
db.users.find()
exit
```

> MongoDB uses **JavaScript syntax** for shell queries.

---

## ⚙️ 2. Configuration for Production

### 🔹 Config File Location

Typical file: `/etc/mongod.conf`

Example config for a secure server:

```yaml
net:
  port: 27017
  bindIp: 127.0.0.1

security:
  authorization: enabled

storage:
  dbPath: /var/lib/mongodb

systemLog:
  destination: file
  path: /var/log/mongodb/mongod.log
  logAppend: true
```

### 🔹 Managing the Service

```
# Debian-based
sudo systemctl restart mongod
sudo systemctl status mongod
```

---

## 👤 3. User and Role Management

### 🔹 Create Admin User

```
use admin
db.createUser({
  user: "admin",
  pwd: "securepass",
  roles: [ { role: "userAdminAnyDatabase", db: "admin" } ]
})
```

### 🔹 Create Application User

```
use mydb
db.createUser({
  user: "appuser",
  pwd: "app123",
  roles: [ { role: "readWrite", db: "mydb" } ]
})
```

### 🔹 List and Drop Users

```
db.getUsers()
db.dropUser("appuser")
```

---

## 📦 4. MongoDB Documents & Collections

### 🔹 Insert and Query Documents

```
db.users.insertOne({ name: "Jane", age: 30, active: true })
db.users.find({ age: { $gte: 18 } })
```

### 🔹 Update and Delete

```
db.users.updateOne({ name: "Jane" }, { $set: { age: 31 } })
db.users.deleteMany({ active: false })
```

> MongoDB is **schema-less**, but good design matters.

---

## 📐 5. Data Modeling in MongoDB

| Strategy         | Description                                          | Example Use Case                     |
|------------------|------------------------------------------------------|--------------------------------------|
| Embedding        | Store related data in the same document              | Blog post with comments              |
| Referencing      | Store relations via ObjectId references              | User references roles or profiles    |
| Denormalization  | Copy data across documents for performance           | Order with duplicated user info      |

### 🔹 Embedded Example

```
{
  _id: ObjectId("..."),
  title: "Post Title",
  comments: [
    { user: "A", text: "Nice!" },
    { user: "B", text: "Cool!" }
  ]
}
```

---

## ⚡ 6. Indexing for Performance

### 🔹 Creating and Listing Indexes

```
db.users.createIndex({ email: 1 }, { unique: true })
db.users.getIndexes()
```

### 🔹 Compound and TTL Indexes

```
db.logs.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
```

> Use **indexes wisely** to speed up reads, but avoid over-indexing.

---

## 🛡️ 7. Security Best Practices

- **Enable authentication** in `mongod.conf`
- Restrict access with `bindIp`
- Enforce **strong passwords**
- Limit privileges to specific databases
- Use **encrypted connections** via TLS/SSL
- Regular backups via `mongodump`

---

## 🛠️ 8. Maintenance & Tools

### 🔹 Backup and Restore

```
# Backup
mongodump --db=mydb --out=/backups/mydb

# Restore
mongorestore --db=mydb /backups/mydb
```

### 🔹 Monitoring

Use:

```
db.serverStatus()
db.stats()
```

Or external tools:

- MongoDB Atlas (GUI + monitoring)
- `mongotop`, `mongostat`
- Prometheus exporters

---

## 🚀 9. Advanced Concepts

### 🔹 Aggregation Framework

```
db.orders.aggregate([
  { $match: { status: "shipped" } },
  { $group: { _id: "$customerId", total: { $sum: "$amount" } } }
])
```

### 🔹 Transactions (Replica Set required)

```
const session = db.getMongo().startSession()
session.startTransaction()
session.getDatabase("mydb").users.updateOne({ id: 1 }, { $inc: { balance: -100 } })
session.getDatabase("mydb").users.updateOne({ id: 2 }, { $inc: { balance: 100 } })
session.commitTransaction()
session.endSession()
```

> Full ACID transactions are supported only in replica sets or sharded clusters.

---

## 📚 Resources

- [MongoDB Docs](https://www.mongodb.com/docs/)
- [MongoDB University (Free Courses)](https://learn.mongodb.com/)
- [MongoDB Atlas](https://www.mongodb.com/cloud/atlas)
- [Mongoose ODM](https://mongoosejs.com/) (for Node.js apps)

---

## ✅ Summary

| Task                 | Tool/Command                                |
|----------------------|---------------------------------------------|
| Connect              | `mongosh`                                   |
| Create DB/User       | `db.createUser()`                           |
| Insert/Query         | `db.collection.find()`, `insertOne()`       |
| Indexing             | `createIndex()`, `getIndexes()`             |
| Security             | `auth`, `bindIp`, roles, password           |
| Backup/Restore       | `mongodump`, `mongorestore`                 |
| Monitor              | `db.stats()`, `db.serverStatus()`           |
| Transactions         | `startSession()`, `commitTransaction()`     |
