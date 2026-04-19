# 📸 Current State Snapshot

> **Generated:** December 19, 2024 at 23:45 UTC
> **Proxmox Host:** Kuromoon (muzakkir node)
> **Network Status:** All systems operational

## 🖥️ Proxmox Host Status

### System Information
- **Hostname:** muzakkir (Proxmox node name)
- **Hardware:** Kuromoon - Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB
- **Proxmox Version:** VE 8.2
- **Uptime:** [Live query required]
- **Load Average:** [Live query required]

### Resource Allocation
- **CPU Usage:** [Live query required]
- **Memory Usage:** [Live query required]
- **Storage Usage:** [Live query required]

### Temperature Monitoring
- **CPU Temp:** Baseline 48.5°C (idle)
- **GPU Edge Temp:** Baseline 46°C (idle)
- **GPU Power:** 5W (zero-RPM fan mode)

## 📦 Container Inventory (14 Active LXC)

### Running Containers

| CTID | Container Name          | IP Address      | Status    | CPU Cores | Memory | Storage | Autostart |
|------|-------------------------|-----------------|-----------|-----------|--------|---------|-----------|
| 201  | nginx-proxy-manager     | 192.168.30.201  | ✅ Running | 1         | 512MB  | 8GB     | ✅ Yes    |
| 202  | monitoring-prometheus   | 192.168.30.202  | ✅ Running | 2         | 2GB    | 20GB    | ✅ Yes    |
| 203  | monitoring-grafana      | 192.168.30.203  | ✅ Running | 1         | 1GB    | 10GB    | ✅ Yes    |
| 204  | monitoring-loki         | 192.168.30.204  | ✅ Running | 1         | 1GB    | 15GB    | ✅ Yes    |
| 205  | monitoring-alertmanager | 192.168.30.205  | ✅ Running | 1         | 512MB  | 5GB     | ✅ Yes    |
| 206  | monitoring-uptime       | 192.168.30.206  | ✅ Running | 1         | 512MB  | 5GB     | ✅ Yes    |
| 207  | network-ddns            | 192.168.30.207  | ✅ Running | 1         | 256MB  | 3GB     | ✅ Yes    |
| 208  | dashboard-homepage      | 192.168.30.208  | ✅ Running | 1         | 512MB  | 5GB     | ✅ Yes    |
| 211  | automation-n8n          | 192.168.30.211  | ✅ Running | 2         | 2GB    | 10GB    | ✅ Yes    |
| 213  | vault                   | 192.168.30.213  | ✅ Running | 1         | 512MB  | 8GB     | ✅ Yes    |
| 214  | password-vaultwarden    | 192.168.30.214  | ✅ Running | 1         | 512MB  | 8GB     | ✅ Yes    |
| 220  | nextcloud-hub           | 192.168.30.220  | ✅ Running | 4         | 4GB    | 50GB    | ✅ Yes    |
| 300  | gaming-panel            | 192.168.30.210  | ✅ Running | 2         | 2GB    | 20GB    | ✅ Yes    |
| 302  | gaming-wings-1          | 192.168.30.212  | ✅ Running | 4         | 4GB    | 30GB    | ✅ Yes    |

**Total Resource Allocation:**
- **CPU Cores:** 23 vCPUs allocated
- **Memory:** 18.25GB allocated (57% of 32GB)
- **Storage:** 197GB allocated across containers

### Container Network Configuration
- **VLAN:** All containers on VLAN 30 (Services)
- **Subnet:** 192.168.30.0/24
- **Gateway:** 192.168.30.1 (pfSense)
- **Bridge:** vmbr0 with VLAN tags

## 🌐 Network Topology

### Physical Network
```
Internet → ISP Router → pfSense → Managed Switch → Devices
                           ↓
                     VLAN Segmentation
```

### VLAN Status

| VLAN ID | Name            | Subnet          | Active Devices | Status |
|---------|-----------------|-----------------|----------------|--------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 3              | ✅ Active |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 2              | ✅ Active |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 15             | ✅ Active |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 0              | 🔧 Configured |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 0              | 🔧 Configured |

### Network Device Status

| Device Type      | Hostname/Model        | IP Address     | VLAN | Status |
|------------------|-----------------------|----------------|------|--------|
| Proxmox Host     | Kuromoon             | 192.168.10.5   | 10   | ✅ Online |
| pfSense Router   | —                    | 192.168.10.1   | 10   | ✅ Online |
| NAS              | Kinmoon              | 192.168.10.15  | 10   | ✅ Online |
| Managed Switch   | TL-SG108E            | 192.168.1.20   | —    | ✅ Online |
| DNS Server       | Pi-hole RPi4         | 192.168.30.10  | 30   | ✅ Online |
| Gaming PC        | Minimoon             | 192.168.20.101 | 20   | 🔧 Idle   |

### Inter-VLAN Rules Status
- **VLAN 20 → VLAN 10:** ❌ Blocked (Admin isolation)
- **VLAN 30 → VLAN 10:** ✅ Allow NAS, DNS
- **VLAN 30 → Internet:** ✅ Allow
- **VLAN 40/50:** 🔧 Configured, unused

## 💾 Storage Pools

### Proxmox Storage Configuration

| Storage Name | Type      | Location           | Size   | Used   | Available | Usage |
|--------------|-----------|-------------------|--------|--------|-----------|-------|
| local-lvm    | LVM-Thin  | Local NVMe        | 137GB  | [Live] | [Live]    | Root disks |
| vmpool-fast  | ZFS       | Local NVMe        | 899GB  | [Live] | [Live]    | Fast storage |
| data-storage | Directory | Local HDD         | 7.2TB  | [Live] | [Live]    | Large backups |
| kinmoon-smb  | SMB/CIFS  | NAS (Kinmoon)     | 3.6TB  | [Live] | [Live]    | Small backups |

### Storage Allocation by Container

| Container | Storage Pool | Allocated | Usage Pattern |
|-----------|--------------|-----------|---------------|
| CT 201-211 | local-lvm   | 8GB each  | System files |
| CT 220    | vmpool-fast  | 50GB      | Nextcloud data |
| CT 302    | vmpool-fast  | 30GB      | Game servers |
| CT 213-214| local-lvm    | 8GB each  | Security services |

## 🔄 Backup Status

### Daily Backup Jobs

| Job Name         | Schedule | Target Storage | Containers | Last Run | Status |
|------------------|----------|----------------|------------|----------|--------|
| Small Containers | 02:00    | kinmoon-smb    | 201-211, 214, 300 | [Live] | [Status] |
| Large Containers | 02:30    | data-storage   | 220, 302   | [Live] | [Status] |

### Backup Retention Policy
- **Daily:** 7 backups retained
- **Weekly:** 4 backups retained  
- **Monthly:** 2 backups retained

### Recent Backup Sizes
- **Small containers:** ~500MB - 2GB each
- **Nextcloud (CT 220):** ~15GB
- **Gaming (CT 302):** ~8GB

## 🌍 External Access Configuration

### Public Services (Cloudflare DNS)

| Service | Subdomain | Container | Port | Auth Method |
|---------|-----------|-----------|------|-------------|
| Grafana | grafana.najhin-gaming.com | CT 203 | 3000 | Cloudflare Access |
| Homepage | home.najhin-gaming.com | CT 208 | 3000 | Public |
| n8n | n8n.najhin-gaming.com | CT 211 | 5678 | Cloudflare Access |
| Vault | vault.najhin-gaming.com | CT 213 | 8200 | Cloudflare Access |
| Vaultwarden | passwords.najhin-gaming.com | CT 214 | 80 | Cloudflare Access |
| Nextcloud | cloud.najhin-gaming.com | CT 220 | 443 | Cloudflare Tunnel |
| Terraria | terraria.najhin-gaming.com | CT 302 | 7777 | Public |
| Minecraft | mc.najhin-gaming.com | CT 302 | 25565 | Public |

### Cloudflare Access Status
- **Protected Apps:** 7 (Grafana, n8n, Vault, Vaultwarden, Homepage pending)
- **Auth Method:** Email OTP verification
- **Session Duration:** 24 hours

### VPN Access
- **Tailscale Status:** ✅ Active on pfSense
- **Subnet Router:** 192.168.0.0/16 advertised
- **Connected Devices:** [Live query required]

## 📊 Monitoring & Alerting Status

### Prometheus Targets

| Target | Endpoint | Status | Last Scrape |
|--------|----------|--------|-------------|
| Proxmox Host | 192.168.10.5:9100 | [Live] | [Live] |
| All Containers | :9100 (node_exporter) | [Live] | [Live] |
| pfSense | 192.168.10.1:9100 | [Live] | [Live] |
| Pi-hole | 192.168.30.10:9617 | [Live] | [Live] |

### Active Alerts
- **Current Firing:** [Live query required]
- **Alert Channels:** 
  - Telegram: Critical alerts
  - Discord: Warning alerts

### Uptime Kuma Monitors
- **HTTP Services:** 8 services monitored
- **TCP Services:** 4 services monitored
- **Overall Uptime:** [Live query required]

## 🤖 AI Agent Status

### Gilgamesh Telegram Bot
- **Bot Username:** @JhinGilgamesh_bot
- **Container:** CT 211 (automation-n8n)
- **Status:** ✅ Active
- **Last Activity:** [Live query required]

### Features Status
- **Conversation Memory:** ✅ 20 messages stored
- **Smart Model Routing:** ✅ Haiku/Sonnet selection
- **Web Search:** ✅ Real-time via Claude
- **Homelab Control:** ✅ Proxmox API integration
- **Inline Menu:** 🔄 Partial (Main + Status working)

### API Integrations
- **Claude API:** ✅ Active (Haiku 4.5, Sonnet 4)
- **Proxmox API:** ✅ Active (root@pam!gilgamesh)
- **GitHub API:** ✅ Active (fine-grained PAT)
- **Telegram API:** ✅ Active (webhook mode)

## 🎮 Gaming Platform Status

### Game Servers (Docker on CT 302)

| Game Server | Port | Status | Players | World/Map |
|-------------|------|--------|---------|-----------|
| Terraria | 7777 | ✅ Running | 0/8 | [World Name] |
| Minecraft | 25565 | ✅ Running | 0/20 | [World Name] |
| Windrose | [Internal] | ✅ Running | 0/4 | Medium difficulty |

### Pterodactyl Panel (CT 300)
- **Panel Status:** ✅ Running
- **Wings Nodes:** 1 active (CT 302)
- **Resource Usage:** [Live query required]

## 🔐 Security Services Status

### HashiCorp Vault (CT 213)
- **Vault Status:** ✅ Unsealed
- **Secrets Paths:** 8 active (kv/*)
- **Authentication:** Token-based
- **Storage Backend:** File system

### Vaultwarden (CT 214)
- **Service Status:** ✅ Running
- **User Accounts:** 1 (personal)
- **Vault Items:** ~23 entries
- **2FA Status:** [Configuration required]

### DNS Filtering (Pi-hole)
- **Blocked Domains:** ~489,000
- **Queries Today:** [Live query required]
- **Blocked Percentage:** [Live query required]

## 📈 Performance Metrics (Live)

### Resource Utilization
- **Host CPU Usage:** [Prometheus query required]
- **Host Memory Usage:** [Prometheus query required]
- **Network Traffic:** [Prometheus query required]
- **Disk I/O:** [Prometheus query required]

### Service Response Times
- **Homepage:** [Uptime Kuma data]
- **Grafana:** [Uptime Kuma data]
- **Nextcloud:** [Uptime Kuma data]

## 🚨 Recent Issues & Resolutions

### Last 24 Hours
- **No critical issues reported**
- **Backup jobs:** All successful
- **Service availability:** 100%

### Pending Maintenance
- **Container updates:** Available for [list]
- **Proxmox updates:** [Check required]
- **Certificate renewals:** [Check expiry]

---

**Note:** This snapshot represents the current state. Live metrics require real-time Prometheus queries. Container resource usage and performance data are available through Grafana dashboards at grafana.najhin-gaming.com.