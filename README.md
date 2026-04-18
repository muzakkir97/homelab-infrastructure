# 🏠 Enterprise Homelab Infrastructure

[![Status](https://img.shields.io/badge/Status-Active-brightgreen)](https://github.com/muzakkir97/homelab-infrastructure)
[![Phases](https://img.shields.io/badge/Phases-16%20Complete-blue)](https://github.com/muzakkir97/homelab-infrastructure)
[![Containers](https://img.shields.io/badge/Containers-12%20Running-orange)](https://github.com/muzakkir97/homelab-infrastructure)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

> Building enterprise-grade infrastructure at home for learning, portfolio development, and career transition to Cloud Engineering / DevOps.

**[📋 Roadmap](ROADMAP.md)** • **[📖 Documentation](docs/)** • **[🤖 Gilgamesh AI](#-gilgamesh-ai-agent)** • **[🌐 Architecture](#-network-architecture)**

---

## 🎯 Project Goals

| Goal | Status |
|------|--------|
| Enterprise virtualization with Proxmox VE | ✅ Complete |
| Network segmentation with VLANs & pfSense | ✅ Complete |
| Full monitoring stack (Prometheus, Grafana, Loki) | ✅ Complete |
| Self-hosted services (Nextcloud, game servers) | ✅ Complete |
| AI-powered automation (Gilgamesh bot) | ✅ Complete |
| Security lab for malware analysis | 📋 Planned |
| Local LLM inference (Ollama + ROCm) | 📋 Planned |

---

## 👤 About Me

| | |
|---|---|
| **Name** | Muzakkir Kholil |
| **Current Role** | Customer Service Engineer @ F-Secure |
| **Target Role** | Infrastructure / Cloud Engineering |
| **Location** | Petaling Jaya, Selangor, Malaysia |
| **Domain** | [najhin-gaming.com](https://najhin-gaming.com) |

I'm documenting my journey from customer service to infrastructure engineering through hands-on learning. This homelab serves as both a learning environment and a professional portfolio.

---

## 🏗️ Architecture Overview

```
Internet
    │
    ▼
┌─────────────────┐
│   ISP Router    │ 192.168.100.1
│  (Bridge Mode)  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│    pfSense      │ 192.168.10.1 (VLAN 10 Gateway)
│  AC8F Mini PC   │ Firewall, Router, Tailscale
└────────┬────────┘
         │ 802.1Q Trunk
         ▼
┌─────────────────┐
│  TP-Link Switch │ TL-SG108E (Managed)
│    8-Port       │ VLAN Tagging
└────────┬────────┘
         │
    ┌────┴────┬──────────┬──────────┬──────────┐
    │         │          │          │          │
 VLAN 10   VLAN 20   VLAN 30    VLAN 40    VLAN 50
  Mgmt      Main    Services     DMZ      Malware
```

### VLAN Design

| VLAN | Name | Subnet | Purpose |
|------|------|--------|---------|
| 10 | VLAN10_MGMT | 192.168.10.0/24 | Infrastructure (Proxmox, pfSense, NAS) |
| 20 | VLAN20_MAIN | 192.168.20.0/24 | Client devices |
| 30 | VLAN30_SERVICES | 192.168.30.0/24 | All service containers + Pi-hole |
| 40 | VLAN40_DMZ | 192.168.40.0/24 | Future public-facing services |
| 50 | VLAN50_MALWARE | 192.168.50.0/24 | Isolated security lab (air-gapped) |

---

## 🖥️ Hardware Inventory

### Servers & Network

| Device | Hostname | Specs | Role |
|--------|----------|-------|------|
| **Proxmox Server** | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB, ZFS NVMe mirror | Hypervisor |
| **pfSense Firewall** | — | AC8F Mini PC, Intel N100 | Router, Firewall |
| **Managed Switch** | — | TP-Link TL-SG108E | Layer 2, VLANs |
| **NAS** | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | Backups (SMB) |
| **DNS Server** | — | Raspberry Pi 4 | Pi-hole (~489K domains blocked) |
| **Gaming PC** | Minimoon | Ryzen 7 7800X3D, RX 9070 XT | Gaming only (not homelab) |

### Storage Architecture

| Pool | Type | Size | Purpose |
|------|------|------|---------|
| local-lvm | NVMe LVM-Thin | 137 GB | Container root disks |
| vmpool-fast | NVMe ZFS | 899 GB | Fast container storage |
| kinmoon-smb | NAS SMB/CIFS | 3.6 TB | Small container backups |
| data-storage | Local HDD | 7.2 TB | Large container backups |

---

## 📦 Container Inventory

All containers run on **Proxmox VE** as **LXC containers** on **VLAN 30** (192.168.30.0/24).

| CTID | Name | IP | Subdomain | Purpose |
|------|------|-----|-----------|---------|
| 201 | nginx-proxy-manager | 192.168.30.201 | — | Reverse proxy, SSL |
| 202 | monitoring-prometheus | 192.168.30.202 | — | Metrics collection |
| 203 | monitoring-grafana | 192.168.30.203 | grafana.najhin-gaming.com | Dashboards |
| 204 | monitoring-loki | 192.168.30.204 | — | Log aggregation |
| 205 | monitoring-alertmanager | 192.168.30.205 | — | Alert routing |
| 206 | monitoring-uptime | 192.168.30.206 | — | Uptime Kuma |
| 207 | network-ddns | 192.168.30.207 | — | Dynamic DNS |
| 208 | dashboard-homepage | 192.168.30.208 | home.najhin-gaming.com | Homepage dashboard |
| 211 | automation-n8n | 192.168.30.211 | n8n.najhin-gaming.com | Workflow automation |
| 220 | nextcloud-hub | 192.168.30.220 | cloud.najhin-gaming.com | File sync & storage |
| 300 | gaming-panel | 192.168.30.210 | — | Pterodactyl Panel |
| 302 | gaming-wings-1 | 192.168.30.212 | terraria/mc.najhin-gaming.com | Game servers |

**Total: 12 LXC containers, all autostart enabled**

---

## 🤖 Gilgamesh AI Agent

The crown jewel of this homelab — a custom AI assistant running on n8n.

```
Telegram (@JhinGilgamesh_bot)
         │
         ▼
┌─────────────────┐
│      n8n        │ Workflow automation
│   (CT 211)      │
└────────┬────────┘
         │
    ┌────┴────┐
    ▼         ▼
┌────────┐ ┌────────┐
│ Haiku  │ │ Sonnet │  Smart routing based on query complexity
│ (fast) │ │(complex)│
└────────┘ └────────┘
         │
         ▼
┌─────────────────┐
│ Memory System   │ Last 20 messages stored
│ Cost Tracking   │ Token usage logged
│ Web Search      │ Real-time information
└─────────────────┘
         │
         ▼
    /update command
         │
    ┌────┴────┐
    ▼         ▼
Nextcloud   GitHub
 (backup)   (sync)
```

### Features

| Feature | Description |
|---------|-------------|
| **Conversation Memory** | Remembers last 20 messages per session |
| **Smart Routing** | Simple queries → Haiku (fast), Complex → Sonnet (powerful) |
| **Web Search** | Real-time information via Claude's web_search tool |
| **Cost Tracking** | Logs token usage to database |
| **Context Sync** | `/update` command pushes summaries to GitHub |
| **Inline Menus** | Telegram keyboard for homelab status (in progress) |

---

## 📊 Monitoring Stack

```
┌─────────────────────────────────────────────────────────┐
│                    Grafana (CT 203)                     │
│              grafana.najhin-gaming.com                  │
│           Protected by Cloudflare Access                │
└─────────────────────────┬───────────────────────────────┘
                          │
          ┌───────────────┼───────────────┐
          ▼               ▼               ▼
    ┌──────────┐    ┌──────────┐    ┌──────────┐
    │Prometheus│    │   Loki   │    │Alertmgr  │
    │ (CT 202) │    │ (CT 204) │    │ (CT 205) │
    │ Metrics  │    │   Logs   │    │  Alerts  │
    └──────────┘    └──────────┘    └──────────┘
          │               │               │
          └───────────────┴───────────────┘
                          │
                   Node Exporters
                   (All hosts)
```

### Alerting

- **Critical alerts** → Telegram (immediate)
- **Warning alerts** → Discord (review during work hours)
- **13 alert rules** covering host availability, CPU, memory, disk, service health

---

## 🔒 Security Architecture

| Layer | Implementation |
|-------|----------------|
| **Perimeter** | ISP Router → pfSense firewall |
| **Segmentation** | 5 VLANs with enforced firewall rules |
| **DNS Filtering** | Pi-hole (~489K domains blocked) |
| **Remote Access** | Tailscale (primary), Cloudflare Tunnel (backup) |
| **Service Auth** | Cloudflare Access (email OTP) for Grafana, n8n |
| **Backups** | Automated daily with 7/4/2 retention (daily/weekly/monthly) |

---

## 💾 Backup Strategy

| Job | Schedule | Target | Containers | Retention |
|-----|----------|--------|------------|-----------|
| Small Containers | 02:00 daily | kinmoon-smb (NAS) | 201-207, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30 daily | data-storage (Local) | 220, 302 | 7 daily, 4 weekly, 2 monthly |

---

## 📋 Project Phases

### Completed ✅

| Phase | Title | Completed |
|-------|-------|-----------|
| 1 | Proxmox VE Installation | Jan 2026 |
| 2 | pfSense Firewall & VLAN Setup | Jan 2026 |
| 3 | Core Services (Pi-hole, NPM, Tailscale) | Jan 2026 |
| 4 | External Access & SSL | Feb 2026 |
| 5 | Monitoring Stack | Feb 2026 |
| 6A-6D | Gaming Platform (Pterodactyl) | Feb 2026 |
| 6E | Homepage Dashboard | Mar 2026 |
| 6F | Infrastructure Audit & Hardening | Mar 9, 2026 |
| 7 | Nextcloud Deployment | Mar 8, 2026 |
| 7A | Backup Strategy | Mar 13, 2026 |
| 7B | n8n Workflow Automation | Apr 2, 2026 |
| 7C | Gilgamesh Telegram Bot | Apr 2, 2026 |
| 7D | Gilgamesh Enhancements | Apr 6, 2026 |
| 7D-Sec | Cloudflare Access for n8n | Apr 7, 2026 |
| 9 | NAS Deployment (Kinmoon) | Mar 3, 2026 |

### In Progress 🔄

| Phase | Title | Status |
|-------|-------|--------|
| 7D-Menu | Gilgamesh Inline Keyboard Menu | In Progress |

### Planned 📋

| Phase | Title | Priority |
|-------|-------|----------|
| 7E | Pain Point Scraper (AI Vision) | High |
| 11 | Ollama + ROCm (Local LLM) | High |
| 10 | Malware Lab + Blue Team AI | Medium |
| 16 | Kubernetes (k3s) | Medium |

**See [ROADMAP.md](ROADMAP.md) for the complete roadmap with 34 planned phases.**

---

## 🎓 Skills Demonstrated

| Category | Technologies |
|----------|--------------|
| **Virtualization** | Proxmox VE, LXC, KVM, ZFS |
| **Networking** | pfSense, VLANs, 802.1Q, Firewall Rules |
| **Monitoring** | Prometheus, Grafana, Loki, Alertmanager |
| **Automation** | n8n, Bash scripting, Claude API |
| **Security** | Cloudflare Access, Tailscale, Network Segmentation |
| **Containers** | Docker, LXC, Nginx Proxy Manager |
| **Cloud Services** | Cloudflare (Tunnel, Access, DNS), GitHub |

---

## 📚 Documentation

### Phase Documentation

```
docs/
├── phase-1-proxmox/
├── phase-2-pfsense-vlans/
├── phase-3-core-services/
├── phase-4-external-access/
├── phase-5-monitoring/
├── phase-6-gaming/
├── phase-7-nextcloud/
├── phase-7a-backup/
├── phase-7b-n8n/
├── phase-7c-gilgamesh/
├── phase-7d-gilgamesh-enhancements/
└── ...
```

### Key Documents

- **[AI-CONTEXT.md](AI-CONTEXT.md)** — Master context document (source of truth)
- **[ROADMAP.md](ROADMAP.md)** — Complete project roadmap
- **[current-state.md](docs/current-state.md)** — Live infrastructure state
- **[troubleshoot.md](docs/troubleshoot.md)** — Error resolutions & lessons learned

---

## 🔮 Long-Term Vision

```
Year 1-2:   Foundation — Complete homelab, build portfolio, first clients
Year 3-4:   Growth — Stable side income (RM 3,000/month target)
Year 5-7:   Transition — Replace day job income, location independent
Year 8-10:  Lifestyle — Move to Bachok, Kelantan, run business remotely
```

---

## 📞 Contact

| Platform | Link |
|----------|------|
| **GitHub** | [@muzakkir97](https://github.com/muzakkir97) |
| **LinkedIn** | [muzakkir-kholil](https://linkedin.com/in/muzakkir-kholil) |
| **Domain** | [najhin-gaming.com](https://najhin-gaming.com) |

---

## 📜 License

This project is licensed under the MIT License — see [LICENSE](LICENSE) for details.

Documentation and configurations are provided as-is for educational purposes.

---

**Last Updated:** April 14, 2026  
**Current Phase:** 7D-Menu (Gilgamesh Inline Keyboard Menu)  
**Next Phase:** 7E (Pain Point Scraper)
