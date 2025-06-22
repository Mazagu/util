# ⚡ Performance Benchmarking Guide for High-Performance Databases

Designing meaningful, reproducible benchmarks is essential for tuning databases under real-world workloads. This guide outlines key **principles**, **strategies**, and **pitfalls to avoid** when benchmarking latency-sensitive systems like high-performance databases, caches, and analytics engines.

---

## 🎯 Goals

- ✅ Tune query and traffic patterns based on workload reality
- ✅ Measure warm-up phases, throttling, and internal database behaviors
- ✅ Correlate **latency**, **throughput**, and **resource usage**
- ✅ Produce **clear**, **reproducible**, and **actionable** reports

---

## 1. 🧪 Designing Real-World Benchmarks

> **"If your benchmark doesn't reflect real-world usage, your tuning won't either."**

### ✅ Strategies
- Use **realistic queries** (not synthetic SELECT 1s)
- Include **read/write mixes** (e.g. 80/20 or 50/50)
- Simulate **production traffic patterns** (e.g. bursts, idle periods)
- Include **warm-up phase** to account for caches, JIT, etc.
- Benchmark **against real data sizes**, not toy datasets

```
# Example: Load test with realistic query patterns
wrk -t4 -c100 -d30s --script=benchmark.lua http://dbproxy.local/query
```

### ❌ Pitfalls
- Measuring only during cold starts (no warm-up)
- Ignoring connection pool reuse
- Testing with unbounded workloads (causes overload)

---

## 2. ⚙️ Infrastructure Sizing & Concurrency

> **"Always rightsize infra to your workload, not the other way around."**

### ✅ Tips
- Match **number of virtual users (VUs)** to peak concurrency expectations
- Add **latency injection** to model network/GC/jitter behavior
- Measure and report **CPU, memory, disk IOPS**, and **network bandwidth**

```
# Example: K6 test with ramping VUs and fixed throughput
export let options = {
  stages: [
    { duration: '1m', target: 50 },
    { duration: '5m', target: 100 }
  ],
  rps: 100,
};
```

---

## 3. ⚠️ Recognize and Mitigate Coordinated Omission

> **"Your benchmark might lie if you don’t simulate realistic delays."**

### What Is It?
If you measure response time only when you send a request, you’ll **miss the long delays** that occur under load when the system is overloaded.

### ✅ Solutions
- Use tools like **HdrHistogram** that track **time between intents** and **actual completions**
- Implement **scheduled request dispatch**, not reactive

```
# Use HdrHistogram to record actual vs intended timing
recordValueWithExpectedInterval(actual_latency_ms, expected_interval_ms)
```

### Learn More
- [Gil Tene’s Coordinated Omission talk](https://www.infoq.com/presentations/latency-response-time/)

---

## 4. 📈 Understand Latency Distributions (Not Just Averages)

> “**P99 latency** tells you what your worst users experience.”

### ✅ Key Metrics
```
| Metric      | Why It Matters                          |
|-------------|------------------------------------------|
| Avg latency | Misleading in non-normal distributions   |
| P95 / P99   | Critical for SLAs, SLOs                  |
| Max latency | Surface worst-case stalls                |
| Std. Dev    | Variability across requests              |
```
```
# Example: Wrapping a test with latency histogram
import hdrhistogram
hist = hdrhistogram.HdrHistogram(1, 3600000, 3)

for latency in results:
    hist.record_value(latency)

print("P95:", hist.get_value_at_percentile(95))
print("P99:", hist.get_value_at_percentile(99))
```

---

## 5. 🔄 Measure Warm-up, Throttling, and Internals

### ✅ Include:
- **Warm-up phases**: discard or tag first X seconds of results
- **Throttling tests**: apply capped RPS to observe graceful degradation
- **GC pauses / compactions**: log at high granularity
- **Connection churn**: does latency spike with reconnects?

```
# Warm-up example: Ignore first N seconds
start_time = time.time()
for i in range(N):
    latency = run_query()
    if time.time() - start_time > 30:
        record(latency)
```

---

## 6. 📊 Latency vs Throughput vs Resource Usage

> “A high-QPS system that spikes CPU at 80% isn't fast — it's on fire.”

### ✅ Correlate:
- Latency vs CPU usage
- Throughput vs memory pressure
- Throughput vs GC/compaction
- IO Wait vs throughput under heavy writes

Use tools like:
- `dstat`, `htop`, `vmstat`, `iostat`, `perf`
- Grafana + Prometheus dashboards
- Cloud-native: AWS CloudWatch, GCP Ops Agent

---

## 7. ✅ Reporting Best Practices

### ✅ Always Report:
- Hardware specs (CPU model, RAM, disk type, GPU if relevant)
- DB version, schema description, indexes
- RPS / concurrency / client-side latency distribution
- Resource usage over time
- Full config (Dockerfile, test scripts, system config)

```
# Basic benchmark report structure
## Setup
- DB: Postgres 15
- Host: 8-core AMD, 64GB RAM
- Client: wrk, 4 threads, 100 connections

## Results
```
| Metric    | Value      |
|-----------|------------|
| Avg Lat   | 5.2 ms     |
| P95       | 12.1 ms    |
| P99       | 21.7 ms    |
| Max       | 65.0 ms    |
| Throughput| 12,700 RPS |
```
## Observations
- P99 spikes during connection churn
- IOWait correlates with spikes on write-heavy phases
```

---

## ✅ Summary Checklist

- [x] Reflect real-world read/write/query patterns
- [x] Measure warm-up and resource warm-state behavior
- [x] Account for concurrency, coordinated omission
- [x] Use histograms and P99s — not just averages
- [x] Report full system config, results, and context

---

## 📚 Further Reading

- [Latency Numbers Every Engineer Should Know](https://people.eecs.berkeley.edu/~rcs/research/interactive_latency.html)
- [Gil Tene on Coordinated Omission](https://www.infoq.com/presentations/latency-response-time/)
- [HDR Histogram](https://hdrhistogram.github.io/HdrHistogram/)
- [Distributed Systems Observability](https://www.oreilly.com/library/view/distributed-systems-observability/9781492033431/)

---
