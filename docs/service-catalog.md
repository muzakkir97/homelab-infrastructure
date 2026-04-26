# 📁 Service Catalog

> **Last Updated:** April 26, 2026  
> **Total Services:** 15 containers + 1 VM = 16 total services  
> **External Services:** 9 publicly accessible via Cloudflare Tunnel

---

## 🌐 Networking & Load Balancing

### Nginx Proxy Manager (CT 201)
- **Container:** nginx-proxy-manager
- **IP Address:** 192.168.30.201
- **Ports:** 80 (HTTP), 443 (HTTPS), 81 (Admin Web UI)
- **Subdomain:** Internal only
- **Access Method:** Local network access to admin UI at 192.168.30.201:81
- **Credentials:** Stored in Vaultwarden
- **Backup Schedule:** Daily 02:05 (Small Containers job)
- **Dependencies:** None
- **Purpose:** Reverse proxy, SSL certificate management, internal traffic routing
- **Status:** ✅ Operational

---

## 📊 Monitoring & Observability

### Prometheus Metrics Collection (CT 202)
- **Container:** monitoring-prometheus
- **IP Address:** 192.168.30.202
- **Ports:** 9090 (Web UI)
- **Subdomain:** Internal only
- **Access Method:** Local network at 192.168.30.202:9090
- **Credentials:** None (internal service)
- **Backup Schedule:** Daily 02:07 (Small Containers job)
- **Dependencies:** Node Exporter, cAdvisor
- **Purpose:** Metrics collection, storage, and alerting rules
- **Status:** ✅ Operational

### Grafana Dashboard (CT 203)
- **Container:** monitoring-grafana
- **IP Address:** 192.168.30.203
- **Ports:** 3000 (Web UI)
- **Subdomain:** grafana.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP)
- **Credentials:** Stored in Vault (kv/grafana)
- **Backup Schedule:** Daily 02:08 (Small Containers job)
- **Dependencies:** Prometheus, Loki
- **Purpose:** Metrics visualization, infrastructure dashboards
- **Status:** ✅ Operational

### Loki Log Aggregation (CT 204)
- **Container:** monitoring-loki
- **IP Address:** 192.168.30.204
- **Ports:** 3100 (HTTP API)
- **Subdomain:** Internal only
- **Access Method:** API access via Grafana and internal services
- **Credentials:** None (internal service)
- **Backup Schedule:** Daily 02:10 (Small Containers job)
- **Dependencies:** None
- **Purpose:** Log aggregation, storage, and querying
- **Status:** ✅ Operational

### Alertmanager (CT 205)
- **Container:** monitoring-alertmanager
- **IP Address:** 192.168.30.205
- **Ports:** 9093 (Web UI), 9094 (Cluster)
- **Subdomain:** Internal only
- **Access Method:** Local network at 192.168.30.205:9093
- **Credentials:** Webhook tokens stored in Vault (kv/alertmanager)
- **Backup Schedule:** Daily 02:12 (Small Containers job)
- **Dependencies:** Prometheus
- **Purpose:** Alert routing to Telegram and Discord
- **Status:** ✅ Operational

### Uptime Kuma (CT 206)
- **Container:** monitoring-uptime
- **IP Address:** 192.168.30.206
- **Ports:** 3001 (Web UI)
- **Subdomain:** Internal only
- **Access Method:** Local network at 192.168.30.206:3001
- **Credentials:** Stored in Vaultwarden
- **Backup Schedule:** Daily 02:13 (Small Containers job)
- **Dependencies:** None
- **Purpose:** Service uptime monitoring, status page
- **Status:** ✅ Operational

---

## 🔧 Automation & Workflow

### n8n Workflow Automation (CT 211)
- **Container:** automation-n8n
- **IP Address:** 192.168.30.211
- **Ports:** 5678 (Web UI)
- **Subdomain:** n8n.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP)
- **Credentials:** Various API keys stored in Vault (kv/n8n)
- **Backup Schedule:** Daily 02:15 (Small Containers job, large container)
- **Dependencies:** Vault for secrets, external APIs
- **Purpose:** Workflow automation, Gilgamesh AI agent, documentation pipeline
- **Status:** ✅ Operational
- **Active Workflows:** 6 (Telegram Agent, Documentation Pipelines, Da Vinci)

### Dynamic DNS (CT 207)
- **Container:** network-ddns
- **IP Address:** 192.168.30.207
- **Ports:** None (background service)
- **Subdomain:** None
- **Access Method:** SSH for configuration
- **Credentials:** Cloudflare API token in Vault (kv/cloudflare)
- **Backup Schedule:** Daily 02:14 (Small Containers job)
- **Dependencies:** External IP detection, Cloudflare API
- **Purpose:** Automatic DNS record updates for najhin-gaming.com
- **Status:** ✅ Operational

---

## 🎮 Gaming Platform

### Pterodactyl Panel (CT 300)
- **Container:** gaming-panel
- **IP Address:** 192.168.30.210
- **Ports:** 80 (HTTP), 443 (HTTPS via NPM)
- **Subdomain:** Internal only
- **Access Method:** Local network management interface
- **Credentials:** Admin account in Vaultwarden
- **Backup Schedule:** Daily 02:16 (Small Containers job)
- **Dependencies:** CT 302 (Wings daemon)
- **Purpose:** Game server management panel
- **Status:** ✅ Operational
- **Managed Servers:** 3 (Terraria, Minecraft, Windrose)

### Gaming Wings Daemon (CT 302)
- **Container:** gaming-wings-1
- **IP Address:** 192.168.30.212
- **Ports:** 8080 (Wings API), 2022 (SFTP), game-specific ports
- **Subdomain:** terraria.najhin-gaming.com, mc.najhin-gaming.com
- **Access Method:** Pterodactyl management + external game access
- **Credentials:** Wings authentication token in Vault
- **Backup Schedule:** Daily 02:58 (Large Containers job)
- **Dependencies:** CT 300 (Panel), Docker runtime
- **Purpose:** Game server execution environment
- **Status:** ✅ Operational
- **Active Games:** Terraria (7777), Minecraft (25565), Windrose (7778)

---

## 💾 Storage & File Services

### Nextcloud Hub (CT 220)
- **Container:** nextcloud-hub
- **IP Address:** 192.168.30.220
- **Ports:** 80 (HTTP), 443 (HTTPS via NPM)
- **Subdomain:** cloud.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP)
- **Credentials:** Admin account in Vault (kv/nextcloud)
- **Backup Schedule:** Daily 02:35 (Large Containers job)
- **Dependencies:** Database (internal), storage pool
- **Purpose:** File sync, collaboration, WebDAV, knowledge base storage
- **Status:** ✅ Operational
- **Storage:** 100GB root disk, 12.8GB used
- **Version:** 33.0.2

---

## 🔒 Security & Secrets Management

### HashiCorp Vault (CT 213)
- **Container:** vault
- **IP Address:** 192.168.30.213
- **Ports:** 8200 (HTTPS Web UI/API)
- **Subdomain:** vault.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP)
- **Credentials:** Root token and unseal keys (offline storage)
- **Backup Schedule:** Daily 02:15 (Small Containers job)
- **Dependencies:** Manual unseal required after reboot
- **Purpose:** Centralized secrets management, API keys, certificates
- **Status:** ✅ Operational (unsealed)
- **KV Paths:** 8 active paths (gilgamesh, cloudflare, proxmox, etc.)

### Vaultwarden Password Manager (CT 214)
- **Container:** password-vaultwarden
- **IP Address:** 192.168.30.214
- **Ports:** 80 (HTTP via NPM)
- **Subdomain:** passwords.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP) with API bypass
- **Credentials:** Admin token in Vault (kv/vaultwarden)
- **Backup Schedule:** Daily 02:15 (Small Containers job)
- **Dependencies:** None
- **Purpose:** Personal password management, Bitwarden compatible
- **Status:** ✅ Operational
- **Stored Items:** 47 password entries

---

## 🏠 Dashboard & Interface

### Homepage Dashboard (CT 208)
- **Container:** dashboard-homepage
- **IP Address:** 192.168.30.208
- **Ports:** 3000 (HTTP)
- **Subdomain:** home.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP, planned)
- **Credentials:** Configuration files only
- **Backup Schedule:** Daily 02:13 (Small Containers job)
- **Dependencies:** Various services for status checks
- **Purpose:** Infrastructure overview, service links, status monitoring
- **Status:** ✅ Operational
- **Features:** Service status, quick links, system metrics

---

## 🤖 AI & Machine Learning

### Ollama GPU Inference (VM 400)
- **VM:** ollama-gpu (KVM with PCIe passthrough)
- **IP Address:** 192.168.30.221
- **Ports:** 11434 (Ollama API), 8080 (Open WebUI)
- **Subdomain:** ollama.najhin-gaming.com
- **Access Method:** External via Cloudflare Access (Email OTP)
- **Credentials:** SSH user 'muzakkir' with sudo, stored in Vault
- **Backup Schedule:** None (VM with passed-through GPU)
- **Dependencies:** RX 6700 XT GPU passthrough, ROCm drivers
- **Purpose:** Local LLM inference, AI model serving
- **Status:** ✅ Operational
- **Models:** qwen3:14b (primary), llama3.2:latest (secondary)
- **Performance:** ~45 tokens/second with GPU acceleration

---

## 🌐 External Access Summary

### Publicly Accessible Services (9 total)
| Service | URL | Authentication | SSL |
|---------|-----|----------------|-----|
| **Grafana** | grafana.najhin-gaming.com | Cloudflare Access + Email OTP | ✅ Let's Encrypt |
| **n8n** | n8n.najhin-gaming.com | Cloudflare Access + Email OTP | ✅ Let's Encrypt |
| **Vault** | vault.najhin-gaming.com | Cloudflare Access + Email OTP | ✅ Let's Encrypt |
| **Vaultwarden** | passwords.najhin-gaming.com | Cloudflare Access + Email OTP¹ | ✅ Let's Encrypt |
| **Ollama/WebUI** | ollama.najhin-gaming.com | Cloudflare Access + Email OTP | ✅ Let's Encrypt |
| **Homepage** | home.najhin-gaming.com | Planned: Cloudflare Access | ✅ Let's Encrypt |
| **Nextcloud** | cloud.najhin-gaming.com | Cloudflare Access + Email OTP | ✅ Let's Encrypt |
| **Terraria** | terraria.najhin-gaming.com | Public (game client auth) | ✅ Let's Encrypt |
| **Minecraft** | mc.najhin-gaming.com | Public (game client auth) | ✅ Let's Encrypt |

¹ *API paths (/api/, /identity/) bypass Cloudflare Access for mobile app compatibility*

### Internal-Only Services (7 total)
- Nginx Proxy Manager (192.168.30.201:81)
- Prometheus (192.168.30.202:9090)  
- Loki (192.168.30.204:3100)
- Alertmanager (192.168.30.205:9093)
- Uptime Kuma (192.168.30.206:3001)
- Dynamic DNS (192.168.30.207, SSH only)
- Pterodactyl Panel (192.168.30.210, internal management)

---

## 🔄 Backup & Recovery

### Backup Job Coverage
| Job Name | Containers Included | Storage Target | Schedule |
|----------|-------------------|----------------|----------|
| **Small Containers** | 201, 202, 203, 204, 205, 206, 207, 214, 300 | kinmoon-nfs (NAS) | Daily 02:00 |
| **Large Containers** | 220, 302 | data-storage (Local HDD) | Daily 02:30 |

### Backup Retention Policy
- **Daily Snapshots:** 7 days retention
- **Weekly Snapshots:** 4 weeks retention  
- **Monthly Snapshots:** 2 months retention
- **Total Storage Used:** ~22.7GB across all backups

### Recovery Procedures
- **Container Restoration:** Proxmox backup restore via web UI
- **File-Level Recovery:** Mount backup and extract specific files
- **Disaster Recovery:** Full container rebuild from backup
- **RTO Target:** <2 hours for critical services
- **RPO Target:** <24 hours (daily backup schedule)

---

## 📡 Service Dependencies

### Critical Path Dependencies
```
pfSense (Gateway) → All services require network
├── Pi-hole (DNS) → Most services require name resolution
├── Nginx Proxy Manager → External access for web services
├── Vault → Secrets required by automation services
└── Nextcloud → File storage for knowledge base and backups
```

### Service Interdependencies
| Service | Depends On | Required By |
|---------|------------|-------------|
| **Vault** | None | n8n, DDNS, all automated services |
| **Prometheus** | Node Exporter | Grafana, Alertmanager |
| **Grafana** | Prometheus, Loki | Dashboard users |
| **Pterodactyl Panel** | None | Wings daemon (CT 302) |
| **Wings Daemon** | Panel (CT 300) | Game servers |
| **n8n** | Vault, external APIs | Gilgamesh bot, documentation |
| **DDNS** | Vault, Cloudflare API | External DNS resolution |

### Single Points of Failure
- **pfSense Firewall:** Total network outage if failed
- **Proxmox Host:** All containers/VM unavailable  
- **Vault (sealed):** Automated services lose credentials
- **NAS (Kinmoon):** Small container backups unavailable
- **Cloudflare Tunnel:** External access lost (VPN remains)

---

## 🔧 Maintenance Windows

### Scheduled Maintenance
- **Weekly:** Sunday 03:00-05:00 UTC (patch updates, non-critical)
- **Monthly:** First Saturday 02:00-06:00 UTC (major updates, reboots)
- **Quarterly:** Planned downtime for hardware maintenance

### Emergency Procedures
- **Service Restart:** Individual container restart via Proxmox
- **Host Reboot:** Full Proxmox host restart (all services down ~5 minutes)
- **Vault Unseal:** Manual operation required after host reboot
- **Network Issues:** Verify pfSense status, check physical connections
- **Storage Issues:** Monitor ZFS status, check disk health

---

## 📊 Performance Baselines

### Expected Response Times
| Service Category | Response Time | Availability Target |
|------------------|---------------|-------------------|
| **Web UIs** | <500ms | 99.5% |
| **APIs** | <200ms | 99.8% |
| **Game Servers** | <50ms | 99.9% |
| **Monitoring** | <100ms | 99.9% |
| **File Access** | <1s | 99.0% |

### Resource Allocation Summary
- **Total CPU Cores:** 30 allocated across all services
- **Total RAM:** 29.25GB allocated (including 16GB for VM 400)
- **Total Storage:** ~440GB across all services
- **Network Bandwidth:** 1Gbps shared (typically <5% utilization)

---

*Service catalog maintained automatically via documentation pipeline*  
*Last automated update: April 26, 2026*