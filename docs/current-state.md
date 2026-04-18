# 📊 Current Infrastructure State

> **Last Updated:** April 14, 2026  
> **Status:** All systems operational

---

## 🖥️ Hardware Status

| Device | Hostname | IP | Status | Notes |
|--------|----------|-----|--------|-------|
| Proxmox Server | Kuromoon | 192.168.10.5 | 🟢 Online | Primary hypervisor |
| pfSense Firewall | — | 192.168.10.1 | 🟢 Online | Router + Firewall |
| Managed Switch | — | 192.168.1.20 | 🟢 Online | Pending IP migration to 192.168.10.20 |
| NAS | Kinmoon | 192.168.10.15 | 🟢 Online | Backup target |
| Pi-hole DNS | — | 192.168.30.10 | 🟢 Online | ~489K domains blocked |
| Gaming PC | Minimoon | 192.168.20.101 | — | Gaming only, not monitored |

---

## 📦 Container Inventory (Proxmox LXC)

All containers on **VLAN 30** (192.168.30.0/24) with **autostart enabled**.

| CTID | Name | IP | CPU | RAM | Disk | Status |
|------|------|-----|-----|-----|------|--------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 1 | 512MB | 8GB | 🟢 Running |
| 202 | monitoring-prometheus | 192.168.30.202 | 2 | 1GB | 20GB | 🟢 Running |
| 203 | monitoring-grafana | 192.168.30.203 | 1 | 512MB | 10GB | 🟢 Running |
| 204 | monitoring-loki | 192.168.30.204 | 2 | 1GB | 50GB | 🟢 Running |
| 205 | monitoring-alertmanager | 192.168.30.205 | 1 | 256MB | 8GB | 🟢 Running |
| 206 | monitoring-uptime | 192.168.30.206 | 1 | 512MB | 8GB | 🟢 Running |
| 207 | network-ddns | 192.168.30.207 | 1 | 256MB | 4GB | 🟢 Running |
| 208 | dashboard-homepage | 192.168.30.208 | 1 | 512MB | 8GB | 🟢 Running |
| 211 | automation-n8n | 192.168.30.211 | 2 | 2GB | 20GB | 🟢 Running |
| 220 | nextcloud-hub | 192.168.30.220 | 2 | 4GB | 100GB | 🟢 Running |
| 300 | gaming-panel | 192.168.30.210 | 2 | 2GB | 20GB | 🟢 Running |
| 302 | gaming-wings-1 | 192.168.30.212 | 4 | 8GB | 50GB | 🟢 Running |

**Total: 12 containers**

---

## 🌐 Network Configuration

### VLAN Summary

| VLAN | Name | Subnet | Gateway | Devices |
|------|------|--------|---------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | 192.168.10.1 | Kuromoon, pfSense, Kinmoon |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | 192.168.20.1 | Minimoon, clients |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All containers, Pi-hole |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | 192.168.40.1 | (Future use) |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | 192.168.50.1 | (Air-gapped, future use) |

### DNS Configuration

- **Primary DNS:** Pi-hole (192.168.30.10)
- **Upstream:** Cloudflare (1.1.1.1, 1.0.0.1)
- **Domains Blocked:** ~489,000
- **Listening Mode:** ALL (for multi-VLAN support)

---

## 🔗 External Access

### Subdomains (via Cloudflare)

| Subdomain | Target | Protection | Status |
|-----------|--------|------------|--------|
| home.najhin-gaming.com | CT 208 (Homepage) | None | 🟢 Active |
| grafana.najhin-gaming.com | CT 203 (Grafana) | Cloudflare Access (email OTP) | 🟢 Active |
| n8n.najhin-gaming.com | CT 211 (n8n) | Cloudflare Access (email OTP) | 🟢 Active |
| cloud.najhin-gaming.com | CT 220 (Nextcloud) | Cloudflare Tunnel | 🟢 Active |
| terraria.najhin-gaming.com | CT 302 (Game Server) | DNS Only (no proxy) | 🟢 Active |
| mc.najhin-gaming.com | CT 302 (Game Server) | DNS Only (no proxy) | 🟢 Active |

### Remote Access

| Method | Status | Use Case |
|--------|--------|----------|
| Tailscale | 🟢 Active | Primary remote access (subnet router on pfSense) |
| Cloudflare Tunnel | 🟢 Active | Nextcloud external access |
| Cloudflare Access | 🟢 Active | Grafana, n8n authentication |

---

## 📊 Monitoring Status

### Prometheus Targets

| Target | Endpoint | Status |
|--------|----------|--------|
| Proxmox (Kuromoon) | 192.168.10.5:9100 | 🟢 UP |
| pfSense | 192.168.10.1:9100 | 🟢 UP |
| Pi-hole | 192.168.30.10:9100 | 🟢 UP |
| All containers | 192.168.30.x:9100 | 🟢 UP |

### Alertmanager Routes

| Severity | Destination | Timing |
|----------|-------------|--------|
| Critical | Telegram | Immediate |
| Warning | Discord | During work hours |

### Alert Rules (13 total)

- Host availability
- CPU usage > 80%
- Memory usage > 85%
- Disk usage > 80%
- Service health checks

---

## 💾 Backup Status

### Backup Jobs

| Job | Schedule | Target | Last Run | Status |
|-----|----------|--------|----------|--------|
| Small Containers | 02:00 daily | kinmoon-smb | — | 🟢 Scheduled |
| Large Containers | 02:30 daily | data-storage | — | 🟢 Scheduled |

### Retention Policy

- **Daily:** 7 backups
- **Weekly:** 4 backups
- **Monthly:** 2 backups

### Storage Usage

| Storage | Used | Total | % Used |
|---------|------|-------|--------|
| local-lvm | — | 137 GB | — |
| vmpool-fast | — | 899 GB | — |
| kinmoon-smb | — | 3.6 TB | — |
| data-storage | — | 7.2 TB | — |

---

## 🤖 Gilgamesh AI Agent Status

| Component | Status | Details |
|-----------|--------|---------|
| Telegram Bot | 🟢 Active | @JhinGilgamesh_bot |
| n8n Workflow | 🟢 Running | CT 211 |
| Conversation Memory | 🟢 Working | Last 20 messages |
| Smart Routing | 🟢 Working | Haiku (simple) / Sonnet (complex) |
| Web Search | 🟢 Working | Claude web_search tool |
| Cost Tracking | 🟢 Working | gilgamesh_costs table |
| /update Command | 🟢 Working | Syncs to Nextcloud + GitHub |
| Inline Keyboard Menu | 🔄 In Progress | Status submenu working |

---

## 🎮 Game Servers Status

| Server | Game | IP:Port | Status |
|--------|------|---------|--------|
| Terraria | Terraria | terraria.najhin-gaming.com:7777 | 🟢 Online |
| Minecraft | Minecraft Java | mc.najhin-gaming.com:25565 | 🟢 Online |

Managed via **Pterodactyl Panel** (CT 300) + **Wings** (CT 302).

---

## 🔧 Pending Tasks

| Task | Priority | Notes |
|------|----------|-------|
| Switch IP migration (192.168.1.20 → 192.168.10.20) | Medium | Requires physical access |
| Remove legacy LAN (192.168.1.0/24) | Medium | After switch migration |
| WiFi AP setup (EAP610) | Medium | Hardware purchased |
| Homepage security (Cloudflare Access) | Low | Currently unprotected |

---

## 📈 System Health Summary

| Metric | Status |
|--------|--------|
| **All Containers** | 🟢 12/12 Running |
| **Monitoring** | 🟢 All targets UP |
| **Backups** | 🟢 Scheduled |
| **External Access** | 🟢 All subdomains active |
| **Security** | 🟢 Cloudflare Access on sensitive services |

---

*Last verified: April 14, 2026*
