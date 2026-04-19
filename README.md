# 🏠 Homelab Infrastructure Project

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Proxmox](https://img.shields.io/badge/Proxmox-VE%208.2-orange)](https://www.proxmox.com/)
[![pfSense](https://img.shields.io/badge/pfSense-2.7-blue)](https://www.pfsense.org/)
[![Containers](https://img.shields.io/badge/Containers-14%20LXC-green)](https://linuxcontainers.org/)
[![Phase](https://img.shields.io/badge/Phase-58%20Complete-brightgreen)](#project-phases)

> **Enterprise-grade homelab for career transition from Cybersecurity to Cloud Engineering/DevOps**

## 🎯 Project Goals

This homelab serves as both a **learning environment** and **professional portfolio** for my career transition from Customer Service Engineer at F-Secure (cybersecurity) to **Cloud Engineering/DevOps** roles. Every component is production-grade, documented, and demonstrates real-world enterprise skills.

**Current Status:** Phase 58 complete. 14 LXC containers running enterprise services with full monitoring, automation, and security.

## 👤 About Me

I'm **Muzakkir Kholil**, a cybersecurity professional transitioning to Cloud Engineering/DevOps. Currently working as a Customer Service Engineer at F-Secure in Petaling Jaya, Malaysia. This homelab showcases my technical growth and passion for infrastructure automation.

- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)
- **Domain:** najhin-gaming.com (managed via Cloudflare)
- **Learning Philosophy:** Prefer complexity over shortcuts — I want to understand the "why"

## 🏗️ Architecture Overview

```
Internet
    ↓
ISP Router (192.168.100.1)
    ↓
pfSense Firewall (192.168.x.1)
    ↓ 802.1Q Trunk
TP-Link Managed Switch (TL-SG108E)
    ↓
┌─────────┬─────────┬─────────┬─────────┬─────────┐
│ VLAN 10 │ VLAN 20 │ VLAN 30 │ VLAN 40 │ VLAN 50 │
│  Mgmt   │  Main   │Services │  DMZ    │Malware  │
│         │Clients  │ (14 CT) │ Future  │ Lab     │
└─────────┴─────────┴─────────┴─────────┴─────────┘
```

### Network Design (Router-on-a-Stick)

| VLAN ID | Name            | Subnet          | Purpose                                |
|---------|-----------------|-----------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.x.x/24  | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.x.x/24  | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.x.x/24  | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.x.x/24  | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.x.x/24  | Isolated security lab (air-gapped)     |

## 🖥️ Hardware Inventory

| Device           | Hostname | Specifications                          | Role                    |
|------------------|----------|-----------------------------------------|-------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT    | Hypervisor              |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                | Router, Firewall        |
| Managed Switch   | —        | TP-Link TL-SG108E                       | Layer 2, VLANs          |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple         | Backup Storage          |
| DNS Server       | —        | Raspberry Pi 4                          | Pi-hole (~489K blocked) |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB        | Gaming only             |

## 📦 Container Inventory (14 LXC)

All containers run on **VLAN 30 Services** with autostart enabled:

### Core Infrastructure (6 containers)
- **nginx-proxy-manager** (CT 201) — Reverse proxy with SSL
- **monitoring-prometheus** (CT 202) — Metrics collection
- **monitoring-grafana** (CT 203) — Visualization dashboard
- **monitoring-loki** (CT 204) — Log aggregation
- **monitoring-alertmanager** (CT 205) — Alert routing
- **monitoring-uptime** (CT 206) — Service monitoring

### Networking & Security (4 containers)
- **network-ddns** (CT 207) — Dynamic DNS updates
- **vault** (CT 213) — HashiCorp Vault secrets management
- **password-vaultwarden** (CT 214) — Password manager
- **dashboard-homepage** (CT 208) — Unified dashboard

### Automation & Cloud (2 containers)
- **automation-n8n** (CT 211) — Workflow automation
- **nextcloud-hub** (CT 220) — Private cloud storage

### Gaming Platform (2 containers)
- **gaming-panel** (CT 300) — Pterodactyl game server panel
- **gaming-wings-1** (CT 302) — Game servers (Terraria, Minecraft, Windrose)

## 🤖 Gilgamesh AI Agent

**Gilgamesh** is my custom Telegram AI assistant integrating Claude API with homelab infrastructure:

```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Claude API
                                      ↓
                               Memory (n8n Tables)
                                      ↓
                               /update → GitHub sync
```

### Features
- **Conversation memory** — Last 20 messages stored
- **Smart routing** — Simple → Haiku 4.5, Complex → Sonnet 4
- **Web search** — Real-time information via Claude
- **Infrastructure control** — Query Proxmox API for status
- **Documentation sync** — /update pushes to GitHub
- **Inline keyboard menu** — Structured navigation

## 📊 Monitoring Stack

### Components
- **Prometheus** — Metrics collection from all containers and hardware
- **Grafana** — Unified visualization dashboards
- **Loki** — Centralized log aggregation
- **AlertManager** — Multi-channel alerting (Telegram + Discord)
- **Uptime Kuma** — Service availability monitoring

### Alerting Channels
- **Telegram** — Critical alerts (host down, high resource usage)
- **Discord** — Warning-level alerts to #alerts channel

## 🔒 Security Architecture

| Layer           | Implementation                                                    |
|-----------------|-------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                     |
| Segmentation    | 5 VLANs with enforced inter-VLAN firewall rules                   |
| DNS Filtering   | Pi-hole blocking ~489K ad/tracking domains                        |
| VPN Access      | Tailscale mesh network (primary admin access)                     |
| External Auth   | Cloudflare Access with email OTP (7 protected apps)               |
| Tunnel Access   | Cloudflare Tunnel for Nextcloud                                   |
| Secrets Mgmt    | HashiCorp Vault (machine secrets) + Vaultwarden (passwords)       |
| Network Isolation | Admin VLAN 20 blocked from accessing Infrastructure VLAN 10     |

### External Access (Public Services)
- **grafana.najhin-gaming.com** — Monitoring dashboards
- **home.najhin-gaming.com** — Homepage dashboard
- **n8n.najhin-gaming.com** — Automation workflows
- **vault.najhin-gaming.com** — Secrets management
- **passwords.najhin-gaming.com** — Password manager
- **cloud.najhin-gaming.com** — Nextcloud (via Cloudflare Tunnel)
- **terraria/mc.najhin-gaming.com** — Game servers

## 💾 Backup Strategy

### Automated Daily Backups
- **Small Containers** (9 containers) → NAS via SMB/CIFS
- **Large Containers** (2 containers) → Local HDD storage
- **Retention:** 7 daily, 4 weekly, 2 monthly backups
- **Monitoring:** Backup job status tracked in Prometheus

### Storage Hierarchy
- **Primary:** Local NVMe (LVM-Thin + ZFS)
- **Backup:** NAS (3.6TB) + Local HDD (7.2TB)
- **Future:** Off-site backup to Backblaze B2

## 🚀 Project Phases

### ✅ Completed (58 phases)
- **Phases 1-6F** — Core infrastructure, networking, monitoring, gaming platform
- **Phase 7-7D** — Nextcloud, automation, Gilgamesh AI agent
- **Phase 9** — NAS deployment and backup strategy
- **Phase 13** — HashiCorp Vault secrets management
- **Phase 23** — Vaultwarden password manager
- **Phase 58** — Windrose game server deployment

### 🔄 In Progress
- **Phase 7D-Menu** — Gilgamesh inline keyboard menu completion

### 📋 Planned Next
- **Phase 11** — Ollama + ROCm AI inference (RX 6700 XT GPU)
- **Monitoring** — Enhanced metrics, temperature monitoring
- **Infrastructure** — WiFi access point setup, legacy network cleanup
- **Security** — Off-site backups, additional 2FA

## 🛠️ Skills Demonstrated

### Infrastructure & Virtualization
- Proxmox VE hypervisor management
- LXC containerization at enterprise scale
- VLAN design and network segmentation
- pfSense firewall configuration and hardening

### Monitoring & Observability
- Prometheus + Grafana monitoring stack
- Log aggregation with Loki
- Multi-channel alerting (Telegram, Discord)
- Infrastructure metrics and dashboards

### Security & Access Control
- Zero-trust network segmentation
- HashiCorp Vault secrets management
- Cloudflare Access authentication
- VPN mesh networking with Tailscale

### Automation & DevOps
- n8n workflow automation
- CI/CD-style documentation updates
- Infrastructure as Code principles
- API integration (Proxmox, Cloudflare, GitHub)

### AI & Integration
- Custom Telegram bot development
- Claude API integration with memory
- Multi-model routing and cost optimization
- Real-time web search and context management

## 📚 Documentation Structure

```
📁 homelab-infrastructure/
├── 📄 README.md              ← You are here
├── 📄 ROADMAP.md              ← All project phases
├── 📄 current-state.md        ← Live infrastructure snapshot
├── 📄 service-catalog.md      ← Service directory
├── 📄 AI-CONTEXT.md           ← Master context (private repo)
└── 📁 docs/                   ← Detailed guides
    ├── phase-guides/          ← Step-by-step implementations
    └── troubleshooting/       ← Lessons learned
```

## 🔮 Long-term Vision

This homelab will continue evolving as a **production-grade learning environment**:

### Year 1 Goals
- **AI/ML Infrastructure** — Local LLM inference with GPU acceleration
- **Enhanced Automation** — Full infrastructure-as-code deployment
- **Career Transition** — Demonstrate enterprise-level skills to employers
- **Knowledge Sharing** — Comprehensive documentation for other learners

### Expansion Areas
- **Kubernetes** — Container orchestration learning environment
- **GitOps** — ArgoCD-style deployment automation
- **Observability** — Distributed tracing and advanced metrics
- **Multi-site** — High availability across locations

## 🤝 Connect & Learn

Interested in the technical details? Want to discuss homelab design or career transitions?

- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)
- **Project Issues:** Use GitHub Issues for technical questions
- **Professional Network:** Connect via LinkedIn

This project is **actively maintained** and documented. Each phase includes detailed implementation guides, troubleshooting notes, and lessons learned.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

**Note:** Configuration examples are provided for educational purposes. Adapt security settings for your environment.