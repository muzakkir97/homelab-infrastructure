# 📋 Service Catalog — Complete Infrastructure Directory

> **Purpose:** Comprehensive directory of all services with access details and credentials
> **Last Updated:** April 24, 2026
> **Total Services:** 23 services across 6 categories

---

## 📊 Service Overview

| Category | Service Count | External Access | Internal Only |
|----------|---------------|-----------------|---------------|
| **Networking** | 4 | 0 | 4 |
| **Monitoring & Observability** | 5 | 1 | 4 |
| **Automation & AI** | 3 | 2 | 1 |
| **Gaming & Entertainment** | 4 | 3 | 1 |
| **Storage & Collaboration** | 1 | 1 | 0 |
| **Security & Secrets** | 6 | 2 | 4 |
| **Total** | **23** | **9** | **14** |

---

## 🌐 Networking Services

### 1. Nginx Proxy Manager
- **Container:** CT 201 (nginx-proxy-manager)
- **IP Address:** 192.168.30.201
- **Ports:** 80, 81, 443
- **Access Method:** Internal web interface (http://192.168.30.201:81)
- **Credentials:** Vaultwarden → "NPM Admin"
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** None (core infrastructure)

### 2. pfSense Firewall
- **Host:** AC8F Mini PC
- **Management IP:** 192.168.10.1
- **Ports:** 80 (HTTP), 443 (HTTPS), SSH (disabled)
- **Access Method:** Internal web interface (https://192.168.10.1)
- **Credentials:** Vaultwarden → "pfSense WebGUI"
- **Backup Schedule:** Weekly configuration backup
- **Dependencies:** None (core infrastructure)

### 3. Pi-hole DNS
- **Host:** Raspberry Pi 4
- **IP Address:** 192.168.30.10
- **Ports:** 53 (DNS), 80 (Web)
- **Access Method:** Internal web interface (http://192.168.30.10/admin)
- **Credentials:** Vault → kv/pihole
- **Backup Schedule:** Configuration backup via n8n
- **Dependencies:** None (critical service)

### 4. Dynamic DNS (DDNS)
- **Container:** CT 207 (network-ddns)
- **IP Address:** 192.168.30.207
- **Ports:** None (outbound only)
- **Access Method:** SSH to container for logs
- **Credentials:** Vault → kv/cloudflare
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Cloudflare API, Internet connectivity

---

## 📊 Monitoring & Observability

### 5. Prometheus
- **Container:** CT 202 (monitoring-prometheus)
- **IP Address:** 192.168.30.202
- **Ports:** 9090
- **Access Method:** Internal web interface (http://192.168.30.202:9090)
- **Credentials:** No authentication (internal only)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Target services for scraping

### 6. Grafana
- **Container:** CT 203 (monitoring-grafana)
- **IP Address:** 192.168.30.203
- **Ports:** 3000
- **Subdomain:** grafana.najhin-gaming.com
- **Access Method:** Cloudflare Access + Email OTP
- **Credentials:** Vaultwarden → "Grafana Admin"
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Prometheus, Loki

### 7. Loki
- **Container:** CT 204 (monitoring-loki)
- **IP Address:** 192.168.30.204
- **Ports:** 3100
- **Access Method:** Internal API (via Grafana)
- **Credentials:** No authentication
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Log shipping agents

### 8. Alertmanager
- **Container:** CT 205 (monitoring-alertmanager)
- **IP Address:** 192.168.30.205
- **Ports:** 9093
- **Access Method:** Internal web interface
- **Credentials:** Vault → kv/alertmanager
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Prometheus, Telegram/Discord webhooks

### 9. Uptime Kuma
- **Container:** CT 206 (monitoring-uptime)
- **IP Address:** 192.168.30.206
- **Ports:** 3001
- **Access Method:** Internal web interface (http://192.168.30.206:3001)
- **Credentials:** Vaultwarden → "Uptime Kuma"
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Target services for monitoring

---

## 🤖 Automation & AI

### 10. n8n Workflow Automation
- **Container:** CT 211 (automation-n8n)
- **IP Address:** 192.168.30.211
- **Ports:** 5678
- **Subdomain:** n8n.najhin-gaming.com
- **Access Method:** Cloudflare Access + Email OTP
- **Credentials:** Vault → kv/n8n
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Various APIs (Claude, GitHub, Telegram, Proxmox)

### 11. Gilgamesh Telegram Bot
- **Platform:** Telegram (@JhinGilgamesh_bot)
- **Backend:** n8n workflow in CT 211
- **Chat ID:** 518832696
- **Access Method:** Telegram app with bot token
- **Credentials:** Vault → kv/gilgamesh
- **Data Storage:** n8n Data Tables (gilgamesh_memory, gilgamesh_costs)
- **Dependencies:** Claude API, n8n, various infrastructure APIs

### 12. Homepage Dashboard
- **Container:** CT 208 (dashboard-homepage)
- **IP Address:** 192.168.30.208
- **Ports:** 3000
- **Subdomain:** home.najhin-gaming.com
- **Access Method:** Public (no authentication)
- **Credentials:** None (read-only dashboard)
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Service APIs for status widgets

---

## 🎮 Gaming & Entertainment

### 13. Pterodactyl Panel
- **Container:** CT 300 (gaming-panel)
- **IP Address:** 192.168.30.210
- **Ports:** 80, 443
- **Access Method:** Internal web interface (http://192.168.30.210)
- **Credentials:** Vaultwarden → "Pterodactyl Admin"
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Wings node (CT 302)

### 14. Terraria Server
- **Container:** CT 302 (gaming-wings-1) - Docker container
- **Host IP:** 192.168.30.212
- **Ports:** 7777
- **Subdomain:** terraria.najhin-gaming.com
- **Access Method:** Terraria game client
- **Server Password:** Stored in Vaultwarden → "Game Servers"
- **Backup Schedule:** Daily 02:30 → data-storage
- **Dependencies:** Pterodactyl Wings, Docker

### 15. Minecraft Server
- **Container:** CT 302 (gaming-wings-1) - Docker container
- **Host IP:** 192.168.30.212
- **Ports:** 25565
- **Subdomain:** mc.najhin-gaming.com
- **Access Method:** Minecraft Java client
- **Server Settings:** Stored in Vaultwarden → "Game Servers"
- **Backup Schedule:** Daily 02:30 → data-storage
- **Dependencies:** Pterodactyl Wings, Docker

### 16. Windrose Server
- **Container:** CT 302 (gaming-wings-1) - Direct deployment
- **Host IP:** 192.168.30.212
- **Ports:** 27015
- **Deploy Path:** /opt/windrose
- **Access Method:** Steam client, invite code NAJHINWINDROSE
- **Game Settings:** 4 max players, Medium difficulty
- **Backup Schedule:** Daily 02:30 → data-storage
- **Dependencies:** Steam client libraries

---

## 💾 Storage & Collaboration

### 17. Nextcloud Hub
- **Container:** CT 220 (nextcloud-hub)
- **IP Address:** 192.168.30.220
- **Ports:** 80, 443
- **Subdomain:** cloud.najhin-gaming.com
- **Access Method:** Cloudflare Tunnel (no Access protection)
- **Credentials:** Vaultwarden → "Nextcloud Admin"
- **Storage:** 100GB root disk (data at /var/lib/nextcloud)
- **Backup Schedule:** Daily 02:30 → data-storage (large)
- **Dependencies:** Database (MariaDB), PHP, web server

---

## 🔒 Security & Secrets

### 18. HashiCorp Vault
- **Container:** CT 213 (vault)
- **IP Address:** 192.168.30.213
- **Ports:** 8200
- **Subdomain:** vault.najhin-gaming.com
- **Access Method:** Cloudflare Access + Email OTP
- **Root Token:** Stored in Vaultwarden → "Vault Root Token"
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Secret Paths:** 8 paths (kv/gilgamesh, kv/cloudflare, kv/proxmox, etc.)

### 19. Vaultwarden (Bitwarden)
- **Container:** CT 214 (password-vaultwarden)
- **IP Address:** 192.168.30.214
- **Ports:** 80, 3012 (WebSocket)
- **Subdomain:** passwords.najhin-gaming.com
- **Access Method:** Cloudflare Access + Email OTP
- **Admin Token:** Stored in Vault → kv/vaultwarden
- **Backup Schedule:** Daily 02:00 → kinmoon-nfs
- **Dependencies:** Database (SQLite), SMTP (optional)

### 20. Tailscale VPN
- **Host:** pfSense firewall
- **Subnet Router:** 192.168.10.1
- **Access Networks:** All VLANs (10, 20, 30, 40, 50)
- **Access Method:** Tailscale clients on devices
- **Admin Console:** https://login.tailscale.com
- **Credentials:** Vaultwarden → "Tailscale Admin"
- **Dependencies:** Internet connectivity, Tailscale control plane

### 21. Cloudflare Access
- **Platform:** Cloudflare for Teams
- **Protected Apps:** 7 applications
- **Authentication:** Email OTP to muzakkir's email
- **Admin Console:** https://one.dash.cloudflare.com
- **Credentials:** Vaultwarden → "Cloudflare Account"
- **Dependencies:** Cloudflare DNS, email delivery

### 22. Fail2ban
- **Installation:** Multiple containers (where applicable)
- **Protection:** SSH, HTTP, application-specific
- **Configuration:** Standard rules + custom
- **Log Location:** /var/log/fail2ban.log
- **Admin Method:** SSH to containers, fail2ban-client
- **Backup:** Configuration included in container backups

### 23. Firewall Rules (pfSense)
- **Platform:** pfSense on AC8F
- **Rule Count:** 47 custom rules
- **Default Policy:** Deny all, explicit allow
- **Management:** pfSense web interface
- **Backup:** Configuration backup weekly
- **Critical Rules:** Inter-VLAN blocking, Tailscale access

---

## 🔗 Service Dependencies Map

### Critical Path Services (Tier 1)
1. **pfSense Firewall** — No dependencies
2. **Pi-hole DNS** — No dependencies  
3. **Proxmox Host** — No dependencies

### Core Services (Tier 2)
4. **Nginx Proxy Manager** → pfSense, DNS
5. **Tailscale VPN** → pfSense, Internet
6. **Prometheus** → Network, target services

### Application Services (Tier 3)
7. **Grafana** → Prometheus, Loki, NPM
8. **n8n** → Vault, external APIs, NPM
9. **Nextcloud** → Database, storage, NPM
10. **Vault** → Network, NPM

### Automation Services (Tier 4)
11. **Gilgamesh Bot** → n8n, Vault, Claude API
12. **Alertmanager** → Prometheus, notification channels
13. **Gaming Services** → Pterodactyl, Docker runtime

---

## 🎯 Access Patterns

### Internal-Only Services (14 services)
Access via Tailscale VPN or internal network:
- All monitoring infrastructure (Prometheus, Loki, etc.)
- Network management (pfSense, Pi-hole)
- Gaming panel and infrastructure

### External Services with Authentication (7 services)
Protected by Cloudflare Access:
- Grafana, n8n, Vault, Vaultwarden (Access + Email OTP)
- Nextcloud (Cloudflare Tunnel, internal auth)
- Game servers (game client authentication)

### Public Services (2 services)
No authentication required:
- Homepage Dashboard (read-only information)
- Windrose server (invite code: NAJHINWINDROSE)

---

## 🔐 Credential Storage Strategy

### Vaultwarden (Personal Passwords)
**Location:** passwords.najhin-gaming.com
**Contents:**
- Application login credentials (Grafana, NPM, Pterodactyl, etc.)
- External service accounts (Cloudflare, GitHub)
- Hardware/system passwords (Proxmox, CT roots)

### HashiCorp Vault (API Keys & Secrets)
**Location:** vault.najhin-gaming.com
**Structure:**
```
kv/
├── gilgamesh/          # Bot tokens, Claude API key
├── cloudflare/         # API tokens, zone IDs
├── proxmox/            # API tokens, node details
├── alertmanager/       # Webhook URLs, tokens
├── github/             # API tokens, repo details
├── nextcloud/          # App passwords, admin tokens
├── n8n/                # Webhook secrets, credentials
└── pihole/             # Admin passwords, API keys
```

---

## 📋 Maintenance Schedule

### Daily Automated Tasks
- **02:00:** Small container backups (9 containers)
- **02:30:** Large container backups (2 containers)
- **03:00:** Log rotation and cleanup
- **04:00:** Certificate renewal checks

### Weekly Automated Tasks
- **Sunday 01:00:** pfSense configuration backup
- **Sunday 02:00:** Security updates check
- **Sunday 03:00:** Disk space cleanup
- **Sunday 04:00:** Performance metrics aggregation

### Monthly Manual Tasks
- **1st:** Infrastructure audit and documentation update
- **15th:** Security review and credential rotation
- **Last day:** Backup restore testing

---

## 🚨 Emergency Access Procedures

### Service Recovery Priority Order
1. **Tier 1:** pfSense, Pi-hole, Proxmox host
2. **Tier 2:** NPM, Tailscale, critical monitoring
3. **Tier 3:** Applications (Nextcloud, n8n, Vault)
4. **Tier 4:** Non-critical services (gaming, dashboards)

### Disaster Recovery Contacts
- **Primary:** Gilgamesh Telegram bot (@JhinGilgamesh_bot)
- **Secondary:** Direct container SSH access
- **Tertiary:** Proxmox host console access

### Critical Service Restore Time Targets
- **Network Services:** 15 minutes
- **Monitoring Stack:** 30 minutes
- **Applications:** 1 hour
- **Gaming Services:** 2 hours

---

*Service catalog auto-generated: April 24, 2026 via Documentation Pipeline*
*Next update: April 25, 2026 (daily refresh)*