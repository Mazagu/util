# 🐳 Docker for Local Development — Deep Dive Guide

---

## 🚀 Overview

**Docker** provides lightweight containers that help developers create isolated environments for applications, services, and tools.

### Why Use Docker for Local Dev?
- Consistency across environments (dev, staging, prod).
- Eliminate "works on my machine" issues.
- Spin up full stacks (DB + services) in seconds.
- Reproducible environments with `Dockerfile` + `docker-compose`.

---

## 📦 Key Concepts

| Concept          | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `Dockerfile`     | Script to build your image. Defines environment, dependencies, etc.         |
| Image            | Built snapshot of your application environment.                             |
| Container        | Running instance of an image.                                                |
| `docker-compose` | Tool to define and manage multi-container apps.                             |
| Volumes          | Mount points to persist data outside of containers.                         |
| Bind Mounts      | Mount local code directly inside containers for live-reload development.     |
| Networks         | Isolated communication channels between containers.                         |

---

## 🛠️ Best Practices for Local Dev

### 1. Use `.dockerignore`

Avoid copying unnecessary files to your image (e.g., `node_modules`, `.git`).

```
.dockerignore
node_modules
.git
.env
```

---

### 2. Use Bind Mounts for Code Sync

Map your local code to the container so changes reflect instantly.

```
# docker-compose.yml (partial)
volumes:
  - ./:/app
```

---

### 3. Use `docker-compose` for Multi-Service Dev

Simplify launching services like DBs, cache, app, etc.

```
# docker-compose.yml (simplified)
version: "3.8"
services:
  app:
    build: .
    volumes:
      - ./:/app
    ports:
      - "3000:3000"
    depends_on:
      - db

  db:
    image: postgres:16
    environment:
      POSTGRES_USER: dev
      POSTGRES_PASSWORD: dev
```

---

### 4. Don’t Hardcode Secrets

Use `.env` files + environment variable injection instead of hardcoded secrets.

```
# docker-compose.yml
env_file:
  - .env
```

---

### 5. Keep Dev Images Lightweight

- Use multi-stage builds.
- Use language-specific slim images (`node:20-slim`, `python:3.12-alpine`, etc.).
- Remove unused dependencies.

---

## 💡 Tips and Tricks

- Use `named volumes` for data persistence (`db-data`, etc.).
- Use `healthcheck:` in `docker-compose` to wait until DB is ready.
- Use `docker cp` to extract logs or snapshots for debugging.
- Use `docker-compose up --build` to rebuild when dependencies change.
- Use `.docker/dev.Dockerfile` and `.docker/prod.Dockerfile` to separate concerns.

---

## 🧪 Common Use Cases

| Use Case         | Tip                                                                 |
|------------------|----------------------------------------------------------------------|
| Live reload      | Use bind mounts + local dev servers like `nodemon`, `air`, etc.     |
| Debugging        | Run interactive shell with `docker exec -it <container> /bin/sh`    |
| CI/CD alignment  | Reuse Dockerfiles in CI to match local build environments           |
| Multiple versions| Use different containers for Node 16, 18, 20 side-by-side           |

---

## 🧱 Example Project Layout

```
project/
├── docker-compose.yml
├── .env
├── Dockerfile
├── .dockerignore
└── src/
    ├── index.js
    └── ...
```

---

## ✅ Tools that Pair Well with Docker

- **Tilt.dev** or **Skaffold**: for iterative development with hot reload.
- **doppler**, **Vault**, **1Password CLI**: for secrets management.
- **direnv** or **asdf**: for managing local tooling when outside containers.

---

## 🧼 Clean Up Tips

```
# Remove unused containers
docker rm $(docker ps -aq)

# Remove dangling images
docker image prune

# Clean up volumes
docker volume prune
```

---

## 📚 Resources

- [Docker Docs](https://docs.docker.com/)
- [Awesome Compose (GitHub)](https://github.com/docker/awesome-compose)
- [Play with Docker](https://labs.play-with-docker.com/)
