# 🌐 Traffic Routing Components
In modern system architecture, it's crucial to understand the differences and overlaps between **reverse proxies**, **API gateways**, and **load balancers**. These components handle traffic routing, improve performance, enforce policies, and increase system resilience — but serve different roles.

---

## 🧭 Overview: Key Concepts

| Component        | Primary Role                                  | Operates At        |
|------------------|------------------------------------------------|---------------------|
| **Load Balancer** | Distributes traffic across servers             | Network / Transport |
| **Reverse Proxy** | Intercepts and forwards client requests        | Application Layer   |
| **API Gateway**   | Manages, routes, and secures API requests      | Application Layer   |
| **Forward Proxy** | Client-side proxy for outbound traffic         | Client-Side         |
| **Ingress Controller** | Manages external access to Kubernetes services | Kubernetes Layer     |

---

## ⚖️ Load Balancer

### 🧠 What It Does

A **Load Balancer** spreads incoming traffic across multiple backend servers to **maximize performance**, ensure **high availability**, and avoid **overloading** any single server.

### 🔧 Types

- **Layer 4 (Transport)**: Balances based on IP/port (TCP/UDP)
- **Layer 7 (Application)**: Balances based on HTTP headers, cookies, paths

```
// Example NGINX L7 Load Balancer
http {
  upstream backend {
    server app1.example.com;
    server app2.example.com;
  }

  server {
    listen 80;

    location / {
      proxy_pass http://backend;
    }
  }
}
```

### ✅ Use Cases

- Distribute web traffic
- Scale horizontal server fleets
- Ensure HA across zones or regions

---

## 🔄 Reverse Proxy

### 🧠 What It Does

A **Reverse Proxy** sits in front of one or more servers and **forwards client requests** to them. It **hides backend implementation details**, can cache responses, and terminate SSL.

```
// Example: NGINX reverse proxy
server {
  listen 80;

  location / {
    proxy_pass http://localhost:3000;
  }
}
```

### ✅ Use Cases

- SSL termination
- Load balancing
- Caching
- URL/path rewriting
- Security enforcement

> 🔎 NGINX and HAProxy are often used as **both** reverse proxies and load balancers.

---

## 🚪 API Gateway

### 🧠 What It Does

An **API Gateway** is a specialized reverse proxy that **manages and routes API traffic**. It often includes features like **authentication**, **rate limiting**, **logging**, and **API versioning**.

```
// Example: API Gateway routes
/api/v1/users → users-service
/api/v1/orders → orders-service
```

### ✨ Features

- Request/response transformation
- API key and token validation (OAuth2, JWT)
- Quotas and throttling
- Monitoring and analytics
- CORS handling
- Header injection

### 🧰 Examples

- **Kong**, **AWS API Gateway**, **Apigee**, **Traefik**, **Istio**, **Zuul**

---

## 🕵️ Forward Proxy (Bonus)

### 🧠 What It Does

Unlike a reverse proxy, a **Forward Proxy** is used **by clients** to access external resources. It acts on behalf of the client.

```
Client → Forward Proxy → Internet
```

### ✅ Use Cases

- Content filtering (corporate firewalls)
- Access control
- Anonymity (Tor, VPNs)

---

## ☸️ Ingress Controller (Kubernetes-Specific)

### 🧠 What It Does

A **Kubernetes Ingress Controller** is a reverse proxy that handles incoming HTTP/HTTPS traffic and routes it to services in the cluster.

```
// Example ingress route
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: app-ingress
spec:
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            backend:
              service:
                name: my-service
                port:
                  number: 80
```

---

## 🧠 Key Differences

| Feature/Role          | Load Balancer      | Reverse Proxy       | API Gateway          |
|-----------------------|--------------------|----------------------|-----------------------|
| Primary Role          | Distribute traffic | Forward HTTP requests | Manage API traffic    |
| Layer                 | L4/L7              | L7                   | L7                    |
| Can modify requests   | ❌ (L4), ✅ (L7)    | ✅                   | ✅ (extensively)      |
| Authentication        | ❌                 | ⚠️ Limited            | ✅ Built-in           |
| Caching               | ❌ (usually)       | ✅                   | ✅                    |
| API versioning        | ❌                 | ❌                   | ✅                    |
| Protocol support      | TCP/UDP/HTTP       | HTTP(S)              | HTTP(S), gRPC         |

---

## 🔥 Real-World Examples

| Use Case                                 | Solution                              |
|------------------------------------------|----------------------------------------|
| Distribute user requests to 5 web servers | AWS ELB, HAProxy, NGINX (Load Balancer) |
| Route `/api/*` to microservices           | Kong, Traefik, AWS API Gateway         |
| Proxy HTTPS to an internal app            | NGINX, Envoy (Reverse Proxy)           |
| Control outbound traffic from VMs         | Squid (Forward Proxy)                  |
| Route external traffic into K8s services  | Ingress Controller (NGINX, Traefik)    |

---

## 🛠️ When to Use What

| Goal                           | Use This                      |
|--------------------------------|-------------------------------|
| Distribute load               | Load Balancer                 |
| Secure and cache static assets| Reverse Proxy                 |
| Expose, secure, and manage APIs| API Gateway                   |
| Control client access to the internet | Forward Proxy           |
| Manage traffic inside Kubernetes | Ingress Controller        |

---

## 📚 Resources

- [NGINX: Reverse Proxy vs Load Balancer](https://docs.nginx.com)
- [Kong API Gateway](https://konghq.com/)
- [AWS ELB vs API Gateway](https://docs.aws.amazon.com/)
- [Cloudflare Load Balancing](https://developers.cloudflare.com/load-balancing/)
- [HAProxy Configuration](https://www.haproxy.org/)

---
