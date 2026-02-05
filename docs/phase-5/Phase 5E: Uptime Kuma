# Phase 5E: Uptime Kuma Setup - Service Availability Monitoring

> **Project:** Enterprise Homelab Infrastructure  
> **Phase:** 5E - Uptime Kuma Service Monitoring  
> **Date:** February 2026  
> **Author:** Muzakkir  
> **GitHub:** https://github.com/muzakkir97

---

## Overview

This guide documents the implementation of Uptime Kuma for service availability monitoring. While Prometheus monitors infrastructure metrics (CPU, memory, disk), Uptime Kuma answers the question: "Is my service actually responding?"

### Uptime Kuma vs Prometheus

| Feature | Prometheus | Uptime Kuma |
|---------|------------|-------------|
| **Focus** | Infrastructure metrics | Service availability |
| **Checks** | Scrape metrics endpoints | HTTP/TCP/DNS/Ping probes |
| **Question** | "How is the system performing?" | "Is the service responding?" |
| **SSL Monitoring** | ❌ | ✅ Certificate expiry alerts |
| **Response Content** | ❌ | ✅ Keyword matching |
| **Status Pages** | ❌ | ✅ Public/private pages |
| **UI** | Technical | User-friendly |

**They complement each other** - use both for comprehensive monitoring.

### Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                    UPTIME KUMA MONITORING                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  ┌──────────────────┐                                          │
│  │   Uptime Kuma    │                                          │
│  │  192.168.20.15   │                                          │
│  │     :3001        │                                          │
│  │                  │                                          │
│  │  HTTP Checks:    │                                          │
│  │  ├─ Grafana      │──────► https://grafana.example.com       │
│  │  ├─ Proxmox      │──────► https://proxmox.example.com       │
│  │  ├─ Prometheus   │──────► http://192.168.20.11:9090         │
│  │  └─ Pi-hole      │──────► http://192.168.20.10/admin/       │
│  │                  │                                          │
│  │  Notifications:  │                                          │
│  │  ├─ Telegram     │                                          │
│  │  └─ Discord      │                                          │
│  └──────────────────┘                                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## Prerequisites

- Proxmox VE with VLAN-aware networking
- Telegram Bot (for notifications)
- Discord Webhook (for notifications)
- Services to monitor (internal or external URLs)

---

## Container Specifications

| Setting | Value |
|---------|-------|
| CT ID | 206 |
| Hostname | monitoring-uptime |
| OS | Debian 12 |
| CPU | 1 core |
| RAM | 512 MB |
| Disk | 8 GB |
| IP | 192.168.20.15/24 (VLAN 20) |
| Gateway | 192.168.20.1 |

---

## Implementation Steps

### Step 1: Create LXC Container

Create the container via Proxmox Web UI with the specifications above.

### Step 2: Install Dependencies

```bash
# Update system
apt update && apt upgrade -y
apt install -y curl git

# Install Node.js 20 LTS
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt install -y nodejs

# Verify installation
node --version   # Should show v20.x
npm --version    # Should show v10.x
```

### Step 3: Create Service User

```bash
useradd --system --shell /bin/false uptime-kuma
```

### Step 4: Install Uptime Kuma

```bash
# Clone repository
cd /opt
git clone https://github.com/louislam/uptime-kuma.git
cd uptime-kuma

# Checkout stable version
git checkout 1.23.15

# Change ownership
chown -R uptime-kuma:uptime-kuma /opt/uptime-kuma

# Run setup as uptime-kuma user
su -s /bin/bash -c "npm run setup" uptime-kuma
```

> **Note:** The setup takes a few minutes. Deprecation warnings are normal and can be ignored.

### Step 5: Create Systemd Service

Create `/etc/systemd/system/uptime-kuma.service`:

```ini
[Unit]
Description=Uptime Kuma - A self-hosted monitoring tool
Documentation=https://github.com/louislam/uptime-kuma
After=network.target

[Service]
Type=simple
User=uptime-kuma
Group=uptime-kuma
WorkingDirectory=/opt/uptime-kuma
ExecStart=/usr/bin/node /opt/uptime-kuma/server/server.js
Restart=on-failure
RestartSec=5s

NoNewPrivileges=yes
ProtectSystem=strict
ProtectHome=yes
ReadWritePaths=/opt/uptime-kuma

[Install]
WantedBy=multi-user.target
```

**Start service:**

```bash
systemctl daemon-reload
systemctl enable uptime-kuma
systemctl start uptime-kuma
systemctl status uptime-kuma
```

### Step 6: Initial Setup

1. **Access Web UI:** `http://192.168.20.15:3001`
2. **Create admin account** (first-time setup)
3. **Configure language** (optional)

### Step 7: Configure Notifications

Navigate to **Settings** → **Notifications** → **Setup Notification**

#### Telegram Notification

| Field | Value |
|-------|-------|
| Notification Type | Telegram |
| Friendly Name | Telegram Alerts |
| Bot Token | `<YOUR_BOT_TOKEN>` |
| Chat ID | `<YOUR_CHAT_ID>` |

Click **Test** to verify, then **Save**.

#### Discord Notification

| Field | Value |
|-------|-------|
| Notification Type | Discord |
| Friendly Name | Discord Alerts |
| Discord Webhook URL | `<YOUR_WEBHOOK_URL>` |

Click **Test** to verify, then **Save**.

### Step 8: Add Monitors

Click **Add New Monitor** for each service:

#### External Services (via public URLs)

| Service | Type | URL | Interval |
|---------|------|-----|----------|
| Grafana | HTTP(s) | https://grafana.example.com | 60s |
| Proxmox | HTTP(s) | https://proxmox.example.com | 60s |

#### Internal Services (via private IPs)

| Service | Type | URL | Interval |
|---------|------|-----|----------|
| Prometheus | HTTP(s) | http://192.168.20.11:9090/-/healthy | 60s |
| Alertmanager | HTTP(s) | http://192.168.20.14:9093/-/healthy | 60s |
| Pi-hole | HTTP(s) | http://192.168.20.10/admin/ | 60s |

**For each monitor:**
1. Set friendly name
2. Configure URL
3. Set heartbeat interval (60 seconds recommended)
4. Set retries (3 recommended)
5. Enable notification channels
6. Save

### Step 9: Install Node Exporter (Self-Monitoring)

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

### Step 10: Add to Prometheus

On your **Prometheus server**, add to `scrape_configs` in `/etc/prometheus/prometheus.yml`:

```yaml
  - job_name: 'uptime-kuma'
    static_configs:
      - targets: ['192.168.20.15:9100']
        labels:
          host: 'monitoring-uptime'
          vlan: 'services'
```

**Reload Prometheus:**

```bash
systemctl reload prometheus
```

---

## Monitor Types Available

| Type | Use Case |
|------|----------|
| HTTP(s) | Web services, APIs, health endpoints |
| TCP Port | Database ports, custom services |
| Ping | Basic host availability |
| DNS | DNS server health |
| Docker | Container status (requires Docker socket) |
| Steam Game Server | Gaming servers |
| MQTT | Message broker health |

---

## Advanced Configuration

### SSL Certificate Monitoring

For HTTPS monitors, Uptime Kuma automatically tracks:
- Certificate expiry date
- Certificate validity
- Alerts before expiration (configurable)

### Response Content Validation

For HTTP monitors, you can verify response content:

| Field | Example |
|-------|---------|
| Expected Status Code | 200 |
| Keyword (must contain) | "healthy" |
| Keyword (must not contain) | "error" |

### Status Pages

Create public or private status pages:

1. Go to **Status Pages**
2. Click **New Status Page**
3. Add monitors to display
4. Configure visibility (public/private)
5. Share URL with stakeholders

---

## Verification Checklist

- [ ] Uptime Kuma service running (`systemctl status uptime-kuma`)
- [ ] Web UI accessible (http://192.168.20.15:3001)
- [ ] Admin account created
- [ ] Telegram notifications tested
- [ ] Discord notifications tested
- [ ] All monitors showing "Up"
- [ ] Node exporter metrics being scraped by Prometheus

---

## Troubleshooting

### Service Won't Start

```bash
# Check logs
journalctl -u uptime-kuma -f

# Common issues:
# - Port 3001 already in use
# - Permission issues on /opt/uptime-kuma
# - Node.js version incompatibility
```

### Monitor Shows Down (False Positive)

```bash
# Test from the container
curl -I <monitor_url>

# Common issues:
# - DNS resolution failure
# - Firewall blocking outbound
# - SSL certificate issues
# - Target requires authentication
```

### Notifications Not Working

1. **Test notification** in Uptime Kuma settings
2. Check bot token / webhook URL
3. Verify internet access from container:
   ```bash
   curl -I https://api.telegram.org
   curl -I https://discord.com
   ```

### High Resource Usage

```bash
# Check resource usage
htop

# Solutions:
# - Increase heartbeat interval (60s → 120s)
# - Reduce number of monitors
# - Increase container RAM
```

---

## Maintenance

### Backup

Uptime Kuma stores data in SQLite. Backup the database:

```bash
cp /opt/uptime-kuma/data/kuma.db /backup/kuma.db.$(date +%Y%m%d)
```

### Update

```bash
cd /opt/uptime-kuma
systemctl stop uptime-kuma
git fetch --all
git checkout <new_version>
su -s /bin/bash -c "npm run setup" uptime-kuma
systemctl start uptime-kuma
```

---

## Complete Monitoring Stack Summary

After Phase 5D and 5E, the monitoring stack includes:

| Component | Purpose | Port |
|-----------|---------|------|
| Prometheus | Metrics collection | 9090 |
| Grafana | Visualization | 3000 |
| Loki | Log aggregation | 3100 |
| Alertmanager | Alert routing | 9093 |
| Uptime Kuma | Service availability | 3001 |

---

## Resources

- [Uptime Kuma GitHub](https://github.com/louislam/uptime-kuma)
- [Uptime Kuma Wiki](https://github.com/louislam/uptime-kuma/wiki)
- [Node.js Downloads](https://nodejs.org/)

---

## Next Steps

- **Phase 5F:** DDNS configuration with Cloudflare
- **Phase 5G:** Additional Prometheus exporters (pfSense, Pi-hole API)
- **Phase 6:** Storage infrastructure with UGREEN NAS
