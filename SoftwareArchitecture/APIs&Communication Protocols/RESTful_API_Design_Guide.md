# 🌐 RESTful API Design Guide

Designing RESTful APIs involves following principles that improve **clarity**, **scalability**, and **developer experience**. This guide outlines best practices, tips, and common pitfalls when designing robust and intuitive REST APIs.

---

## 📦 Domain Model Driven Paths

Base your endpoint structure on **domain entities** and relationships rather than technical implementation.

```
/users/{userId}/orders
/products/{productId}/reviews
/teams/{teamId}/members
```

Avoid verbs in the path — actions should be implied by the HTTP method.

---

## 🔧 Use Proper HTTP Methods

Stick to the semantics of HTTP:

| Method | Use Case               |
|--------|------------------------|
| GET    | Retrieve resource      |
| POST   | Create a new resource  |
| PUT    | Replace an entire resource |
| PATCH  | Partially update a resource |
| DELETE | Remove a resource      |

```
GET    /users           -> list users
POST   /users           -> create new user
GET    /users/42        -> get user with ID 42
PUT    /users/42        -> replace user 42
PATCH  /users/42        -> update user 42 partially
DELETE /users/42        -> delete user 42
```

💡 Tip: PATCH is useful but requires careful idempotent implementation. Consider using PUT for simplicity when applicable.

---

## 🔁 Implement Idempotency

**Idempotent APIs** produce the same result regardless of how many times the operation is repeated.

- `GET`, `PUT`, `DELETE`, and `PATCH` should be idempotent.
- For `POST`, use **idempotency keys** or client-generated IDs.

```
POST /payments
Headers: Idempotency-Key: abc123
```

---

## 🧾 Choose Consistent HTTP Status Codes

Use a consistent subset of HTTP status codes for clarity:

| Code | Meaning                       |
|------|-------------------------------|
| 200  | OK                            |
| 201  | Created                       |
| 204  | No Content (e.g., after DELETE) |
| 400  | Bad Request                   |
| 401  | Unauthorized                  |
| 403  | Forbidden                     |
| 404  | Not Found                     |
| 409  | Conflict                      |
| 422  | Unprocessable Entity (e.g., validation) |
| 500  | Internal Server Error         |

---

## 🔢 API Versioning

Avoid breaking clients by versioning your API:

- URI versioning: most common

```
GET /v1/users
```

- Header-based or content negotiation is more advanced and flexible but harder to adopt universally.

---

## 📖 Semantic & Intuitive Paths

Paths should be **descriptive, consistent, and pluralized**.

❌ Avoid:
```
/getAllUsers
```

✅ Prefer:
```
/users
```

Use **nouns** (resources), not verbs (actions).

---

## 🧩 Batch Processing

Enable bulk actions using a consistent keyword and path suffix:

```
POST /orders/batch
PATCH /users/batch
```

Ensure partial failures return informative responses.

---

## 🔍 Flexible Query Language

Support advanced querying capabilities using standard query params:

- **Filtering**:  
  ```/users?role=admin```
  
- **Pagination**:  
  ```/users?limit=10&offset=20```
  
- **Sorting**:  
  ```/products?sort=-price```

Tip: Use **RSQL** or **OData-style** query specs for consistency if the API becomes complex.

---

## 🧪 Validation & Errors

- Validate input and return **helpful error messages**.
- Use a consistent error response format.

```
{
  "error": {
    "code": "INVALID_EMAIL",
    "message": "Email must be a valid format.",
    "field": "email"
  }
}
</codexample>

---

## 🛡️ Security

- Use **HTTPS** only.
- Apply **rate limiting** and **authentication** (OAuth2, API keys).
- Avoid leaking sensitive data in error responses.
- Use **allowlists** to restrict access to IPs or clients.

---

## 🔁 HATEOAS (Optional)

Hypermedia As The Engine Of Application State — not always required but good for discoverability:

```
{
  "user": {
    "id": "42",
    "name": "Alice",
    "links": [
      { "rel": "self", "href": "/users/42" },
      { "rel": "orders", "href": "/users/42/orders" }
    ]
  }
}
```

---

## 📚 Example RESTful Resources

| Entity        | Endpoints                         |
|---------------|------------------------------------|
| Users         | `/users`, `/users/{id}`           |
| Orders        | `/orders`, `/orders/{id}`         |
| Products      | `/products`, `/products/{id}`     |
| Auth          | `/login`, `/logout`, `/register`  |
| Metrics       | `/health`, `/metrics`, `/status`  |

---

## ✅ Final Checklist

- [x] Use nouns, not verbs in paths
- [x] Stick to proper HTTP methods
- [x] Apply consistent status codes
- [x] Enable query flexibility
- [x] Secure your API
- [x] Validate input and structure errors
- [x] Design with versioning in mind

---

A well-designed RESTful API is **predictable**, **understandable**, and **future-proof**. Stick to the principles outlined here to improve maintainability and developer experience.
