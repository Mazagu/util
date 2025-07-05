# ❌ Error Response Schema Guide (REST, GraphQL, gRPC, OpenAPI)

A consistent and expressive error schema helps developers debug issues and systems stay maintainable. Below are patterns and best practices for multiple protocols.

---

## 🔁 REST Error Format

```
{
  "error": {
    "code": "RESOURCE_NOT_FOUND",
    "message": "The requested resource was not found.",
    "status": 404,
    "details": [
      {
        "field": "userId",
        "issue": "User ID does not exist"
      }
    ],
    "traceId": "abc123xyz789"
  }
}
```

---

## ⚙️ GraphQL Error Handling

GraphQL always returns `200 OK`, and errors appear inside an `errors` array.

```
{
  "data": null,
  "errors": [
    {
      "message": "User not found",
      "locations": [{ "line": 2, "column": 3 }],
      "path": ["user"],
      "extensions": {
        "code": "NOT_FOUND",
        "status": 404,
        "traceId": "abc123xyz789"
      }
    }
  ]
}
```

✅ **Tips**:
- Add `extensions.code` to carry custom error types (e.g., `UNAUTHORIZED`, `VALIDATION_FAILED`).
- Use `traceId` for observability.
- Use middleware to standardize GraphQL errors.

---

## 🔌 gRPC Error Handling (with gRPC Status Codes)

gRPC uses `status.proto` to define structured errors. Use `google.rpc.Status`.

```
{
  "code": 5,
  "message": "User not found",
  "details": [
    {
      "@type": "type.googleapis.com/google.rpc.DebugInfo",
      "detail": "User ID does not exist in DB"
    }
  ]
}
```

✅ **Tips**:
- Use standard gRPC codes like `NOT_FOUND`, `INVALID_ARGUMENT`, `PERMISSION_DENIED`.
- Embed metadata using `status.details` for error context.
- Use gRPC interceptors to attach `traceId` or logging metadata.

---

## 🧾 OpenAPI (Swagger) Error Response

Define response schemas in OpenAPI YAML for each HTTP error status:

```
components:
  responses:
    ErrorResponse:
      description: Generic error response
      content:
        application/json:
          schema:
            type: object
            properties:
              error:
                type: object
                properties:
                  code:
                    type: string
                  message:
                    type: string
                  status:
                    type: integer
                  traceId:
                    type: string
```

✅ **Tips**:
- Reuse `ErrorResponse` in multiple endpoints with `$ref`.
- Document each `code` value in your API documentation for consistency.

---

## 🧠 Final Recommendations

- **Always include a trace ID** (and propagate it end-to-end).
- Keep responses simple but **structured** enough for clients and logs.
- Stick to **a common set of error codes** for all teams across services.
- For REST, **don’t overload 500** – use 422, 403, 409, etc. meaningfully.
