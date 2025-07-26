# 📘 API Documentation Guide: Best Practices & Tools

---

## 🎯 Why API Documentation Matters

Good API documentation is essential for:
- Enabling fast onboarding for developers  
- Reducing support requests  
- Enhancing developer experience (DX)  
- Preventing miscommunication between teams  

---

## ✅ Best Practices

### 1. **Start Early, Update Often**
Document alongside development to prevent drift between code and docs.

### 2. **Use Open Standards**
Prefer OpenAPI (formerly Swagger) or API Blueprint to ensure compatibility with tooling.

### 3. **Include These Core Sections**

| Section             | Description                                                  |
|---------------------|--------------------------------------------------------------|
| **Overview**         | High-level description, use cases, authentication            |
| **Base URL**         | API root endpoint(s)                                         |
| **Authentication**   | Tokens, headers, OAuth, etc.                                 |
| **Endpoints**        | HTTP method, path, request/response schema                   |
| **Examples**         | Real, copy-paste-able request/response examples              |
| **Error Handling**   | Status codes, messages, and how to handle them               |
| **Rate Limits**      | Limits and retry logic                                       |
| **Changelog**        | API version history                                          |

---

## ✍️ Example Endpoint Documentation

### `GET /api/v1/users/{id}`

Returns user details for a given user ID.

#### Request:
- Method: GET  
- URL: `/api/v1/users/123`  
- Headers:  
  - `Authorization: Bearer <token>`

#### Response:

**200 OK**
```
{
  "id": 123,
  "name": "Alice",
  "email": "alice@example.com"
}
```

**404 Not Found**
```
{
  "error": "User not found"
}
```

---

## 🛠️ Popular Tools

### 🧩 Specification Formats

| Format        | Description                          |
|---------------|--------------------------------------|
| **OpenAPI**   | Widely adopted, JSON/YAML format     |
| **Blueprint** | Markdown-like spec, human-friendly   |
| **RAML**      | Designed for RESTful APIs            |

### 📚 Documentation Generators

| Tool          | Description                                 |
|---------------|---------------------------------------------|
| **Swagger UI**| Interactive docs from OpenAPI spec          |
| **Redoc**     | Beautiful static docs from OpenAPI          |
| **Slate**     | Static site generator using Markdown        |
| **Stoplight** | Visual API editor and documentation tool    |
| **Postman**   | API testing and auto-generated docs         |
| **Rapidoc**   | Fast, embeddable OpenAPI renderer           |

---

## 🔒 Documenting Authentication

Always describe:
- Type of authentication: API Key, OAuth2, JWT, etc.
- Header format and usage
- Token lifespan and refresh behavior
- Example request with authentication

---

## 🧪 Documenting Error Codes

Use consistent, clear structure:

| Code | Meaning              | Description                     |
|------|----------------------|---------------------------------|
| 200  | OK                   | Successful operation            |
| 400  | Bad Request          | Invalid input or parameters     |
| 401  | Unauthorized         | Missing/invalid auth token      |
| 403  | Forbidden            | No permission for action        |
| 404  | Not Found            | Resource doesn’t exist          |
| 500  | Internal Server Error| Server-side exception           |

---

## 🔁 Versioning Docs

### Common Strategies:
- **URL-based:** `/v1/users`
- **Header-based:** `Accept-Version: v1`
- **Subdomain-based:** `v1.api.example.com`

✅ Maintain separate documentation per version  
✅ Indicate deprecations clearly

---

## 📏 Schema & Validation

Use JSON Schema or OpenAPI components:

```
{
  "type": "object",
  "properties": {
    "email": {
      "type": "string",
      "format": "email"
    },
    "age": {
      "type": "integer",
      "minimum": 18
    }
  },
  "required": ["email"]
}
```

This enables:
- Code generation
- Validation in clients
- Autocomplete in IDEs

---

## 📦 Toolchain Suggestions by Stack

| Stack         | Tool Recommendations                                   |
|---------------|---------------------------------------------------------|
| Node.js       | Swagger UI, Redoc, Postman, Docusaurus                 |
| Laravel       | L5 Swagger, Scribe, Stoplight                          |
| Spring Boot   | SpringDoc OpenAPI, Swagger UI                          |
| FastAPI       | Built-in OpenAPI docs                                  |
| Go (Gin/Echo) | swaggo/swag, Redoc, Rapidoc                            |

---

## 📈 Advanced Tips

- Auto-generate docs from annotations (e.g., `@api` comments)
- Host on `/docs` or `docs.example.com`
- Provide Postman collections or curl examples
- Use GitHub Actions or CI to validate OpenAPI specs
- Provide “Try It Out” interactive explorer

---
