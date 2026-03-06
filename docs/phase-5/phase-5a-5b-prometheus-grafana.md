# Phase 5A & 5B: Monitoring Stack Deployment

## Phase Overview

| Phase | Component | Status |
|-------|-----------|--------|
| 5A | Prometheus + Node Exporters | ✅ Complete |
| 5B | Grafana + External Access + 2FA | ✅ Complete |

---

# Phase 5A: Prometheus & Node Exporter

## Objectives

Deploy a comprehensive metrics collection infrastructure using Prometheus and node_exporter agents across all monitored hosts.

## Architecture

```
┌─────────────────────────────────────────────────────────────────────┐
│                        VLAN 1 (Management)                          │
│   ┌─────────────────┐                                               │
│   │  Proxmox Host   │◄──────────────────┐                          │
│   │  [MGMT_NET]     │  node_exporter    │                          │
│   │      :9100      │  port 9100        │                          │
│   └─────────────────┘                   │                          │
├─────────────────────────────────────────┼──────────────────────────┤
│                        VLAN 20 (Services)                           │
│   ┌─────────────────┐     Scrapes       │                          │
│   │   Prometheus    │───────────────────┘                          │
│   │ [SERVICES_NET]  │                                              │
│   │   :9090/:9100   │◄──────────────────┐                          │
│   └────────┬────────┘                   │                          │
│            │                            │                          │
│   ┌────────▼────────┐  node_exporter    │                          │
│   │    Pi-hole      │  port 9100        │                          │
│   │ [SERVICES_NET]  │───────────────────┤                          │
│   └─────────────────┘                   │                          │
├─────────────────────────────────────────┼──────────────────────────┤
│                        VLAN 30 (DMZ)                                │
│   ┌─────────────────┐  node_exporter    │                          │
│   │      NPM        │  port 9100        │                          │
│   │   [DMZ_NET]     │───────────────────┘                          │
│   └─────────────────┘                                              │
└─────────────────────────────────────────────────────────────────────┘
```

## Component Specifications

### Prometheus Container

| Setting | Value | Rationale |
|---------|-------|-----------|
| Container Type | LXC (Proxmox) | Lower overhead than VM |
| CT ID | 202 | Monitoring stack numbering (2xx) |
| Hostname | monitoring-prometheus | Function-based naming |
| OS | Debian 12 | Stable, well-supported |
| CPU | 2 cores | TSDB compaction needs |
| RAM | 2048 MB | Active time-series in memory |
| Disk | 50 GB | 30-day retention storage |
| VLAN | 20 (Services) | Isolated from DMZ |

### Node Exporter Deployments

| Host | Architecture | Port | Purpose |
|------|--------------|------|---------|
| Prometheus LXC | amd64 | 9100 | Self-monitoring |
| Proxmox Host | amd64 | 9100 | Hypervisor metrics |
| Pi-hole | arm64 | 9100 | DNS server metrics |
| NPM Container | amd64 | 9100 | Reverse proxy metrics |

## Implementation Guide

### 1. Create Prometheus LXC Container

```
Proxmox Web UI → Create CT

General:
  - CT ID: 202
  - Hostname: monitoring-prometheus

Resources:
  - Disk: 50GB
  - CPU: 2 cores
  - Memory: 2048 MB

Network:
  - Bridge: vmbr0
  - VLAN Tag: 20
  - IPv4: Static (e.g., 192.168.20.11/24)
  - Gateway: VLAN 20 gateway
  - DNS: Pi-hole IP
```

### 2. Install Prometheus

```bash
# System preparation
apt update && apt upgrade -y
apt install -y curl wget gnupg2 software-properties-common

# Create prometheus user
useradd --no-create-home --shell /bin/false prometheus
mkdir -p /etc/prometheus /var/lib/prometheus

# Download and install
cd /tmp
wget https://github.com/prometheus/prometheus/releases/download/v2.51.2/prometheus-2.51.2.linux-amd64.tar.gz
tar xvf prometheus-2.51.2.linux-amd64.tar.gz
cd prometheus-2.51.2.linux-amd64

cp prometheus promtool /usr/local/bin/
cp -r consoles console_libraries /etc/prometheus/

chown -R prometheus:prometheus /etc/prometheus /var/lib/prometheus
chown prometheus:prometheus /usr/local/bin/prometheus /usr/local/bin/promtool
```

### 3. Configure Prometheus

#### `/etc/prometheus/prometheus.yml`

```yaml
global:
  scrape_interval: 15s
  evaluation_interval: 15s
  external_labels:
    monitor: 'homelab-prometheus'
    environment: 'production'

alerting:
  alertmanagers:
    - static_configs:
        - targets: []  # Configure in Phase 5D

rule_files: []  # Add in Phase 5D

scrape_configs:
  - job_name: 'prometheus'
    static_configs:
      - targets: ['localhost:9090']
        labels:
          instance: 'monitoring-prometheus'

  - job_name: 'proxmox-host'
    static_configs:
      - targets: ['<PROXMOX_IP>:9100']
        labels:
          instance: 'proxmox'
          vlan: 'management'

  - job_name: 'pihole-host'
    static_configs:
      - targets: ['<PIHOLE_IP>:9100']
        labels:
          instance: 'pihole'
          vlan: 'services'

  - job_name: 'npm-host'
    static_configs:
      - targets: ['<NPM_IP>:9100']
        labels:
          instance: 'npm'
          vlan: 'dmz'

  - job_name: 'prometheus-host'
    static_configs:
      - targets: ['localhost:9100']
        labels:
          instance: 'monitoring-prometheus'
          vlan: 'services'
```

#### `/etc/systemd/system/prometheus.service`

```ini
[Unit]
Description=Prometheus Time Series Database
Documentation=https://prometheus.io/docs/introduction/overview/
Wants=network-online.target
After=network-online.target

[Service]
User=prometheus
Group=prometheus
Type=simple
ExecStart=/usr/local/bin/prometheus \
    --config.file=/etc/prometheus/prometheus.yml \
    --storage.tsdb.path=/var/lib/prometheus/ \
    --storage.tsdb.retention.time=30d \
    --storage.tsdb.retention.size=40GB \
    --web.console.templates=/etc/prometheus/consoles \
    --web.console.libraries=/etc/prometheus/console_libraries \
    --web.listen-address=0.0.0.0:9090 \
    --web.enable-lifecycle

ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 4. Install Node Exporter

#### For amd64 Systems

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvf node_exporter-1.7.0.linux-amd64.tar.gz
cp node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/

useradd --no-create-home --shell /bin/false node_exporter
chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

#### For arm64 Systems (Raspberry Pi)

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-arm64.tar.gz
tar xvf node_exporter-1.7.0.linux-arm64.tar.gz
sudo cp node_exporter-1.7.0.linux-arm64/node_exporter /usr/local/bin/

sudo useradd --no-create-home --shell /bin/false node_exporter
sudo chown node_exporter:node_exporter /usr/local/bin/node_exporter
```

#### `/etc/systemd/system/node_exporter.service`

```ini
[Unit]
Description=Node Exporter
Documentation=https://prometheus.io/docs/guides/node-exporter/
Wants=network-online.target
After=network-online.target

[Service]
User=node_exporter
Group=node_exporter
Type=simple
ExecStart=/usr/local/bin/node_exporter \
    --web.listen-address=:9100 \
    --collector.systemd \
    --collector.processes

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 5. Firewall Configuration (pfSense)

| Source | Destination | Port | Description |
|--------|-------------|------|-------------|
| Prometheus IP | Proxmox IP | 9100/TCP | Scrape Proxmox metrics |
| Prometheus IP | NPM IP | 9100/TCP | Scrape NPM metrics |

---

# Phase 5B: Grafana Deployment

## Objectives

Deploy Grafana for visualization with external access and Cloudflare Access for 2FA.

## Component Specifications

### Grafana Container

| Setting | Value |
|---------|-------|
| CT ID | 203 |
| Hostname | monitoring-grafana |
| OS | Debian 12 |
| CPU | 1 core |
| RAM | 1024 MB |
| Disk | 10 GB |
| VLAN | 20 (Services) |
| Port | 3000 |

## Implementation Guide

### 1. Create Grafana LXC Container

Same process as Prometheus, but with smaller resources.

### 2. Install Grafana

```bash
apt update && apt upgrade -y
apt install -y apt-transport-https software-properties-common wget gnupg2

# Add Grafana repository
mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | tee /etc/apt/sources.list.d/grafana.list

# Install
apt update
apt install -y grafana

# Enable and start
systemctl daemon-reload
systemctl enable grafana-server
systemctl start grafana-server
```

### 3. Configure Prometheus Data Source

1. Access Grafana: `http://<GRAFANA_IP>:3000`
2. Default login: admin/admin (change immediately)
3. Go to **Connections** → **Data sources** → **Add data source**
4. Select **Prometheus**
5. URL: `http://<PROMETHEUS_IP>:9090`
6. Click **Save & test**

### 4. Import Dashboards

| Dashboard | Grafana ID | Purpose |
|-----------|------------|---------|
| Node Exporter Full | 1860 | Detailed host metrics |

Import via: **Dashboards** → **New** → **Import** → Enter ID

### 5. External Access Setup

#### DNS Record (Cloudflare)

| Type | Name | Content | Proxy |
|------|------|---------|-------|
| A | grafana | Your public IP | DNS only (initially) |

#### NPM Proxy Host

| Field | Value |
|-------|-------|
| Domain | grafana.yourdomain.com |
| Scheme | http |
| Forward IP | Grafana container IP |
| Forward Port | 3000 |
| SSL | Request Let's Encrypt |
| Force SSL | ✅ |
| Websockets | ✅ |

#### Firewall Rule (pfSense)

| Source | Destination | Port | Description |
|--------|-------------|------|-------------|
| NPM IP | Grafana IP | 3000/TCP | NPM to Grafana reverse proxy |

### 6. Cloudflare Access (2FA Alternative)

Since Grafana OSS doesn't support native TOTP 2FA, use Cloudflare Access:

1. Go to Cloudflare Zero Trust dashboard
2. **Access** → **Applications** → **Add application**
3. Select **Self-hosted**
4. Configure:
   - Application name: Grafana
   - Domain: grafana.yourdomain.com
5. Create policy:
   - Policy name: Allow Me
   - Selector: Emails
   - Value: your-email@example.com
6. Save application

---

## Troubleshooting

### Issue: Container Can't Reach Gateway on VLAN

**Symptom:** New LXC container on VLAN 20 can't ping gateway

**Cause:** VLAN not added to Proxmox bridge

**Solution:**
```bash
# On Proxmox host
bridge vlan add vid 20 dev vmbr0 self
bridge vlan add vid 20 dev nic0

# Verify
bridge vlan show dev vmbr0
```

**Note:** This may need to be repeated after Proxmox reboot.

### Issue: Prometheus Target Shows "Connection Refused"

**Cause:** node_exporter not running on target

**Solution:**
```bash
systemctl status node_exporter
systemctl start node_exporter
```

### Issue: Prometheus Target Shows "Context Deadline Exceeded"

**Cause:** Firewall blocking cross-VLAN traffic

**Solution:** Add firewall rule in pfSense allowing Prometheus to reach target on port 9100

---

## Test Queries (PromQL)

```promql
# CPU Usage (%)
100 - (avg by(instance) (rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Memory Usage (%)
(1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100

# Disk Usage (%)
100 - ((node_filesystem_avail_bytes{mountpoint="/"} / node_filesystem_size_bytes{mountpoint="/"}) * 100)

# Network Receive Rate
rate(node_network_receive_bytes_total[5m])
```

---

## Security Summary

| Layer | Protection |
|-------|------------|
| Network | VLAN segmentation, minimal firewall rules |
| Transport | SSL/TLS via Let's Encrypt |
| Application | Grafana authentication |
| Access Control | Cloudflare Access (email OTP) |
| Infrastructure | Cloudflare proxy (DDoS, hidden origin IP) |

---

## Files Created

| Path | Host | Purpose |
|------|------|---------|
| `/etc/prometheus/prometheus.yml` | Prometheus CT | Main config |
| `/etc/systemd/system/prometheus.service` | Prometheus CT | Service file |
| `/etc/systemd/system/node_exporter.service` | All hosts | Exporter service |
| `/etc/grafana/grafana.ini` | Grafana CT | Grafana config |

---

## Next Steps

- **Phase 5C:** Loki + Promtail (Log aggregation)
- **Phase 5D:** Alertmanager (Notifications)
- **Phase 5E:** Uptime Kuma (Service monitoring)
- **Phase 5F:** DDNS (Dynamic DNS)
