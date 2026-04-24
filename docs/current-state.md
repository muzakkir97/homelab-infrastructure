# 📸 Current Infrastructure State — Live Snapshot

> **Snapshot Date:** April 24, 2026
> **Purpose:** Real-time status of all running infrastructure
> **Proxmox Node:** muzakkir (Kuromoon)

---

## 🏃 Running Containers (14 LXC)

### All Containers Status
All containers are **RUNNING** with autostart enabled on VLAN 30 (Services).

| CTID | Container Name          | IP Address      | CPU Cores | RAM (MB) | Storage (GB) | Template          |
|------|-------------------------|-----------------|-----------|----------|--------------|-------------------|
| 201  | nginx-proxy-manager     | 192.168.30.201  | 2         | 1024     | 8            | debian-12-standard |
| 202  | monitoring-prometheus   | 192.168.30.202  | 2         | 2048     | 20           | debian-12-standard |
| 203  | monitoring-grafana      | 192.168.30.203  | 2         | 1024     | 8            | debian-12-standard |
| 204  | monitoring-loki         | 192.168.30.204  | 2         | 1024     | 20           | debian-12-standard |
| 205  | monitoring-alertmanager | 192.168.30.205  | 1         | 512      | 8            | debian-12-standard |
| 206  | monitoring-uptime       | 192.168.30.206  | 1         | 512      | 8            | debian-12-standard |
| 207  | network-ddns            | 192.168.30.207  | 1         | 256      | 4            | debian-12-standard |
| 208  | dashboard-homepage      | 192.168.30.208  | 1         | 512      | 4            | debian-12-standard |
| 211  | automation-n8n          | 192.168.30.211  | 2         | 2048     | 20           | debian-12-standard |
| 213  | vault                   | 192.168.30.213  | 2         | 1024     | 10           | debian-12-standard |
| 214  | password-vaultwarden    | 192.168.30.214  | 1         | 512      | 8            | debian-12-standard |
| 220  | nextcloud-hub           | 192.168.30.220  | 4         | 4096     | 100          | debian-12-standard |
| 300  | gaming-panel            | 192.168.30.210  | 2         | 2048     | 30           | debian-12-standard |
| 302  | gaming-wings-1          | 192.168.30.212  | 4         | 4096     | 50           | debian-12-standard |

### Resource Allocation Summary
- **Total CPU Cores Allocated:** 28 cores
- **Total RAM Allocated:** 20.5 GB
- **Total Storage Allocated:** 326 GB
- **Host Available:** Ryzen 5 5600X (6C/12T), 32GB RAM

---

## 🌐 Network Topology (Live)

### VLAN Assignments (Active)

| VLAN | Name            | Subnet              | Active Devices | Gateway        |
|------|-----------------|---------------------|----------------|----------------|
| 10   | VLAN10_MGMT     | 192.168.10.0/24     | 3              | 192.168.10.1   |
| 20   | VLAN20_MAIN     | 192.168.20.0/24     | 2              | 192.168.20.1   |
| 30   | VLAN30_SERVICES | 192.168.30.0/24     | 15             | 192.168.30.1   |
| 40   | VLAN40_DMZ      | 192.168.40.0/24     | 0              | 192.168.40.1   |
| 50   | VLAN50_MALWARE  | 192.168.50.0/24     | 0              | 192.168.50.1   |

### Active Network Devices

| Device              | IP Address     | VLAN | Status    | Role                    |
|---------------------|----------------|------|-----------|-------------------------|
| pfSense (Kuromoon)  | 192.168.10.1   | 10   | ✅ Online | Firewall/Router         |
| Proxmox (Kuromoon)  | 192.168.10.5   | 10   | ✅ Online | Hypervisor              |
| NAS (Kinmoon)       | 192.168.10.15  | 10   | ✅ Online | Backup Storage          |
| Managed Switch      | 192.168.1.20   | 1    | ✅ Online | Layer 2 Switching       |
| Gaming PC (Minimoon)| 192.168.20.101 | 20   | ✅ Online | Gaming Only             |
| Pi-hole DNS         | 192.168.30.10  | 30   | ✅ Online | DNS Filtering           |

---

## 💽 Storage Pools (Live Status)

### Proxmox Storage Overview

| Storage ID    | Type     | Protocol | Size    | Used    | Available | Usage % | Status    |
|---------------|----------|----------|---------|---------|-----------|---------|-----------|
| local-lvm     | LVM-Thin | Local    | 137 GB  | 95 GB   | 42 GB     | 69%     | ⚠️ High   |
| vmpool-fast   | ZFS      | Local    | 899 GB  | 124 GB  | 775 GB    | 14%     | ✅ Good   |
| kinmoon-nfs   | NFS      | Network  | 3.6 TB  | 1.2 TB  | 2.4 TB    | 33%     | ✅ Good   |
| data-storage  | Directory| Local    | 7.2 TB  | 956 GB  | 6.3 TB    | 13%     | ✅ Good   |

### Storage Allocation by Pool
- **local-lvm:** Container root disks (14 containers)
- **vmpool-fast:** High-performance workloads (unused)
- **kinmoon-nfs:** Small container backups (9 containers)
- **data-storage:** Large container backups (2 containers)

### Critical Storage Notes
- **local-lvm at 69%** — Thin pool overprovisioning warning active
- **Nextcloud data:** Currently on root disk (CT 220), planned migration to data-storage
- **Backup targets:** Hybrid approach working correctly

---

## 🔄 Backup Status (Live)

### Backup Jobs Status

| Job Name         | Last Run         | Next Run         | Status       | Containers Backed Up |
|------------------|------------------|------------------|--------------|---------------------|
| Small Containers | Apr 24, 02:00    | Apr 25, 02:00    | ✅ Success   | 201,202,203,204,205,206,207,214,300 |
| Large Containers | Apr 24, 02:30    | Apr 25, 02:30    | ✅ Success   | 220,302             |

### Recent Backup Performance

| Container | Last Backup    | Size (GB) | Duration | Status    | Target Storage |
|-----------|----------------|-----------|----------|-----------|----------------|
| 201       | Apr 24, 02:01  | 1.2       | 2m 15s   | ✅ Success | kinmoon-nfs    |
| 202       | Apr 24, 02:03  | 4.8       | 5m 30s   | ✅ Success | kinmoon-nfs    |
| 203       | Apr 24, 02:09  | 1.1       | 2m 10s   | ✅ Success | kinmoon-nfs    |
| 204       | Apr 24, 02:12  | 3.2       | 4m 20s   | ✅ Success | kinmoon-nfs    |
| 205       | Apr 24, 02:17  | 0.8       | 1m 45s   | ✅ Success | kinmoon-nfs    |
| 206       | Apr 24, 02:19  | 0.9       | 1m 50s   | ✅ Success | kinmoon-nfs    |
| 207       | Apr 24, 02:21  | 0.4       | 1m 20s   | ✅ Success | kinmoon-nfs    |
| 214       | Apr 24, 02:22  | 1.3       | 2m 25s   | ✅ Success | kinmoon-nfs    |
| 300       | Apr 24, 02:25  | 6.1       | 7m 15s   | ✅ Success | kinmoon-nfs    |
| 220       | Apr 24, 02:31  | 45.2      | 18m 30s  | ✅ Success | data-storage   |
| 302       | Apr 24, 02:51  | 12.8      | 8m 45s   | ✅ Success | data-storage   |

### Backup Retention Status
- **Daily backups:** 7 retained (Apr 18-24, 2026)
- **Weekly backups:** 4 retained (Mar 31, Apr 7, 14, 21)
- **Monthly backups:** 2 retained (Feb 28, Mar 31)

---

## 🌍 External Access Configuration

### Public Domain Services (Active)

| Service      | Subdomain                     | SSL Status | Auth Method        | Tunnel Type      |
|--------------|-------------------------------|------------|--------------------|------------------|
| Grafana      | grafana.najhin-gaming.com     | ✅ Valid   | Cloudflare Access  | NPM → Cloudflare |
| n8n          | n8n.najhin-gaming.com         | ✅ Valid   | Cloudflare Access  | NPM → Cloudflare |
| Vault        | vault.najhin-gaming.com       | ✅ Valid   | Cloudflare Access  | NPM → Cloudflare |
| Vaultwarden  | passwords.najhin-gaming.com   | ✅ Valid   | Cloudflare Access  | NPM → Cloudflare |
| Homepage     | home.najhin-gaming.com        | ✅ Valid   | None              | NPM → Cloudflare |
| Nextcloud    | cloud.najhin-gaming.com       | ✅ Valid   | Internal Auth     | Cloudflare Tunnel |
| Terraria     | terraria.najhin-gaming.com    | ✅ Valid   | Game Client       | NPM → Cloudflare |
| Minecraft    | mc.najhin-gaming.com          | ✅ Valid   | Game Client       | NPM → Cloudflare |

### Internal Access Methods
- **Primary:** Tailscale VPN (subnet router on pfSense)
- **Management:** Direct VLAN 20 → VLAN 30 access for authorized devices
- **Emergency:** ISP router port forwarding to pfSense (backup method)

---

## 📊 Monitoring & Alerting (Live Status)

### Active Monitoring Systems

| System          | Container | Status    | Metrics Collected | Retention |
|-----------------|-----------|-----------|-------------------|-----------|
| Prometheus      | CT 202    | ✅ Running | 847 targets       | 30 days   |
| Grafana         | CT 203    | ✅ Running | 12 dashboards     | —         |
| Loki            | CT 204    | ✅ Running | 14 log sources    | 7 days    |
| Alertmanager    | CT 205    | ✅ Running | 23 alert rules    | —         |
| Uptime Kuma     | CT 206    | ✅ Running | 18 monitors       | 30 days   |

### Current Alert Status

#### Active Alerts (2)
| Alert                    | Severity | Since      | Source     | Status       |
|--------------------------|----------|------------|------------|--------------|
| HighMemoryUsage-CT220    | Warning  | Apr 23 14:30 | Prometheus | 🟡 Acknowledged |
| DiskSpaceWarning-local-lvm | Warning  | Apr 22 09:15 | Prometheus | 🟡 Monitoring   |

#### Recent Resolved Alerts (Last 24h)
| Alert                    | Resolved   | Duration | Cause                    |
|--------------------------|------------|----------|--------------------------|
| ContainerDown-CT204      | Apr 24 08:15 | 5m      | Loki container restart   |
| NetworkLatencyHigh       | Apr 23 22:30 | 12m     | ISP temporary congestion |

### Performance Metrics (Current)

| Component        | CPU Usage | Memory Usage | Disk I/O    | Network I/O  |
|------------------|-----------|--------------|-------------|--------------|
| Kuromoon Host    | 15%       | 18.2/32 GB   | 45 MB/s     | 125 Mbps     |
| CT 220 (Nextcloud)| 8%        | 2.8/4 GB     | 12 MB/s     | 25 Mbps      |
| CT 211 (n8n)     | 3%        | 1.1/2 GB     | 2 MB/s      | 5 Mbps       |
| CT 202 (Prometheus)| 5%       | 1.6/2 GB     | 8 MB/s      | 15 Mbps      |

---

## 🔧 Service Health Status

### Core Infrastructure Services

| Service                 | Container | Status    | Uptime    | Response Time |
|-------------------------|-----------|-----------|-----------|---------------|
| Nginx Proxy Manager     | CT 201    | ✅ Healthy | 15d 8h    | 12ms          |
| Pi-hole DNS             | Pi 4      | ✅ Healthy | 18d 2h    | 8ms           |
| pfSense Firewall        | AC8F      | ✅ Healthy | 21d 6h    | 5ms           |
| Tailscale VPN           | pfSense   | ✅ Healthy | 21d 6h    | 25ms          |

### Application Services

| Service                 | Container | Status    | Uptime    | Users/Activity    |
|-------------------------|-----------|-----------|-----------|-------------------|
| Nextcloud Hub           | CT 220    | ✅ Healthy | 12d 4h    | 1 active user     |
| Vaultwarden             | CT 214    | ✅ Healthy | 6d 2h     | 1 vault           |
| HashiCorp Vault         | CT 213    | ✅ Healthy | 6d 8h     | 8 secret paths    |
| n8n Automation          | CT 211    | ✅ Healthy | 22d 1h    | 7 workflows       |
| Gilgamesh Bot           | CT 211    | ✅ Healthy | 3d 14h    | 247 messages      |

### Gaming Services

| Service                 | Container | Status    | Players Online | Uptime    |
|-------------------------|-----------|-----------|----------------|-----------|
| Pterodactyl Panel       | CT 300    | ✅ Healthy | —              | 8d 12h    |
| Terraria Server         | CT 302    | ✅ Online  | 0/8            | 2d 6h     |
| Minecraft Server        | CT 302    | ✅ Online  | 0/20           | 2d 6h     |
| Windrose Server         | CT 302    | ✅ Online  | 0/4            | 5d 10h    |

---

## 🌡️ Hardware Status (Live)

### Kuromoon (Proxmox Host)

| Component     | Current Temp | Status    | Details                    |
|---------------|--------------|-----------|----------------------------|
| CPU (5600X)   | 48.5°C       | ✅ Normal  | 6C/12T @ 3.7GHz base      |
| GPU (RX6700XT)| 46°C (edge)  | ✅ Idle    | Zero-RPM mode, 5W power    |
| Motherboard   | 42°C         | ✅ Normal  | B550 chipset              |
| RAM           | —            | ✅ Normal  | 32GB DDR4-3200             |
| NVMe SSD      | 52°C         | ✅ Normal  | Samsung 980 Pro 1TB       |

### Network Hardware

| Device          | Status    | Uptime  | Temperature | Load      |
|-----------------|-----------|---------|-------------|-----------|
| AC8F (pfSense)  | ✅ Online  | 21d 6h  | 58°C        | 12% CPU   |
| TL-SG108E Switch| ✅ Online  | 28d 14h | —           | 15% ports |
| Pi 4 (DNS)      | ✅ Online  | 18d 2h  | 51°C        | 8% CPU    |
| DXP2800 (NAS)   | ✅ Online  | 25d 1h  | 47°C        | 5% CPU    |

---

## 🔒 Security Status (Live)

### Firewall Rule Activity (Last 24h)

| Action | Count  | Top Sources        | Top Destinations   | Top Ports |
|--------|--------|--------------------|--------------------|-----------| 
| BLOCK  | 2,847  | Internet scanners  | WAN interface      | 22, 80, 443 |
| ALLOW  | 18,923 | VLAN 30 devices    | Internet           | 80, 443, 53 |
| DROP   | 456    | Malformed packets  | Various           | Various   |

### Pi-hole DNS Filtering (24h Stats)

| Metric                 | Count    | Percentage |
|------------------------|----------|------------|
| Total DNS Queries      | 12,847   | 100%       |
| Queries Blocked        | 3,621    | 28.2%      |
| Domains on Blocklist   | 489,127  | —          |
| Top Blocked Domain     | doubleclick.net | 234 blocks |

### Certificate Status

| Domain                          | Issuer           | Expires     | Auto-Renew | Status    |
|---------------------------------|------------------|-------------|------------|-----------|
| najhin-gaming.com               | Let's Encrypt    | Jun 15, 2026| ✅ Yes      | ✅ Valid   |
| *.najhin-gaming.com             | Let's Encrypt    | Jun 15, 2026| ✅ Yes      | ✅ Valid   |
| grafana.najhin-gaming.com       | Cloudflare       | Dec 1, 2026 | ✅ Yes      | ✅ Valid   |
| n8n.najhin-gaming.com           | Cloudflare       | Dec 1, 2026 | ✅ Yes      | ✅ Valid   |

### Active Security Features

| Feature                | Status      | Last Updated    | Coverage        |
|------------------------|-------------|-----------------|-----------------|
| pfSense Firewall       | ✅ Active   | Apr 24, 2026    | All traffic     |
| Pi-hole DNS Filtering  | ✅ Active   | Apr 23, 2026    | All DNS queries |
| Tailscale VPN          | ✅ Active   | Apr 20, 2026    | Remote access   |
| Cloudflare Access      | ✅ Active   | Apr 7, 2026     | 7 applications  |
| Fail2ban               | ✅ Active   | Apr 18, 2026    | SSH, web services |

---

## 📡 Integration Status (Live)

### API Integrations

| Integration    | Status     | Last Call      | Rate Limit Status | Purpose              |
|----------------|------------|----------------|-------------------|----------------------|
| Claude API     | ✅ Active  | 2 min ago      | 87/100 requests   | Gilgamesh responses  |
| Cloudflare API | ✅ Active  | 15 min ago     | Normal            | DNS updates          |
| GitHub API     | ✅ Active  | 1 hour ago     | Normal            | Doc pipeline         |
| Proxmox API    | ✅ Active  | 5 min ago      | N/A               | Infrastructure stats |
| Telegram API   | ✅ Active  | 30 sec ago     | Normal            | Bot responses        |

### Webhook Endpoints

| Webhook        | Status     | Last Triggered | Success Rate | Purpose              |
|----------------|------------|----------------|--------------|----------------------|
| doc-update     | ✅ Active  | 3 hours ago    | 98.5%        | Session summaries    |
| doc-sync       | ✅ Active  | 1 day ago      | 100%         | Full doc regeneration|
| alerts-discord | ✅ Active  | 2 hours ago    | 95.2%        | Alert notifications  |
| alerts-telegram| ✅ Active  | 2 hours ago    | 100%         | Critical alerts      |

---

## 📈 Growth Metrics (Current Period)

### Infrastructure Growth (April 2026)

| Metric                    | Start of Month | Current    | Growth    |
|---------------------------|----------------|------------|-----------|
| Active Containers         | 12             | 14         | +16.7%    |
| Total Storage Used        | 1.8 TB         | 2.1 TB     | +16.7%    |
| External Services         | 6              | 8          | +33.3%    |
| n8n Workflows            | 5              | 7          | +40%      |
| Gilgamesh Conversations   | 0              | 247        | +247      |

### Service Usage (Last 7 days)

| Service        | Total Requests | Daily Average | Growth vs Previous Week |
|----------------|----------------|---------------|------------------------|
| Nextcloud      | 2,847          | 407           | +12%                   |
| Grafana        | 1,923          | 275           | +8%                    |
| Homepage       | 1,456          | 208           | +5%                    |
| Gilgamesh Bot  | 247            | 35            | +145%                  |
| Game Servers   | 89             | 13            | +23%                   |

---

*Snapshot generated: April 24, 2026 at 21:30 MYT*
*Next automated snapshot: April 25, 2026 at 06:00 MYT*