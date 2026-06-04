# homelab-monitoring

Two VMs communicating over a local network — one exposing system metrics, the other collecting and displaying them on a Grafana dashboard.

Built to practice system configuration, network analysis and infrastructure setup.

## How it works

```
VM1 — target                       VM2 — observability
────────────────────               ──────────────────────────
Node Exporter  :9100   ──────────► Prometheus  :9090
cAdvisor       :8080   ──────────► Alertmanager :9093
                                   Grafana      :3000
```

Prometheus runs on VM2 and scrapes metrics from VM1 every 15 seconds.
Grafana queries Prometheus and displays the data on dashboards.

## Stack

| Service | Role | Port |
|---|---|---|
| Node Exporter | Host metrics — CPU, memory, disk, network | 9100 |
| cAdvisor | Docker container metrics | 8080 |
| Prometheus | Collects and stores metrics | 9090 |
| Alertmanager | Handles alerts | 9093 |
| Grafana | Dashboard visualization | 3000 |

## Project structure

```
homelab-monitoring/
├── vm1/
│   └── docker-compose.yml
└── vm2/
    ├── docker-compose.yml
    └── prometheus/
        └── prometheus.yml
```

## Setup

**VM1:**
```bash
git clone https://github.com/s3vla/homelab-monitoring.git
cd homelab-monitoring/vm1
docker compose up -d
```

**VM2:**

Edit `vm2/prometheus/prometheus.yml` and replace `IP_DA_VM1` with the actual IP of VM1:

```yaml
- targets: ["VM1_IP:9100"]
- targets: ["VM1_IP:8080"]
```

```bash
cd homelab-monitoring/vm2
docker compose up -d
```

## Access

| Service | URL |
|---|---|
| Grafana | http://VM2_IP:3000 |
| Prometheus | http://VM2_IP:9090 |
| Alertmanager | http://VM2_IP:9093 |

Default Grafana credentials: `admin` / `admin123`

## Dashboard

Uses **Node Exporter Full** (ID: 1860) from Grafana.com.

## Requirements

- Two VMs running Ubuntu Server
- Docker and Docker Compose on both
- Both VMs on the same local network (Bridge Adapter on VirtualBox)

## Next steps

- Configure alerts on Alertmanager
- Add Loki for log collection
- Replace Grafana with a custom React + Node.js dashboard
- Deploy on a dedicated physical server
