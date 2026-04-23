# 🔍 Current State — Live Infrastructure Snapshot

> **Snapshot Date:** April 24, 2026  
> **Uptime:** 99.9%  
> **Active Containers:** 14/14  
> **System Health:** ✅ All Green  

## 📊 Infrastructure Overview

### Hypervisor Status (Kuromoon)
- **Proxmox Version:** 8.1.4
- **Node Name:** muzakkir (not kuromoon)
- **CPU:** AMD Ryzen 5 5600X @ 48.5°C (idle)
- **RAM:** 32GB total, ~60% allocated to containers
- **GPU:** RX 6700 XT @ 46°C, 5W (zero-RPM idle)
- **Storage:** 137GB local-lvm + 899GB vmpool-fast (ZFS)
- **Network:** 4x Intel NICs (igc0-3), VLAN trunk on igc2

### Network Infrastructure
- **pfSense:** 192.168.10.1, Intel N100 AC8F mini PC
- **Switch:** TP-Link TL-SG108E at 192.168.1.20 (legacy IP)
- **Pi-hole DNS:** 192.168.30.10, blocking 489K+ domains
- **Tailscale:** Subnet router active on pfSense
- **VPN Status:** ✅ Connected, 0ms latency to homelab

---

## 📦 Container Status (All LXC)

### Running Containers (14/14)

| CTID | Name | IP | CPU | RAM | Disk | Status | Autostart |
|------|------|----|-----|-----|------|--------|-----------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 1 core | 1GB | 8GB | ✅ Running | ✅ |
| 202 | monitoring-prometheus | 192.168.30.202 | 2 cores | 2GB | 20GB | ✅ Running | ✅ |
| 203 | monitoring-grafana | 192.168.30.203 | 1 core | 1GB | 8GB | ✅ Running | ✅ |
| 204 | monitoring-loki | 192.168.30.204 | 1 core | 1GB | 12GB | ✅ Running | ✅ |
| 205 | monitoring-alertmanager | 192.168.30.205 | 1 core | 512MB | 4GB | ✅ Running | ✅ |
| 206 | monitoring-uptime | 192.168.30.206 | 1 core | 512MB | 4GB | ✅ Running | ✅ |
| 207 | network-ddns | 192.168.30.207 | 1 core | 512MB | 4GB | ✅ Running | ✅ |
| 208 | dashboard-homepage | 192.168.30.208 | 1 core | 512MB | 4GB | ✅ Running | ✅ |
| 211 | automation-n8n | 192.168.30.211 | 2 cores | 2GB | 12GB | ✅ Running | ✅ |
| 213 | vault | 192.168.30.213 | 1 core | 1GB | 8GB | ✅ Running | ✅ |
| 214 | password-vaultwarden | 192.168.30.214 | 1 core | 1GB | 8GB | ✅ Running | ✅ |
| 220 | nextcloud-hub | 192.168.30.220 | 4 cores | 4GB | 50GB | ✅ Running | ✅ |
| 300 | gaming-panel | 192.168.30.210 | 2 cores | 2GB | 20GB | ✅ Running | ✅ |
| 302 | gaming-wings-1 | 192.168.30.212 | 4 cores | 4GB | 50GB | ✅ Running | ✅ |

**Total Allocation:** 27 CPU cores, 21.5GB RAM, 212GB disk

### Container Details by Function

#### Core Infrastructure (6 containers)
- **NPM:** Reverse proxy, SSL termination
- **Prometheus:** Metrics from 20+ targets
- **Grafana:** 15+ dashboards, 500+ queries/day
- **Loki:** 50MB/day log ingestion
- **Alertmanager:** 12 notification channels
- **Uptime Kuma:** Monitoring 25+ endpoints

#### Automation & Tools (4 containers)
- **n8n:** 7 active workflows, 500+ executions/day
- **DDNS:** Cloudflare DNS updates every 5 minutes
- **Homepage:** Service catalog with 30+ links
- **Vault:** 8 secret paths, unsealed

#### Storage & Security (2 containers)
- **Nextcloud:** 15GB data, 3 active sync clients
- **Vaultwarden:** 200+ password entries

#### Gaming Platform (2 containers)
- **Pterodactyl Panel:** Managing 3 game servers
- **Wings Node:** Docker runtime for game servers

---

## 🌐 Network Topology

### VLAN Configuration (Active)

| VLAN | Network | Gateway | Devices | Purpose |
|------|---------|---------|---------|---------|
| 10 | 192.168.10.0/24 | 192.168.10.1 | 3 | Infrastructure |
| 20 | 192.168.20.0/24 | 192.168.20.1 | 8 | Client devices |
| 30 | 192.168.30.0/24 | 192.168.30.1 | 15 | Service containers |
| 40 | 192.168.40.0/24 | 192.168.40.1 | 0 | DMZ (unused) |
| 50 | 192.168.50.0/24 | 192.168.50.1 | 0 | Malware lab (unused) |

### Infrastructure Devices (VLAN 10)
- **192.168.10.1:** pfSense firewall
- **192.168.10.5:** Proxmox host (Kuromoon)
- **192.168.10.15:** NAS (Kinmoon) - SMB/NFS shares

### Service Containers (VLAN 30)
All 14 LXC containers plus Pi-hole (192.168.30.10)

### Client Devices (VLAN 20)
- **192.168.20.101:** Gaming PC (Minimoon)
- **192.168.20.x:** Mobile devices, laptops (8 total)

### Firewall Rules Status
- ✅ VLAN 20 → VLAN 10 blocked (client isolation)
- ✅ VLAN 30 → VLAN 10 limited (service access)
- ✅ n8n SSH access: 192.168.30.211 → 192.168.10.5:22
- ✅ Tailscale subnet routing enabled

---

## 💾 Storage Architecture

### Proxmox Storage Pools

| Pool | Type | Size | Used | Available | Purpose |
|------|------|------|------|-----------|---------|
| local | dir | 20GB | 15GB | 5GB | ISO/templates |
| local-lvm | LVM-Thin | 137GB | 85GB | 52GB | Root disks |
| vmpool-fast | ZFS | 899GB | 250GB | 649GB | Fast storage |
| kinmoon-nfs | NFS | 3.6TB | 800GB | 2.8TB | Backup target |

### NAS Storage (Kinmoon)
- **Total Capacity:** 3.6TB WD Purple drives
- **Used Space:** 800GB (22%)
- **Primary Use:** Container backups
- **Protocols:** SMB/CIFS, NFS
- **Network:** 192.168.10.15 via 1Gbps

### Storage Performance
- **NVMe (local-lvm):** 3000+ IOPS, <1ms latency
- **ZFS (vmpool-fast):** 2000+ IOPS, ARC hit ratio 95%+
- **NAS (kinmoon-nfs):** 100MB/s sequential, 10ms latency

---

## 🔄 Backup Status

### Automated Backup Jobs

| Job | Last Run | Status | Next Run | Retention |
|-----|----------|--------|----------|-----------|
| Small Containers | 02:00 Today | ✅ Success | 02:00 Tomorrow | 7d/4w/2m |
| Large Containers | 02:30 Today | ✅ Success | 02:30 Tomorrow | 7d/4w/2m |

### Backup Details

#### Small Container Job (9 containers)
- **Containers:** 201, 202, 203, 204, 205, 206, 207, 214, 300
- **Total Size:** 45GB compressed
- **Storage:** kinmoon-nfs (NAS)
- **Duration:** ~15 minutes
- **Compression:** LZ4

#### Large Container Job (2 containers)  
- **Containers:** 220 (Nextcloud), 302 (Gaming)
- **Total Size:** 85GB compressed
- **Storage:** data-storage (Local HDD)
- **Duration:** ~30 minutes
- **Compression:** LZ4

### Backup Health Metrics
- **Success Rate:** 100% (last 30 days)
- **Average Duration:** 22 minutes
- **Storage Usage:** 130GB total across both jobs
- **Recovery Test:** Last performed March 15, 2026

---

## 🔌 External Access Configuration

### Public Services (Cloudflare)

| Service | Subdomain | Container | Access Method |
|---------|-----------|-----------|---------------|
| Grafana | grafana.najhin-gaming.com | CT 203 | Cloudflare Access (email OTP) |
| n8n | n8n.najhin-gaming.com | CT 211 | Cloudflare Access (email OTP) |
| Vault | vault.najhin-gaming.com | CT 213 | Cloudflare Access (email OTP) |
| Vaultwarden | passwords.najhin-gaming.com | CT 214 | Cloudflare Access (email OTP) |
| Homepage | home.najhin-gaming.com | CT 208 | Direct (no auth) |
| Nextcloud | cloud.najhin-gaming.com | CT 220 | Cloudflare Tunnel |
| Terraria | terraria.najhin-gaming.com | CT 302 | Direct game port |
| Minecraft | mc.najhin-gaming.com | CT 302 | Direct game port |

### Access Control Status
- **Cloudflare Access:** 7 applications protected
- **Session Duration:** 30 days with device trust
- **MFA:** Email OTP for all protected services
- **Tunnel Health:** ✅ Connected, 15ms latency

### Internal Services (Tailscale VPN Only)
- **Proxmox Web:** https://192.168.10.5:8006
- **pfSense Admin:** https://192.168.10.1
- **NPM Admin:** http://192.168.30.201:81
- **Pi-hole Admin:** http://192.168.30.10/admin

---

## 📊 Monitoring & Alerting

### Prometheus Targets (20+ active)

| Target | Status | Last Scrape | Metrics |
|--------|--------|-------------|---------|
| Proxmox Node | ✅ Up | 30s | System metrics |
| pfSense | ✅ Up | 60s | Network metrics |
| All 14 Containers | ✅ Up | 30s | cAdvisor metrics |
| Pi-hole | ✅ Up | 30s | DNS metrics |
| Cloudflare | ✅ Up | 300s | Zone metrics |

### Active Alerts (Alertmanager)

| Alert | Status | Severity | Duration |
|-------|--------|----------|----------|
| All Clear | ✅ | — | — |

### Alert Routing Configuration
- **Critical:** Telegram to personal chat
- **Warning:** Discord webhook to #alerts channel
- **Info:** Grafana dashboard annotations only

### Grafana Dashboard Status
- **Dashboards:** 15 active
- **Panels:** 200+ across all dashboards
- **Queries/Day:** 500+
- **Data Sources:** Prometheus, Loki, TestData

### Key Dashboards
1. **Infrastructure Overview** — Node health, resource usage
2. **Network Monitoring** — VLAN traffic, firewall stats
3. **Container Resources** — CPU, RAM, disk per container
4. **Gaming Servers** — Player count, server performance
5. **Backup Monitoring** — Job status, storage usage

---

## 🤖 Automation Status

### Gilgamesh AI Agent (Telegram)
- **Bot:** @JhinGilgamesh_bot
- **Status:** ✅ Active
- **Daily Interactions:** 50+ messages
- **Memory:** 20 message conversation buffer
- **Cost Tracking:** $15 estimated April usage
- **Last /update:** April 24, 2026

### n8n Workflows (7 active)

| Workflow | Status | Executions/Day | Success Rate |
|----------|--------|----------------|--------------|
| Telegram Agent | ✅ Active | 50+ | 99% |
| Doc Pipeline - Update | ✅ Active | 1-2 | 100% |
| Doc Pipeline - Sync Docs | ✅ Active | 0-1 | 100% |
| Update Nextcloud File | 🚫 Unpublished | — | — |
| Push to GitHub | 🚫 Unpublished | — | — |
| Test SQLite | 🗑️ To Delete | — | — |

### Automation Health
- **n8n Uptime:** 99.8% (last 30 days)
- **Webhook Endpoints:** 2 active (doc-update, doc-sync)
- **API Rate Limits:** Claude API at 60% of limit
- **Error Rate:** <1% across all workflows

---

## 🎮 Gaming Platform Status

### Active Game Servers (Docker on CT 302)

| Server | Image | Status | Players | Memory | Ports |
|--------|-------|--------|---------|--------|-------|
| Terraria | ryshe/terraria | ✅ Running | 0/8 | 512MB | 7777 |
| Minecraft | itzg/minecraft-server | ✅ Running | 0/20 | 2GB | 25565 |
| Windrose | indifferentbroccoli/windrose-server-docker | ✅ Running | 0/4 | 1GB | 27015 |

### Gaming Infrastructure
- **Panel:** Pterodactyl at http://192.168.30.210
- **Node:** Wings daemon on CT 302
- **Storage:** /opt/windrose for dedicated server
- **Monitoring:** Game server status in Grafana
- **Invite Code:** NAJHINWINDROSE (Medium difficulty)

### Resource Usage (CT 302)
- **CPU:** 2.1/4.0 cores (52%)
- **RAM:** 3.2/4.0 GB (80%)
- **Disk:** 35/50 GB (70%)
- **Network:** 10Mbps avg, 1Gbps peak

---

## 🔐 Security Posture

### Access Control Status
- **Tailscale VPN:** ✅ Connected, 8 devices
- **Cloudflare Access:** ✅ Active on 7 apps
- **Pi-hole Blocking:** 489,432 domains
- **Firewall Rules:** 25 active rules, 0 violations

### Secrets Management
- **HashiCorp Vault:** ✅ Unsealed, 8 secret paths
- **Vaultwarden:** ✅ Active, 200+ entries synced
- **SSH Keys:** Managed, no password auth except CT 302

### Security Metrics (Last 24h)
- **Blocked DNS Queries:** 15,420 (23% of total)
- **Firewall Drops:** 1,250 packets
- **Failed SSH Attempts:** 0 (VPN-only access)
- **SSL Certificate Expiry:** All valid 90+ days

### Vulnerability Status
- **Container Images:** Updated within 7 days
- **Proxmox:** Latest stable (8.1.4)
- **pfSense:** Latest stable (2.7.2)
- **Critical CVEs:** 0 unpatched

---

## 📈 Performance Metrics (Last 24h)

### Resource Utilization
- **CPU Usage:** 35% average, 65% peak
- **Memory Usage:** 18.5/32GB (58%)
- **Storage I/O:** 150 IOPS average
- **Network:** 50Mbps average, 200Mbps peak

### Service Response Times
- **Homepage:** 150ms average
- **Grafana:** 300ms average
- **n8n:** 250ms average
- **Nextcloud:** 500ms average

### Uptime Statistics
- **Infrastructure:** 99.95% (last 30 days)
- **Gaming Servers:** 99.8% (last 30 days)
- **External Services:** 99.9% (last 30 days)

---

## 🎯 Next Session Priorities

### Immediate Tasks
1. **Fix cost_usd column** in Gilgamesh costs table
2. **Store new passwords** in Vaultwarden (Proxmox root, CT 302 root)
3. **Share Windrose invite code** with gaming friends
4. **Monitor CT 302 RAM usage** with Windrose active

### Upcoming Phases
1. **MERLIN Agent** — Critical reminder system
2. **Phase 22** — Obsidian Knowledge Base installation
3. **Phase 38** — Ollama + ROCm local LLM
4. **Phase 16.3** — Monthly infrastructure audit cron

---

*This snapshot is automatically updated via the Gilgamesh documentation pipeline and represents the live state as of the timestamp above.*