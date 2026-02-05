# Phase 5F: Cloudflare Dynamic DNS (DDNS) Setup

## Overview

This guide covers deploying a dedicated LXC container running Cloudflare DDNS to automatically update DNS records when your public IP address changes. This is essential for homelabs with dynamic IP addresses from ISPs.

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                      DDNS ARCHITECTURE                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ISP Changes Public IP                                       │
│         │                                                    │
│         ▼                                                    │
│  ┌─────────────────────┐     ┌─────────────────────┐        │
│  │  CT 207             │     │  Cloudflare API     │        │
│  │  network-ddns       │────►│                     │        │
│  │  VLAN 20 (Services) │     │  Updates:           │        │
│  │                     │     │  - example.com      │        │
│  │  Checks every 5 min │     │  - *.example.com    │        │
│  └─────────────────────┘     └─────────────────────┘        │
│         │                                                    │
│         ▼                                                    │
│  ┌─────────────────────┐                                    │
│  │  Monitoring Stack   │                                    │
│  │  - Prometheus       │  ← Metrics on container health     │
│  │  - Loki            │  ← Logs of IP changes              │
│  │  - Uptime Kuma     │  ← Service availability            │
│  └─────────────────────┘                                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

## Prerequisites

- Proxmox VE hypervisor with VLAN configuration
- Cloudflare account with domain configured
- Existing monitoring stack (Prometheus, Loki, Grafana)
- pfSense or similar router handling VLANs

## Container Specifications

| Attribute | Value |
|-----------|-------|
| CT ID | 207 |
| Hostname | `network-ddns` |
| IP Address | 192.168.20.16/24 |
| Gateway | 192.168.20.1 |
| VLAN | 20 (Services) |
| Resources | 512MB RAM, 1 CPU, 4GB disk |
| OS | Debian 12 |

---

## Step 1: Create Cloudflare API Token

### Why Scoped Tokens?

**Principle of Least Privilege**: Only grant the minimum permissions needed. A scoped token limits damage if compromised.

| Method | Access Level | Risk |
|--------|--------------|------|
| Global API Key | Full account access | High - attacker controls everything |
| Scoped API Token | Only DNS edit for one zone | Low - limited to DNS changes |

### Create the Token

1. Log in to [Cloudflare Dashboard](https://dash.cloudflare.com)
2. Navigate to: **Profile** → **API Tokens** → **Create Token**
3. Select **Create Custom Token**
4. Configure:
   - **Token name**: `homelab-ddns`
   - **Permissions**: Zone → DNS → Edit
   - **Zone Resources**: Include → Specific zone → `your-domain.com`
5. Click **Continue to summary** → **Create Token**
6. **Copy and securely store the token** (shown only once)

### Verify Token

```bash
curl -X GET "https://api.cloudflare.com/client/v4/user/tokens/verify" \
  -H "Authorization: Bearer YOUR_TOKEN_HERE" \
  -H "Content-Type: application/json"
```

Expected response:
```json
{
  "result": { "status": "active" },
  "success": true
}
```

> ⚠️ **Security Warning**: Never share API tokens in chat, email, or commit to Git repositories. Store in a password manager or secrets management tool.

---

## Step 2: Create LXC Container

### Proxmox Web UI

1. Navigate to **Datacenter** → **pve** → **Create CT**
2. Configure each tab:

**General:**
| Field | Value |
|-------|-------|
| CT ID | 207 |
| Hostname | network-ddns |
| Password | (secure password) |

**Template:**
| Field | Value |
|-------|-------|
| Template | debian-12-standard |

**Disks:**
| Field | Value |
|-------|-------|
| Storage | local-zfs |
| Size | 4 GB |

**CPU:**
| Field | Value |
|-------|-------|
| Cores | 1 |

**Memory:**
| Field | Value |
|-------|-------|
| Memory | 512 MB |
| Swap | 256 MB |

**Network:**
| Field | Value |
|-------|-------|
| Bridge | vmbr0 |
| VLAN Tag | 20 |
| IPv4 | Static |
| IPv4/CIDR | 192.168.20.16/24 |
| Gateway | 192.168.20.1 |

**DNS:**
| Field | Value |
|-------|-------|
| DNS servers | 192.168.20.10 |

3. Check **Start after created** and click **Finish**

---

## Step 3: Install Docker

Access the container:
```bash
pct enter 207
```

Update and install prerequisites:
```bash
apt update && apt upgrade -y
apt install -y curl wget gnupg lsb-release ca-certificates apt-transport-https
```

Install Docker:
```bash
# Add Docker GPG key
curl -fsSL https://download.docker.com/linux/debian/gpg | gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Add repository
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | tee /etc/apt/sources.list.d/docker.list > /dev/null

# Install Docker
apt update
apt install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin

# Verify
systemctl status docker
```

---

## Step 4: Configure Cloudflare DDNS

### Create Configuration Directory

```bash
mkdir -p /opt/cloudflare-ddns
cd /opt/cloudflare-ddns
```

### Create Docker Compose File

```bash
cat > /opt/cloudflare-ddns/docker-compose.yml << 'EOF'
services:
  cloudflare-ddns:
    image: favonia/cloudflare-ddns:latest
    container_name: cloudflare-ddns
    restart: unless-stopped
    network_mode: host
    environment:
      # Cloudflare API Token (Edit Zone DNS permission)
      - CF_API_TOKEN=YOUR_TOKEN_HERE
      
      # Domains to update (comma-separated)
      - DOMAINS=example.com,*.example.com
      
      # Enable Cloudflare proxy (orange cloud)
      - PROXIED=true
      
      # Update interval (every 5 minutes)
      - UPDATE_CRON=*/5 * * * *
      
      # IP detection method
      - IP4_PROVIDER=cloudflare.trace
      - IP6_PROVIDER=none
      
      # Timezone
      - TZ=Asia/Kuala_Lumpur
    cap_drop:
      - ALL
    read_only: true
    security_opt:
      - no-new-privileges:true
EOF
```

### Add Your API Token

```bash
nano /opt/cloudflare-ddns/docker-compose.yml
```

Replace `YOUR_TOKEN_HERE` with your actual Cloudflare API token, then save.

### Start the Service

```bash
cd /opt/cloudflare-ddns
docker compose up -d
docker logs cloudflare-ddns
```

### Expected Output

```
🌟 Cloudflare DDNS (v1.15.1)
📖 Reading settings . . .
📖 Current settings:
   🔧 Domains, IP providers, and WAF lists:
      🔸 IPv4-enabled domains:    *.example.com, example.com
      🔸 IPv4 provider:           cloudflare.trace
   🔧 Scheduling:
      🔸 Update schedule:         */5 * * * *
      🔸 Update on start?         true
🌐 Detected the IPv4 address xxx.xxx.xxx.xxx
🐣 Added a new A record of *.example.com
🐣 Added a new A record of example.com
⏰ Checking the IP addresses in about 5m0s . . .
```

---

## Step 5: Install Monitoring Agents

### Node Exporter (Prometheus Metrics)

```bash
# Download
cd /tmp
wget https://github.com/prometheus/node_exporter/releases/download/v1.7.0/node_exporter-1.7.0.linux-amd64.tar.gz

# Install
tar xvfz node_exporter-1.7.0.linux-amd64.tar.gz
cp node_exporter-1.7.0.linux-amd64/node_exporter /usr/local/bin/
chmod +x /usr/local/bin/node_exporter

# Create systemd service
cat > /etc/systemd/system/node_exporter.service << 'EOF'
[Unit]
Description=Node Exporter
Wants=network-online.target
After=network-online.target

[Service]
User=root
ExecStart=/usr/local/bin/node_exporter
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Enable and start
systemctl daemon-reload
systemctl enable node_exporter
systemctl start node_exporter
```

### Promtail (Loki Log Shipping)

```bash
# Download
cd /tmp
wget https://github.com/grafana/loki/releases/download/v2.9.3/promtail-linux-amd64.zip
apt install -y unzip
unzip promtail-linux-amd64.zip
mv promtail-linux-amd64 /usr/local/bin/promtail
chmod +x /usr/local/bin/promtail

# Create config directory
mkdir -p /etc/promtail /var/lib/promtail

# Create configuration
cat > /etc/promtail/promtail.yml << 'EOF'
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
  filename: /var/lib/promtail/positions.yaml

clients:
  - url: http://192.168.20.13:3100/loki/api/v1/push

scrape_configs:
  - job_name: system
    static_configs:
      - targets:
          - localhost
        labels:
          job: varlogs
          host: network-ddns
          __path__: /var/log/*.log

  - job_name: syslog
    static_configs:
      - targets:
          - localhost
        labels:
          job: syslog
          host: network-ddns
          __path__: /var/log/syslog

  - job_name: docker
    static_configs:
      - targets:
          - localhost
        labels:
          job: docker
          host: network-ddns
          container: cloudflare-ddns
          __path__: /var/lib/docker/containers/*/*.log
EOF

# Create systemd service
cat > /etc/systemd/system/promtail.service << 'EOF'
[Unit]
Description=Promtail Log Collector
Wants=network-online.target
After=network-online.target

[Service]
ExecStart=/usr/local/bin/promtail -config.file=/etc/promtail/promtail.yml
Restart=always
RestartSec=5

[Install]
WantedBy=multi-user.target
EOF

# Install rsyslog for syslog
apt install -y rsyslog
systemctl enable rsyslog && systemctl start rsyslog

# Enable and start promtail
systemctl daemon-reload
systemctl enable promtail
systemctl start promtail
```

---

## Step 6: Update Prometheus Configuration

On your Prometheus server, add the new scrape target:

```bash
nano /etc/prometheus/prometheus.yml
```

Add under `scrape_configs`:

```yaml
  - job_name: 'ddns-host'
    static_configs:
      - targets: ['192.168.20.16:9100']
        labels:
          host: 'network-ddns'
```

Validate and reload:

```bash
/usr/local/bin/promtool check config /etc/prometheus/prometheus.yml
systemctl reload prometheus
```

---

## Step 7: Add Uptime Kuma Monitor

In Uptime Kuma, add a new monitor:

| Field | Value |
|-------|-------|
| Monitor Type | HTTP(s) |
| Friendly Name | Cloudflare DDNS |
| URL | http://192.168.20.16:9100/metrics |
| Heartbeat Interval | 60 |
| Retries | 3 |

---

## Verification

### Check DDNS Logs

```bash
docker logs cloudflare-ddns --tail 50
```

### Check Prometheus Target

Visit Prometheus → Status → Targets and verify `ddns-host` shows **UP**.

### Check Cloudflare DNS

In Cloudflare Dashboard → DNS → Records, verify your A records show the correct IP.

### View Logs in Grafana

In Grafana → Explore → Loki:

```
{host="network-ddns"}
```

---

## Troubleshooting

### Container Can't Reach Internet

If the container can't reach the gateway on VLAN 20:

```bash
# On Proxmox host
bridge vlan add vid 20 dev vmbr0 self
```

### Docker Permission Denied

```bash
systemctl restart docker
```

### DDNS Not Updating

Check container logs:
```bash
docker logs cloudflare-ddns
```

Common issues:
- Invalid API token (revoked or wrong permissions)
- Domain not in Cloudflare zone
- Network connectivity issues

### Prometheus Can't Scrape

Verify node_exporter is running:
```bash
curl http://192.168.20.16:9100/metrics
```

---

## Security Considerations

1. **API Token Scope**: Only grant DNS edit permission for specific zone
2. **Token Storage**: Never commit tokens to Git; use secrets management
3. **Network Isolation**: Container runs on dedicated VLAN
4. **Container Security**: Runs with dropped capabilities and read-only filesystem
5. **Monitoring**: Full observability through Prometheus, Loki, and Uptime Kuma

---

## Files Reference

| File | Purpose |
|------|---------|
| `/opt/cloudflare-ddns/docker-compose.yml` | DDNS container configuration |
| `/etc/systemd/system/node_exporter.service` | Prometheus metrics agent |
| `/etc/systemd/system/promtail.service` | Loki log shipping agent |
| `/etc/promtail/promtail.yml` | Promtail configuration |

---

## Related Documentation

- [Cloudflare DDNS Docker Image](https://github.com/favonia/cloudflare-ddns)
- [Cloudflare API Tokens](https://developers.cloudflare.com/api/tokens/)
- [Prometheus Node Exporter](https://github.com/prometheus/node_exporter)
- [Grafana Loki](https://grafana.com/docs/loki/latest/)

---

## Changelog

| Date | Change |
|------|--------|
| 2026-02-05 | Initial deployment |
