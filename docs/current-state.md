# 📊 Current Infrastructure State

> **Snapshot Date:** April 24, 2026  
> **Purpose:** Live infrastructure status and configuration  
> **Scope:** All running services, resources, and configurations

---

## 🖥️ Hardware Status

### Proxmox Host: Kuromoon
- **Hardware:** Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB
- **IP Address:** 192.168.10.5
- **Status:** ✅ Online
- **Uptime:** [Dynamic]
- **Temperature:** CPU 48.5°C baseline, GPU 46°C edge (idle)
- **GPU Power:** 5W with zero-RPM fan (idle baseline)
- **IOMMU Groups:** 
  - GPU: Group 18 (2d:00.0)
  - Audio: Group 19 (2d:00.1)

### Network Infrastructure
- **pfSense Firewall:** AC8F Mini PC, Intel N100 @ 192.168.10.1
- **Managed Switch:** TP-Link TL-SG108E @ 192.168.1.20
- **DNS Server:** Raspberry Pi 4 @ 192.168.30.10 (~489K domains blocked)
- **NAS:** UGREEN DXP2800 @ 192.168.10.15 (3.6TB WD Purple)
- **WiFi AP:** TP-Link EAP610 (purchased, pending setup)

---

## 📦 Container Inventory (15 Total)

### Management & Infrastructure

| CTID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | 1   | 512MB | 8GB  | ✅ Running |
| 207  | network-ddns            | 192.168.30.207 | 1   | 256MB | 8GB  | ✅ Running |
| 208  | dashboard-homepage      | 192.168.30.208 | 1   | 512MB | 8GB  | ✅ Running |

### Monitoring Stack

| CTID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 202  | monitoring-prometheus   | 192.168.30.202 | 2   | 2GB   | 8GB  | ✅ Running |
| 203  | monitoring-grafana      | 192.168.30.203 | 1   | 1GB   | 8GB  | ✅ Running |
| 204  | monitoring-loki         | 192.168.30.204 | 1   | 1GB   | 8GB  | ✅ Running |
| 205  | monitoring-alertmanager | 192.168.30.205 | 1   | 512MB | 8GB  | ✅ Running |
| 206  | monitoring-uptime       | 192.168.30.206 | 1   | 512MB | 8GB  | ✅ Running |

### Security & Secrets

| CTID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 213  | vault                   | 192.168.30.213 | 1   | 512MB | 8GB  | ✅ Running |
| 214  | password-vaultwarden    | 192.168.30.214 | 1   | 512MB | 8GB  | ✅ Running |

### Productivity & Automation

| CTID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 211  | automation-n8n          | 192.168.30.211 | 2   | 2GB   | 8GB  | ✅ Running |
| 220  | nextcloud-hub           | 192.168.30.220 | 2   | 4GB   | 100GB| ✅ Running |

### Gaming Platform

| CTID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 300  | gaming-panel            | 192.168.30.210 | 2   | 2GB   | 20GB | ✅ Running |
| 302  | gaming-wings-1          | 192.168.30.212 | 4   | 8GB   | 80GB | ✅ Running |

### AI & Local LLM

| VMID | Name                    | IP             | CPU | RAM   | Disk | Status     |
|------|-------------------------|----------------|-----|-------|------|------------|
| 400  | ollama-gpu              | 192.168.30.221 | 8   | 16GB  | 60GB | ✅ Running |

**Total Resource Allocation:**
- **CPU Cores:** 28 total allocated
- **RAM:** 35.5GB total allocated  
- **Storage:** ~300GB total allocated

---

## 🌐 Network Configuration

### VLAN Assignment

| VLAN | Subnet          | Active Devices | Primary Services |
|------|-----------------|----------------|------------------|
| 10   | 192.168.10.0/24 | 3             | Kuromoon, pfSense, Kinmoon |
| 20   | 192.168.20.0/24 | 2             | Minimoon, client devices |
| 30   | 192.168.30.0/24 | 15+           | All containers, Pi-hole |
| 40   | 192.168.40.0/24 | 0             | Unused (future DMZ) |
| 50   | 192.168.50.0/24 | 0             | Unused (malware lab) |

### Active Firewall Rules
- **VLAN 30 → VLAN 10:** Allow Gilgamesh SSH (TCP 22 from 211 to Kuromoon)
- **VLAN 20 → VLAN 10:** Blocked (security isolation)
- **Internet Access:** All VLANs via pfSense NAT
- **Inter-VLAN:** Controlled via pfSense rules

### DNS Resolution
- **Primary:** Pi-hole @ 192.168.30.10
- **Forwarding:** pfSense DNS Resolver → Pi-hole
- **Filtering:** ~489,000 domains blocked
- **Custom Records:** *.najhin-gaming.com → Cloudflare

---

## 💾 Storage Pools

### Proxmox Storage Configuration

| Storage ID   | Type       | Content        | Size     | Usage | Status |
|--------------|------------|----------------|----------|-------|--------|
| local-lvm    | LVM-Thin   | Container disk | 137GB    | 85%   | ⚠️ High |
| vmpool-fast  | ZFS        | VM disk        | 899GB    | 12%   | ✅ Good |
| kinmoon-nfs  | NFS        | Backups        | 3.6TB    | 15%   | ✅ Good |
| data-storage | Local HDD  | Large backups  | 7.3TB    | 8%    | ✅ Good |

### Container Storage Details

| Container | Root Disk | Data Location | Notes |
|-----------|-----------|---------------|-------|
| CT 220 (Nextcloud) | 100GB | /var/lib/nextcloud | Recently resized from 20GB |
| CT 302 (Wings) | 80GB | Docker volumes | Game server storage |
| VM 400 (Ollama) | 60GB | Ubuntu filesystem | GPU-accelerated AI models |
| Others | 8GB each | Standard allocation | Default sizing |

---

## 🔄 Backup Status

### Daily Backup Jobs

| Job Name         | Schedule | Status        | Last Run      | Next Run      |
|------------------|----------|---------------|---------------|---------------|
| Small Containers | 02:00    | ✅ Success    | [Dynamic]     | [Dynamic]     |
| Large Containers | 02:30    | ✅ Success    | [Dynamic]     | [Dynamic]     |

### Backup Coverage

| Container Group | Storage Target | Retention | Size |
|----------------|----------------|-----------|------|
| Small (201,202,203,204,205,206,207,214,300) | kinmoon-nfs | 7/4/2 | ~2GB total |
| Large (220,302,400) | data-storage | 7/4/2 | ~50GB total |

### Recent Backup Issues
- **Resolved:** NFS permission issues (switched to SMB, then back with proper permissions)
- **Resolved:** Large container I/O errors (split to local storage)
- **Active:** Monitor thin pool usage during backups

---

## 🔐 External Access Configuration

### Cloudflare Integration

| Service | Subdomain | Access Method | Auth |
|---------|-----------|---------------|------|
| Grafana | grafana.najhin-gaming.com | Cloudflare Tunnel | Email OTP |
| n8n | n8n.najhin-gaming.com | Cloudflare Tunnel | Email OTP |
| Vault | vault.najhin-gaming.com | Cloudflare Tunnel | Email OTP |
| Vaultwarden | passwords.najhin-gaming.com | Cloudflare Tunnel | Email OTP |
| Nextcloud | cloud.najhin-gaming.com | Cloudflare Tunnel | Direct |
| Homepage | home.najhin-gaming.com | Cloudflare Tunnel | Direct |
| Ollama UI | ollama.najhin-gaming.com | Cloudflare Tunnel | Email OTP |

### Tailscale VPN
- **Subnet Router:** pfSense with subnet advertisement
- **Connected Devices:** [Dynamic count]
- **Admin Access:** Primary method for infrastructure management
- **Status:** ✅ Active

---

## 📊 Active Monitoring & Alerting

### Prometheus Targets

| Target | Endpoint | Status | Last Scrape |
|--------|----------|--------|-------------|
| Node Exporter | kuromoon:9100 | ✅ UP | [Dynamic] |
| Proxmox | 192.168.10.5:8006 | ✅ UP | [Dynamic] |
| Containers | Various ports | ✅ UP | [Dynamic] |

### Grafana Dashboards
- **Infrastructure Overview:** System metrics, container status
- **Gaming Servers:** Player counts, resource usage
- **Network Traffic:** VLAN utilization, firewall stats
- **Storage:** Disk usage, backup status

### Alert Rules Active

| Alert | Condition | Destination | Status |
|-------|-----------|-------------|--------|
| Host Down | Node unreachable >5m | Telegram | ✅ Armed |
| High CPU | >80% for 10m | Telegram | ✅ Armed |
| High Memory | >90% for 5m | Telegram | ✅ Armed |
| Disk Full | >95% usage | Telegram | ✅ Armed |
| Container Down | Service stopped | Discord | ✅ Armed |
| Backup Failed | Job error | Telegram | ✅ Armed |

---

## 🤖 Automation Status

### Gilgamesh AI Agent
- **Platform:** Telegram (@JhinGilgamesh_bot)
- **Memory:** 20 message rolling window (n8n Data Tables)
- **Commands:** 8 slash commands active
- **Menu:** Full inline keyboard system operational
- **Cost Tracking:** Token usage logged (cost calculation estimated)
- **Last Interaction:** [Dynamic]

### n8n Workflows (7 Active)

| Workflow | Purpose | Nodes | Status |
|----------|---------|-------|--------|
| Telegram Agent | Main Gilgamesh bot | 15 | ✅ Active |
| Documentation Pipeline - Update | Session summaries | 7 | ✅ Active |
| Documentation Pipeline - Sync Docs | Full doc regeneration | 7 | ✅ Active |
| Update Nextcloud File | Legacy | 5 | 📴 Unpublished |
| Push to GitHub | Legacy | 4 | 📴 Unpublished |

### Automation Services
- **DDNS Updates:** Cloudflare dynamic DNS (CT 207)
- **SSL Renewals:** Let's Encrypt via Nginx Proxy Manager
- **Backup Jobs:** Proxmox scheduled tasks
- **Health Checks:** Uptime Kuma monitoring
- **Alert Routing:** Alertmanager to Telegram/Discord

---

## 🎮 Gaming Services Status

### Pterodactyl Panel (CT 300)
- **Status:** ✅ Running
- **Wings Node:** CT 302 connected
- **Resource Usage:** [Dynamic]
- **Admin Panel:** Internal access only

### Active Game Servers (CT 302)

| Game | Status | Players | Port | Version |
|------|--------|---------|------|---------|
| Windrose | ✅ Running | [Dynamic] | 7777 | Early Access |
| Terraria | ⏸️ Stopped | 0/8 | 7778 | 1.4.4.9 |
| Minecraft | ⏸️ Stopped | 0/20 | 25565 | Latest |

### Gaming Server Details
- **Windrose:** 4 max players, Medium difficulty, invite: NAJHINWINDROSE
- **Resource Monitoring:** RAM usage tracking with Windrose active
- **Auto-shutdown:** Idle server shutdown via Pterodactyl

---

## 🧠 AI & Local LLM Status

### Ollama Service (VM 400)
- **Platform:** Ubuntu 22.04 with ROCm 6.1.3
- **GPU:** RX 6700 XT (12GB VRAM) with PCIe passthrough
- **Status:** ✅ Running
- **API:** http://192.168.30.221:11434

### Active Models

| Model | Size | VRAM Usage | Status |
|-------|------|------------|--------|
| qwen2.5:14b | 9GB | Primary | ✅ Loaded |
| llama3.2:latest | 2GB | Secondary | ✅ Available |

### Open WebUI
- **Access:** ollama.najhin-gaming.com
- **Status:** ✅ Running (Docker container)
- **Authentication:** Cloudflare Access (Email OTP)
- **Connection:** Docker bridge to Ollama API

---

## 📚 Knowledge Management

### Obsidian Vault
- **Location:** C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **Sync:** Via Nextcloud to CT 220
- **Status:** ✅ Active
- **Plugins:** 6 installed (Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban)

### Vault Structure
```
second-brain/
├── 00-inbox/          # 15 quick notes
├── 01-homelab/        # 8 infrastructure docs
├── 02-career/         # 3 transition materials
├── 03-knowledge/      # 12 learning notes
├── 04-personal/       # 6 management items
│   ├── subscriptions/ # 11 subscription trackers
│   └── dashboard/     # Cost overview
├── 05-templates/      # 2 note templates
└── 06-archive/        # 0 items
```

### Subscription Tracking
- **Total Subscriptions:** 11 tracked
- **Monthly Cost:** [Calculated via Dataview]
- **Dashboard:** Auto-updating via Dataview queries

---

## ⚠️ Current Issues & Warnings

### High Priority
- **local-lvm thin pool:** 85% usage, overprovisioned (address in infrastructure cleanup)
- **Cost tracking bug:** gilgamesh_costs.cost_usd column always returns 0
- **API spending:** $10 limit set, monitor usage closely

### Medium Priority
- **Nextcloud data location:** Still on root disk (100GB), plan migration to data-storage
- **Windrose ground loot:** Cannot remove existing world items, requires fresh world

### Monitoring Required
- **VM 400 GPU usage:** Monitor temperature and power with AI workloads
- **CT 302 RAM usage:** Track memory with Windrose server active
- **Backup timing:** Ensure jobs complete before business hours

---

## 📈 Resource Trends

### Recent Growth
- **Storage:** +80GB (Nextcloud resize) + 60GB (Ollama VM)
- **RAM allocation:** +16GB (Ollama VM)
- **External services:** +1 (ollama.najhin-gaming.com)
- **Documentation:** 7 auto-generated files from AI context

### Planned Capacity
- **Next month:** Phase 41 (Hybrid routing), MERLIN agent
- **Q2 2026:** Additional game servers, service updates
- **Q3 2026:** AWS lab environment, Kubernetes cluster

---

*This snapshot is accurate as of April 24, 2026. Resource usage and service status are dynamic and may have changed since this report was generated.*