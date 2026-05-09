# 🗂️ Service Catalog

> **Last Updated:** May 9, 2026  
> **Total Services:** 15 containers + 1 VM  
> **External Access:** 7 services via Cloudflare Access  
> **Internal Services:** 9 services (LAN only)

---

## 🌐 External Access Services

### Core Infrastructure Access

| Service | Container | IP | External URL | Port | Credentials | Backup | Dependencies |
|---------|-----------|----|--------------|----- |-------------|--------|--------------|
| **Grafana** | CT 203 | 192.168.30.203 | grafana.najhin-gaming.com | 3000 | Vaultwarden | Daily 02:00 | Prometheus, Loki |
| **n8n Automation** | CT 211 | 192.168.30.211 | n8n.najhin-gaming.com | 5678 | Vaultwarden | Daily 02:00 | Vault, Nextcloud |
| **HashiCorp Vault** | CT 213 | 192.168.30.213 | vault.najhin-gaming.com | 8200 | Manual unseal | Daily 02:00 | None |
| **Vaultwarden** | CT 214 | 192.168.30.214 | passwords.najhin-gaming.com | 80 | Master password | Daily 02:00 | None |
| **Homepage Dashboard** | CT 208 | 192.168.30.208 | home.najhin-gaming.com | 3000 | None (view-only) | Daily 02:00 | All services |
| **Nextcloud Hub** | CT 220 | 192.168.30.220 | cloud.najhin-gaming.com | 80 | Vaultwarden | Daily 02:30 | PostgreSQL |
| **Ollama AI** | VM 400 | 192.168.30.221 | ollama.najhin-gaming.com | 11434 | None (read-only) | Not backed up | GPU passthrough |

**Access Method:** All services protected by Cloudflare Access (Email OTP to muzakkir.kholil06@gmail.com only)

---

## 🏠 Internal Services (LAN Only)

### Networking & DNS

| Service | Container | IP | Internal Port | Purpose | Credentials | Backup | Dependencies |
|---------|-----------|----|--------------|---------|-----------|---------| -------------|
| **Nginx Proxy Manager** | CT 201 | 192.168.30.201 | 81 | Reverse proxy, SSL termination | Vaultwarden | Daily 02:00 | None |
| **Pi-hole DNS** | Physical | 192.168.30.10 | 80, 53 | DNS filtering (~489K blocked) | Vault kv/pihole | External device | None |
| **Dynamic DNS** | CT 207 | 192.168.30.207 | N/A | Cloudflare DNS updates | Vault kv/cloudflare | Daily 02:00 | None |

### Monitoring & Observability

| Service | Container | IP | Internal Port | Purpose | Credentials | Backup | Dependencies |
|---------|-----------|----|--------------|---------|-----------|---------| -------------|
| **Prometheus** | CT 202 | 192.168.30.202 | 9090 | Metrics collection & storage | None | Daily 02:00 | Node exporters |
| **Loki** | CT 204 | 192.168.30.204 | 3100 | Centralized logging | None | Daily 02:00 | Promtail |
| **Alertmanager** | CT 205 | 192.168.30.205 | 9093 | Alert routing & notifications | Vault kv/alertmanager | Daily 02:00 | Prometheus |
| **Uptime Kuma** | CT 206 | 192.168.30.206 | 3001 | Service availability monitoring | Local admin | Daily 02:00 | None |

### Gaming Platform

| Service | Container | IP | Internal Port | External URL | Credentials | Backup | Dependencies |
|---------|-----------|----|--------------|--------------|-----------|---------| -------------|
| **Pterodactyl Panel** | CT 300 | 192.168.30.210 | 80 | N/A (internal) | Vaultwarden | Daily 02:00 | MySQL |
| **Game Servers** | CT 302 | 192.168.30.212 | Various | terraria/mc.najhin-gaming.com | Pterodactyl panel | Daily 02:30 | Wings daemon |

**Active Game Servers:**
- **Terraria:** Port 7777, terraria.najhin-gaming.com
- **Minecraft:** Port 25565, mc.najhin-gaming.com  
- **Windrose:** Port 7778, invite code NAJHINWINDROSE, ~9GB RAM usage

---

## 🔐 Credential Storage Matrix

### Vaultwarden (Personal Passwords)
```
Service Accounts Stored:
├── Grafana admin user
├── n8n workflow user  
├── Vaultwarden master password
├── Nextcloud admin user
├── Pterodactyl admin user
├── Proxmox root user
├── Homepage service keys
└── Various API tokens
```

### HashiCorp Vault (Automation Secrets)
```
KV Paths (8 configured):
├── kv/gilgamesh → Claude API keys, Telegram bot token
├── kv/cloudflare → Tunnel token (⚠️ truncated, needs fix)
├── kv/proxmox → API token for automation
├── kv/alertmanager → Telegram/Discord webhook URLs
├── kv/github → Repository access tokens
├── kv/nextcloud → WebDAV credentials for automation
├── kv/n8n → Service integration passwords
└── kv/pihole → Admin password for API calls
```

**Vault Status:** 🔒 SEALED (manual unseal required after reboot)

---

## 🔄 Backup Schedule & Strategy

### Daily Backups (02:00 GMT+8)
**Small Containers** → NAS RAID 1 (kinmoon-nfs)
```
Containers: 201, 202, 203, 204, 205, 206, 207, 214, 300
Total Size: ~1.2GB
Retention: 7 daily, 4 weekly, 2 monthly
Status: ✅ Automated via Proxmox
```

### Daily Backups (02:30 GMT+8)  
**Large Containers** → Local Storage (data-storage)
```
Containers: 220 (Nextcloud), 302 (Gaming)
Total Size: ~5.7GB
Retention: 7 daily, 4 weekly, 2 monthly
Status: ✅ Automated via Proxmox
```

### Not Backed Up
```
VM 400 (ollama-gpu): Excluded due to size and recreatable nature
Models can be re-downloaded via ollama pull
Configuration stored in infrastructure docs
```

---

## 📊 Service Dependencies

### Critical Path Analysis
```
Core Infrastructure:
Proxmox → pfSense → Pi-hole → NPM → All Services

External Access:
Cloudflare DNS → Tunnel → Access → NPM → Services

Monitoring Stack:
Prometheus ← Node Exporters
Grafana ← Prometheus + Loki  
Alertmanager ← Prometheus
```

### Inter-Service Communication
```
n8n Integrations:
├── Vault (secret retrieval)
├── Nextcloud (file operations via WebDAV)
├── GitHub (documentation sync)
├── Telegram (Gilgamesh bot)
├── Proxmox (container management)
├── Prometheus (metrics queries)
├── Alertmanager (alert status)
└── Ollama (AI inference)

Grafana Data Sources:
├── Prometheus (metrics)
├── Loki (logs)
└── Alertmanager (alerts)
```

---

## 🚀 Resource Allocation

### High-Resource Services
```
gaming-wings-1 (CT 302): 9.2GB RAM, 25% CPU
├── Windrose: ~8.9GB when active
├── Terraria: ~200MB
└── Minecraft: ~300MB

ollama-gpu (VM 400): 7.9GB RAM, 20% CPU  
├── Ollama models: qwen3:14b (9.3GB), llama3.2 (3B)
├── Open WebUI: Docker container
└── GPU: RX 6700 XT 12GB (full passthrough)

monitoring-prometheus (CT 202): 2.1GB RAM, 12% CPU
├── Metrics storage: 12.5GB
├── Retention: 15 days
└── Scrape interval: 15s
```

### Resource Optimization Notes
```
✅ Gaming servers auto-stop when idle (planned in Mash agent)
✅ Monitoring retention tuned for homelab scale
⚠️ Total RAM usage: 78% (128GB upgrade ordered)
✅ ZFS pools have ample free space
```

---

## 🔧 Maintenance Windows

### Scheduled Maintenance
```
Daily (02:00-03:00): Automated backups
Weekly (Sunday 03:00): System updates (planned)
Monthly (First Sunday): Backup restore tests
Quarterly: SSL certificate renewal checks
```

### Manual Procedures
```
Vault Unseal: pct exec 213 -- vault operator unseal
Container Updates: Manual approval via EMIYA (planned)
Resource Monitoring: Daily checks via MERLIN agent
Health Checks: Automated via Uptime Kuma + Prometheus
```

---

## 📱 Access Methods Summary

### Management Access
```
Primary: Tailscale VPN (mesh network)
Backup: Local LAN access (192.168.x.x)
Emergency: IPMI/iDRAC (not configured)
```

### User Access
```
External: Cloudflare Access (7 services)
Gaming: Direct domain access (game servers only)
Internal: Local network only (monitoring, infrastructure)
```

### API Access
```
Proxmox: root@pam!gilgamesh token
Vault: Manual token generation
n8n: Webhook-based automation
Nextcloud: WebDAV for file operations
GitHub: Personal access token for documentation
```

---

*Service catalog current as of: May 9, 2026*  
*Next update: After Phase 22.8C completion*