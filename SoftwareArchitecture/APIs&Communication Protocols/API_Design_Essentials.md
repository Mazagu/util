# 🌐 API Design Essentials: Principles, Patterns & Best Practices

API design is about creating intuitive, scalable, and maintainable interfaces for software communication. This guide focuses on **RESTful design**, but many concepts apply to **GraphQL**, **gRPC**, and other protocols.

---

## 📐 1. Core API Design Principles

### ✅ Consistency
Use consistent naming, versioning, and responses.

### 📎 Resource-Oriented Design
Model endpoints around **nouns**, not verbs.

| Action        | Good URL Example         | Bad Example           |
|---------------|---------------------------|------------------------|
| Get user      | `/users/123`              | `/getUser?id=123`     |
| Create user   | `POST /users`             | `/createUser`         |
| Delete post   | `DELETE /posts/456`       | `/removePost?id=456`  |

### 🔁 Statelessness (REST)
Each request must contain **all necessary context** (e.g., tokens, IDs). The server stores no session state.

---

## 🧾 2. HTTP Methods & Usage

| Method   | Purpose                | Idempotent |
|----------|------------------------|------------|
| `GET`    | Read resource          | ✅          |
| `POST`   | Create resource        | ❌          |
| `PUT`    | Update/replace         | ✅          |
| `PATCH`  | Partially update       | ✅          |
| `DELETE` | Remove resource        | ✅          |

```
GET /users/101
POST /users
PUT /users/101
PATCH /users/101
DELETE /users/101
```

---

## 🧭 3. Resource Naming Conventions

- Use **plural nouns**: `/users`, `/products`
- Use **hyphens** not underscores: `/user-roles`
- Use **lowercase** in URLs
- Sub-resources: `/users/123/posts/456`

---

## 🔢 4. Versioning

Version early, version clearly:

```
/v1/users
```

- URI versioning: `/v1/...` (most common)
- Header versioning: `Accept: application/vnd.myapi.v2+json`

> Never break compatibility between versions.

---

## 📦 5. Standard Response Structure

Use **consistent response formats**.

```
{
  "data": {
    "id": 123,
    "name": "Alice"
  },
  "meta": {
    "timestamp": "2025-06-19T20:00:00Z"
  },
  "error": null
}
```

### ❌ Error Example

Use structured errors with HTTP status codes.

```
{
  "error": {
    "code": "USER_NOT_FOUND",
    "message": "User with ID 123 not found."
  }
}
```

---

## 🔒 6. Authentication & Security

### ✅ Recommended

- **HTTPS only**
- Use **OAuth 2.0**, **JWT**, or **API keys**
- Apply **rate limiting**, **input validation**, **RBAC**

```
Authorization: Bearer eyJhbGciOi...
```

---

## 📃 7. Pagination, Filtering, Sorting

### 🔹 Pagination (offset-based)

```
GET /products?limit=20&offset=40
```

### 🔹 Filtering

```
GET /orders?status=delivered&customer_id=456
```

### 🔹 Sorting

```
GET /products?sort=price_desc
```

> Use cursor-based pagination for large datasets (`?after=...`).

---

## 🧪 8. API Documentation

Use **OpenAPI (Swagger)** or **Postman collections**.

Benefits:
- Auto-generated UI for devs
- Enables testing and mocks
- Syncs with client SDKs

Example tools:
- [Swagger UI](https://swagger.io/tools/swagger-ui/)
- [Redoc](https://redocly.com/)
- [Stoplight](https://stoplight.io/)

---

## 🧰 9. Tools and Standards

| Tool          | Purpose                          |
|---------------|----------------------------------|
| **OpenAPI**   | Spec and schema format            |
| **Postman**   | API design, testing, monitoring   |
| **Insomnia**  | Lightweight testing alternative   |
| **Stoplight** | Visual API modeling               |
| **Prism**     | Mock server from OpenAPI spec     |
| **GitHub Actions** | API test + deploy workflows |

---

## 🚀 10. API Design Best Practices Checklist

- [x] RESTful naming conventions
- [x] Clear versioning
- [x] Proper use of HTTP methods and status codes
- [x] Secure authentication (JWT/OAuth)
- [x] Rate limiting, retries, idempotency where needed
- [x] Pagination, sorting, filtering
- [x] Structured and documented errors
- [x] JSON response consistency
- [x] Fully documented (OpenAPI / Postman)
- [x] Automated tests + CI

---

## 📚 Learn More

- [RESTful API Design Guide – Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [HTTP Status Codes Reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
- [OAuth 2.0 Explained](https://oauth.net/2/)
- [OpenAPI Spec](https://swagger.io/specification/)

---
