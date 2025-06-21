# ⚖️ Guide: Load Balancing — A Systems Design Deep Dive

**Load balancing** is a critical pattern in distributed systems, ensuring high availability, fault tolerance, and scalable performance by distributing traffic across multiple backend components.

This guide explores:
- What load balancing is and why it matters
- Layer 4 vs Layer 7 strategies
- Algorithms, health checks, session handling
- Real-world implementations and pitfalls

---

## 📦 1. What is Load Balancing?

Load balancing is the process of distributing incoming network traffic across multiple backend servers (or nodes) to ensure:

- No single server is overwhelmed
- High availability during failure
- Efficient utilization of resources
- Lower latency and higher throughput

### Basic Architecture

```
Client ─────┐
            ├──▶ Load Balancer ──▶ Server 1
            ├──▶                  └─▶ Server 2
            └──▶                  └─▶ Server 3
```

---

## ⚙️ 2. Types of Load Balancing

### 🧱 Layer 4: Transport-level (TCP/UDP)

- Works at network layer
- Balances IP packets
- Fast, simple, protocol-agnostic
- Doesn’t understand HTTP, gRPC, etc.

**Example tools**: HAProxy (TCP mode), AWS NLB

```
frontend tcp-in
  bind *:443
  default_backend web-servers

backend web-servers
  balance roundrobin
  server s1 10.0.0.1:443 check
  server s2 10.0.0.2:443 check
```

---

### 🌐 Layer 7: Application-level (HTTP/HTTPS)

- Understands protocols like HTTP, gRPC
- Can inspect headers, URLs, cookies
- Allows advanced routing, auth, A/B testing

**Example tools**: NGINX, Envoy, AWS ALB, Traefik

```
location /api/v1/ {
  proxy_pass http://backend-api-v1;
}
```

---

## 📊 3. Load Balancing Algorithms

| Algorithm          | Description                                 | Use Case                         |
|--------------------|---------------------------------------------|----------------------------------|
| **Round Robin**     | Evenly distribute requests in order         | Stateless services               |
| **Least Connections** | Send to server with fewest active requests| Long-lived or variable load      |
| **IP Hash**          | Hash IP to route consistently              | Session affinity                 |
| **Random**           | Select randomly from pool                  | Simple but not optimal           |
| **Weighted**         | Favor stronger machines                    | Mixed-capacity nodes             |

---

## ❤️ 4. Session Persistence ("Sticky Sessions")

Some apps (like login sessions) need a user to always talk to the same backend server.

### Options:
- **IP Hashing**: Simple but not accurate under NAT
- **Cookie-Based**: LB injects cookie to maintain affinity
- **Session Stores**: Use Redis or DB for external state

```
// In NGINX with sticky cookie
upstream app {
  sticky cookie srv_id expires=1h domain=.yourapp.com path=/;
  server 10.0.0.1;
  server 10.0.0.2;
}
```

---

## 🛠️ 5. Health Checks & Failover

Liveness and readiness checks prevent LBs from routing traffic to unhealthy nodes.

- **Active checks**: Ping endpoints or make synthetic requests
- **Passive checks**: Observe failed responses and eject nodes

```
// AWS-style HTTP health check
GET /healthz HTTP/1.1
Host: your-service.com

-- returns HTTP 200 if healthy
```

---

## 🔐 6. Load Balancers in Multi-tier Systems

```
Client → Global LB (GeoDNS / Anycast)
        → Regional LB (CDN / Edge)
            → App LB (L7 routing)
                → Internal LB (DB / Cache sharding)
```

Each layer may use a different technique and tool, e.g.:

| Layer             | Tool                         |
|-------------------|------------------------------|
| Global            | Cloudflare, Route53, GSLB    |
| Regional          | Fastly, Akamai, AWS CloudFront |
| L7 App Balancer   | Envoy, NGINX, ALB            |
| Internal L4       | HAProxy, NLB, gRPC round robin|

---

## 🧠 7. Trade-offs & Gotchas

| Concern               | Watch Out For                            |
|-----------------------|------------------------------------------|
| Session Affinity      | Can break scalability if not managed     |
| Health Check Frequency| Too aggressive = false positives         |
| DNS TTL               | Short TTL needed for dynamic IPs         |
| Cache Invalidation    | Some LBs cache incorrectly at L7         |
| Cold Starts           | New containers might fail health checks  |
| Coordinated Omission  | Don’t ignore tail latencies in tests     |

---

## 📚 8. Real-World Load Balancers

| Tool           | Type     | Notes                                |
|----------------|----------|--------------------------------------|
| **NGINX**      | L7       | Popular reverse proxy & HTTP LB     |
| **HAProxy**    | L4/L7    | Fast, widely used in infra          |
| **Envoy**      | L7       | gRPC/native HTTP2, dynamic config   |
| **AWS ALB/NLB**| L7/L4    | Fully managed, auto-scaling         |
| **Traefik**    | L7       | Modern, cloud-native, dynamic       |
| **Kubernetes Services** | L4/L7 | Built-in kube-proxy & ingress |

---

## 🧪 9. Benchmarking Load Balancer Performance

To test throughput, latency, error recovery:

- Use tools like **wrk**, **hey**, **vegeta**, or **Locust**
- Simulate burst traffic and failure conditions
- Measure tail latencies (P95, P99) and error rates

```
# Example: 100 connections for 30s test
wrk -t10 -c100 -d30s http://localhost:8080
```

---

## 🧰 10. Load Balancing in Kubernetes

- **Service**: Basic L4 load balancer via kube-proxy
- **Ingress Controller**: NGINX/Traefik/HAProxy handling L7 HTTP traffic
- **Service Mesh**: Istio, Linkerd use **Envoy sidecars** to handle intra-cluster traffic

---

## ✅ Summary Table

| Feature            | L4 Load Balancer     | L7 Load Balancer         |
|--------------------|----------------------|---------------------------|
| Protocol Awareness | TCP/UDP only         | HTTP, HTTPS, gRPC aware   |
| Routing Logic      | IP/Port based        | URL, header, cookie       |
| Performance        | Faster               | Slight overhead           |
| Flexibility        | Lower                | Very flexible             |
| Use Case           | DBs, gRPC, raw APIs  | Web apps, APIs, proxies   |

---

## 📚 Further Reading

- [NGINX Load Balancing Guide](https://docs.nginx.com/nginx/load-balancing/)
- [HAProxy Configuration Examples](https://www.haproxy.com/documentation/)
- [AWS ALB vs NLB Comparison](https://aws.amazon.com/elasticloadbalancing/features/)
- [Google SRE Book - Load Balancing](https://sre.google/sre-book/load-balancing-frontends/)
- [Envoy Proxy Docs](https://www.envoyproxy.io/)

---
