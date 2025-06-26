# 🛢️ PostgreSQL Essentials: CLI, Configuration, Users, and SQL

This guide covers day-to-day **PostgreSQL** usage, including CLI access, configuration, user/role management, SQL syntax, and admin tasks — ideal for server-side environments and SRE workflows.

---

## 📟 1. Using the PostgreSQL Client

### 🔹 Accessing `psql`

```
# Connect as default user
psql

# Connect to specific DB
psql -U postgres -d mydb -h 127.0.0.1 -p 5432
```

> Use `\q` to quit the shell, and `\?` for help.

### 🔹 Basic psql Commands

```
\l              -- list databases
\c mydb         -- connect to database
\dt             -- list tables
\d users        -- describe table schema
\du             -- list roles
```

---

## ⚙️ 2. PostgreSQL Configuration

### 🔹 Config File Locations

| File                     | Purpose                          |
|--------------------------|----------------------------------|
| `/etc/postgresql/14/main/postgresql.conf` | Main settings     |
| `/etc/postgresql/14/main/pg_hba.conf`     | Client access rules |
| `/etc/postgresql/14/main/pg_ident.conf`   | User mapping rules |

> On Alpine or Docker, you may find them under `/var/lib/postgresql/data/`.

### 🔹 Key Config Parameters

In `postgresql.conf`:

```conf
listen_addresses = '*'
port = 5432
max_connections = 100
shared_buffers = 256MB
log_min_duration_statement = 500  # slow query log
```

In `pg_hba.conf`:

```
# Allow remote access (host-based auth)
host    all             all             0.0.0.0/0           md5
```

Apply config changes with:

```
sudo systemctl restart postgresql
```

---

## 👤 3. Managing Roles and Access

### 🔹 Creating Users (Roles)

```
CREATE ROLE devuser WITH LOGIN PASSWORD 's3cret';
```

### 🔹 Creating Databases

```
CREATE DATABASE myappdb OWNER devuser;
```

### 🔹 Granting Privileges

```
GRANT ALL PRIVILEGES ON DATABASE myappdb TO devuser;
\c myappdb
GRANT SELECT, INSERT ON ALL TABLES IN SCHEMA public TO devuser;
```

### 🔹 Dropping Roles/DBs

```
DROP DATABASE myappdb;
DROP ROLE devuser;
```

---

## 🧠 4. PostgreSQL SQL Basics

### 🔹 Data Types

- `INTEGER`, `SERIAL`, `TEXT`, `VARCHAR`, `BOOLEAN`, `JSONB`, `TIMESTAMP`
- Use `UUID` and `JSONB` for modern systems

### 🔹 Creating Tables

```
CREATE TABLE users (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  email TEXT UNIQUE,
  metadata JSONB,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 🔹 Common Queries

```
SELECT * FROM users WHERE email = 'john@example.com';
INSERT INTO users (name, email) VALUES ('Jane', 'jane@example.com');
UPDATE users SET name = 'Janet' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

### 🔹 Joins and Aggregations

```
SELECT u.name, o.total
FROM users u
JOIN orders o ON u.id = o.user_id
WHERE o.total > 100;
```

---

## 🧪 5. Admin & Performance

### 🔹 Backups and Restores

```
# Backup
pg_dump -U postgres mydb > backup.sql

# Restore
psql -U postgres mydb < backup.sql
```

Use `pg_dumpall` for backing up all roles and DBs.

### 🔹 Analyze Queries

```
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'a@b.com';
```

### 🔹 Tune Performance

Edit `postgresql.conf`:

```conf
work_mem = 4MB
maintenance_work_mem = 64MB
effective_cache_size = 1GB
```

Use [`pgtune`](https://pgtune.leopard.in.ua/) to auto-suggest config values based on system memory.

---

## 🛡️ 6. PostgreSQL Security Tips

- Never expose port 5432 directly to the internet
- Always enforce `md5` or `scram-sha-256` auth in `pg_hba.conf`
- Use `ssl = on` for encrypted traffic
- Backup `pg_hba.conf` and `pg_ident.conf`

---

## 🧰 Useful psql Tips

```
-- Output query results as aligned table
\x

-- Export query to CSV
\COPY (SELECT * FROM users) TO 'users.csv' CSV HEADER;

-- Change prompt
\set PROMPT1 '%n@%/%R%# '
```

---

## 📚 Resources

- [PostgreSQL Official Docs](https://www.postgresql.org/docs/)
- [Postgres Cheatsheet](https://www.postgresqltutorial.com/)
- [pgAdmin UI Tool](https://www.pgadmin.org/)
- [pgtune Config Tool](https://pgtune.leopard.in.ua/)

---

## ✅ Summary

| Task                 | Command                                    |
|----------------------|--------------------------------------------|
| Connect to DB        | `psql -U user -d db`                       |
| Create Role/DB       | `CREATE ROLE`, `CREATE DATABASE`           |
| Grant Permissions    | `GRANT SELECT ON ... TO user`              |
| Backup               | `pg_dump`, `pg_dumpall`                    |
| Query Analysis       | `EXPLAIN ANALYZE ...`                      |
| Configure Access     | `pg_hba.conf`, `postgresql.conf`           |
