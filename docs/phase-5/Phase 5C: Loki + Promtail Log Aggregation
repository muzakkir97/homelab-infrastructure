# Phase 5C: Loki + Promtail Log Aggregation

## Phase Overview

| Attribute | Value |
|-----------|-------|
| **Phase** | 5C |
| **Component** | Loki Log Aggregation + Promtail Agents |
| **Duration** | ~1-2 hours |
| **Difficulty** | Intermediate |
| **Prerequisites** | Phase 5A & 5B complete |

## Objectives

Deploy centralized log aggregation using Loki and Promtail agents across all monitored hosts.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                        LOG FLOW                                  │
│                                                                  │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐    │
│  │ Proxmox  │   │ Pi-hole  │   │   NPM    │   │ Grafana  │    │
│  │          │   │          │   │          │   │          │    │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘    │
│       │              │              │              │           │
│       ▼              ▼              ▼              ▼           │
│  ┌──────────┐   ┌──────────┐   ┌──────────┐   ┌──────────┐    │
│  │ Promtail │   │ Promtail │   │ Promtail │   │ Promtail │    │
│  └────┬─────┘   └────┬─────┘   └────┬─────┘   └────┬─────┘    │
│       │              │              │              │           │
│       └──────────────┴──────┬───────┴──────────────┘           │
│                             │                                   │
│                             ▼                                   │
│                      ┌──────────┐                              │
│                      │   Loki   │                              │
│                      │ CT 204   │                              │
│                      └────┬─────┘                              │
│                           │                                     │
│                           ▼                                     │
│                      ┌──────────┐                              │
│                      │ Grafana  │                              │
│                      └──────────┘                              │
└─────────────────────────────────────────────────────────────────┘
```

## Component Specifications

### Loki Container

| Setting | Value | Rationale |
|---------|-------|-----------|
| Container Type | LXC | Lower overhead |
| CT ID | 204 | Monitoring stack numbering |
| Hostname | monitoring-loki | Function-based naming |
| OS | Debian 12 | Stable |
| CPU | 2 cores | Log processing needs |
| RAM | 2048 MB | Index and chunk caching |
| Disk | 50 GB | 14-day log retention |
| VLAN | 20 (Services) | Same as other monitoring |
| Port | 3100 | Loki API |

### Promtail Deployments

| Host | Architecture | Port | Log Sources |
|------|--------------|------|-------------|
| monitoring-loki | amd64 | 9080 | syslog |
| proxmox | amd64 | 9080 | syslog, auth.log, pveproxy |
| prometheus | amd64 | 9080 | syslog |
| grafana | amd64 | 9080 | syslog, grafana logs |
| npm | amd64 | 9080 | syslog, nginx access/error |
| pihole | arm64 | 9080 | syslog, pihole logs |

## Implementation Guide

### 1. Create Loki LXC Container

**Important:** Add VLAN to bridge before creating container:

```bash
# On Proxmox host
bridge vlan add vid 20 dev vmbr0 self
bridge vlan add vid 20 dev nic0
```

Create container via Proxmox Web UI with specifications above.

### 2. Install Loki

```bash
apt update && apt upgrade -y
apt install -y wget unzip curl

# Create loki user
useradd --no-create-home --shell /bin/false loki
mkdir -p /etc/loki /var/lib/loki

# Download Loki
cd /tmp
wget https://github.com/grafana/loki/releases/download/v3.0.0/loki-linux-amd64.zip
unzip loki-linux-amd64.zip
mv loki-linux-amd64 /usr/local/bin/loki
chmod +x /usr/local/bin/loki

chown loki:loki /usr/local/bin/loki
chown -R loki:loki /etc/loki /var/lib/loki
```

### 3. Configure Loki

#### `/etc/loki/loki-config.yml`

```yaml
auth_enabled: false

server:
  http_listen_port: 3100
  grpc_listen_port: 9096

common:
  instance_addr: 127.0.0.1
  path_prefix: /var/lib/loki
  storage:
    filesystem:
      chunks_directory: /var/lib/loki/chunks
      rules_directory: /var/lib/loki/rules
  replication_factor: 1
  ring:
    kvstore:
      store: inmemory

query_range:
  results_cache:
    cache:
      embedded_cache:
        enabled: true
        max_size_mb: 100

schema_config:
  configs:
    - from: 2020-10-24
      store: tsdb
      object_store: filesystem
      schema: v13
      index:
        prefix: index_
        period: 24h

ruler:
  alertmanager_url: http://localhost:9093

limits_config:
  retention_period: 14d
  ingestion_rate_mb: 10
  ingestion_burst_size_mb: 20

compactor:
  working_directory: /var/lib/loki/compactor
  compaction_interval: 10m
  retention_enabled: true
  retention_delete_delay: 2h
  delete_request_store: filesystem
```

#### `/etc/systemd/system/loki.service`

```ini
[Unit]
Description=Loki Log Aggregation System
Documentation=https://grafana.com/docs/loki/latest/
Wants=network-online.target
After=network-online.target

[Service]
User=loki
Group=loki
Type=simple
ExecStart=/usr/local/bin/loki -config.file=/etc/loki/loki-config.yml

Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 4. Install Promtail (All-in-One Script)

#### For amd64 Hosts

```bash
apt install -y wget unzip rsyslog
systemctl enable rsyslog && systemctl start rsyslog

cd /tmp
wget -q https://github.com/grafana/loki/releases/download/v3.0.0/promtail-linux-amd64.zip
unzip -o promtail-linux-amd64.zip
mv promtail-linux-amd64 /usr/local/bin/promtail
chmod +x /usr/local/bin/promtail

useradd --no-create-home --shell /bin/false promtail 2>/dev/null
mkdir -p /etc/promtail /var/lib/promtail
usermod -aG adm promtail
chown -R promtail:promtail /etc/promtail /var/lib/promtail /usr/local/bin/promtail
```

#### For arm64 Hosts (Raspberry Pi)

```bash
sudo apt install -y wget unzip rsyslog
sudo systemctl enable rsyslog && sudo systemctl start rsyslog

cd /tmp
wget -q https://github.com/grafana/loki/releases/download/v3.0.0/promtail-linux-arm64.zip
unzip -o promtail-linux-arm64.zip
sudo mv promtail-linux-arm64 /usr/local/bin/promtail
sudo chmod +x /usr/local/bin/promtail

sudo useradd --no-create-home --shell /bin/false promtail 2>/dev/null
sudo mkdir -p /etc/promtail /var/lib/promtail
sudo usermod -aG adm promtail
sudo chown -R promtail:promtail /etc/promtail /var/lib/promtail /usr/local/bin/promtail
```

### 5. Promtail Configuration Template

#### `/etc/promtail/promtail-config.yml`

```yaml
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /var/lib/promtail/positions.yaml

clients:
  - url: http://<LOKI_IP>:3100/loki/api/v1/push

scrape_configs:
  - job_name: syslog
    static_configs:
      - targets:
          - localhost
        labels:
          job: syslog
          host: <HOSTNAME>
          __path__: /var/log/syslog
```

#### Promtail Systemd Service

```ini
[Unit]
Description=Promtail Log Collector
Wants=network-online.target
After=network-online.target

[Service]
User=promtail
Group=promtail
Type=simple
ExecStart=/usr/local/bin/promtail -config.file=/etc/promtail/promtail-config.yml
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
```

### 6. Add Loki Data Source in Grafana

1. Go to **Connections** → **Data sources**
2. Click **Add data source** → Select **Loki**
3. URL: `http://<LOKI_IP>:3100`
4. Click **Save & test**

## Useful LogQL Queries

```logql
# All syslog entries
{job="syslog"}

# Logs from specific host
{host="proxmox"}

# Search for errors
{job="syslog"} |= "error"

# Search for failed SSH attempts
{job="authlog"} |= "Failed password"

# Pi-hole DNS queries
{host="pihole", job="pihole"}

# NPM access logs
{host="npm", job="npm"}

# Rate of logs per host
sum(rate({job="syslog"}[5m])) by (host)
```

## Troubleshooting

### Issue: No Labels Found in Grafana

**Cause:** Promtail not sending logs or Loki not receiving them

**Solution:**
1. Check Promtail status: `systemctl status promtail`
2. Check Promtail targets: `curl http://localhost:9080/targets`
3. Check Loki is ready: `curl http://<LOKI_IP>:3100/ready`

### Issue: Promtail Targets Show "0/1 Ready"

**Cause:** Log files don't exist (rsyslog not installed)

**Solution:**
```bash
apt install -y rsyslog
systemctl enable rsyslog
systemctl start rsyslog
systemctl restart promtail
```

### Issue: /var/log/syslog Doesn't Exist

**Cause:** Minimal Debian/Proxmox installations use journald, not rsyslog

**Solution:** Install rsyslog (see above)

## Files Created

| Path | Host | Purpose |
|------|------|---------|
| `/etc/loki/loki-config.yml` | Loki CT | Loki configuration |
| `/etc/systemd/system/loki.service` | Loki CT | Loki service |
| `/etc/promtail/promtail-config.yml` | All hosts | Promtail configuration |
| `/etc/systemd/system/promtail.service` | All hosts | Promtail service |

## Version Information

| Component | Version |
|-----------|---------|
| Loki | 3.0.0 |
| Promtail | 3.0.0 |

## Next Steps

- **Phase 5D:** Alertmanager (Telegram + Discord notifications)
- **Phase 5E:** Uptime Kuma (Service monitoring)
- **Phase 5F:** DDNS (Cloudflare dynamic DNS)
