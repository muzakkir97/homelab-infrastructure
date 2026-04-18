# 🗂️ Service Catalog

> Complete inventory of all services running in the homelab infrastructure.  
> **Last Updated:** April 14, 2026

---

## 📊 Service Overview

| Category | Services | Containers |
|----------|----------|------------|
| Reverse Proxy & SSL | 1 | CT 201 |
| Monitoring & Observability | 5 | CT 202-206 |
| Network Services | 2 | CT 207, Pi-hole |
| Dashboard | 1 | CT 208 |
| Automation & AI | 1 | CT 211 |
| File Storage | 1 | CT 220 |
| Gaming | 2 | CT 300, 302 |
| **Total** | **13** | **12 containers + 1 Pi** |

---

## 🔀 Reverse Proxy & SSL

### Nginx Proxy Manager (CT 201)

| Property | Value |
|----------|-------|
| **Container ID** | 201 |
| **Hostname** | nginx-proxy-manager |
| **IP Address** | 192.168.30.201 |
| **VLAN** | 30 (Services) |
| **Purpose** | Reverse proxy, SSL termination |
| **Web UI** | http://192.168.30.201:81 |
| **Upstream** | All web services |

**Proxy Hosts Configured:**
- grafana.najhin-gaming.com → CT 203:3000
- n8n.najhin-gaming.com → CT 211:5678
- cloud.najhin-gaming.com → CT 220:80
- home.najhin-gaming.com → CT 208:3000

---

## 📈 Monitoring & Observability

### Prometheus (CT 202)

| Property | Value |
|----------|-------|
| **Container ID** | 202 |
| **Hostname** | monitoring-prometheus |
| **IP Address** | 192.168.30.202 |
| **VLAN** | 30 (Services) |
| **Purpose** | Metrics collection & storage |
| **Port** | 9090 |
| **Retention** | 30 days |

**Scrape Targets:**
- Proxmox (Kuromoon): 192.168.10.5:9100
- pfSense: 192.168.10.1:9100
- Pi-hole: 192.168.30.10:9100
- All containers: 192.168.30.x:9100

---

### Grafana (CT 203)

| Property | Value |
|----------|-------|
| **Container ID** | 203 |
| **Hostname** | monitoring-grafana |
| **IP Address** | 192.168.30.203 |
| **VLAN** | 30 (Services) |
| **Purpose** | Dashboards & visualization |
| **Port** | 3000 |
| **External URL** | grafana.najhin-gaming.com |
| **Authentication** | Cloudflare Access (email OTP) |

**Data Sources:**
- Prometheus (metrics)
- Loki (logs)

---

### Loki (CT 204)

| Property | Value |
|----------|-------|
| **Container ID** | 204 |
| **Hostname** | monitoring-loki |
| **IP Address** | 192.168.30.204 |
| **VLAN** | 30 (Services) |
| **Purpose** | Log aggregation |
| **Port** | 3100 |
| **Retention** | 14 days |

**Log Sources:**
- Promtail agents on all hosts
- Syslog, auth logs, application logs

---

### Alertmanager (CT 205)

| Property | Value |
|----------|-------|
| **Container ID** | 205 |
| **Hostname** | monitoring-alertmanager |
| **IP Address** | 192.168.30.205 |
| **VLAN** | 30 (Services) |
| **Purpose** | Alert routing & notifications |
| **Port** | 9093 |

**Alert Routes:**
| Severity | Destination | Timing |
|----------|-------------|--------|
| Critical | Telegram | Immediate |
| Warning | Discord | Work hours |

**Alert Rules:** 13 rules covering host availability, CPU, memory, disk, service health.

---

### Uptime Kuma (CT 206)

| Property | Value |
|----------|-------|
| **Container ID** | 206 |
| **Hostname** | monitoring-uptime |
| **IP Address** | 192.168.30.206 |
| **VLAN** | 30 (Services) |
| **Purpose** | Service availability monitoring |
| **Port** | 3001 |

**Monitors:**
- All external subdomains (HTTP)
- Internal services (TCP/HTTP)
- Game servers (TCP)

---

## 🌐 Network Services

### DDNS (CT 207)

| Property | Value |
|----------|-------|
| **Container ID** | 207 |
| **Hostname** | network-ddns |
| **IP Address** | 192.168.30.207 |
| **VLAN** | 30 (Services) |
| **Purpose** | Dynamic DNS updates |
| **Provider** | Cloudflare |

---

### Pi-hole (Raspberry Pi 4)

| Property | Value |
|----------|-------|
| **Hardware** | Raspberry Pi 4 8GB |
| **IP Address** | 192.168.30.10 |
| **VLAN** | 30 (Services) |
| **Purpose** | DNS filtering & ad blocking |
| **Port** | 53 (DNS), 80 (Web UI) |
| **Domains Blocked** | ~489,000 |
| **Listening Mode** | ALL (multi-VLAN support) |

**Upstream DNS:** Cloudflare (1.1.1.1, 1.0.0.1)

---

## 🏠 Dashboard

### Homepage (CT 208)

| Property | Value |
|----------|-------|
| **Container ID** | 208 |
| **Hostname** | dashboard-homepage |
| **IP Address** | 192.168.30.208 |
| **VLAN** | 30 (Services) |
| **Purpose** | Homelab dashboard |
| **Software** | gethomepage.dev |
| **Port** | 3000 |
| **External URL** | home.najhin-gaming.com |

**Widgets:**
- Proxmox cluster status
- Pi-hole statistics
- Uptime Kuma monitors
- Prometheus metrics
- Weather (Petaling Jaya)
- Game server status

---

## 🤖 Automation & AI

### n8n (CT 211)

| Property | Value |
|----------|-------|
| **Container ID** | 211 |
| **Hostname** | automation-n8n |
| **IP Address** | 192.168.30.211 |
| **VLAN** | 30 (Services) |
| **Purpose** | Workflow automation |
| **Software** | n8n |
| **Port** | 5678 |
| **External URL** | n8n.najhin-gaming.com |
| **Authentication** | Cloudflare Access (email OTP) |

**Key Workflows:**
| Workflow | Purpose |
|----------|---------|
| Telegram Agent | Gilgamesh AI bot |
| GitHub Sync | AI-CONTEXT.md updates |

**Gilgamesh Features:**
- Conversation memory (20 messages)
- Smart routing (Haiku/Sonnet)
- Web search capability
- Cost tracking
- /update command for context sync

---

## 📁 File Storage

### Nextcloud (CT 220)

| Property | Value |
|----------|-------|
| **Container ID** | 220 |
| **Hostname** | nextcloud-hub |
| **IP Address** | 192.168.30.220 |
| **VLAN** | 30 (Services) |
| **Purpose** | File sync & storage |
| **Software** | Nextcloud |
| **Port** | 80 |
| **External URL** | cloud.najhin-gaming.com |
| **External Access** | Cloudflare Tunnel |
| **Storage** | 100GB (vmpool-fast) |

**Features:**
- File synchronization
- AI-CONTEXT.md backup location
- WebDAV access

---

## 🎮 Gaming

### Pterodactyl Panel (CT 300)

| Property | Value |
|----------|-------|
| **Container ID** | 300 |
| **Hostname** | gaming-panel |
| **IP Address** | 192.168.30.210 |
| **VLAN** | 30 (Services) |
| **Purpose** | Game server management |
| **Software** | Pterodactyl Panel |
| **Port** | 80 |

---

### Pterodactyl Wings (CT 302)

| Property | Value |
|----------|-------|
| **Container ID** | 302 |
| **Hostname** | gaming-wings-1 |
| **IP Address** | 192.168.30.212 |
| **VLAN** | 30 (Services) |
| **Purpose** | Game server execution |
| **Software** | Pterodactyl Wings |

**Game Servers:**
| Game | Subdomain | Port |
|------|-----------|------|
| Terraria | terraria.najhin-gaming.com | 7777 |
| Minecraft | mc.najhin-gaming.com | 25565 |

**Note:** Game server DNS uses "DNS only" mode (grey cloud), not proxied.

---

## 🔐 Security Services

### Cloudflare Access

| Service | Protection | Method |
|---------|------------|--------|
| Grafana | ✅ Protected | Email OTP |
| n8n | ✅ Protected | Email OTP |
| Homepage | ❌ Unprotected | — |
| Nextcloud | ✅ Protected | Cloudflare Tunnel |

### Tailscale

| Property | Value |
|----------|-------|
| **Subnet Router** | pfSense |
| **Networks Exposed** | 192.168.10.0/24, 192.168.20.0/24, 192.168.30.0/24 |
| **Purpose** | Primary remote access |

---

## 💾 Storage Services

### Kinmoon NAS

| Property | Value |
|----------|-------|
| **Hardware** | UGREEN DXP2800 |
| **IP Address** | 192.168.10.15 |
| **VLAN** | 10 (Management) |
| **Capacity** | 3.6 TB (WD Purple) |
| **Protocol** | SMB/CIFS |
| **Proxmox Storage ID** | kinmoon-smb |
| **Purpose** | Container backups |

---

## 📋 Port Reference

### Internal Ports

| Port | Service | Container |
|------|---------|-----------|
| 53 | Pi-hole DNS | Pi (Raspberry Pi) |
| 80 | Various web UIs | Multiple |
| 81 | NPM Admin | CT 201 |
| 3000 | Grafana / Homepage | CT 203 / CT 208 |
| 3001 | Uptime Kuma | CT 206 |
| 3100 | Loki | CT 204 |
| 5678 | n8n | CT 211 |
| 9090 | Prometheus | CT 202 |
| 9093 | Alertmanager | CT 205 |
| 9100 | Node Exporter | All hosts |

### External Ports (Port Forwarded)

| Port | Service | Destination |
|------|---------|-------------|
| 7777 | Terraria | CT 302 |
| 25565 | Minecraft | CT 302 |

---

## 🔄 Service Dependencies

```
                    ┌─────────────┐
                    │   Pi-hole   │ DNS for all
                    │ (Pi 4)      │
                    └──────┬──────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │   NPM    │ │ Prometheus│ │  n8n     │
        │ (CT 201) │ │ (CT 202) │ │ (CT 211) │
        └────┬─────┘ └────┬─────┘ └────┬─────┘
             │            │            │
             │      ┌─────┴─────┐      │
             │      ▼           ▼      │
             │ ┌────────┐ ┌────────┐   │
             │ │ Grafana│ │  Loki  │   │
             │ │(CT 203)│ │(CT 204)│   │
             │ └────────┘ └────────┘   │
             │                         │
             ▼                         ▼
    ┌─────────────────┐      ┌─────────────────┐
    │  All Web UIs    │      │   Gilgamesh     │
    │  (via reverse   │      │   (via Claude   │
    │   proxy)        │      │    API)         │
    └─────────────────┘      └─────────────────┘
```

---

*Last Updated: April 14, 2026*
