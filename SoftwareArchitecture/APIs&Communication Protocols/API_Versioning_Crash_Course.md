# 🚦 API Versioning Crash Course

API versioning is **essential** for evolving an API **without breaking existing clients**. This crash course covers **why** versioning matters, **strategies**, and **best practices**.

---

## 📌 Why API Versioning?

APIs evolve. Changes can break existing users if not managed correctly. Versioning provides a **contract** between providers and consumers:

- 🔒 Preserve backward compatibility
- 🔁 Enable iterative improvements
- 🧪 Test new features without breaking production

---

## 🚩 When Do You Need Versioning?

✅ When introducing:
- Breaking changes to response structure
- Removed or renamed fields
- Behavior modifications (e.g., different sorting, pagination)
- Deprecation of endpoints or functionality

❌ Not always necessary for:
- Internal APIs
- Non-breaking changes (e.g., adding optional fields)

---

## 🎯 Common API Versioning Strategies

Here are the most common ways to version your APIs, each with pros and cons:

---

### 1. **URI Path Versioning**

Put the version directly in the URL path.

```
// Example
GET /v1/users/123
GET /v2/users/123
```

✅ Pros:
- Clear and explicit
- Easy to cache and route

❌ Cons:
- Breaks RESTful URL purity
- Requires duplicate documentation/routes

---

### 2. **Query Parameter Versioning**

Specify the version via a query string.

```
// Example
GET /users/123?version=2
```

✅ Pros:
- Non-invasive URL change
- Useful for quick testing or experimentation

❌ Cons:
- Harder to cache
- Less common in public APIs

---

### 3. **Header Versioning**

Send version info via custom headers.

```
// Example
GET /users/123
Headers:
  Accept: application/vnd.myapp.v2+json
```

✅ Pros:
- Clean URLs
- RESTful principles preserved

❌ Cons:
- Hidden from browsers and some tools (like cURL)
- Harder to debug or document

---

### 4. **Content Negotiation (Media Type Versioning)**

Use MIME types in the `Accept` header.

```
// Example
Accept: application/vnd.example.v1+json
```

✅ Pros:
- Fine-grained control
- Works well for APIs with multiple formats

❌ Cons:
- Requires custom logic on server
- Not always intuitive for consumers

---

## 🧠 Best Practices

- 📢 **Communicate changes** clearly in changelogs and documentation.
- 🚨 **Deprecate gradually**: support multiple versions with sunset notices.
- ⚙️ **Keep old versions stable** and free of breaking changes.
- 📦 **Group related changes** into a new version; don’t version every minor tweak.
- ✅ **Default to latest** only if safe, or require versioning explicitly.
- 🧪 **Use automated tests** to ensure old versions still function as expected.

---

## 🔁 Versioning Approaches in REST vs GraphQL

| Style     | Versioning Style          | Notes                                    |
|-----------|---------------------------|------------------------------------------|
| REST      | URI, header, or param     | Most popular styles use URI paths        |
| GraphQL   | Schema evolution / field deprecation | Avoids versioning whole schema |

GraphQL typically avoids versioning entire schemas by:
- Adding new fields instead of replacing
- Deprecating fields with `@deprecated`

---

## 🧰 Tooling & Support

- **OpenAPI** (Swagger) supports version tagging and documentation per version.
- **Postman** supports versioned collections.
- **API Gateways** (e.g., Kong, AWS API Gateway) allow version-based routing.

---

## ✅ TL;DR Cheat Sheet

| Strategy        | URL Example                  | Good For                 | Drawbacks                  |
|-----------------|------------------------------|--------------------------|----------------------------|
| URI Path        | `/v1/resource`               | Simplicity, public APIs  | Clutters endpoint hierarchy |
| Query Param     | `/resource?version=1`        | Quick tests              | Caching issues             |
| Header          | `Accept: vnd.app.v1+json`    | Clean URL, advanced users| Harder to debug/document   |
| GraphQL (no ver)| `schema.graphql` + `@deprecated` | Evolution without breaking | Requires planning          |

---

## 📚 Further Reading

- [REST API Versioning Best Practices – Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design#versioning)
- [Google Cloud API Design Guide](https://cloud.google.com/apis/design/versioning)
- [API Evolution in GraphQL](https://graphql.org/learn/best-practices/#versioning)

---
