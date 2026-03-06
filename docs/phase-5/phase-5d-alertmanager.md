# Phase 5D: Alertmanager Setup - Homelab Monitoring Stack

> **Project:** Enterprise Homelab Infrastructure  
> **Phase:** 5D - Alertmanager with Telegram & Discord Notifications  
> **Date:** February 2026  
> **Author:** Muzakkir  
> **GitHub:** https://github.com/muzakkir97

---

## Overview

This guide documents the implementation of Alertmanager for proactive infrastructure alerting. Alertmanager receives alerts from Prometheus and routes them to appropriate notification channels based on severity.

### What We're Building

```
┌─────────────────────────────────────────────────────────────────┐
│                    ALERTING ARCHITECTURE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────┐        ┌──────────────┐                      │
│  │  Prometheus  │───────►│ Alertmanager │                      │
│  │              │  :9093 │              │                      │
│  │ alert_rules  │        │   Routes:    │                      │
│  │  - HostDown  │        │  critical ──►├──► Telegram + Discord│
│  │  - HighCPU   │        │  warning ───►├──► Discord only      │
│  │  - DiskLow   │        │              │                      │
│  └──────────────┘        └──────────────┘                      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Why Alertmanager?

| Without Alertmanager | With Alertmanager |
|---------------------|-------------------|
| Check Grafana manually | Get notified automatically |
| Discover problems after impact | Know before users are affected |
| No severity routing | Critical alerts wake you up |
| Alert fatigue from duplicates | Grouped, deduplicated alerts |

---

## Prerequisites

- Prometheus running and collecting metrics
- Telegram Bot (created via @BotFather)
- Discord Webhook URL
- Network access from Alertmanager to internet (for APIs)

---

## Container Specifications

| Setting | Value |
|---------|-------|
| CT ID | 205 |
| Hostname | monitoring-alertmanager |
| OS | Debian 12 |
| CPU | 1 core |
| RAM | 512 MB |
| Disk | 5 GB |
| IP | 192.168.20.14/24 (VLAN 20) |
| Gateway | 192.168.20.1 |

---

## Implementation Steps

### Step 1: Create LXC Container

Create the container via Proxmox Web UI with the specifications above.

**Important:** If using VLANs, ensure the bridge has VLAN awareness:

```bash
# On Proxmox host - add VLAN to bridge if needed
bridge vlan add vid 20 dev vmbr0 self
```

### Step 2: Install Alertmanager

```bash
# Update system
apt update && apt upgrade -y
apt install -y wget curl tar

# Create system user
useradd --no-create-home --shell /bin/false alertmanager

# Download Alertmanager
cd /tmp
wget https://github.com/prometheus/alertmanager/releases/download/v0.27.0/alertmanager-0.27.0.linux-amd64.tar.gz

# Extract and install
tar xvfz alertmanager-0.27.0.linux-amd64.tar.gz
mv alertmanager-0.27.0.linux-amd64/alertmanager /usr/local/bin/
mv alertmanager-0.27.0.linux-amd64/amtool /usr/local/bin/

# Set ownership
chown alertmanager:alertmanager /usr/local/bin/alertmanager
chown alertmanager:alertmanager /usr/local/bin/amtool

# Create directories
mkdir -p /etc/alertmanager /var/lib/alertmanager
chown -R alertmanager:alertmanager /etc/alertmanager
chown -R alertmanager:alertmanager /var/lib/alertmanager

# Verify installation
/usr/local/bin/alertmanager --version
```

### Step 3: Configure Alertmanager

Create `/etc/alertmanager/alertmanager.yml`:

```yaml
# Alertmanager Configuration
global:
  resolve_timeout: 5m

receivers:
  # Critical alerts - Telegram + Discord
  - name: 'critical-alerts'
    telegram_configs:
      - bot_token: '<YOUR_TELEGRAM_BOT_TOKEN>'
        chat_id: <YOUR_CHAT_ID>
        parse_mode: 'HTML'
        message: |
          🚨 <b>CRITICAL ALERT</b> 🚨
          
          <b>Alert:</b> {{ .CommonAnnotations.summary }}
          <b>Severity:</b> {{ .CommonLabels.severity }}
          <b>Instance:</b> {{ .CommonLabels.instance }}
          
          <b>Description:</b>
          {{ .CommonAnnotations.description }}
    discord_configs:
      - webhook_url: '<YOUR_DISCORD_WEBHOOK_URL>'
        title: '🚨 CRITICAL: {{ .CommonAnnotations.summary }}'
        message: |
          **Severity:** {{ .CommonLabels.severity }}
          **Instance:** {{ .CommonLabels.instance }}
          **Description:** {{ .CommonAnnotations.description }}

  # Warning alerts - Discord only
  - name: 'warning-alerts'
    discord_configs:
      - webhook_url: '<YOUR_DISCORD_WEBHOOK_URL>'
        title: '⚠️ WARNING: {{ .CommonAnnotations.summary }}'
        message: |
          **Severity:** {{ .CommonLabels.severity }}
          **Instance:** {{ .CommonLabels.instance }}
          **Description:** {{ .CommonAnnotations.description }}

  # Default fallback
  - name: 'default'
    discord_configs:
      - webhook_url: '<YOUR_DISCORD_WEBHOOK_URL>'
        title: 'ℹ️ Alert: {{ .CommonAnnotations.summary }}'
        message: |
          **Instance:** {{ .CommonLabels.instance }}
          **Description:** {{ .CommonAnnotations.description }}

route:
  receiver: 'default'
  group_by: ['alertname', 'instance']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  
  routes:
    - match:
        severity: critical
      receiver: 'critical-alerts'
      repeat_interval: 1h
      
    - match:
        severity: warning
      receiver: 'warning-alerts'
      repeat_interval: 4h

# Suppress warnings when critical fires for same alert
inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'instance']
```

**Set permissions:**

```bash
chown alertmanager:alertmanager /etc/alertmanager/alertmanager.yml
chmod 640 /etc/alertmanager/alertmanager.yml
```

**Validate configuration:**

```bash
/usr/local/bin/amtool check-config /etc/alertmanager/alertmanager.yml
```

### Step 4: Create Systemd Service

Create `/etc/systemd/system/alertmanager.service`:

```ini
[Unit]
Description=Alertmanager for Prometheus
Documentation=https://prometheus.io/docs/alerting/alertmanager/
Wants=network-online.target
After=network-online.target

[Service]
Type=simple
User=alertmanager
Group=alertmanager
ExecStart=/usr/local/bin/alertmanager \
  --config.file=/etc/alertmanager/alertmanager.yml \
  --storage.path=/var/lib/alertmanager \
  --web.listen-address=:9093 \
  --cluster.listen-address=

ExecReload=/bin/kill -HUP $MAINPID
Restart=on-failure
RestartSec=5s

NoNewPrivileges=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/var/lib/alertmanager

[Install]
WantedBy=multi-user.target
```

**Start service:**

```bash
systemctl daemon-reload
systemctl enable alertmanager
systemctl start alertmanager
systemctl status alertmanager

# Verify health
curl -s http://localhost:9093/-/healthy
```

### Step 5: Create Prometheus Alert Rules

On your **Prometheus server**, create `/etc/prometheus/alert_rules.yml`:

```yaml
groups:
  - name: host_alerts
    rules:
      # Host Down - Critical
      - alert: HostDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Host {{ $labels.instance }} is DOWN"
          description: "{{ $labels.instance }} has been unreachable for more than 1 minute."

      # CPU Alerts
      - alert: HighCpuUsage
        expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage on {{ $labels.instance }}"
          description: "CPU usage is above 80% (current: {{ $value | printf \"%.1f\" }}%)."

      - alert: CriticalCpuUsage
        expr: 100 - (avg by(instance) (irate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 95
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Critical CPU usage on {{ $labels.instance }}"
          description: "CPU usage is above 95% (current: {{ $value | printf \"%.1f\" }}%)."

      # Memory Alerts
      - alert: HighMemoryUsage
        expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High memory usage on {{ $labels.instance }}"
          description: "Memory usage is above 80% (current: {{ $value | printf \"%.1f\" }}%)."

      - alert: CriticalMemoryUsage
        expr: (1 - (node_memory_MemAvailable_bytes / node_memory_MemTotal_bytes)) * 100 > 95
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Critical memory usage on {{ $labels.instance }}"
          description: "Memory usage is above 95% (current: {{ $value | printf \"%.1f\" }}%)."

      # Disk Alerts
      - alert: DiskSpaceLow
        expr: (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"} / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"})) * 100 > 80
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Disk space low on {{ $labels.instance }}"
          description: "Disk usage on {{ $labels.mountpoint }} is above 80%."

      - alert: DiskSpaceCritical
        expr: (1 - (node_filesystem_avail_bytes{fstype!~"tmpfs|overlay"} / node_filesystem_size_bytes{fstype!~"tmpfs|overlay"})) * 100 > 95
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Disk space critical on {{ $labels.instance }}"
          description: "Disk usage on {{ $labels.mountpoint }} is above 95%."

      # System Alerts
      - alert: HighLoadAverage
        expr: node_load5 / count without(cpu) (node_cpu_seconds_total{mode="idle"}) > 2
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "High load average on {{ $labels.instance }}"
          description: "5-minute load average is more than 2x the CPU count."

      - alert: SystemdServiceFailed
        expr: node_systemd_unit_state{state="failed"} == 1
        for: 1m
        labels:
          severity: warning
        annotations:
          summary: "Systemd service failed on {{ $labels.instance }}"
          description: "Service {{ $labels.name }} has failed."

  - name: prometheus_alerts
    rules:
      - alert: PrometheusTargetMissing
        expr: up == 0
        for: 2m
        labels:
          severity: critical
        annotations:
          summary: "Prometheus target missing: {{ $labels.instance }}"
          description: "A Prometheus scrape target has been down for more than 2 minutes."

  - name: alertmanager_alerts
    rules:
      - alert: AlertmanagerNotificationsFailing
        expr: rate(alertmanager_notifications_failed_total[5m]) > 0
        for: 5m
        labels:
          severity: warning
        annotations:
          summary: "Alertmanager notifications failing"
          description: "Alertmanager is failing to send notifications."
```

### Step 6: Update Prometheus Configuration

Add to `/etc/prometheus/prometheus.yml`:

```yaml
# Add after global section
alerting:
  alertmanagers:
    - static_configs:
        - targets:
            - 192.168.20.14:9093

rule_files:
  - "alert_rules.yml"

# Add to scrape_configs
scrape_configs:
  - job_name: 'alertmanager'
    static_configs:
      - targets: ['192.168.20.14:9100']
        labels:
          host: 'monitoring-alertmanager'
          vlan: 'services'
```

**Reload Prometheus:**

```bash
systemctl reload prometheus
```

### Step 7: Install Node Exporter (Self-Monitoring)

```bash
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz
tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz
mv node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/

# Create service
cat > /etc/systemd/system/node_exporter.service << 'EOF'
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
Type=simple
ExecStart=/usr/local/bin/node_exporter

[Install]
WantedBy=multi-user.target
EOF

systemctl daemon-reload
systemctl enable node_exporter
systemctl start node_exporter
```

---

## Testing Alerts

### Send Test Alert

```bash
curl -XPOST http://localhost:9093/api/v2/alerts \
  -H "Content-Type: application/json" \
  -d '[
    {
      "labels": {
        "alertname": "TestAlert",
        "severity": "critical",
        "instance": "test-instance:9100"
      },
      "annotations": {
        "summary": "This is a TEST alert",
        "description": "Testing notifications. If you see this, it works!"
      }
    }
  ]'
```

### Verify Alert Routes

| Severity | Telegram | Discord |
|----------|----------|---------|
| Critical | ✅ | ✅ |
| Warning | ❌ | ✅ |

---

## Verification Checklist

- [ ] Alertmanager service running (`systemctl status alertmanager`)
- [ ] Web UI accessible (http://192.168.20.14:9093)
- [ ] Alert rules loaded in Prometheus (/alerts page)
- [ ] Test alert received on Telegram
- [ ] Test alert received on Discord
- [ ] Node exporter metrics being scraped

---

## Troubleshooting

### Config Validation Failed

```bash
# Check syntax
/usr/local/bin/amtool check-config /etc/alertmanager/alertmanager.yml

# Common issues:
# - YAML indentation (use spaces, not tabs)
# - Missing quotes around URLs
# - Invalid chat_id format (should be integer)
```

### Notifications Not Sending

```bash
# Check Alertmanager logs
journalctl -u alertmanager -f

# Common issues:
# - Firewall blocking outbound HTTPS
# - Invalid bot token or webhook URL
# - Rate limiting from Telegram/Discord
```

### Prometheus Not Connecting

```bash
# Check Prometheus config
promtool check config /etc/prometheus/prometheus.yml

# Verify Alertmanager is reachable from Prometheus
curl http://192.168.20.14:9093/-/healthy
```

---

## Alert Summary

| Alert Name | Condition | Severity |
|------------|-----------|----------|
| HostDown | Target unreachable > 1min | Critical |
| CriticalCpuUsage | CPU > 95% for 2min | Critical |
| HighCpuUsage | CPU > 80% for 5min | Warning |
| CriticalMemoryUsage | Memory > 95% for 2min | Critical |
| HighMemoryUsage | Memory > 80% for 5min | Warning |
| DiskSpaceCritical | Disk > 95% for 2min | Critical |
| DiskSpaceLow | Disk > 80% for 5min | Warning |
| HighLoadAverage | Load > 2x CPUs for 5min | Warning |
| SystemdServiceFailed | Service failed | Warning |

---

## Resources

- [Alertmanager Documentation](https://prometheus.io/docs/alerting/latest/alertmanager/)
- [Prometheus Alerting Rules](https://prometheus.io/docs/prometheus/latest/configuration/alerting_rules/)
- [Telegram Bot API](https://core.telegram.org/bots/api)
- [Discord Webhooks](https://discord.com/developers/docs/resources/webhook)

---

## Next Steps

- **Phase 5E:** Uptime Kuma for service availability monitoring
- **Phase 5F:** DDNS configuration with Cloudflare
- **Phase 6:** Storage infrastructure with UGREEN NAS
