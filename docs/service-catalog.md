# 📋 Service Catalog — Complete Service Directory

> **Last Updated:** April 24, 2026  
> **Total Services:** 25+ active services  
> **External Access:** 8 public endpoints  
> **All credentials stored in Vaultwarden/Vault**

## 🌐 Networking Infrastructure

### Core Network Services

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **pfSense Firewall** | Bare Metal | 192.168.10.1 | 443, 80 | — | Tailscale VPN only | Vaultwarden: pfSense Admin | N/A (config backup) | ISP connection |
| **Pi-hole DNS** | Bare Metal | 192.168.30.10 | 53, 80 | — | Tailscale VPN only | Vaultwarden: Pi-hole Admin | Daily (system) | pfSense |
| **Nginx Proxy Manager** | CT 201 | 192.168.30.201 | 80, 443, 81 | — | Internal admin only | Vaultwarden: NPM Admin | Daily 02:00 | pfSense |
| **Network DDNS** | CT 207 | 192.168.30.207 | — | — | Automated service | Vault: kv/cloudflare | Daily 02:00 | Cloudflare API |

### VPN & Remote Access

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Tailscale VPN** | pfSense | 192.168.10.1 | 41641/UDP | — | Tailscale client apps | Vaultwarden: Tailscale | N/A (cloud managed) | Internet |

---

## 📊 Monitoring & Observability

### Metrics & Dashboards

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Prometheus** | CT 202 | 192.168.30.202 | 9090 | — | Tailscale VPN only | No auth | Daily 02:00 | All monitored targets |
| **Grafana** | CT 203 | 192.168.30.203 | 3000 | grafana.najhin-gaming.com | Cloudflare Access (email OTP) | Vaultwarden: Grafana Admin | Daily 02:00 | Prometheus, Loki |
| **Loki** | CT 204 | 192.168.30.204 | 3100 | — | Internal API only | No auth | Daily 02:00 | Log sources |
| **Alertmanager** | CT 205 | 192.168.30.205 | 9093 | — | Internal API only | Vault: kv/alertmanager | Daily 02:00 | Prometheus |
| **Uptime Kuma** | CT 206 | 192.168.30.206 | 3001 | — | Tailscale VPN only | Vaultwarden: Uptime Kuma | Daily 02:00 | Monitored endpoints |

### Logging & Alerting

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Homelab Alerts (Telegram)** | CT 205 | — | — | — | Automated notifications | Vault: kv/alertmanager | N/A (config only) | Telegram Bot API |
| **Homelab Alerts (Discord)** | CT 205 | — | — | — | Automated notifications | Vault: kv/alertmanager | N/A (config only) | Discord Webhook |

---

## 🤖 Automation & AI

### Workflow Automation

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **n8n Automation** | CT 211 | 192.168.30.211 | 5678 | n8n.najhin-gaming.com | Cloudflare Access (email OTP) | Vaultwarden: n8n Admin | Daily 02:30 | Various APIs |
| **Gilgamesh AI Agent** | CT 211 | — | — | — | Telegram (@JhinGilgamesh_bot) | Vault: kv/gilgamesh | Daily 02:30 | Claude API, n8n |

### n8n Workflows (Active)

| Workflow | Purpose | Trigger | Dependencies |
|----------|---------|---------|-------------|
| **Telegram Agent** | Gilgamesh AI bot main handler | Telegram webhook | Claude API, Proxmox API |
| **Documentation Pipeline - Update** | Session summary to docs | Webhook (doc-update) | Nextcloud, GitHub APIs |
| **Documentation Pipeline - Sync Docs** | Full doc regeneration | Webhook (doc-sync) | Nextcloud, GitHub APIs |
| **Update Nextcloud File** | Legacy (unpublished) | — | — |
| **Push to GitHub** | Legacy (unpublished) | — | — |

---

## 🎮 Gaming Platform

### Game Management

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Pterodactyl Panel** | CT 300 | 192.168.30.210 | 80 | — | Tailscale VPN only | Vaultwarden: Pterodactyl Admin | Daily 02:00 | Wings node |
| **Pterodactyl Wings** | CT 302 | 192.168.30.212 | 8080 | — | Internal API only | Vault: kv/pterodactyl | Daily 02:30 | Docker daemon |

### Game Servers (Docker on CT 302)

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Terraria Server** | Docker | 192.168.30.212 | 7777 | terraria.najhin-gaming.com | Direct game client | Server password | Daily 02:30 | Wings daemon |
| **Minecraft Server** | Docker | 192.168.30.212 | 25565 | mc.najhin-gaming.com | Direct game client | Server password | Daily 02:30 | Wings daemon |
| **Windrose Server** | Docker | 192.168.30.212 | 27015 | — | Direct game client | Invite code: NAJHINWINDROSE | Daily 02:30 | Wings daemon |

---

## 💾 Storage & File Management

### File Services

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Nextcloud Hub** | CT 220 | 192.168.30.220 | 80 | cloud.najhin-gaming.com | Cloudflare Tunnel | Vaultwarden: Nextcloud Admin | Daily 02:30 | PostgreSQL, Redis |
| **NAS Storage (Kinmoon)** | Bare Metal | 192.168.10.15 | 445, 2049 | — | SMB/NFS shares | Vaultwarden: NAS Admin | N/A (backup target) | Network connectivity |

### Backup Infrastructure

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Proxmox Backup Jobs** | Proxmox | 192.168.10.5 | — | — | Proxmox web interface | Vault: kv/proxmox | Self-managed | Storage targets |

**Backup Job Details:**
- **Small Containers (02:00):** CT 201,202,203,204,205,206,207,214,300 → kinmoon-nfs
- **Large Containers (02:30):** CT 220,302 → data-storage

---

## 🔐 Security & Secrets

### Identity & Access Management

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **HashiCorp Vault** | CT 213 | 192.168.30.213 | 8200 | vault.najhin-gaming.com | Cloudflare Access (email OTP) | Vault root token | Daily 02:00 | Consul (built-in) |
| **Vaultwarden** | CT 214 | 192.168.30.214 | 80 | passwords.najhin-gaming.com | Cloudflare Access (email OTP) | Master password | Daily 02:00 | PostgreSQL |

### Vault Secret Paths (8 total)

| Path | Purpose | Used By |
|------|---------|---------|
| `kv/gilgamesh` | Telegram bot token, Claude API key | n8n Gilgamesh workflows |
| `kv/cloudflare` | DNS API token, tunnel credentials | DDNS, Cloudflare services |
| `kv/proxmox` | API token for infrastructure queries | Gilgamesh, monitoring |
| `kv/alertmanager` | Telegram/Discord webhook URLs | Alert routing |
| `kv/github` | API token for documentation pipeline | n8n doc workflows |
| `kv/nextcloud` | App password for file operations | Documentation pipeline |
| `kv/n8n` | Database credentials, encryption key | n8n application |
| `kv/pihole` | Admin password, API token | DNS management |

### Access Control (Cloudflare)

| Application | Users | Session Duration | MFA |
|-------------|-------|------------------|-----|
| Grafana | Personal only | 30 days | Email OTP |
| n8n | Personal only | 30 days | Email OTP |
| Vault | Personal only | 30 days | Email OTP |
| Vaultwarden | Personal only | 30 days | Email OTP |

---

## 🏠 Management & Dashboards

### Administrative Interfaces

| Service | Container | IP | Port(s) | Subdomain | Access Method | Credentials | Backup Schedule | Dependencies |
|---------|-----------|----|---------|-----------|--------------|---------|---------|----|
| **Homepage Dashboard** | CT 208 | 192.168.30.208 | 3000 | home.najhin-gaming.com | Public (no auth) | None | Daily 02:00 | Service APIs |
| **Proxmox Web** | Bare Metal | 192.168.10.5 | 8006 | — | Tailscale VPN only | Vault: kv/proxmox | N/A (config backup) | Proxmox cluster |

---

## 📡 External Integrations

### Third-Party Services

| Service | Provider | Purpose | Credentials | Rate Limits |
|---------|----------|---------|-------------|-------------|
| **Claude API** | Anthropic | Gilgamesh AI responses | Vault: kv/gilgamesh | $10/month limit |
| **Telegram Bot API** | Telegram | Gilgamesh chat interface | Vault: kv/gilgamesh | 30 msg/sec |
| **Cloudflare API** | Cloudflare | DNS updates, tunnel management | Vault: kv/cloudflare | 1200 req/5min |
| **GitHub API** | GitHub | Documentation pipeline | Vault: kv/github | 5000 req/hour |
| **Discord Webhooks** | Discord | Alert notifications | Vault: kv/alertmanager | 30 msg/min |

---

## 🔧 Service Dependencies Map

### Critical Path Services
```
Internet → pfSense → Switch → Proxmox → Containers
                 ↓
              Pi-hole DNS ← All containers
                 ↓
           Service resolution
```

### Monitoring Stack Dependencies
```
Prometheus ← All targets (20+)
     ↓
  Grafana ← Loki (logs)
     ↓
Alertmanager → Telegram/Discord
```

### AI Agent Dependencies
```
Telegram → n8n → Claude API
              ↓
         Proxmox API (status)
              ↓
       GitHub/Nextcloud (docs)
```

---

## 🚨 Service Health Monitoring

### Critical Services (Must be 99.9%+ uptime)
- pfSense Firewall
- Pi-hole DNS  
- Proxmox Hypervisor
- NPM Reverse Proxy
- Prometheus Monitoring

### Important Services (99%+ uptime target)
- Grafana Dashboards
- n8n Automation
- Gilgamesh AI Agent
- Gaming Platform

### Nice-to-Have Services (95%+ uptime target)
- Homepage Dashboard
- Uptime Kuma
- Documentation Pipeline

---

## 📞 Emergency Procedures

### Service Recovery Priority

1. **Network (pfSense)** — Restore internet connectivity
2. **DNS (Pi-hole)** — Restore name resolution  
3. **Monitoring (Prometheus)** — Restore visibility
4. **Automation (n8n)** — Restore Gilgamesh capabilities
5. **Gaming** — Restore community services

### Backup Recovery Locations

| Service Type | Primary Backup | Secondary Backup |
|--------------|----------------|------------------|
| Small containers | kinmoon-nfs (NAS) | Local snapshots |
| Large containers | data-storage (Local) | Planned: Backblaze B2 |
| Configuration | Proxmox backups | Git repositories |
| Secrets | Vault snapshots | Encrypted exports |

---

## 📊 Service Utilization Stats

### Daily Usage (Average)
- **Gilgamesh Interactions:** 50+ messages/day
- **n8n Workflow Executions:** 500+ executions/day
- **Grafana Queries:** 500+ queries/day
- **Gaming Server Connections:** 5-10 sessions/day
- **Nextcloud File Operations:** 100+ operations/day

### Monthly Resource Costs
- **Claude API:** $15/month (Gilgamesh usage)
- **Cloudflare:** $0/month (Free plan)
- **Domain (najhin-gaming.com):** $10/year
- **Electricity:** ~$25/month (estimated)

---

*This service catalog is maintained through the Gilgamesh documentation pipeline and updated with each deployment session.*