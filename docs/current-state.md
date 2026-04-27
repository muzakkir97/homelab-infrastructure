# 📸 Current Infrastructure State

> **Snapshot Date:** April 27, 2026  
> **Status:** All systems operational  
> **Total Resources:** 14 LXC containers + 1 KVM VM across 5 VLANs

---

## 🖥️ Proxmox Host Status

| Metric              | Current Value    | Threshold | Status |
|---------------------|------------------|-----------|--------|
| **CPU Usage**       | ~15%             | <80%      | ✅ Good |
| **Memory Usage**    | 26.8GB / 32GB    | <85%      | ✅ Good |
| **Storage (NVMe)**  | 137GB available  | >20GB     | ✅ Good |
| **Storage (HDD)**   | 7.3TB available  | >1TB      | ✅ Good |
| **Temperature**     | CPU 48.5°C       | <70°C     | ✅ Good |
| **Uptime**          | Running stable   | —         | ✅ Good |

**Host:** Kuromoon (Ryzen 5 5600X, 32GB RAM, RX 6700 XT)  
**Hypervisor:** Proxmox VE 8.x  
**Node Name:** muzakkir

---

## 📦 Container Status & Resource Allocation

### Management & Infrastructure

| CTID | Name                    | IP             | Status    | CPU | RAM     | Storage | Autostart |
|------|-------------------------|----------------|-----------|-----|---------|---------|-----------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | ✅ Running | 1   | 512MB   | 8GB     | ✅         |
| 207  | network-ddns            | 192.168.30.207 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |
| 208  | dashboard-homepage      | 192.168.30.208 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |
| 211  | automation-n8n          | 192.168.30.211 | ✅ Running | 2   | 1GB     | 16GB    | ✅         |
| 213  | vault                   | 192.168.30.213 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |
| 214  | password-vaultwarden    | 192.168.30.214 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |

### Monitoring Stack

| CTID | Name                    | IP             | Status    | CPU | RAM     | Storage | Autostart |
|------|-------------------------|----------------|-----------|-----|---------|---------|-----------|
| 202  | monitoring-prometheus   | 192.168.30.202 | ✅ Running | 2   | 2GB     | 32GB    | ✅         |
| 203  | monitoring-grafana      | 192.168.30.203 | ✅ Running | 1   | 512MB   | 8GB     | ✅         |
| 204  | monitoring-loki         | 192.168.30.204 | ✅ Running | 1   | 1GB     | 16GB    | ✅         |
| 205  | monitoring-alertmanager | 192.168.30.205 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |
| 206  | monitoring-uptime       | 192.168.30.206 | ✅ Running | 1   | 256MB   | 4GB     | ✅         |

### Storage & Knowledge

| CTID | Name                    | IP             | Status    | CPU | RAM     | Storage | Autostart |
|------|-------------------------|----------------|-----------|-----|---------|---------|-----------|
| 220  | nextcloud-hub           | 192.168.30.220 | ✅ Running | 2   | 2GB     | 100GB   | ✅         |

### Gaming Platform

| CTID | Name                    | IP             | Status    | CPU | RAM     | Storage | Autostart |
|------|-------------------------|----------------|-----------|-----|---------|---------|-----------|
| 300  | gaming-panel            | 192.168.30.210 | ✅ Running | 1   | 1GB     | 16GB    | ✅         |
| 302  | gaming-wings-1          | 192.168.30.212 | ✅ Running | 2   | 12GB    | 64GB    | ✅         |

**Note:** CT 302 high memory due to Windrose server (~9GB RAM when active)

### AI & Local LLM

| VMID | Name       | IP             | Status    | CPU | RAM  | Storage | Autostart |
|------|------------|----------------|-----------|-----|------|---------|-----------|
| 400  | ollama-gpu | 192.168.30.221 | ✅ Running | 8   | 16GB | 64GB    | ✅         |

**Type:** KVM VM with PCIe GPU passthrough (RX 6700 XT + audio)  
**OS:** Ubuntu 22.04  
**GPU:** ROCm 6.1.3 runtime with HSA_OVERRIDE_GFX_VERSION=10.3.0

---

## 🌐 Network Topology & VLAN Status

### Active VLANs

| VLAN | Name            | Subnet          | Gateway       | Active Devices |
|------|-----------------|-----------------|---------------|----------------|
| 10   | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1  | 3 (Proxmox, pfSense, NAS) |
| 20   | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1  | 5 (clients, Gaming PC) |
| 30   | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1  | 15 (all containers + VM) |
| 40   | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1  | 0 (reserved) |
| 50   | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1  | 0 (isolated lab) |

### Network Devices

| Device           | IP             | Status      | Purpose                |
|------------------|----------------|-------------|------------------------|
| pfSense Router   | 192.168.10.1   | ✅ Online   | Gateway, firewall      |
| Proxmox Server   | 192.168.10.5   | ✅ Online   | Hypervisor            |
| NAS (Kinmoon)    | 192.168.10.15  | ✅ Online   | Backup storage        |
| Pi-hole DNS      | 192.168.30.10  | ✅ Online   | DNS filtering         |
| Managed Switch   | 192.168.1.20   | ✅ Online   | Layer 2 switching     |

**Firewall Rules:** All inter-VLAN traffic blocked except DNS queries to Pi-hole  
**VPN Access:** Tailscale subnet router active on pfSense

---

## 💾 Storage Pool Status

### Local Storage

| Pool Name    | Type      | Size   | Used   | Available | Health |
|--------------|-----------|--------|--------|-----------|--------|
| local-lvm    | LVM-Thin  | 137GB  | ~80GB  | ~57GB     | ✅ Good |
| vmpool-fast  | ZFS       | 899GB  | ~100GB | ~799GB    | ✅ Good |

### External Storage

| Storage      | Type | Size   | Used   | Available | Purpose              |
|--------------|------|--------|--------|-----------|----------------------|
| kinmoon-nfs  | NFS  | 3.6TB  | ~500GB | ~3.1TB    | Container backups    |
| data-storage | HDD  | 7.3TB  | ~200GB | ~7.1TB    | Large container data |

**Storage Health:** All pools healthy with adequate free space

---

## 🔄 Backup Job Status

### Scheduled Backup Jobs

| Job Name         | Schedule | Last Run      | Status      | Duration | Storage Target |
|------------------|----------|---------------|-------------|----------|----------------|
| Small Containers | 02:00    | Apr 27, 2026  | ✅ Success  | 15 min   | kinmoon-nfs    |
| Large Containers | 02:30    | Apr 27, 2026  | ✅ Success  | 45 min   | data-storage   |

### Backup Coverage

| Container Group | Containers | Retention Policy | Last Backup |
|-----------------|------------|------------------|-------------|
| Small (9 CT)    | 201,202,203,204,205,206,207,214,300 | 7/4/2 | ✅ Apr 27 |
| Large (2 CT)    | 220,302 | 7/4/2 | ✅ Apr 27 |

**Backup Restoration:** Last restore test not performed (pending task for CT 207)

---

## 🌍 External Access Configuration

### Cloudflare Tunnel Status

| Service         | Subdomain                | Status      | Auth Method      | Tunnel Status |
|-----------------|--------------------------|-------------|------------------|---------------|
| Grafana         | grafana.najhin-gaming.com | ✅ Active   | Cloudflare Access | ✅ Connected  |
| n8n             | n8n.najhin-gaming.com     | ✅ Active   | Cloudflare Access | ✅ Connected  |
| Vault           | vault.najhin-gaming.com   | ✅ Active   | Cloudflare Access | ✅ Connected  |
| Vaultwarden     | passwords.najhin-gaming.com | ✅ Active | Cloudflare Access | ✅ Connected  |
| Ollama          | ollama.najhin-gaming.com  | ✅ Active   | Cloudflare Access | ✅ Connected  |
| Homepage        | home.najhin-gaming.com    | ✅ Active   | Public Access    | ✅ Connected  |
| Nextcloud       | cloud.najhin-gaming.com   | ✅ Active   | Cloudflare Access | ✅ Connected  |
| Terraria        | terraria.najhin-gaming.com | ✅ Active  | Public Access    | ✅ Connected  |
| Minecraft       | mc.najhin-gaming.com      | ✅ Active   | Public Access    | ✅ Connected  |

### SSL Certificate Status

| Domain                    | Issuer        | Expires     | Auto-Renew | Status |
|---------------------------|---------------|-------------|------------|--------|
| *.najhin-gaming.com       | Let's Encrypt | Jul 14, 2026| ✅ Yes     | ✅ Valid |

**Authentication:** Cloudflare Access configured for Email OTP (muzakkir.kholil06@gmail.com only)

---

## 📊 Active Monitoring & Alerting

### Prometheus Targets

| Target                  | Status      | Last Scrape | Labels |
|-------------------------|-------------|-------------|--------|
| Kuromoon Host           | ✅ UP       | < 1min      | host   |
| All Containers (15)     | ✅ UP       | < 1min      | container |
| pfSense                 | ✅ UP       | < 5min      | network |
| Pi-hole                 | ✅ UP       | < 1min      | dns    |

### Active Alerts

| Alert                | Severity | Status     | Last Fired | Duration |
|----------------------|----------|------------|------------|----------|
| HostDown             | Critical | ✅ Clear   | Never      | —        |
| HighCPUUsage (>90%)  | Warning  | ✅ Clear   | Never      | —        |
| HighMemoryUsage (>85%) | Warning | ✅ Clear   | Never      | —        |
| DiskSpaceLow (<10%)  | Warning  | ✅ Clear   | Never      | —        |

### Notification Channels

| Channel  | Type            | Status      | Last Message |
|----------|-----------------|-------------|--------------|
| Telegram | Critical Alerts | ✅ Active   | Apr 25, 2026 |
| Discord  | Warning Alerts  | ✅ Active   | Apr 24, 2026 |

---

## 🤖 Active AI Agents & Workflows

### Agent Status

| Agent      | Platform | Status      | Last Activity | Daily Activity |
|------------|----------|-------------|---------------|----------------|
| Gilgamesh  | Telegram | ✅ Active   | < 1min        | High          |
| Da Vinci   | n8n      | ⚡ Partial  | Apr 25, 2026  | As needed     |
| Midas      | n8n      | ✅ Active   | 9:00 AM       | Daily brief   |
| MERLIN     | n8n      | ✅ Active   | 8:00 AM       | Daily checks  |

### n8n Workflows (12 Active)

| Workflow                     | Status      | Last Execution | Trigger     |
|------------------------------|-------------|----------------|-------------|
| Telegram Agent               | ✅ Running  | < 1min         | Telegram    |
| Documentation Pipeline — Update | ✅ Running | Apr 27, 2026   | Webhook     |
| Documentation Pipeline — Sync Docs | ✅ Running | Apr 26, 2026 | Webhook     |
| Da Vinci Documentation Pipeline | ✅ Running | Apr 25, 2026   | Webhook     |
| Midas — CFO Report           | ✅ Running  | On demand      | Webhook     |
| Midas — Daily Brief          | ✅ Running  | 9:00 AM        | Schedule    |
| MERLIN — Reminders           | ✅ Running  | 8:00 AM        | Schedule    |
| Daily Note Creator           | ✅ Running  | Midnight       | Schedule    |
| Morning Briefing             | ✅ Running  | 7:00 AM        | Schedule    |

### API Usage & Costs (April 2026)

| Service       | Usage Type    | Current Month | Projected Cost | Status |
|---------------|---------------|---------------|----------------|--------|
| Claude Sonnet | API calls     | $3.50         | ~$8.00         | ✅ Under budget |
| Claude Haiku  | API calls     | $1.20         | ~$2.50         | ✅ Under budget |
| Ollama Local  | Free compute  | $0.00         | $0.00          | ✅ Optimal      |

**Monthly Budget:** $10 API spend limit  
**Savings:** ~$25/month from local Ollama inference

---

## 🔐 Security Status

### Access Control

| Service Type     | Authentication Method | Status      | Last Review |
|------------------|-----------------------|-------------|-------------|
| Administrative   | Tailscale VPN only    | ✅ Secured  | Apr 27, 2026 |
| External Services| Cloudflare Access     | ✅ Secured  | Apr 24, 2026 |
| Internal Network | VLAN segmentation     | ✅ Secured  | Mar 9, 2026  |

### Secrets Management

| Secret Store | Status      | Seal Status | Last Backup |
|--------------|-------------|-------------|-------------|
| HashiCorp Vault | ✅ Running | 🔓 Unsealed | Apr 27, 2026 |
| Vaultwarden  | ✅ Running  | —           | Apr 27, 2026 |

**Vault Paths:** 8 secret paths configured  
**Manual Unseal Required:** After each Proxmox reboot

### DNS Security

| Filter Type      | Domains Blocked | Status      | Last Update |
|------------------|-----------------|-------------|-------------|
| Malware/Tracking | ~489,000        | ✅ Active   | Daily       |

---

## 📈 Performance Metrics (Last 24h)

### System Load Averages

| Host          | 1min | 5min | 15min | Status |
|---------------|------|------|-------|--------|
| Kuromoon      | 0.85 | 0.92 | 1.05  | ✅ Normal |

### Top Resource Consumers

| Container     | CPU Avg | Memory Avg | Purpose          |
|---------------|---------|------------|------------------|
| gaming-wings-1| 25%     | 9.2GB      | Windrose server  |
| ollama-gpu    | 15%     | 7.9GB      | AI inference     |
| nextcloud-hub | 10%     | 1.8GB      | File sync        |
| prometheus    | 8%      | 1.9GB      | Metrics          |

### Network Traffic (Daily)

| Interface | Ingress | Egress | Peak Time |
|-----------|---------|--------|-----------|
| vmbr0     | 2.3GB   | 1.8GB  | Evening   |

---

## 🎯 Critical Action Items

### Immediate (High Priority)

- [ ] **Schedule backup restore test** — CT 207 recommended, update MERLIN lastTestDate
- [ ] **Fix Cloudflare API token** — Full token in Vault (currently truncated)
- [ ] **Activate MERLIN workflow** — Currently inactive, needs toggle on
- [ ] **Monitor Windrose RAM** — Stop Docker container when not playing (9GB baseline)

### Infrastructure (Medium Priority)

- [ ] **Address local-lvm overprovisioning** — Plan during infrastructure cleanup
- [ ] **Vault manual unseal** — Required after each Proxmox reboot
- [ ] **WiFi AP deployment** — TP-Link EAP610 purchased, pending setup

---

*Snapshot captured: April 27, 2026 at 23:45 MYT*