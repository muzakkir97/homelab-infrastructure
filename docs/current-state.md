# 📊 Current Infrastructure State

> **Snapshot Date:** April 26, 2026  
> **Infrastructure Status:** 🟢 Operational  
> **Total Containers:** 14 LXC + 1 KVM VM  
> **Uptime:** 99.8% (last 30 days)

---

## 🖥️ Container Status

### All Containers (VLAN 30 - Services)

| CTID | Type | Name | IP Address | Status | CPU | RAM | Disk | Autostart |
|------|------|------|------------|---------|-----|-----|------|-----------|
| 201 | LXC | nginx-proxy-manager | 192.168.30.201 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 202 | LXC | monitoring-prometheus | 192.168.30.202 | ✅ Running | 2 cores | 2GB | 20GB | ✅ Yes |
| 203 | LXC | monitoring-grafana | 192.168.30.203 | ✅ Running | 1 core | 1GB | 8GB | ✅ Yes |
| 204 | LXC | monitoring-loki | 192.168.30.204 | ✅ Running | 1 core | 1GB | 20GB | ✅ Yes |
| 205 | LXC | monitoring-alertmanager | 192.168.30.205 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 206 | LXC | monitoring-uptime | 192.168.30.206 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 207 | LXC | network-ddns | 192.168.30.207 | ✅ Running | 1 core | 256MB | 4GB | ✅ Yes |
| 208 | LXC | dashboard-homepage | 192.168.30.208 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 211 | LXC | automation-n8n | 192.168.30.211 | ✅ Running | 2 cores | 2GB | 20GB | ✅ Yes |
| 213 | LXC | vault | 192.168.30.213 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 214 | LXC | password-vaultwarden | 192.168.30.214 | ✅ Running | 1 core | 512MB | 8GB | ✅ Yes |
| 220 | LXC | nextcloud-hub | 192.168.30.220 | ✅ Running | 4 cores | 4GB | 100GB | ✅ Yes |
| 300 | LXC | gaming-panel | 192.168.30.210 | ✅ Running | 2 cores | 2GB | 20GB | ✅ Yes |
| 302 | LXC | gaming-wings-1 | 192.168.30.212 | ✅ Running | 4 cores | 4GB | 50GB | ✅ Yes |
| 400 | VM | ollama-gpu | 192.168.30.221 | ✅ Running | 8 cores | 16GB | 150GB | ✅ Yes |

### Resource Summary
- **Total CPU Allocation:** 30 cores (VM 400 uses 8 cores)
- **Total RAM Allocation:** 29.25GB (VM 400 uses 16GB)
- **Total Disk Usage:** ~440GB across all containers/VM

### Special Notes
- **VM 400 (ollama-gpu):** KVM virtual machine with PCIe GPU passthrough (RX 6700 XT + audio)
- **CT 220 (nextcloud-hub):** Recently resized from 20GB to 100GB root disk
- **All containers:** Set to autostart on boot
- **All services:** Running on VLAN 30 (192.168.30.0/24)

---

## 🌐 Network Topology

### VLAN Configuration
| VLAN | Network | Gateway | Active Devices | Purpose |
|------|---------|---------|----------------|---------|
| 10 | 192.168.10.0/24 | 192.168.10.1 | 3 devices | Management (Proxmox, NAS, pfSense) |
| 20 | 192.168.20.0/24 | 192.168.20.1 | 2 devices | Client devices (Gaming PC, etc.) |
| 30 | 192.168.30.0/24 | 192.168.30.1 | 16 devices | All services + Pi-hole |
| 40 | 192.168.40.0/24 | 192.168.40.1 | 0 devices | DMZ (planned) |
| 50 | 192.168.50.0/24 | 192.168.50.1 | 0 devices | Malware lab (air-gapped) |

### Physical Network Devices
| Device | IP Address | VLAN | Status | Notes |
|--------|------------|------|---------|-------|
| **pfSense Firewall** | 192.168.10.1 | 10 | ✅ Online | AC8F Mini PC, Intel N100 |
| **Proxmox Host** | 192.168.10.5 | 10 | ✅ Online | Kuromoon - Ryzen 5 5600X |
| **NAS (Kinmoon)** | 192.168.10.15 | 10 | ✅ Online | UGREEN DXP2800, 3.6TB |
| **Managed Switch** | 192.168.1.20 | Legacy | ✅ Online | TP-Link TL-SG108E |
| **Pi-hole DNS** | 192.168.30.10 | 30 | ✅ Online | Raspberry Pi 4 |
| **Gaming PC** | 192.168.20.101 | 20 | ✅ Online | Minimoon - Gaming only |

### Network Services Status
| Service | Status | Performance | Notes |
|---------|---------|------------|-------|
| **DNS Resolution** | ✅ Healthy | <5ms avg | Pi-hole blocking 489K domains |
| **Internet Connectivity** | ✅ Stable | 1Gbps down/up | TIME Fiber |
| **VPN (Tailscale)** | ✅ Connected | <10ms mesh | Subnet router on pfSense |
| **DHCP Services** | ✅ Active | 95% lease util | Per-VLAN DHCP pools |
| **Firewall Rules** | ✅ Enforced | 0 violations | Inter-VLAN restrictions active |

---

## 💾 Storage Pools & Usage

### Proxmox Storage Pools
| Pool Name | Type | Total | Used | Available | Usage % | Notes |
|-----------|------|-------|------|-----------|---------|-------|
| **local-lvm** | LVM-Thin | 137GB | 89GB | 48GB | 65% | Container root disks |
| **vmpool-fast** | ZFS | 899GB | 234GB | 665GB | 26% | Fast container storage |
| **data-storage** | Directory | 7.3TB | 2.1TB | 5.2TB | 29% | Large data, backups |
| **kinmoon-nfs** | NFS | 3.6TB | 1.8TB | 1.8TB | 50% | NAS backup target |

### Storage Health
- **Local NVMe SSD:** ✅ Healthy (SMART status good)
- **Secondary HDD:** ✅ Healthy (7200 RPM WD Purple)
- **NAS Storage:** ✅ Healthy (WD Purple surveillance drive)
- **ZFS Pool:** ✅ No errors, scrub completed successfully

### Critical Storage Alerts
- ⚠️ **local-lvm overprovisioning** — 65% used, thin pool optimization needed (Phase 33)
- ✅ All other pools within safe operating thresholds

---

## 🔄 Backup Job Status

### Automated Backup Schedule
| Job Name | Schedule | Last Run | Status | Duration | Size |
|----------|----------|----------|---------|----------|------|
| **Small Containers** | Daily 02:00 | Apr 26 02:05 | ✅ Success | 12 minutes | 4.2GB |
| **Large Containers** | Daily 02:30 | Apr 26 02:45 | ✅ Success | 28 minutes | 18.5GB |

### Individual Container Backups
| Container | Last Backup | Status | Size | Retention |
|-----------|------------|---------|------|-----------|
| CT 201 | Apr 26 02:05 | ✅ Success | 0.8GB | 7d/4w/2m |
| CT 202 | Apr 26 02:07 | ✅ Success | 1.2GB | 7d/4w/2m |
| CT 203 | Apr 26 02:08 | ✅ Success | 0.6GB | 7d/4w/2m |
| CT 204 | Apr 26 02:10 | ✅ Success | 0.9GB | 7d/4w/2m |
| CT 205 | Apr 26 02:12 | ✅ Success | 0.3GB | 7d/4w/2m |
| CT 206 | Apr 26 02:13 | ✅ Success | 0.4GB | 7d/4w/2m |
| CT 207 | Apr 26 02:14 | ✅ Success | 0.2GB | 7d/4w/2m |
| CT 214 | Apr 26 02:15 | ✅ Success | 0.3GB | 7d/4w/2m |
| CT 300 | Apr 26 02:16 | ✅ Success | 1.1GB | 7d/4w/2m |
| CT 220 | Apr 26 02:35 | ✅ Success | 12.8GB | 7d/4w/2m |
| CT 302 | Apr 26 02:58 | ✅ Success | 5.7GB | 7d/4w/2m |

### Backup Health
- **Success Rate:** 100% (last 7 days)
- **Average Duration:** 20 minutes
- **Storage Used:** 22.7GB total backup size
- **Oldest Backup:** March 26, 2026 (30 days retention working)

---

## 🔗 External Access Configuration

### Cloudflare Tunnel Status
| Service | Subdomain | Status | SSL | Auth Method |
|---------|-----------|---------|-----|------------|
| **Grafana** | grafana.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **n8n** | n8n.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Vault** | vault.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Vaultwarden** | passwords.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Ollama** | ollama.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Homepage** | home.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Nextcloud** | cloud.najhin-gaming.com | ✅ Active | ✅ Valid | Email OTP |
| **Terraria** | terraria.najhin-gaming.com | ✅ Active | ✅ Valid | Public |
| **Minecraft** | mc.najhin-gaming.com | ✅ Active | ✅ Valid | Public |

### SSL Certificate Status
- **Certificate Authority:** Let's Encrypt via Cloudflare
- **Renewal Status:** Auto-renewal active
- **Expiration:** All certificates valid for 90+ days
- **HTTPS Redirect:** Enforced on all services

### Authentication Configuration
- **Cloudflare Access:** Email OTP (muzakkir.kholil06@gmail.com only)
- **Bypass Rules:** Vaultwarden API paths (/api/, /identity/)
- **Session Duration:** 24 hours
- **Failed Attempts:** Locked after 5 attempts

---

## 📊 Active Monitoring & Alerting

### Prometheus Metrics Collection
| Target | Status | Last Scrape | Response Time |
|---------|---------|------------|---------------|
| **Node Exporter (Kuromoon)** | ✅ UP | 30s ago | 12ms |
| **cAdvisor (Containers)** | ✅ UP | 30s ago | 8ms |
| **Alertmanager** | ✅ UP | 30s ago | 5ms |
| **Grafana** | ✅ UP | 30s ago | 15ms |
| **Loki** | ✅ UP | 30s ago | 20ms |

### Current System Metrics
| Metric | Current Value | Threshold | Status |
|---------|--------------|-----------|---------|
| **Host CPU Usage** | 15.2% | 80% | ✅ Normal |
| **Host Memory Usage** | 18.5GB/32GB (58%) | 85% | ✅ Normal |
| **Host Disk Usage** | 2.4TB/8.2TB (29%) | 90% | ✅ Normal |
| **CPU Temperature** | 48.5°C | 80°C | ✅ Normal |
| **GPU Temperature** | 46°C | 85°C | ✅ Normal |
| **GPU Power** | 5W (idle) | 220W max | ✅ Normal |

### Active Alerts (Last 24 Hours)
| Severity | Count | Status |
|----------|-------|---------|
| **Critical** | 0 | ✅ None |
| **Warning** | 0 | ✅ None |
| **Info** | 2 | ℹ️ Backup completion notifications |

### Alert Routing
- **Critical Alerts:** Telegram (@JhinGilgamesh_bot)
- **Warning Alerts:** Discord #alerts channel
- **Backup Notifications:** Telegram
- **Maintenance Windows:** Suppressed during scheduled maintenance

### Grafana Dashboard Status
| Dashboard | Panels | Status | Last Updated |
|-----------|---------|---------|--------------|
| **Infrastructure Overview** | 12 panels | ✅ Active | Real-time |
| **Container Metrics** | 16 panels | ✅ Active | Real-time |
| **Network Performance** | 8 panels | ✅ Active | Real-time |
| **Gaming Servers** | 6 panels | ✅ Active | Real-time |
| **Backup Status** | 4 panels | ✅ Active | Daily |

---

## 🤖 AI Agent Status

### Gilgamesh Telegram Bot
- **Status:** ✅ Active and responding
- **Response Time:** <2 seconds average
- **Memory:** 20 message conversation context
- **Cost Today:** $0.15 (local inference + minimal API usage)
- **Route Distribution:** 75% Ollama, 20% Haiku, 5% Sonnet

### Ollama Local LLM (VM 400)
- **Primary Model:** qwen3:14b (9.3GB)
- **Secondary Model:** llama3.2:latest (3B)
- **GPU Utilization:** 12GB VRAM available
- **Status:** ✅ Running with ROCm acceleration
- **Performance:** ~45 tokens/second inference

### n8n Workflow Status
| Workflow | Status | Last Execution | Success Rate |
|----------|---------|---------------|--------------|
| **Telegram Agent** | ✅ Active | 5 minutes ago | 99.8% |
| **Documentation Pipeline — Update** | ✅ Active | 2 hours ago | 100% |
| **Documentation Pipeline — Sync Docs** | ✅ Active | 1 day ago | 100% |
| **Da Vinci Documentation Pipeline** | ✅ Active | 2 hours ago | 100% |

---

## 🔒 Security Status

### Firewall Rules Active
- **Inter-VLAN Restrictions:** ✅ Enforced
- **Management Access:** ✅ Tailscale only
- **External Services:** ✅ Cloudflare Tunnel only
- **Port Forwarding:** ✅ None (secure by design)

### Secret Management
- **HashiCorp Vault:** ✅ Operational (manual unseal required after reboot)
- **Vaultwarden:** ✅ Active with 47 password entries
- **API Keys:** ✅ All stored in Vault KV engine
- **SSH Keys:** ✅ Managed and rotated

### DNS Security
- **Pi-hole Status:** ✅ Blocking 489,247 domains
- **Query Response Time:** 5ms average
- **Block Rate:** 31.2% of queries blocked today
- **Top Blocked:** Google Analytics, Facebook trackers, ads

### VPN Access
- **Tailscale Mesh:** ✅ 4 devices connected
- **Subnet Routing:** ✅ pfSense advertising all VLANs
- **ACLs:** ✅ Configured for role-based access
- **Last Activity:** 15 minutes ago

---

## ⚡ Performance Metrics

### Host System (Kuromoon)
- **CPU:** AMD Ryzen 5 5600X @ 3.7GHz (15.2% avg load)
- **Memory:** 18.5GB/32GB used (58% utilization)
- **Storage I/O:** 15 IOPS average
- **Network:** 1Gbps interface, 45Mbps current usage
- **Temperature:** CPU 48.5°C, GPU 46°C (both nominal)

### Container Performance Top 5 Resource Users
1. **VM 400 (ollama-gpu):** 16GB RAM, 8 cores, 5W GPU idle
2. **CT 220 (nextcloud-hub):** 4GB RAM, 4 cores
3. **CT 302 (gaming-wings-1):** 4GB RAM, 4 cores
4. **CT 202 (prometheus):** 2GB RAM, 2 cores
5. **CT 211 (n8n):** 2GB RAM, 2 cores

### Network Performance
- **Internal Latency:** <1ms between VLANs
- **Internet Latency:** 12ms to 8.8.8.8
- **DNS Resolution:** 5ms average via Pi-hole
- **Tailscale Mesh:** 8ms peer-to-peer
- **Throughput:** 950Mbps down, 945Mbps up (speedtest)

---

## 🎮 Gaming Services Status

### Active Game Servers
| Game | Status | Players | CPU | RAM | Uptime |
|------|---------|---------|-----|-----|--------|
| **Terraria** | ✅ Running | 0/8 | 2% | 156MB | 7 days |
| **Minecraft** | ✅ Running | 0/20 | 1% | 1.2GB | 7 days |
| **Windrose** | ✅ Running | 0/4 | 1% | 89MB | 7 days |

### Pterodactyl Panel
- **Status:** ✅ Operational
- **Version:** Latest stable
- **Servers Managed:** 3 active
- **Resource Usage:** 25% of allocated limits
- **Backup Schedule:** Daily automated snapshots

---

## 📈 Trends & Growth

### 30-Day Metrics
- **Average CPU:** 12.8% (trending stable)
- **Peak Memory:** 22.1GB (during backup window)
- **Storage Growth:** +156GB (mostly logs and backups)
- **Alert Volume:** 0 critical, 3 warnings (all resolved)
- **Uptime:** 99.8% (1 planned maintenance window)

### Capacity Planning
- **RAM:** 58% utilized, no immediate scaling needed
- **Storage:** 29% full, good headroom for growth
- **Network:** <5% utilization, plenty of bandwidth
- **Recommendation:** Monitor local-lvm thin pool, optimize in Phase 33

---

*Infrastructure snapshot current as of April 26, 2026 02:00 UTC*