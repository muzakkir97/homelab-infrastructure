# 🗂️ Service Catalog

> **Last Updated:** April 24, 2026  
> **Scope:** Complete inventory of all running services  
> **Total Services:** 28 services across 15 containers

---

## 🔧 Infrastructure & Management

### Nginx Proxy Manager
- **Container:** CT 201 (nginx-proxy-manager)
- **IP Address:** 192.168.30.201
- **Ports:** 80 (HTTP), 443 (HTTPS), 81 (Admin)
- **Access Method:** Internal admin panel
- **Credentials:** Stored in Vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** None
- **Purpose:** Reverse proxy, SSL termination, certificate management

### Dynamic DNS Service
- **Container:** CT 207 (network-ddns)
- **IP Address:** 192.168.30.207
- **Ports:** None (cron service)
- **Access Method:** Background service
- **Credentials:** Cloudflare API token in HashiCorp Vault (kv/cloudflare)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Internet connectivity, Cloudflare API
- **Purpose:** Updates Cloudflare DNS for dynamic IP changes

### Homepage Dashboard
- **Container:** CT 208 (dashboard-homepage)
- **IP Address:** 192.168.30.208
- **Ports:** 3000 (HTTP)
- **Subdomain:** home.najhin-gaming.com
- **Access Method:** Cloudflare Tunnel (no auth)
- **Credentials:** None required
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Nginx Proxy Manager
- **Purpose:** Central homelab dashboard with service links

## 📊 Monitoring & Observability

### Prometheus
- **Container:** CT 202 (monitoring-prometheus)
- **IP Address:** 192.168.30.202
- **Ports:** 9090 (HTTP)
- **Access Method:** Internal web UI
- **Credentials:** None (internal access only)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Node exporters, targets
- **Purpose:** Metrics collection and time-series database

### Grafana
- **Container:** CT 203 (monitoring-grafana)
- **IP Address:** 192.168.30.203
- **Ports:** 3000 (HTTP)
- **Subdomain:** grafana.najhin-gaming.com
- **Access Method:** Cloudflare Access (Email OTP)
- **Credentials:** Admin login in Vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Prometheus, Loki
- **Purpose:** Metrics visualization and dashboards

### Loki
- **Container:** CT 204 (monitoring-loki)
- **IP Address:** 192.168.30.204
- **Ports:** 3100 (HTTP)
- **Access Method:** API only (Grafana datasource)
- **Credentials:** None required
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Log shippers
- **Purpose:** Log aggregation and storage

### Alertmanager
- **Container:** CT 205 (monitoring-alertmanager)
- **IP Address:** 192.168.30.205
- **Ports:** 9093 (HTTP)
- **Access Method:** Internal web UI
- **Credentials:** Telegram/Discord tokens in HashiCorp Vault (kv/alertmanager)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Prometheus
- **Purpose:** Alert routing to Telegram (critical) and Discord (warnings)

### Uptime Kuma
- **Container:** CT 206 (monitoring-uptime)
- **IP Address:** 192.168.30.206
- **Ports:** 3001 (HTTP)
- **Access Method:** Internal web UI
- **Credentials:** Admin login in Vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Target services
- **Purpose:** Service availability monitoring

## 🔒 Security & Secrets Management

### HashiCorp Vault
- **Container:** CT 213 (vault)
- **IP Address:** 192.168.30.213
- **Ports:** 8200 (HTTPS)
- **Subdomain:** vault.najhin-gaming.com
- **Access Method:** Cloudflare Access (Email OTP)
- **Credentials:** Root token in Vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** None
- **Purpose:** Centralized secrets management
- **Secrets Stored:** 8 paths (kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager, kv/github, kv/nextcloud, kv/n8n, kv/pihole)

### Vaultwarden
- **Container:** CT 214 (password-vaultwarden)
- **IP Address:** 192.168.30.214
- **Ports:** 80 (HTTP)
- **Subdomain:** passwords.najhin-gaming.com
- **Access Method:** Cloudflare Access (Email OTP)
- **Credentials:** Master password (personal)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** None
- **Purpose:** Personal password management with Bitwarden compatibility

## 🤖 Automation & Workflows

### n8n Workflow Automation
- **Container:** CT 211 (automation-n8n)
- **IP Address:** 192.168.30.211
- **Ports:** 5678 (HTTP)
- **Subdomain:** n8n.najhin-gaming.com
- **Access Method:** Cloudflare Access (Email OTP)
- **Credentials:** Admin login in Vaultwarden, API tokens in HashiCorp Vault (kv/n8n)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** External APIs (Claude, Telegram, GitHub, Proxmox)
- **Purpose:** Workflow automation, Gilgamesh AI agent, documentation pipeline
- **Active Workflows:** 7 workflows (5 published, 2 unpublished)

### Gilgamesh AI Agent (Telegram Bot)
- **Platform:** Telegram (@JhinGilgamesh_bot)
- **Backend:** n8n workflow in CT 211
- **Chat ID:** 518832696
- **Access Method:** Telegram app
- **Credentials:** Bot token in HashiCorp Vault (kv/gilgamesh)
- **Dependencies:** Claude API, n8n, Proxmox API
- **Purpose:** Personal AI assistant, infrastructure control, documentation automation
- **Features:** 8 slash commands, inline keyboard menu, conversation memory, web search

## ☁️ Productivity & Cloud Services

### Nextcloud Hub
- **Container:** CT 220 (nextcloud-hub)
- **IP Address:** 192.168.30.220
- **Ports:** 80 (HTTP), 443 (HTTPS)
- **Subdomain:** cloud.najhin-gaming.com
- **Access Method:** Cloudflare Tunnel (direct access)
- **Credentials:** Admin login in Vaultwarden, app passwords in HashiCorp Vault (kv/nextcloud)
- **Backup Schedule:** Daily 02:30 → data-storage (local HDD)
- **Dependencies:** Database, storage
- **Purpose:** File sync, calendar, contacts, Obsidian vault sync
- **Storage:** 100GB root disk, data at /var/lib/nextcloud

## 🎮 Gaming Platform

### Pterodactyl Panel
- **Container:** CT 300 (gaming-panel)
- **IP Address:** 192.168.30.210
- **Ports:** 80 (HTTP), 443 (HTTPS)
- **Access Method:** Internal web UI only
- **Credentials:** Admin login in Vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Wings node (CT 302)
- **Purpose:** Game server management panel

### Pterodactyl Wings Node
- **Container:** CT 302 (gaming-wings-1)
- **IP Address:** 192.168.30.212
- **Ports:** 8080 (Wings API), 2022 (SFTP), game server ports
- **Access Method:** API communication with panel
- **Credentials:** Wings token configured automatically
- **Backup Schedule:** Daily 02:30 → data-storage (local HDD)
- **Dependencies:** Pterodactyl Panel, Docker
- **Purpose:** Game server execution environment

### Terraria Server
- **Platform:** Docker container on CT 302
- **Port:** 7777 (UDP)
- **Subdomain:** terraria.najhin-gaming.com
- **Access Method:** Game client connection
- **Status:** ⏸️ Stopped (on-demand)
- **Players:** 0/8 slots
- **Version:** 1.4.4.9
- **Purpose:** Multiplayer Terraria gaming

### Minecraft Server
- **Platform:** Docker container on CT 302
- **Port:** 25565 (TCP)
- **Subdomain:** mc.najhin-gaming.com
- **Access Method:** Game client connection
- **Status:** ⏸️ Stopped (on-demand)
- **Players:** 0/20 slots
- **Version:** Latest stable
- **Purpose:** Multiplayer Minecraft gaming

### Windrose Server
- **Platform:** Docker container on CT 302
- **Port:** 7777 (UDP)
- **Access Method:** Game client (invite code)
- **Status:** ✅ Running
- **Players:** 0/4 slots (dynamic)
- **Settings:** Medium difficulty, Early Access
- **Invite Code:** NAJHINWINDROSE
- **Purpose:** Private co-op survival game

## 🤖 AI & Local LLM Services

### Ollama LLM Service
- **Container:** VM 400 (ollama-gpu)
- **IP Address:** 192.168.30.221
- **Ports:** 11434 (HTTP API)
- **Access Method:** API calls, Open WebUI
- **Credentials:** None (local access)
- **Backup Schedule:** Daily 02:30 → data-storage (local HDD)
- **Dependencies:** ROCm 6.1.3, RX 6700 XT GPU passthrough
- **Purpose:** Local LLM inference with GPU acceleration
- **Models:** qwen2.5:14b (primary), llama3.2:latest (secondary)

### Open WebUI
- **Platform:** Docker container on VM 400
- **Port:** 3000 (HTTP) → 8080 (external)
- **Subdomain:** ollama.najhin-gaming.com
- **Access Method:** Cloudflare Access (Email OTP)
- **Credentials:** Local user accounts
- **Dependencies:** Ollama service
- **Purpose:** Web interface for local LLM interaction, ChatGPT-like UI

## 🌐 External Network Services

### Pi-hole DNS
- **Hardware:** Raspberry Pi 4
- **IP Address:** 192.168.30.10
- **Ports:** 53 (DNS), 80 (Admin)
- **Access Method:** Web admin interface
- **Credentials:** Admin password in Vaultwarden, pihole API token in HashiCorp Vault (kv/pihole)
- **Backup Schedule:** Manual configuration export
- **Dependencies:** Internet connectivity
- **Purpose:** DNS filtering and ad blocking (~489K domains blocked)

### pfSense Firewall
- **Hardware:** AC8F Mini PC
- **IP Address:** 192.168.10.1 (management)
- **Ports:** 443 (HTTPS admin), various firewall rules
- **Access Method:** Web admin interface via Tailscale
- **Credentials:** Admin login in Vaultwarden
- **Backup Schedule:** Manual configuration export
- **Dependencies:** Internet connection
- **Purpose:** Routing, firewall, VLAN management, Tailscale subnet router

### UGREEN NAS (Kinmoon)
- **Hardware:** UGREEN DXP2800
- **IP Address:** 192.168.10.15
- **Ports:** 445 (SMB), 2049 (NFS)
- **Access Method:** SMB/NFS shares
- **Credentials:** Admin credentials in Vaultwarden
- **Purpose:** Backup storage target (kinmoon-nfs in Proxmox)
- **Capacity:** 3.6TB WD Purple drive

## 📱 Notification & Communication

### Homelab Alerts (Telegram)
- **Platform:** Telegram bot via Alertmanager
- **Purpose:** Critical infrastructure alerts
- **Triggers:** Host down, high CPU/memory/disk, backup failures
- **Credentials:** Bot token in HashiCorp Vault (kv/alertmanager)
- **Dependencies:** Alertmanager, Prometheus

### Homelab Alerts (Discord)
- **Platform:** Discord webhook via Alertmanager
- **Purpose:** Warning-level alerts
- **Channel:** #alerts
- **Triggers:** Service warnings, non-critical issues
- **Credentials:** Webhook URL in HashiCorp Vault (kv/alertmanager)
- **Dependencies:** Alertmanager, Prometheus

## 🔄 Backup & Recovery Services

### Proxmox Backup Jobs
- **Small Containers Job:**
  - **Schedule:** Daily 02:00
  - **Targets:** CT 201, 202, 203, 204, 205, 206, 207, 214, 300
  - **Storage:** kinmoon-nfs (NAS)
  - **Retention:** 7 daily, 4 weekly, 2 monthly

- **Large Containers Job:**
  - **Schedule:** Daily 02:30  
  - **Targets:** CT 220, 302, VM 400
  - **Storage:** data-storage (local HDD)
  - **Retention:** 7 daily, 4 weekly, 2 monthly

## 📋 Service Dependencies Matrix

| Service | Critical Dependencies | Optional Dependencies |
|---------|----------------------|----------------------|
| Nginx Proxy Manager | None | Let's Encrypt |
| Prometheus | Node Exporters | All monitored services |
| Grafana | Prometheus | Loki |
| Alertmanager | Prometheus | Telegram, Discord APIs |
| Gilgamesh | n8n, Claude API | Proxmox API, GitHub API |
| Gaming Servers | Wings Node | Pterodactyl Panel |
| Nextcloud | Database | External storage |
| Ollama | ROCm, GPU passthrough | Open WebUI |
| Open WebUI | Ollama | User authentication |

---

## 🔧 Service Management Commands

### Container Management
```bash
# Start/stop containers
pct start <CTID>
pct stop <CTID>
pct reboot <CTID>

# Enter container
pct enter <CTID>

# Resource monitoring
pct status <CTID>
```

### Service Status Checks
```bash
# Docker services (CT 302)
docker ps
docker logs <container>

# Systemd services
systemctl status <service>
journalctl -u <service>
```

### Backup Management
```bash
# Manual backup
vzdump <CTID> --storage kinmoon-nfs
vzdump <CTID> --storage data-storage

# Restore backup
qmrestore <backup-file> <CTID>
```

---

*This service catalog is auto-generated from the master AI context and reflects the current infrastructure state as of April 24, 2026.*