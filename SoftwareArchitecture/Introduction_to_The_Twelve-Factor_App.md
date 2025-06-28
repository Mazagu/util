# 🧪 Introduction to The Twelve-Factor App

**The Twelve-Factor App** is a methodology for building **software-as-a-service (SaaS)** apps that are **portable, resilient, and scalable**. Originally developed by engineers at Heroku, it defines **twelve principles** that guide the development of cloud-native applications.

This guide walks through each factor with clear explanations and practical examples.

---

## 🧱 1. Codebase

**One codebase tracked in version control, many deploys**

- A single code repository (e.g., Git) per app.
- Multiple environments (dev, staging, prod) use the **same codebase**, but different **configurations**.

✅ *Best Practice*: Use Git with clearly separated branches per environment.

---

## ⚙️ 2. Dependencies

**Explicitly declare and isolate dependencies**

- Declare all dependencies (libraries, frameworks) via a manifest:
  - `package.json` for Node.js
  - `requirements.txt` for Python
  - `pom.xml` for Java/Maven

- Use dependency isolation tools like **virtualenv**, **Docker**, or **language-specific environments**.

```
// Node.js example
{
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

---

## 🔧 3. Config

**Store configuration in the environment**

- Environment-specific config (DB URLs, API keys) should not live in the codebase.
- Use environment variables or secret managers.

```
// Bad
db.connect('postgres://user:pass@localhost')

// Good
db.connect(process.env.DATABASE_URL)
```

---

## 💾 4. Backing Services

**Treat backing services as attached resources**

- Databases, caches, and queues should be treated as **external resources**.
- Changing a backing service should only require a config change.

Examples:
- Redis, RabbitMQ, PostgreSQL, S3, SMTP

---

## 🚀 5. Build, Release, Run

**Strictly separate build and run stages**

- **Build**: Compile assets, install dependencies
- **Release**: Combine build with config (env vars)
- **Run**: Execute the app (e.g., start the web server)

Each stage should be **repeatable and idempotent**.

---

## 👷 6. Processes

**Execute the app as one or more stateless processes**

- App state should be stored in **backing services** (databases, queues, etc.).
- Processes can be restarted or replicated without concern for local state.

---

## 🛣️ 7. Port Binding

**Export services via port binding**

- The app should self-contain an HTTP server and bind to a port (e.g., `localhost:5000`), instead of depending on a runtime web server like Apache or Nginx.

```
// Express.js example
app.listen(process.env.PORT || 3000)
```

---

## 🧪 8. Concurrency

**Scale out via the process model**

- Scale by **adding more instances** (horizontal scaling).
- Use process types: web, worker, scheduler.

Example: In a Procfile (Heroku format):
```
web: node server.js
worker: node worker.js
```

---

## ⏳ 9. Disposability

**Maximize robustness with fast startup and graceful shutdown**

- Fast startup allows scaling and recovery.
- Graceful shutdown ensures no data is lost on termination (listen for `SIGTERM`).

---

## 📈 10. Dev/Prod Parity

**Keep development, staging, and production as similar as possible**

- Minimize the **"it works on my machine"** problem.
- Sync tooling, OS, dependencies, and configuration across environments.

---

## 📋 11. Logs

**Treat logs as event streams**

- Applications should not manage log files.
- Instead, write logs to `stdout`/`stderr`; use log aggregation tools to capture and analyze logs (e.g., ELK, Loki, Datadog).

```
// Example log output
console.log("User signed in", user.id)
```

---

## 🛠️ 12. Admin Processes

**Run admin/management tasks as one-off processes**

- Tasks like migrations, data cleanup, and debugging should be **run in isolated one-off processes** using the same environment and config as the app.

```
// Example: DB migration command
node migrate.js
```

---

## 📌 Summary Table

| # | Factor               | Purpose                                           |
|---|----------------------|---------------------------------------------------|
| 1 | Codebase             | Single repo, many deploys                        |
| 2 | Dependencies         | Explicit and isolated                            |
| 3 | Config               | Store in env vars                                |
| 4 | Backing Services     | Treat as attached, swappable resources           |
| 5 | Build, Release, Run  | Separate stages of deployment                    |
| 6 | Processes            | Stateless, share-nothing architecture            |
| 7 | Port Binding         | Self-contained service via ports                 |
| 8 | Concurrency          | Scale via process model                          |
| 9 | Disposability        | Fast startup and shutdown                        |
|10 | Dev/Prod Parity      | Keep environments as similar as possible         |
|11 | Logs                 | Stream logs, external aggregation                |
|12 | Admin Processes      | One-off admin tasks as standalone processes      |

---

## 📚 Further Reading

- [12factor.net (Official Site)](https://12factor.net/)
- [Heroku Developer Docs](https://devcenter.heroku.com/)

---
