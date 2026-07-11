# Prometheus Basics

## Architecture
- **Prometheus Server**: scrapes and stores metrics
- **Exporters**: expose metrics (node_exporter, etc.)
- **Alertmanager**: handles alerts
- **Grafana**: visualization

## Key Concepts
- **Metric types**: Counter, Gauge, Histogram, Summary
- **Labels**: key-value pairs for filtering
- **PromQL**: query language

```promql
rate(http_requests_total[5m])
```