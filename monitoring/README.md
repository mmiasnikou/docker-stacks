# Monitoring Stack

Production-ready monitoring stack with Prometheus, Grafana, and Alertmanager.

## Components

| Service | Port | Description |
|---------|------|-------------|
| Prometheus | 9090 | Metrics collection & storage |
| Grafana | 3000 | Visualization & dashboards |
| Alertmanager | 9093 | Alert routing & notifications |
| Node Exporter | 9100 | System metrics |
| cAdvisor | 8080 | Container metrics |
| Loki | 3100 | Log aggregation (optional) |
| Promtail | - | Log shipper (optional) |

## Quick Start

```bash
# Start core stack
docker-compose up -d

# With log aggregation (Loki + Promtail)
docker-compose --profile logging up -d

# Access
# Grafana:      http://localhost:3000 (admin/admin)
# Prometheus:   http://localhost:9090
# Alertmanager: http://localhost:9093
```

## Features

### Metrics Collection
- System metrics (CPU, memory, disk, network)
- Container metrics (resource usage, restarts)
- Custom application metrics support

### Alerting
Pre-configured alerts for:
- High CPU usage (>80%)
- High memory usage (>85%)
- Low disk space (<15%, <5%)
- Node/container down
- Container restarts

### Visualization
- Pre-provisioned Prometheus datasource
- Dashboard auto-provisioning
- Loki integration for logs

## Configuration

### Add Alert Notifications

Edit `alertmanager/alertmanager.yml`:

**Telegram:**
```yaml
receivers:
  - name: 'critical'
    telegram_configs:
      - bot_token: 'YOUR_BOT_TOKEN'
        chat_id: YOUR_CHAT_ID
```

**Email:**
```yaml
receivers:
  - name: 'critical'
    email_configs:
      - to: 'admin@example.com'
        from: 'alerts@example.com'
        smarthost: 'smtp.gmail.com:587'
        auth_username: 'user@gmail.com'
        auth_password: 'app-password'
```

### Add Custom Metrics Target

Edit `prometheus/prometheus.yml`:

```yaml
scrape_configs:
  - job_name: 'myapp'
    static_configs:
      - targets: ['myapp:8080']
    metrics_path: /metrics
```

Then reload:
```bash
curl -X POST http://localhost:9090/-/reload
```

### Import Grafana Dashboards

Recommended dashboards from grafana.com:
- **Node Exporter Full**: ID 1860
- **Docker & System**: ID 893
- **cAdvisor**: ID 14282

Import via Grafana UI: Dashboards → Import → Enter ID

## Directory Structure

```
monitoring/
├── docker-compose.yml
├── prometheus/
│   ├── prometheus.yml
│   └── alerts/
│       └── alerts.yml
├── alertmanager/
│   └── alertmanager.yml
├── grafana/
│   └── provisioning/
│       ├── datasources/
│       └── dashboards/
└── loki/
    ├── loki-config.yml
    └── promtail-config.yml
```

## Useful Commands

```bash
# Reload Prometheus config
curl -X POST http://localhost:9090/-/reload

# Check Prometheus targets
curl http://localhost:9090/api/v1/targets

# Check active alerts
curl http://localhost:9093/api/v1/alerts

# View container logs
docker-compose logs -f prometheus

# Backup Grafana dashboards
docker exec grafana grafana-cli admin export-dashboards
```

## Scaling

For production, consider:
- Prometheus remote write to long-term storage (Thanos, Cortex)
- Grafana with external database
- Alertmanager in cluster mode
- Separate network for monitoring

## Author

Mikhail Miasnikou — System Administrator / DevOps Engineer
