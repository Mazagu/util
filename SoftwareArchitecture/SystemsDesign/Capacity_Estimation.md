# 📊 System Design: Capacity Estimation (Deep Dive)

Capacity estimation helps you **predict the resources** your system will need to handle **expected load**, prevent downtime, and plan for **scaling**.

---

## 🎯 Why Capacity Estimation Matters

- Avoids underprovisioning (crashes) and overprovisioning (wasted cost)
- Helps choose storage engines, compute resources, and databases wisely
- Provides confidence in meeting SLAs and SLOs
- Sets a solid baseline for autoscaling or load balancing

---

## 🧠 Inputs to Estimate

1. **Traffic Load**
   - Requests per second (RPS) or queries per second (QPS)
   - Daily Active Users (DAU) / Monthly Active Users (MAU)
   - Peak load vs average load

2. **Data Size**
   - Average payload size per request/response
   - Daily data growth rate (e.g., logs, user uploads, analytics)

3. **Retention Requirements**
   - How long data needs to be stored (e.g., 30 days, 6 months, forever)

4. **Processing Overhead**
   - CPU usage per request
   - Memory footprint per user/session

5. **Network Throughput**
   - Bandwidth required for serving requests
   - Ingress/Egress load between services

---

## 🔍 Example Questions to Ask

```
- What is the expected RPS at launch?
- What’s the peak traffic multiplier? (e.g., 5x daily average)
- How big is each write or read operation in bytes?
- How long do we store user logs, uploads, or sessions?
- How many database read/write ops per second are expected?
```

---

## 📐 Estimation Formula Examples

### 📦 Storage Estimation

```
Daily Storage = Requests per Day × Payload Size  
Total Storage (N days) = Daily Storage × Retention Period

Example:
1,000,000 requests/day × 2 KB = ~2 GB/day  
Retention = 90 days → 180 GB total
```

---

### 🧮 Compute Estimation

```
Requests per second = Total Requests per Day / 86,400

If each request takes 20ms of CPU:
  CPU Load = RPS × 20ms
  = 1000 RPS × 20ms = 20 CPU cores

Add headroom (~30%) → 26 cores
```

---

### 🔄 Database IOPS Estimation

```
- Read Ops = RPS × Read ops/request
- Write Ops = RPS × Write ops/request

Add 50% overhead for replication/indexing
```

---

## ⚙️ Tools & Techniques

- **Spreadsheets** – Manual calculations for traffic, storage, CPU
- **Load Testing Tools** – Apache JMeter, k6, Locust
- **Monitoring Baselines** – Use real metrics if available (Grafana, Datadog)
- **Capacity Planning Sheets** – Structured templates to track estimates

---

## 🧱 Capacity Estimation Framework

```
1. Estimate baseline RPS / QPS
2. Factor in peak load (e.g., 2x or 10x burst traffic)
3. Estimate per-request data and processing size
4. Project data growth (daily, monthly, yearly)
5. Estimate backend capacity (DB IOPS, bandwidth)
6. Add buffer (25–50%) for safety and growth
```

---

## 💡 Pro Tips

- Always plan for **peak**, not average.
- Use **real usage data** from similar apps when possible.
- Leave room for **future features** and traffic spikes.
- Validate assumptions with engineers and business teams.
- Revisit estimations as product evolves or usage patterns shift.
