# 📸 Current Infrastructure State

> **Snapshot Date:** May 9, 2026  
> **System Status:** ✅ All services operational  
> **Last Hardware Change:** May 9, 2026 (motherboard recovery + RAM order)  
> **Active Containers:** 14 LXC + 1 KVM VM

---

## 🖥️ Physical Infrastructure

### Kuromoon (Proxmox Host)
```
Hostname: kuromoon.local
IP Address: 192.168.10.5
Proxmox VE: 8.2.x
Motherboard: ASUS TUF B550M-E (mATX, 4 DIMM slots)
CPU: AMD Ryzen 5 5600X (6C/12T)
RAM: 32GB DDR4 (128GB ordered - 4x32GB Corsair Vengeance arriving June 2026)
GPU: AMD Radeon RX 6700 XT 12GB (passed to VM 400)
Storage:
  - local-lvm: 137GB (NVMe, container root disks)
  - vmpool-fast: 899GB (ZFS, fast container storage)
  - data-storage: 7.2TB (HDD, large container backups)
IOMMU: Enabled
AMD-V (SVM): Enabled
Uptime: Stable since motherboard recovery
```

### Kinmoon NAS
```
Model: UGREEN DXP2800 (2-bay)
IP Address: 192.168.10.100
Configuration: RAID 1 (mirrored)
Total Capacity: 5.4TB
Usable Capacity: 2.6TB
Filesystem: ext4
NFS Export: /volume1/proxmox-backups
Status: Normal (drives synced)
Purpose: Proxmox backup storage
```

### Network Infrastructure
```
pfSense Firewall: 192.168.10.1 (AC8F Mini PC, Intel N100)
Managed Switch: 192.168.1.20 (TP-Link TL-SG108E) - pending migration to 192.168.10.20
Pi-hole DNS: 192.168.30.10 (Raspberry Pi 4) - ~489K domains blocked
Gaming PC: 192.168.20.101 (Minimoon) - NEVER used for homelab
WiFi AP: Purchased (TP-Link EAP610) - pending deployment
```

---

## 📦 Container Status (15 Total)

### VLAN 30 Services (192.168.30.0/24)

| CTID | Type | Name                    | IP             | RAM Usage | CPU % | Disk Usage | Status    |
|------|------|-------------------------|----------------|-----------|-------|------------|-----------|
| 201  | LXC  | nginx-proxy-manager     | 192.168.30.201 | 512MB     | <5%   | 8.2GB      | ✅ Running |
| 202  | LXC  | monitoring-prometheus   | 192.168.30.202 | 2.1GB     | 12%   | 12.5GB     | ✅ Running |
| 203  | LXC  | monitoring-grafana      | 192.168.30.203 | 256MB     | <5%   | 3.8GB      | ✅ Running |
| 204  | LXC  | monitoring-loki         | 192.168.30.204 | 512MB     | 8%    | 4.2GB      | ✅ Running |
| 205  | LXC  | monitoring-alertmanager | 192.168.30.205 | 128MB     | <2%   | 1.1GB      | ✅ Running |
| 206  | LXC  | monitoring-uptime       | 192.168.30.206 | 256MB     | <5%   | 2.3GB      | ✅ Running |
| 207  | LXC  | network-ddns            | 192.168.30.207 | 128MB     | <2%   | 0.8GB      | ✅ Running |
| 208  | LXC  | dashboard-homepage      | 192.168.30.208 | 256MB     | <5%   | 1.5GB      | ✅ Running |
| 211  | LXC  | automation-n8n          | 192.168.30.211 | 1.2GB     | 15%   | 8.7GB      | ✅ Running |
| 213  | LXC  | vault                   | 192.168.30.213 | 512MB     | <5%   | 2.1GB      | 🔒 Sealed |
| 214  | LXC  | password-vaultwarden    | 192.168.30.214 | 256MB     | <2%   | 1.8GB      | ✅ Running |
| 220  | LXC  | nextcloud-hub           | 192.168.30.220 | 1.8GB     | 18%   | 95GB       | ✅ Running |
| 300  | LXC  | gaming-panel            | 192.168.30.210 | 512MB     | 8%    | 6.4GB      | ✅ Running |
| 302  | LXC  | gaming-wings-1          | 192.168.30.212 | 9.2GB     | 25%   | 12GB       | ✅ Running |
| 400  | VM   | ollama-gpu              | 192.168.30.221 | 7.9GB     | 20%   | 45GB       | ✅ Running |

**Total Resource Usage:**
- **RAM:** 25.1GB / 32GB (78% utilization)
- **CPU:** Average 15% across all containers
- **Storage:** 216GB used across all storage pools

**High Resource Consumers:**
- gaming-wings-1 (CT 302): 9.2GB RAM (Windrose + game servers)
- ollama-gpu (VM 400): 7.9GB RAM (AI models + Open WebUI)
- monitoring-prometheus (CT 202): 2.1GB RAM (metrics storage)

---

## 🌐 Network Topology

### VLAN Configuration (Active)
```
VLAN 10 (MGMT): 192.168.10.0/24
├── Kuromoon: 192.168.10.5
├── pfSense: 192.168.10.1
└── Kinmoon NAS: 192.168.10.100

VLAN 20 (MAIN): 192.168.20.0/24
└── Minimoon Gaming PC: 192.168.20.101

VLAN 30 (SERVICES): 192.168.30.0/24
├── Pi-hole: 192.168.30.10
└── All 14 LXC containers + VM 400

VLAN 40 (DMZ): 192.168.40.0/24 [Unused]
VLAN 50 (MALWARE): 192.168.50.0/24 [Unused]
```

### Bridge Configuration
```
vmbr0: 192.168.10.5/24 (Management)
├── Bridge-VIDS: 2-4094
├── VLAN-aware: Yes
└── Physical Interface: enp5s0
```

### External Access
```
Domain: najhin-gaming.com (Cloudflare managed)
Tunnel: Cloudflare Zero Trust
Access Control: Email OTP (muzakkir.kholil06@gmail.com only)
Services Exposed: 7 total
├── grafana.najhin-gaming.com
├── n8n.najhin-gaming.com
├── vault.najhin-gaming.com
├── passwords.najhin-gaming.com
├── home.najhin-gaming.com
├── cloud.najhin-gaming.com
└── ollama.najhin-gaming.com
```

---

## 💾 Storage Utilization

### Storage Pools
```
local-lvm (LVM-Thin): 137GB total
├── Used: 89GB (65%)
├── Available: 48GB
└── Purpose: Container root filesystems

vmpool-fast (ZFS): 899GB total
├── Used: 127GB (14%)
├── Available: 772GB
└── Purpose: High-performance container storage

data-storage (Local): 7.2TB total
├── Used: 2.1TB (29%)
├── Available: 5.1TB
└── Purpose: Large file storage, backups

kinmoon-nfs (NAS): 2.6TB total
├── Used: 89GB (3%)
├── Available: 2.5TB
└── Purpose: Automated backup storage
```

### Backup Status
```
Last Backup: May 9, 2026 02:00 GMT+8
Small Containers Job: ✅ Success (9 containers, 1.2GB)
Large Containers Job: ✅ Success (2 containers, 5.7GB)
Total Backup Size: 6.9GB
Retention: 7 daily, 4 weekly, 2 monthly
Next Backup: May 10, 2026 02:00 GMT+8
```

---

## 🔍 Monitoring & Health

### Prometheus Targets
```
✅ node-exporter (Kuromoon): UP
✅ prometheus (CT 202): UP
✅ grafana (CT 203): UP
✅ loki (CT 204): UP
✅ alertmanager (CT 205): UP
✅ uptime-kuma (CT 206): UP
✅ n8n (CT 211): UP
```

### Active Alerts
```
Current Status: 🟢 All Clear
Last Alert: None (system stable)
Alert Threshold: Memory usage >85% (raised from 80%)
Next Check: Continuous monitoring
```

### System Performance
```
Host CPU Usage: 35% average (Ryzen 5 5600X)
Host Memory: 25.1GB / 32GB (78%)
Host Temperature: 48.5°C idle
GPU Temperature: 46°C idle (RX 6700 XT)
Network Throughput: <100Mbps typical
Disk I/O: Normal levels across all pools
```

---

## 🤖 Active Automation

### n8n Workflows (12 Active)
```
✅ Telegram Agent (Gilgamesh main bot)
✅ Documentation Pipeline — Update (3 files)
✅ Documentation Pipeline — Sync Docs (7 files)
✅ Da Vinci Documentation Pipeline (async processing)
✅ Midas — CFO Report (/midas command)
✅ Midas — Daily Brief (9am scheduled)
✅ MERLIN — Reminders (8am daily checks)
✅ Daily Note Creator (midnight Obsidian)
✅ Morning Briefing (7am daily summary)
✅ Update Nextcloud File (legacy)
✅ Push to GitHub (legacy)
```

### Scheduled Tasks
```
00:00 Daily: Create Obsidian daily note
02:00 Daily: Small container backups
02:30 Daily: Large container backups
07:00 Daily: Morning briefing (MERLIN)
08:00 Daily: Infrastructure checks (MERLIN)
09:00 Daily: Cost analysis brief (Midas)
```

### AI Agents Status
```
✅ Gilgamesh: Active (Telegram @JhinGilgamesh_bot)
✅ Midas: Active (cost tracking, daily briefs)
✅ MERLIN: Active (reminders, health checks)
⚡ Da Vinci: Partial (Stage 1 only, Stage 2 pending)
📋 Guardian: Planned (security monitoring)
📋 EMIYA: Planned (infrastructure automation)
```

---

## 🔐 Security Status

### Vault Status
```
Container: CT 213 (vault.najhin-gaming.com)
Status: 🔒 SEALED (requires manual unseal after reboot)
KV Secrets: 8 paths configured
├── kv/gilgamesh (API keys)
├── kv/cloudflare (tunnel token - needs fix)
├── kv/proxmox (API credentials)
├── kv/alertmanager (webhook URLs)
├── kv/github (repository access)
├── kv/nextcloud (admin credentials)
├── kv/n8n (service passwords)
└── kv/pihole (admin password)
```

### Access Control
```
Cloudflare Access: 7 applications protected
Email OTP: muzakkir.kholil06@gmail.com only
Tailscale VPN: Active mesh network
Local Firewall: pfSense with VLAN rules
DNS Filtering: Pi-hole blocking ~489K domains
```

---

## ⚠️ Known Issues & Pending

### High Priority
```
🔒 Vault sealed - requires manual unseal: pct exec 213 -- vault operator unseal
🔧 Cloudflare token truncated in Vault - needs full token re-storage
📊 MERLIN workflow inactive - toggle on after testing
🧠 High RAM usage (78%) - 128GB upgrade ordered
```

### Medium Priority
```
🌡️ Homelab → Temps SSH bug in Gilgamesh menu
📍 Switch management IP migration (192.168.1.20 → 192.168.10.20)
💾 Backup restore testing overdue (last test: never)
🗂️ Temp backups cleanup (saves 6.9GB storage)
```

### Planned Upgrades
```
📦 128GB DDR4 RAM (4x32GB Corsair) - shipped from Taobao
📶 WiFi AP deployment (TP-Link EAP610 purchased)
🔧 Homepage dashboard widgets (Phase 22.8C)
🛡️ Guardian security agent (after 22.8C)
```

---

## 📊 Performance Metrics (Last 24h)

### Resource Trends
```
Average CPU: 35% (normal load)
Peak Memory: 82% (during Windrose active)
Network Peak: 150Mbps (game server downloads)
Disk I/O: <50% utilization
GPU Utilization: 15% average (AI inference)
```

### Service Availability
```
Uptime: 99.9% (all services)
Last Restart: May 9 (planned maintenance)
Failed Services: 0
Performance Issues: None
```

### Backup Health
```
Success Rate: 100% (last 7 days)
Average Backup Time: 45 minutes
Restore Test Status: ⚠️ Overdue
Storage Growth: +2GB/week
```

---

*Current state captured: May 9, 2026 23:45 GMT+8*  
*Next state update: After Phase 22.8C completion*