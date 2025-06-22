# 🛡️ Guide: API Gateway — A Systems Design Deep Dive

An **API Gateway** is a centralized entry point that manages, routes, secures, and scales API requests in a **microservices architecture**.

It decouples clients from backend services, enabling **simplified API access**, **load management**, **security**, and **observability**.

---

## 🧠 1. What Is an API Gateway?

An **API Gateway** acts as a **reverse proxy** between clients and backend services.

It:
- Accepts all client API requests
- Routes them to the appropriate internal service
- Enforces policies such as authentication, rate limiting, caching, etc.

```
Client → API Gateway → Auth Service
                    → User Service
                    → Payment Service
```

---

## 🎯 2. Why Use an API Gateway?
| Purpose               | Benefit                                         |
|------------------------|------------------------------------------------|
| **Decoupling**         | Clients are not tied to internal service structure |
| **Security**           | Central place to enforce authentication, TLS  |
| **Rate Limiting**      | Prevents overload, abuse                      |
| **Routing & Load Balancing** | Directs traffic efficiently              |
| **Protocol Translation**| Converts REST ↔ gRPC ↔ WebSockets            |
| **Aggregation**        | Combines responses from multiple services     |
| **Monitoring**         | Unified logging and metrics                   |

---

## 🧱 3. Core Components
| Component              | Role                                           |
|------------------------|-----------------------------------------------|
| **Request Router**     | Routes to appropriate backend                 |
| **Authentication Layer**| Verifies tokens, keys                        |
| **Rate Limiter**       | Prevents overuse per client or route          |
| **Caching Layer**      | Stores frequent responses                     |
| **Transformation Layer**| Request/response shaping (e.g., JSON → XML)  |
| **Logging & Analytics**| Centralized observability                     |

---

## 🧰 4. API Gateway vs Load Balancer
| Feature               | API Gateway                        | Load Balancer               |
|------------------------|------------------------------------|-----------------------------|
| Layer                 | Application layer (L7)              | Transport layer (L4/L7)     |
| Aware of APIs         | Yes                                 | No                          |
| Auth, Throttling      | Built-in                            | Not available (unless L7)   |
| Protocol Translation  | Yes (REST ↔ gRPC, etc.)             | No                          |
| Routing Logic         | Fine-grained (method/path/service) | IP/port-based               |

---

## 🧪 5. API Gateway Patterns

### 🛠️ 1. Simple Gateway

Routes requests directly based on path or method:
```
GET /users → User Service  
POST /payments → Payment Service  
```

---

### 🔐 2. Gateway with Auth & Rate Limiting

```
// Request Flow:
→ Verify JWT Token  
→ Check quota in Redis  
→ Forward to service  
→ Log + return response
```

---

### 📦 3. Backend for Frontend (BFF)

Custom gateway per frontend (mobile, web, admin):
- Tailored aggregation and payload shaping
- Avoids over-fetching/under-fetching

---

### 📊 4. Aggregator Gateway

Combines multiple backend calls into one response:
```
GET /profile → calls:
  /user/123
  /user/123/orders
  /user/123/preferences
→ returns combined JSON
```

---

## ⚙️ 6. API Gateway Architecture Diagram

```
               ┌───────────────┐
Client ───────▶│ API Gateway   │────▶ Auth Service
               │               │────▶ User Service
               │               │────▶ Order Service
               └───────────────┘
                      │
                Logging & Metrics
```

---

## 🧱 7. Real-World Gateways
| Tool/Platform       | Features                                             |
|---------------------|------------------------------------------------------|
| **Kong**            | Open source, plugin-based, Lua core                  |
| **NGINX**           | Lightweight reverse proxy, Lua scripting available   |
| **Envoy**           | L7 proxy with native gRPC support                    |
| **AWS API Gateway** | Fully managed, integrates with Lambda, IAM, etc.     |
| **Istio Ingress Gateway** | Part of service mesh, tightly integrated with Envoy |
| **Zuul (Netflix)**  | Java-based, Spring Cloud integrated                  |

---

## 🧠 8. Common Design Considerations
| Concern                  | Solution / Notes                              |
|--------------------------|-----------------------------------------------|
| Latency                 | Add caching, keep transformations minimal      |
| Single point of failure | Deploy gateway replicas + load balancer        |
| Scaling                 | Use stateless design, autoscale pods           |
| Auth overhead           | Use opaque token + caching                     |
| Rate limiting           | Global counters in Redis or in-memory          |

---

## 🧪 9. Example: Kong Gateway Rate Limiting Plugin (Declarative Config)

```
plugins:
- name: rate-limiting
  config:
    minute: 100
    policy: local
```

---

## ✅ 10. Summary
| Feature             | Why It Matters                                 |
|---------------------|-------------------------------------------------|
| Entry point         | One place to manage all API traffic             |
| Central policies    | Auth, rate limiting, monitoring                 |
| Decoupled services  | Clients stay unaware of backend complexity      |
| Scalable            | Stateless, horizontally scalable                |
| Flexible            | Works across REST, GraphQL, gRPC, WebSockets   |

---

## 📚 Further Reading

- [Kong Gateway](https://docs.konghq.com)
- [Envoy Proxy](https://www.envoyproxy.io/)
- [AWS API Gateway](https://docs.aws.amazon.com/apigateway/latest/developerguide/welcome.html)
- [Backend for Frontend Pattern](https://samnewman.io/patterns/architectural/bff/)
- [Martin Fowler: API Gateway](https://martinfowler.com/articles/microservices.html#APIGateway)

---
