# 📊 Guide: Setting Up Grafana for Benchmarking Visualization

This guide walks you through setting up **Grafana** to monitor and visualize results from **performance benchmarking**, including metrics like latency percentiles, throughput, CPU, memory, and I/O. It’s designed for engineers benchmarking **databases, services**, or **systems under load**.

---

## 🧱 Requirements
```
| Tool       | Purpose                           |
|------------|-----------------------------------|
| **Grafana**| Visualization layer               |
| **Prometheus** | Metrics time-series database    |
| **Node Exporter** | Captures CPU, memory, disk stats |
| **Your Benchmark Tool** | Emits custom metrics (e.g., K6, wrk, custom script) |
```
---

## 1. 🔧 Set Up Prometheus + Node Exporter

### 1.1 Install Prometheus

```
docker run -d \
  --name=prometheus \
  -p 9090:9090 \
  -v $(pwd)/prometheus.yml:/etc/prometheus/prometheus.yml \
  prom/prometheus
```

### 1.2 Sample `prometheus.yml`

```
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'node'
    static_configs:
      - targets: ['localhost:9100']
```

### 1.3 Run Node Exporter (on target machine)

```
docker run -d \
  --name=node-exporter \
  -p 9100:9100 \
  prom/node-exporter
```

---

## 2. 📦 Set Up Grafana

```
docker run -d \
  --name=grafana \
  -p 3000:3000 \
  grafana/grafana
```

- Access Grafana at `http://localhost:3000`
- Default login: `admin / admin`

---

## 3. ➕ Add Prometheus as a Data Source in Grafana

1. Go to `Settings` > `Data Sources` > `Add data source`
2. Select **Prometheus**
3. URL: `http://prometheus:9090`
4. Click **Save & Test**

---

## 4. 📊 Import a Benchmark Dashboard

### Dashboard Template JSON

```
{
  "title": "Benchmark Performance Dashboard",
  "panels": [
    {
      "title": "Latency (P95)",
      "type": "timeseries",
      "targets": [
        {
          "expr": "histogram_quantile(0.95, rate(request_latency_bucket[1m]))",
          "legendFormat": "P95 Latency"
        }
      ]
    },
    {
      "title": "Throughput (RPS)",
      "type": "timeseries",
      "targets": [
        {
          "expr": "rate(request_count_total[1m])",
          "legendFormat": "Requests/sec"
        }
      ]
    },
    {
      "title": "CPU Usage (%)",
      "type": "timeseries",
      "targets": [
        {
          "expr": "100 - avg(irate(node_cpu_seconds_total{mode=\"idle\"}[1m])) * 100",
          "legendFormat": "CPU %"
        }
      ]
    },
    {
      "title": "Memory Usage",
      "type": "timeseries",
      "targets": [
        {
          "expr": "node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes",
          "legendFormat": "Used Memory"
        }
      ]
    }
  ]
}
```

### How to Import

1. Go to Grafana > Dashboards > Import
2. Paste the above JSON
3. Select Prometheus as the data source
4. Click **Import**

---

## 5. 📡 Emit Custom Metrics (Optional)

If you're using a custom benchmark tool, emit metrics to a **Pushgateway** or expose an endpoint Prometheus can scrape.

```
# Example Prometheus metric format (text/plain)
request_latency_seconds_bucket{le="0.01"}  52
request_latency_seconds_bucket{le="0.05"}  107
request_latency_seconds_bucket{le="0.1"}   151
request_latency_seconds_count  180
request_latency_seconds_sum    7.39
```

---

## 6. 📈 What to Track
```
| Metric             | Prometheus Query Example                                      |
|--------------------|---------------------------------------------------------------|
| P95 latency        | `histogram_quantile(0.95, rate(latency_bucket[1m]))`          |
| Throughput         | `rate(request_count_total[1m])`                               |
| CPU %              | `100 - avg(rate(node_cpu_seconds_total{mode="idle"}[1m])) * 100` |
| Memory used        | `node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes` |
| Disk I/O (reads)   | `rate(node_disk_reads_completed_total[1m])`                   |
```
---

## ✅ Tips for Reliable Dashboards

- Add **annotations** for load test start/end times
- Tag data with **build version**, **commit hash**, or **run ID**
- Export dashboards via JSON for version control
- Create alert rules (e.g. P95 > threshold)

---

## 📚 Resources

- [Grafana Docs](https://grafana.com/docs/)
- [Prometheus Docs](https://prometheus.io/docs/introduction/overview/)
- [Node Exporter Metrics Reference](https://github.com/prometheus/node_exporter)
- [Grafana Loki](https://grafana.com/oss/loki/) for logs alongside metrics

---

## 📦 Appendix: Docker Compose Setup for Grafana Benchmarking Stack

```
version: "3.8"

services:
  prometheus:
    image: prom/prometheus
    container_name: prometheus
    ports:
      - "9090:9090"
    volumes:
      - ./prometheus/prometheus.yml:/etc/prometheus/prometheus.yml
      - prometheus_data:/prometheus
    networks:
      - monitoring

  grafana:
    image: grafana/grafana
    container_name: grafana
    ports:
      - "3000:3000"
    environment:
      - GF_SECURITY_ADMIN_USER=admin
      - GF_SECURITY_ADMIN_PASSWORD=admin
    volumes:
      - grafana_data:/var/lib/grafana
    depends_on:
      - prometheus
    networks:
      - monitoring

  node-exporter:
    image: prom/node-exporter
    container_name: node_exporter
    ports:
      - "9100:9100"
    networks:
      - monitoring

volumes:
  prometheus_data:
  grafana_data:

networks:
  monitoring:
```

---

### 📄 Sample `prometheus.yml`

Save as `./prometheus/prometheus.yml`:

```
global:
  scrape_interval: 5s

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['prometheus:9090']

  - job_name: 'node_exporter'
    static_configs:
      - targets: ['node-exporter:9100']
```

---

### ▶️ How to Run

```bash
docker compose up -d
```

- Prometheus: [http://localhost:9090](http://localhost:9090)
- Grafana: [http://localhost:3000](http://localhost:3000)
  - Username: `admin`, Password: `admin`
- Node Exporter Metrics: `http://localhost:9100/metrics`

---
