# 📋 Service Catalog

> **Last Updated:** April 27, 2026  
> **Total Services:** 15 containers + 1 VM across 6 service categories  
> **External Access:** 9 services via Cloudflare Tunnel

---

## 🌐 Networking & Infrastructure

### Reverse Proxy & Load Balancer

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Nginx Proxy Manager** | CT 201 | 192.168.30.201 | 80, 443, 81 | — | Internal only | Vaultwarden | Daily 02:00 | None |

**Purpose:** Reverse proxy for all external services, SSL termination  
**Web UI:** http://192.168.30.201:81  
**Features:** Let's Encrypt automation, access lists, custom locations

### Dynamic DNS

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **DDNS Updater** | CT 207 | 192.168.30.207 | — | — | Internal only | Vault: kv/cloudflare | Daily 02:00 | Cloudflare API |

**Purpose:** Updates Cloudflare DNS records when ISP IP changes  
**Update Frequency:** Every 5 minutes  
**Provider:** Cloudflare

### Dashboard

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Homepage Dashboard** | CT 208 | 192.168.30.208 | 3000 | home.najhin-gaming.com | Public | None | Daily 02:00 | NPM |

**Purpose:** Infrastructure overview dashboard  
**Features:** Service status, system metrics, quick links  
**Access:** Open access (no authentication required)

---

## 📊 Monitoring & Observability

### Metrics Collection

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Prometheus** | CT 202 | 192.168.30.202 | 9090 | — | Internal only | None | Daily 02:00 | Node exporters |

**Purpose:** Time-series metrics database  
**Web UI:** http://192.168.30.202:9090  
**Scrape Targets:** Host, containers, pfSense, Pi-hole  
**Retention:** 15 days

### Visualization

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Grafana** | CT 203 | 192.168.30.203 | 3000 | grafana.najhin-gaming.com | Cloudflare Access | Vaultwarden | Daily 02:00 | Prometheus, Loki |

**Purpose:** Metrics visualization and dashboards  
**Authentication:** Email OTP via Cloudflare Access  
**Dashboards:** Host metrics, container stats, network monitoring

### Log Aggregation

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Loki** | CT 204 | 192.168.30.204 | 3100 | — | Internal only | None | Daily 02:00 | Promtail agents |

**Purpose:** Log aggregation and storage  
**Web UI:** http://192.168.30.204:3100  
**Log Sources:** Containers, host system  
**Retention:** 7 days

### Alerting

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Alertmanager** | CT 205 | 192.168.30.205 | 9093 | — | Internal only | Vault: kv/alertmanager | Daily 02:00 | Prometheus |

**Purpose:** Alert routing and notification management  
**Web UI:** http://192.168.30.205:9093  
**Channels:** Telegram (critical), Discord (warnings)  
**Features:** Grouping, inhibition, silencing

### Uptime Monitoring

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Uptime Kuma** | CT 206 | 192.168.30.206 | 3001 | — | Internal only | Local admin | Daily 02:00 | None |

**Purpose:** Service availability monitoring  
**Web UI:** http://192.168.30.206:3001  
**Monitors:** All external services, internal endpoints

---

## 🤖 Automation & AI

### Workflow Automation

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **n8n** | CT 211 | 192.168.30.211 | 5678 | n8n.najhin-gaming.com | Cloudflare Access | Vault: kv/n8n | Daily 02:00 | PostgreSQL |

**Purpose:** Workflow automation platform  
**Authentication:** Email OTP via Cloudflare Access  
**Active Workflows:** 12 (including AI agents)  
**Features:** Webhook endpoints, scheduled jobs, API integrations

**Active Workflows:**
- Telegram Agent (Gilgamesh)
- Documentation Pipeline (Update & Sync)
- Da Vinci Documentation Pipeline
- Midas CFO Report & Daily Brief
- MERLIN Reminders
- Daily Note Creator & Morning Briefing

### Local AI Inference

| Service | Virtual Machine | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Ollama + Open WebUI** | VM 400 | 192.168.30.221 | 11434, 8080 | ollama.najhin-gaming.com | Cloudflare Access | Vault: VM 400 | Not backed up | RX 6700 XT GPU |

**Purpose:** Local LLM inference with GPU acceleration  
**Type:** KVM VM with PCIe passthrough (not LXC)  
**GPU:** RX 6700 XT 12GB with ROCm 6.1.3  
**Models:** qwen3:14b (primary), llama3.2:latest (secondary)  
**SSH Access:** `ssh muzakkir@192.168.30.221` (use muzakkir user, not root)

---

## 🎮 Gaming Platform

### Game Server Panel

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Pterodactyl Panel** | CT 300 | 192.168.30.210 | 80 | — | Internal only | Local admin | Daily 02:00 | MySQL |

**Purpose:** Game server management interface  
**Web UI:** http://192.168.30.210  
**Features:** Server creation, resource management, file manager

### Game Server Wings

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Pterodactyl Wings + Docker** | CT 302 | 192.168.30.212 | 8080, 25565, 7777 | terraria/mc.najhin-gaming.com | Public | Vault: CT 302 | Daily 02:30 | Pterodactyl Panel |

**Purpose:** Game server execution environment  
**Hosted Games:**
- **Terraria:** terraria.najhin-gaming.com:7777
- **Minecraft:** mc.najhin-gaming.com:25565  
- **Windrose:** 4 max players, Medium difficulty, invite code: NAJHINWINDROSE

**Resource Usage:** ~9GB RAM when Windrose is active (baseline consideration)  
**Note:** Stop Windrose Docker container when not playing to free RAM

---

## 💾 Storage & Knowledge

### File Sync & Collaboration

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Nextcloud Hub** | CT 220 | 192.168.30.220 | 80 | cloud.najhin-gaming.com | Cloudflare Access | Vault: kv/nextcloud | Daily 02:30 | PostgreSQL |

**Purpose:** File synchronization, calendar, knowledge base  
**Authentication:** Email OTP via Cloudflare Access  
**Storage:** 100GB root disk, 7.3TB data storage planned  
**WebDAV URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/  
**Special Use:** Obsidian vault sync for second-brain knowledge base

**Obsidian Integration:**
- **Local Path:** C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **Vault Structure:** 10 organized folders with templates
- **Automation:** Daily notes via n8n, morning briefings

---

## 🔒 Security & Secrets

### Secrets Management

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **HashiCorp Vault** | CT 213 | 192.168.30.213 | 8200 | vault.najhin-gaming.com | Cloudflare Access | Root token | Daily 02:00 | None |

**Purpose:** Centralized secrets management  
**Authentication:** Email OTP via Cloudflare Access  
**Web UI:** https://vault.najhin-gaming.com  
**Engine:** KV v2 at path `kv/`  
**Auto-seal:** Disabled (manual unseal required after reboot)

**Secret Paths (8 total):**
- kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager
- kv/github, kv/nextcloud, kv/n8n, kv/pihole

### Password Manager

| Service | Container | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Vaultwarden** | CT 214 | 192.168.30.214 | 80 | passwords.najhin-gaming.com | Cloudflare Access | Master password | Daily 02:00 | None |

**Purpose:** Personal password manager (Bitwarden-compatible)  
**Authentication:** Email OTP via Cloudflare Access (bypasses /api/ and /identity/)  
**Web UI:** https://passwords.najhin-gaming.com  
**Features:** Browser extension, mobile apps, sharing

---

## 📡 External DNS & Security

### DNS Ad Blocking

| Service | External Device | IP | Port | Subdomain | Access | Credentials | Backup | Dependencies |
|---------|-----------|----|----- |-----------|--------|-------------|--------|--------------|
| **Pi-hole** | Raspberry Pi 4 | 192.168.30.10 | 53, 80 | — | Internal only | Vault: kv/pihole | Manual | Internet |

**Purpose:** Network-wide ad and tracker blocking  
**Web UI:** http://192.168.30.10/admin  
**Blocked Domains:** ~489,000  
**Query Types:** DNS filtering for all network traffic  
**Integration:** pfSense DNS Resolver forwards to Pi-hole

---

## 🔗 Service Dependencies

### Critical Path Dependencies

```
Internet → pfSense → Managed Switch → Proxmox Host
                                          ↓
                            All containers depend on host
                                          ↓
                    NPM required for all external services
                                          ↓
                    Cloudflare Tunnel for secure access
```

### Service Interconnections

| Service | Depends On | Used By |
|---------|------------|---------|
| **Prometheus** | Node exporters | Grafana, Alertmanager |
| **Grafana** | Prometheus, Loki | Dashboard users |
| **Alertmanager** | Prometheus | Telegram/Discord notifications |
| **n8n** | PostgreSQL, Vault | All AI agents |
| **Vault** | None | All services needing secrets |
| **NPM** | None | All external services |
| **Nextcloud** | PostgreSQL | Obsidian sync, file storage |

### External Dependencies

| Service | External Dependency | Purpose |
|---------|-------------------|---------|
| **All Services** | pfSense firewall | Network routing |
| **External Access** | Cloudflare Tunnel | Secure public access |
| **Authentication** | Cloudflare Access | Email OTP verification |
| **DNS Resolution** | Pi-hole | Ad/tracker blocking |
| **Backups** | NAS (Kinmoon) | Automated backup storage |

---

## 🚨 Service Health Monitoring

### Monitoring Coverage

| Service Category | Prometheus Metrics | Log Collection | Uptime Monitoring | Alerting |
|-----------------|-------------------|----------------|------------------|----------|
| Infrastructure | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Monitoring | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Automation | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Gaming | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Storage | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |
| Security | ✅ Yes | ✅ Yes | ✅ Yes | ✅ Yes |

### Alert Thresholds

| Metric | Warning | Critical | Notification |
|--------|---------|----------|--------------|
| **CPU Usage** | >80% | >90% | Discord / Telegram |
| **Memory Usage** | >85% | >95% | Discord / Telegram |
| **Disk Space** | <20% | <10% | Discord / Telegram |
| **Service Down** | — | Any service | Telegram |

---

## 🔄 Backup & Recovery

### Backup Schedule

| Time | Job | Containers | Storage | Retention |
|------|-----|------------|---------|-----------|
| **02:00** | Small containers | 9 containers | kinmoon-nfs (NAS) | 7 daily, 4 weekly, 2 monthly |
| **02:30** | Large containers | 2 containers | data-storage (local HDD) | 7 daily, 4 weekly, 2 monthly |

### Recovery Procedures

| Service Type | RTO | RPO | Recovery Method |
|-------------|-----|-----|-----------------|
| **Critical Services** | <1 hour | <24 hours | Proxmox backup restore |
| **Data Services** | <4 hours | <24 hours | File-level restore from NAS |
| **Configuration** | <30 minutes | <1 hour | Git repository restore |

---

*Last updated: April 27, 2026*