# 🌐 HTTP Status Code Quick Guide

HTTP status codes are issued by a server in response to a client's request. They indicate whether a request has been successfully completed or if errors occurred.

```
## ✅ 2xx – Success

- **200 OK**: Standard response for successful GET, PUT, PATCH, or DELETE.
- **201 Created**: Resource has been successfully created (e.g., POST).
- **202 Accepted**: Request accepted for processing but not completed yet.
- **204 No Content**: Success, but no content to return (e.g., after DELETE).

## 🔁 3xx – Redirection

- **301 Moved Permanently**: Resource has permanently moved to a new URL.
- **302 Found**: Temporary redirect.
- **304 Not Modified**: Cached version is still valid (used with ETags).

## ❗ 4xx – Client Errors

- **400 Bad Request**: Request is malformed (e.g., invalid JSON, missing params).
- **401 Unauthorized**: Authentication required or failed.
- **403 Forbidden**: Authenticated, but not allowed to access resource.
- **404 Not Found**: Resource does not exist.
- **405 Method Not Allowed**: HTTP method not supported for this resource.
- **409 Conflict**: Request conflicts with server state (e.g., duplicate resource).
- **422 Unprocessable Entity**: Valid request but semantic error (e.g., validation failed).
- **429 Too Many Requests**: Rate limit exceeded.

## 💥 5xx – Server Errors

- **500 Internal Server Error**: Generic server error.
- **502 Bad Gateway**: Invalid response from upstream server.
- **503 Service Unavailable**: Server is temporarily unavailable (e.g., overloaded).
- **504 Gateway Timeout**: Upstream server didn't respond in time.
```

---

## 🧠 Tips

- Use **200** for successful GET, **201** for successful POST.
- **204** is ideal for DELETE or successful updates without a body.
- Avoid exposing sensitive info in **500** error messages.
- Rate limit APIs and use **429** with a `Retry-After` header.
- Validate inputs and return **400** or **422** with clear messages.
