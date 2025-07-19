# 🎨 System Design: Interface Design (API & Component Interfaces)

Interface design in system architecture defines how different parts of a system communicate—whether internal modules, external clients, or third-party services. Good interface design ensures **clarity**, **loose coupling**, and **future scalability**.

---

## 🧭 1. Why Interface Design Matters

- **Abstraction**: Hides implementation details
- **Encapsulation**: Prevents misuse of internal logic
- **Flexibility**: Allows swapping implementations
- **Testability**: Simplifies mocking and testing

---

## 🔌 2. API Interface Design (External Interfaces)

### 📦 RESTful API Design
- Use **resource-oriented URIs**: `/users`, `/orders/{id}`
- Choose appropriate **HTTP verbs**:
  - `GET`, `POST`, `PUT`, `PATCH`, `DELETE`
- Consistent **status codes**: 200, 201, 400, 404, 500
- Versioning: `/v1/users`

### 🧠 Semantic Rules
- Use nouns in URIs, not verbs
- Return consistent JSON response structures
- Example:

```json
{
  "data": { "id": "123", "name": "Alice" },
  "errors": [],
  "meta": { "timestamp": "..." }
}
```

### 🧰 Tools & Practices
OpenAPI / Swagger for documentation

Use API gateways for routing, auth, rate-limiting

Design for backward compatibility

## 🔁 3. Internal Component Interfaces
### 👓 Clear Contracts
- Define input/output data structures
- Avoid leaking implementation details
- Prefer stable interfaces even if internals change

### 🧱 Patterns
- Interfaces in OOP (Java, C#, Go)
- Ports and Adapters (Hexagonal Architecture)
- Functional Interfaces (passing functions as arguments)

```java
// Java Example
public interface PaymentProcessor {
    void charge(Customer customer, double amount);
}
```

```go
// Go Example
type Storage interface {
    Save(data []byte) error
}
```
## 🧪 4. Designing for Testability
- Favor dependency injection
- Use mockable interfaces
- Avoid hidden state or tight coupling

## 🛠️ 5. Tools and Practices
- IDLs (Interface Definition Languages):
- gRPC / Protobuf for microservices
- Thrift or Avro for schema evolution
- JSON Schema or Zod (TypeScript) for validation
- Use interface segregation (keep interfaces small and focused)

## 🔐 6. Security & Stability
- Define clear error handling contracts
- Rate limiting and timeouts at the boundary
- Consider forward/backward compatibility of interfaces

## 🔄 7. Versioning Strategy
- APIs should evolve without breaking consumers
- Use URI-based versioning: /api/v1/...
- Or header-based versioning: Accept: application/vnd.company.v1+json

## 📋 8. Interface Review Checklist
- [ ] Does the interface abstract internal logic?
- [ ] Is the input/output well-documented and typed?
- [ ] Is it intuitive and semantically clear?
- [ ] Is it stable and backward compatible?
- [ ] Is it secure and limited in exposure?
- [ ] Can it be mocked/tested easily?