# homelab-monitoring

Two-VM observability stack built with Docker Compose.


## Architecture

VM1 (target) exposes metrics via Node Exporter and cAdvisor.
VM2 (observability) runs Prometheus, Grafana and Alertmanager.


## Stack
- Prometheus - metrics collection and storage
- Grafana - visualization and dashboards
- Node Exporter - host metrics (CPU, memory, disk, network)
- cAdvisor - container metrics
- Alertmanager - alerting

## Structure 
vm1/ - target VM configuration
vm2/ - observability VM configuration


## Requirements
- Two VMs with Ubuntu Server
- Docker and Docker Compose installed on both


