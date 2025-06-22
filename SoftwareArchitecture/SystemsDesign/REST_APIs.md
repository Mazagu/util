# 🌐 Guide: REST APIs — A Systems Design Deep Dive

**REST (Representational State Transfer)** is an architectural style for building **scalable, resource-oriented web services** using standard HTTP protocols.

This guide focuses on **core principles**, **idempotency**, **pagination**, and **security** — key to designing robust, scalable APIs.

---

## 🧠 1. Core REST Principles

| Principle            | Description                                                     |
|----------------------|-----------------------------------------------------------------|
| **Resource-based**   | Use nouns (`/users`, `/orders`) not verbs (`/getUser`)          |
| **Stateless**        | Each request must contain all the context                      |
| **Client-Server**    | Separation of UI/client and server logic                        |
| **Cacheable**        | Responses can be cached to improve performance                  |
| **Uniform interface**| Consistent resource naming and operations across endpoints      |

### Common HTTP Methods
| Method   | Action          | Idempotent? | Safe?  |
|----------|------------------|-------------|--------|
| `GET`    | Retrieve         | ✅           | ✅      |
| `POST`   | Create           | ❌           | ❌      |
| `PUT`    | Replace          | ✅           | ❌      |
| `PATCH`  | Partial update   | ✅ (ideally) | ❌      |
| `DELETE` | Delete resource  | ✅           | ❌      |

```
// RESTful Example
GET /users/42           → retrieve user 42
POST /users             → create new user
PUT /users/42           → replace user 42
PATCH /users/42         → update user 42's fields
DELETE /users/42        → delete user 42
```

---

## 🔁 2. Idempotency in REST APIs

**Idempotency** means that making the same request **multiple times** results in the **same effect**.

### Why it matters:
- **Retries** (due to timeouts, client errors) shouldn't cause data corruption
- **Safe behavior** for network instability

| Method     | Idempotent | Notes                              |
|------------|------------|-------------------------------------|
| `GET`      | ✅          | Read-only                           |
| `DELETE`   | ✅          | Deleting an already deleted item is OK |
| `PUT`      | ✅          | Always sets a resource to a given state |
| `POST`     | ❌ (usually) | Can be made idempotent via unique IDs |

```
// Making POST idempotent using client-supplied ID
POST /orders
{
  "id": "client-generated-uuid",
  "item": "book"
}
```

Server should ensure no duplicate order is created for same UUID.

---

## 📚 3. Pagination

Pagination helps scale APIs that return large data sets by **limiting the number of results per request**.

### Common Pagination Types

| Type           | How it works                                    | Use case                     |
|----------------|--------------------------------------------------|------------------------------|
| **Offset**     | Skip `n` items → `GET /users?offset=20&limit=10`| Simpler, can have gaps       |
| **Cursor**     | Use pointer/key to next page → `?after=abc123`  | Scalable for large data sets |
| **Page number**| Simple pages → `GET /products?page=2`           | Good for UI pagination       |

### Include metadata in response:
```
{
  "data": [...],
  "pagination": {
    "limit": 10,
    "offset": 20,
    "total": 174,
    "next": "/users?offset=30&limit=10"
  }
}
```

✅ Helps clients know how to fetch more  
✅ Reduces payload size and memory pressure

---

## 🔐 4. REST API Security

Security is critical when exposing REST APIs publicly.

### 🔑 1. Authentication

- **OAuth 2.0 / OpenID Connect**: Secure delegated access
- **JWT (JSON Web Tokens)**: Compact, stateless access tokens
- **API Keys**: Lightweight, but limited for public use

```
Authorization: Bearer eyJhbGciOi...
```

---

### 🔒 2. Authorization

Use **RBAC** (role-based access control) or **ABAC** (attribute-based) to control what users can do.

| Example Role   | Access Control                               |
|----------------|----------------------------------------------|
| Admin          | Full access to resources                     |
| Editor         | Modify owned content                         |
| Viewer         | Read-only access                             |

---

### 🧯 3. Rate Limiting and Throttling

Protects APIs from abuse:

- Use **per-IP or per-token limits**
- Return HTTP `429 Too Many Requests`
- Use headers like `Retry-After`

```
X-RateLimit-Limit: 1000  
X-RateLimit-Remaining: 200  
Retry-After: 60
```

---

### 🕵️ 4. Input Validation & Sanitization

Prevent:
- **SQL injection**
- **XSS attacks**
- **Broken JSON payloads**

Always validate input types, ranges, and structure using:
- JSON Schema
- Backend validators (e.g. `express-validator`, `pydantic`, etc.)

---

### ✅ 5. HTTPS and TLS

- Enforce **HTTPS-only** endpoints
- Prevents MITM and eavesdropping
- Use **HSTS headers** and TLS 1.2+

---

## ✅ 5. Best Practices

| Practice                         | Benefit                                      |
|----------------------------------|----------------------------------------------|
| Use proper HTTP status codes     | Clear client understanding                   |
| Version your API (`/v1/`)        | Safe evolution over time                     |
| Use nouns for resource names     | Follows REST conventions                     |
| Keep URLs lowercase + hyphenated| Better readability                           |
| Return standardized error formats| Easier for clients to parse and debug        |

---

## 📚 Further Reading

- [RESTful API Design Guidelines — Microsoft](https://github.com/microsoft/api-guidelines)
- [JSON API Spec](https://jsonapi.org/)
- [OAuth 2.0 Spec](https://oauth.net/2/)
- [OWASP REST Security Guide](https://cheatsheetseries.owasp.org/cheatsheets/REST_Security_Cheat_Sheet.html)
- [Best Practices for Designing RESTful APIs (Google)](https://cloud.google.com/apis/design)

---
