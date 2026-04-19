# 📋 Service Catalog

> **Last Updated:** December 19, 2024
> **Total Services:** 14 containers + 8 external integrations
> **Network:** All services on VLAN 30 (192.168.30.0/24)

## 🔗 Quick Access Links

### Public Services (External Access)
- **Grafana:** [grafana.najhin-gaming.com](https://grafana.najhin-gaming.com) 🔒 Cloudflare Access
- **Homepage:** [home.najhin-gaming.com](https://home.najhin-gaming.com) 🌐 Public
- **n8n:** [n8n.najhin-gaming.com](https://n8n.najhin-gaming.com) 🔒 Cloudflare Access
- **Vault:** [vault.najhin-gaming.com](https://vault.najhin-gaming.com) 🔒 Cloudflare Access
- **Vaultwarden:** [passwords.najhin-gaming.com](https://passwords.najhin-gaming.com) 🔒 Cloudflare Access
- **Nextcloud:** [cloud.najhin-gaming.com](https://cloud.najhin-gaming.com) 🚇 Cloudflare Tunnel
- **Terraria:** terraria.najhin-gaming.com:7777 🎮 Game Server
- **Minecraft:** mc.najhin-gaming.com:25565 🎮 Game Server

---

## 🌐 Networking Services

### Nginx Proxy Manager
- **Container:** CT 201 (nginx-proxy-manager)
- **IP Address:** 192.168.30.201
- **Ports:** 80 (HTTP), 443 (HTTPS), 81 (Admin)
- **Access Method:** Internal admin panel at 192.168.30.201:81
- **Credentials:** Stored in Vaultwarden (NPM Admin)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** None (core infrastructure)
- **Purpose:** Reverse proxy, SSL certificate management

### Dynamic DNS
- **Container:** CT 207 (network-ddns)
- **IP Address:** 192.168.30.207
- **Ports:** None (service daemon)
- **Access Method:** Cloudflare API integration
- **Credentials:** kv/cloudflare in Vault, API token in Vaultwarden
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Cloudflare account
- **Purpose:** Automatic DNS record updates for najhin-gaming.com

---

## 📊 Monitoring Services

### Prometheus
- **Container:** CT 202 (monitoring-prometheus)
- **IP Address:** 192.168.30.202
- **Ports:** 9090 (Web UI)
- **Access Method:** Internal at http://192.168.30.202:9090
- **Credentials:** No authentication (internal only)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** node_exporter on all targets
- **Purpose:** Metrics collection and time-series database

### Grafana
- **Container:** CT 203 (monitoring-grafana)
- **IP Address:** 192.168.30.203
- **Ports:** 3000 (Web UI)
- **Access Method:** External at grafana.najhin-gaming.com
- **Credentials:** Admin account in Vaultwarden (Grafana Admin)
- **External Auth:** Cloudflare Access (email OTP)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Prometheus (data source)
- **Purpose:** Metrics visualization and dashboards

### Loki
- **Container:** CT 204 (monitoring-loki)
- **IP Address:** 192.168.30.204
- **Ports:** 3100 (API)
- **Access Method:** Internal API only
- **Credentials:** No authentication (internal only)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Promtail agents (future)
- **Purpose:** Log aggregation and storage

### AlertManager
- **Container:** CT 205 (monitoring-alertmanager)
- **IP Address:** 192.168.30.205
- **Ports:** 9093 (Web UI)
- **Access Method:** Internal at http://192.168.30.205:9093
- **Credentials:** Webhook tokens in kv/alertmanager (Vault)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Prometheus (alert rules)
- **Purpose:** Alert routing to Telegram and Discord

### Uptime Kuma
- **Container:** CT 206 (monitoring-uptime)
- **IP Address:** 192.168.30.206
- **Ports:** 3001 (Web UI)
- **Access Method:** Internal at http://192.168.30.206:3001
- **Credentials:** Admin account in Vaultwarden (Uptime Kuma)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Services being monitored
- **Purpose:** Service availability monitoring

---

## 🤖 Automation Services

### n8n Workflow Automation
- **Container:** CT 211 (automation-n8n)
- **IP Address:** 192.168.30.211
- **Ports:** 5678 (Web UI)
- **Access Method:** External at n8n.najhin-gaming.com
- **Credentials:** Admin account in Vaultwarden (n8n Admin)
- **External Auth:** Cloudflare Access (email OTP)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** Telegram, Claude API, GitHub API
- **Purpose:** Workflow automation, Gilgamesh AI agent hosting

---

## 🔒 Security Services

### HashiCorp Vault
- **Container:** CT 213 (vault)
- **IP Address:** 192.168.30.213
- **Ports:** 8200 (Web UI/API)
- **Access Method:** External at vault.najhin-gaming.com
- **Credentials:** Root token in Vaultwarden (Vault Root Token)
- **External Auth:** Cloudflare Access (email OTP)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** None (core security infrastructure)
- **Purpose:** Secrets management for API keys and tokens

### Vaultwarden
- **Container:** CT 214 (password-vaultwarden)
- **IP Address:** 192.168.30.214
- **Ports:** 80 (Web UI), 3012 (WebSocket)
- **Access Method:** External at passwords.najhin-gaming.com
- **Credentials:** Master password (personal)
- **External Auth:** Cloudflare Access (email OTP)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** None
- **Purpose:** Password manager for service credentials

---

## ☁️ Cloud Storage

### Nextcloud Hub
- **Container:** CT 220 (nextcloud-hub)
- **IP Address:** 192.168.30.220
- **Ports:** 80 (HTTP), 443 (HTTPS)
- **Access Method:** External at cloud.najhin-gaming.com (Cloudflare Tunnel)
- **Credentials:** Admin account in Vaultwarden (Nextcloud Admin)
- **External Auth:** Cloudflare Tunnel (no additional auth layer)
- **Backup Schedule:** Daily 02:30 → Local HDD (large container)
- **Dependencies:** None
- **Purpose:** Private cloud storage, file synchronization

---

## 🎮 Gaming Services

### Pterodactyl Panel
- **Container:** CT 300 (gaming-panel)
- **IP Address:** 192.168.30.210
- **Ports:** 80 (HTTP), 443 (HTTPS)
- **Access Method:** Internal at http://192.168.30.210
- **Credentials:** Admin account in Vaultwarden (Pterodactyl Admin)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** CT 302 (Wings node)
- **Purpose:** Game server management panel

### Pterodactyl Wings Node
- **Container:** CT 302 (gaming-wings-1)
- **IP Address:** 192.168.30.212
- **Ports:** 8080 (Wings API), 2022 (SFTP)
- **Game Ports:** 7777 (Terraria), 25565 (Minecraft), Internal (Windrose)
- **Access Method:** Managed via Pterodactyl Panel
- **Credentials:** Wings token in Vaultwarden (Wings Node Token)
- **Backup Schedule:** Daily 02:30 → Local HDD (large container)
- **Dependencies:** CT 300 (Panel), Docker runtime
- **Purpose:** Game server hosting (Terraria, Minecraft, Windrose)

#### Game Servers on CT 302
| Game Server | Port | External Access | Status | Players |
|-------------|------|-----------------|--------|---------|
| **Terraria** | 7777 | terraria.najhin-gaming.com:7777 | ✅ Running | 0/8 |
| **Minecraft** | 25565 | mc.najhin-gaming.com:25565 | ✅ Running | 0/20 |
| **Windrose** | Internal | Invite code required | ✅ Running | 0/4 |

---

## 🏠 Dashboard Services

### Homepage Dashboard
- **Container:** CT 208 (dashboard-homepage)
- **IP Address:** 192.168.30.208
- **Ports:** 3000 (Web UI)
- **Access Method:** External at home.najhin-gaming.com
- **Credentials:** No authentication (public dashboard)
- **External Auth:** None (pending Cloudflare Access)
- **Backup Schedule:** Daily 02:00 → NAS
- **Dependencies:** All services (status checking)
- **Purpose:** Unified service dashboard and homepage

---

## 🔗 External Integrations

### DNS Resolution (Pi-hole)
- **Hardware:** Raspberry Pi 4
- **IP Address:** 192.168.30.10
- **Ports:** 53 (DNS), 80 (Admin)
- **Access Method:** Internal at http://192.168.30.10/admin
- **Credentials:** Admin password in kv/pihole (Vault)
- **Dependencies:** None (critical infrastructure)
- **Purpose:** Ad/tracker blocking (~489K domains)

### API Integrations
| Service | Purpose | Credential Storage | Container Using |
|---------|---------|-------------------|-----------------|
| **Claude API** | AI agent responses | kv/gilgamesh (Vault) | CT 211 |
| **Telegram Bot API** | Gilgamesh bot | kv/gilgamesh (Vault) | CT 211 |
| **GitHub API** | Documentation sync | kv/github (Vault) | CT 211 |
| **Cloudflare API** | DNS updates | kv/cloudflare (Vault) | CT 207 |
| **Proxmox API** | Infrastructure queries | kv/proxmox (Vault) | CT 211 |

---

## 🔄 Backup Configuration

### Backup Jobs
| Job Name | Schedule | Source Containers | Target Storage | Retention |
|----------|----------|-------------------|----------------|-----------|
| **Small Containers** | Daily 02:00 | 201, 202, 203, 204, 205, 206, 207, 214, 300 | kinmoon-smb (NAS) | 7/4/2 |
| **Large Containers** | Daily 02:30 | 220, 302 | data-storage (Local HDD) | 7/4/2 |

### Storage Targets
- **kinmoon-smb:** UGREEN NAS, 3.6TB via SMB/CIFS
- **data-storage:** Local HDD, 7.2TB direct attachment

---

## 🌍 External Access Summary

### Authentication Methods
- 🔒 **Cloudflare Access:** Email OTP (7 services)
- 🚇 **Cloudflare Tunnel:** Direct access (1 service)
- 🌐 **Public:** No authentication (3 services)
- 🏠 **Internal Only:** Local network access (4 services)

### SSL Certificate Management
- **Provider:** Let's Encrypt via Nginx Proxy Manager
- **Renewal:** Automatic via Cloudflare DNS API
- **Wildcard:** *.najhin-gaming.com

### Port Forwarding (pfSense)
| External Port | Internal Target | Service |
|---------------|-----------------|---------|
| 80 | 192.168.30.201:80 | HTTP → NPM |
| 443 | 192.168.30.201:443 | HTTPS → NPM |
| 7777 | 192.168.30.212:7777 | Terraria |
| 25565 | 192.168.30.212:25565 | Minecraft |

---

## 🚨 Critical Dependencies

### Service Dependency Map
```
Internet Access
├── Cloudflare (DNS/CDN)
├── pfSense (Firewall) → All external access
└── NPM (CT 201) → Web services

Core Infrastructure
├── Vault (CT 213) → API secrets
├── Vaultwarden (CT 214) → Service credentials  
└── Prometheus (CT 202) → All monitoring

AI Agent (Gilgamesh)
├── n8n (CT 211) → Workflow engine
├── Claude API → AI responses
├── Telegram API → Bot interface
└── GitHub API → Documentation sync
```

### Single Points of Failure
- **Internet:** ISP connection
- **Power:** UPS recommended for critical services
- **Storage:** Local storage failure affects backups
- **pfSense:** Network isolation on failure

---

This service catalog is actively maintained and reflects the current production environment. All credentials are stored securely in either HashiCorp Vault (API keys/tokens) or Vaultwarden (service logins).