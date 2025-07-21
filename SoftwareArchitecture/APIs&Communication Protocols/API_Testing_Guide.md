# ✅ API Testing Guide

API testing ensures your services behave as expected under different conditions. It's essential for validating business logic, security, performance, and integration.

---

## 🧪 Common Types of API Testing

### 1. **Functional Testing**
- Ensures the API endpoints behave as expected.
- Verifies response codes, headers, payloads.

🔧 Example:
- `GET /users` returns 200 and JSON body with user data.

---

### 2. **Validation Testing**
- Confirms the correctness of data sent and received.
- Checks schema, data types, field validation.

🔧 Example:
- POST /users with a missing `email` should return `400 Bad Request`.

---

### 3. **Load Testing**
- Measures how the API handles a high volume of requests.
- Detects bottlenecks or limits.

🔧 Tools: JMeter, k6, Artillery.

---

### 4. **Stress Testing**
- Pushes the API beyond expected limits to find the breaking point.
- Helps determine system robustness.

---

### 5. **Security Testing**
- Ensures the API is protected from attacks like XSS, SQL injection, CSRF, etc.
- Tests auth tokens, rate limiting, role-based access.

🔧 Tools: OWASP ZAP, Postman, Burp Suite.

---

### 6. **Integration Testing**
- Verifies the interaction between different APIs or between API and DB/external services.

🔧 Example:
- Ensure creating a user also initializes settings in another microservice.

---

### 7. **Unit Testing**
- Tests individual functions or endpoints in isolation.
- Simulates dependencies via mocks or stubs.

🔧 Tools: Jest (JS), PHPUnit (Laravel), JUnit (Java), pytest (Python).

---

### 8. **Regression Testing**
- Ensures new changes haven't broken existing functionality.
- Run after updates, especially before deployment.

---

### 9. **Runtime/Error Testing**
- Validates proper error handling during edge cases or invalid input.
- Checks if error messages, codes, and logging are correct.

---

### 10. **UI/API Contract Testing**
- Ensures the frontend and backend communicate under a shared schema.
- Useful in microservices or frontend/backend decoupled architectures.

🔧 Tools: Pact, Postman schema validation, Swagger/OpenAPI validators.

---

## 🧰 Popular API Testing Tools

| Tool       | Use Case                          |
|------------|-----------------------------------|
| Postman    | Manual, automated functional tests |
| REST Assured | Java-based API testing           |
| Supertest  | Node.js/Express testing           |
| Insomnia   | Exploratory testing               |
| JMeter     | Load/stress testing               |
| k6         | Performance testing (code-based)  |
| Swagger    | Contract validation               |
| SoapUI     | SOAP/REST enterprise testing      |

---

## ✅ Best Practices

- Test both **positive and negative** scenarios
- Keep **environments isolated** (staging vs prod)
- Use **mock servers** for early testing
- Automate testing in CI/CD pipelines
- Validate **status codes, headers, and response time**
- Use **assertions** for exact field values
- Include **token/authorization** scenarios

---

## 🔄 Example (Postman Test Script)

```
// Tests tab in Postman
pm.test("Status code is 200", function () {
    pm.response.to.have.status(200);
});

pm.test("Body contains user ID", function () {
    var jsonData = pm.response.json();
    pm.expect(jsonData.id).to.exist;
});
```

