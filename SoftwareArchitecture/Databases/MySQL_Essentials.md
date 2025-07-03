# 🛢️ MySQL Essentials: Client, Configuration, Users, and SQL

This guide covers all essential aspects of working with **MySQL** in a server environment — including the MySQL CLI client, configuration settings, user and privilege management, and SQL best practices.

---

## 📟 1. Using the MySQL Client

### 🔹 Connecting to a MySQL Server

```
mysql -u root -p
mysql -h 127.0.0.1 -P 3306 -u myuser -p
```

> Use `--protocol=tcp` to avoid socket-based connections if needed.

### 🔹 Basic Client Commands

```
SHOW DATABASES;
USE mydb;
SHOW TABLES;
DESCRIBE users;
EXIT;
```

### 🔹 Running SQL Scripts

```
mysql -u root -p mydb < ./setup.sql
```

---

## ⚙️ 2. Configuration and Server Settings

### 🔹 Key Configuration File Locations

| Distro     | Default Config File              |
|------------|----------------------------------|
| Debian     | `/etc/mysql/my.cnf`              |
| Alpine     | `/etc/my.cnf`                    |
| Docker     | Custom volume or ENV configs     |

### 🔹 Important Configuration Directives

```ini
[mysqld]
bind-address = 0.0.0.0       # Allow external connections
port = 3306
max_connections = 200
sql_mode = STRICT_TRANS_TABLES,NO_ENGINE_SUBSTITUTION
innodb_buffer_pool_size = 1G
log_error = /var/log/mysql/error.log
```

Use `SHOW VARIABLES;` to inspect live settings.

### 🔹 Service Management

```
# Debian/Ubuntu
sudo systemctl restart mysql
sudo systemctl status mysql

# Alpine/OpenRC
sudo rc-service mariadb start
```

---

## 👤 3. Managing MySQL Users and Permissions

### 🔹 Creating Users

```
CREATE USER 'appuser'@'%' IDENTIFIED BY 's3cret';
```

- `'%'` means any host (you can use `localhost` or specific IP)
- Avoid using `root` in applications

### 🔹 Granting Permissions

```
GRANT SELECT, INSERT, UPDATE ON mydb.* TO 'appuser'@'%';
FLUSH PRIVILEGES;
```

### 🔹 Checking Permissions

```
SHOW GRANTS FOR 'appuser'@'%';
```

### 🔹 Deleting a User

```
DROP USER 'appuser'@'%';
```

---

## 🧠 4. MySQL SQL Features & Tips

### 🔹 Data Types

- Common: `INT`, `VARCHAR(n)`, `TEXT`, `DATE`, `DATETIME`, `BOOLEAN`
- Use `BIGINT` for IDs in high-scale systems

### 🔹 Table Creation

```
CREATE TABLE users (
  id INT AUTO_INCREMENT PRIMARY KEY,
  name VARCHAR(100),
  email VARCHAR(100) UNIQUE,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 🔹 Indexing for Performance

```
CREATE INDEX idx_email ON users(email);
SHOW INDEX FROM users;
```

### 🔹 Query Basics

```
SELECT * FROM users WHERE email = 'john@example.com';
INSERT INTO users (name, email) VALUES ('Jane', 'jane@example.com');
UPDATE users SET name = 'Janet' WHERE id = 1;
DELETE FROM users WHERE id = 1;
```

### 🔹 Transactions

```
START TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;
```

> Use `ROLLBACK;` if something goes wrong.

---

## 🧪 5. Useful Utilities & Maintenance

### 🔹 Backup and Restore

```
# Dump
mysqldump -u root -p mydb > backup.sql

# Restore
mysql -u root -p mydb < backup.sql
</codexample>

### 🔹 Check Slow Queries

Edit config:

```ini
slow_query_log = 1
slow_query_log_file = /var/log/mysql/slow.log
long_query_time = 1
```

```
# Analyze slow queries
mysqldumpslow /var/log/mysql/slow.log
```

### 🔹 Monitoring

- `SHOW PROCESSLIST;`
- `SHOW STATUS;`
- `performance_schema` for advanced insights

---

## 🛡️ 6. Security Tips

- Don't allow root login from external hosts
- Use `mysql_secure_installation`
- Keep MySQL up to date
- Regularly rotate user passwords
- Use SSL for remote connections

---

## 📚 Resources

- [MySQL Documentation](https://dev.mysql.com/doc/)
- [MariaDB vs MySQL](https://mariadb.org/about/)
- [DigitalOcean MySQL Tutorials](https://www.digitalocean.com/community/tags/mysql)

---

## ✅ Summary

| Topic        | Command/Tool                             |
|--------------|-------------------------------------------|
| Connect      | `mysql -u root -p`                        |
| Config       | `/etc/mysql/my.cnf`, `SHOW VARIABLES;`    |
| Users        | `CREATE USER`, `GRANT`, `SHOW GRANTS`     |
| SQL          | `SELECT`, `JOIN`, `INDEX`, `TRANSACTION`  |
| Admin        | `mysqldump`, `slow_query_log`, `systemctl`|
