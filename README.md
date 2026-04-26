# 🏠 Homelab Infrastructure Project

[![GitHub License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)
[![Documentation Status](https://img.shields.io/badge/docs-up%20to%20date-brightgreen.svg)](https://github.com/muzakkir97/homelab-infrastructure)
[![Infrastructure Status](https://img.shields.io/badge/infrastructure-operational-success.svg)](current-state.md)
[![Containers Running](https://img.shields.io/badge/containers-14%20LXC%20%2B%201%20VM-blue.svg)](service-catalog.md)

> **Enterprise-grade homelab for career transition from Customer Service Engineering to Cloud Engineering / DevOps**

## 🎯 Project Goals

This homelab serves as both a learning environment and professional portfolio, demonstrating enterprise infrastructure practices in a home environment. The project showcases skills in virtualization, networking, monitoring, automation, security, and DevOps practices required for cloud engineering roles.

**Primary Objective:** Career transition from Customer Service Engineer (F-Secure, cybersecurity) to Cloud Engineering / DevOps through hands-on infrastructure experience.

## 👤 About Me

| Field | Details |
|-------|---------|
| **Name** | Muzakkir Kholil |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location** | Petaling Jaya, Selangor, Malaysia |
| **Target Role** | Cloud Engineering / DevOps |
| **Domain** | najhin-gaming.com (Cloudflare managed) |

## 🏗️ Architecture Overview

### Network Topology
```
Internet → ISP Router → pfSense Firewall
                            ↓
                     802.1Q Trunk
                            ↓
                  TP-Link Managed Switch
                            ↓
        ┌─────────┬─────────┬─────────┬─────────┐
      VLAN10    VLAN20    VLAN30    VLAN40    VLAN50
      Mgmt      Main     Services    DMZ     Malware
```

### VLAN Design

| VLAN | Name | Subnet | Purpose |
|------|------|---------|----------|
| 10 | MGMT | 192.168.10.0/24 | Infrastructure (Proxmox, pfSense, NAS) |
| 20 | MAIN | 192.168.20.0/24 | Client devices |
| 30 | SERVICES | 192.168.30.0/24 | All service containers + Pi-hole |
| 40 | DMZ | 192.168.40.0/24 | Future public-facing services |
| 50 | MALWARE | 192.168.50.0/24 | Isolated security lab |

## 🖥️ Hardware Inventory

| Device | Hostname | Specs | Role |
|--------|----------|-------|------|
| **Proxmox Server** | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | Hypervisor |
| **pfSense Firewall** | — | AC8F Mini PC, Intel N100 | Router, Firewall |
| **Managed Switch** | — | TP-Link TL-SG108E | Layer 2, VLANs |
| **NAS** | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | Backup storage |
| **DNS Server** | — | Raspberry Pi 4 | Pi-hole (~489K domains blocked) |
| **Gaming PC** | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB | Gaming only |
| **WiFi AP** | — | TP-Link EAP610 | Wireless access (pending setup) |

## 📦 Container Inventory

**Total: 14 LXC containers + 1 KVM VM**

### Core Infrastructure (VLAN 30)
| ID | Name | Purpose | External Access |
|----|------|---------|----------------|
| 201 | nginx-proxy-manager | Reverse proxy | Internal only |
| 202 | monitoring-prometheus | Metrics collection | Internal only |
| 203 | monitoring-grafana | Metrics visualization | grafana.najhin-gaming.com |
| 204 | monitoring-loki | Log aggregation | Internal only |
| 205 | monitoring-alertmanager | Alert management | Internal only |
| 206 | monitoring-uptime | Uptime monitoring | Internal only |
| 207 | network-ddns | Dynamic DNS updates | Internal only |
| 208 | dashboard-homepage | Infrastructure dashboard | home.najhin-gaming.com |
| 211 | automation-n8n | Workflow automation | n8n.najhin-gaming.com |
| 213 | vault | Secrets management | vault.najhin-gaming.com |
| 214 | password-vaultwarden | Password manager | passwords.najhin-gaming.com |
| 220 | nextcloud-hub | File sync & collaboration | cloud.najhin-gaming.com |
| 300 | gaming-panel | Pterodactyl panel | Internal only |
| 302 | gaming-wings-1 | Game servers | terraria/mc.najhin-gaming.com |
| 400 | ollama-gpu | AI inference (KVM VM) | ollama.najhin-gaming.com |

## 🤖 Gilgamesh AI Agent

**Advanced Telegram bot** providing infrastructure management and personal assistance through intelligent routing:

### Smart Hybrid Architecture
- **Ollama qwen3:14b** — Primary local LLM (free, fast, privacy-focused)
- **Claude Haiku 4.5** — Fallback for availability
- **Claude Sonnet 4** — Complex queries and advanced reasoning

### Key Features
- **Conversation Memory** — Retains context across 20+ messages
- **Cost Optimization** — Routes simple queries to free local model
- **Web Search** — Real-time information access
- **Infrastructure Control** — Container status, metrics, alerts
- **Documentation Pipeline** — Automated session summaries and doc updates

### Slash Commands
```
/help      - Quick reference guide
/clear     - Reset conversation memory
/memory    - View recent conversation
/cost      - Token usage and cost tracking
/alerts    - Active system alerts
/backup    - Backup job status
/update    - Generate session summary
/sync-docs - Full documentation regeneration
```

## 🎮 Gaming Platform

Self-hosted gaming infrastructure with automated management:

### Supported Games
- **Terraria** — terraria.najhin-gaming.com
- **Minecraft** — mc.najhin-gaming.com
- **Windrose** — 4-player co-op, managed via Pterodactyl

### Management
- Pterodactyl Panel for server lifecycle
- Automated backups and updates
- Resource monitoring and scaling
- Discord integration (planned)

## 📊 Monitoring Stack

Comprehensive observability with Prometheus ecosystem:

### Components
- **Prometheus** — Metrics collection and storage
- **Grafana** — Visualization and dashboards
- **Loki** — Log aggregation and analysis
- **Alertmanager** — Alert routing (Telegram + Discord)
- **Uptime Kuma** — Service availability monitoring

### Key Metrics
- Container resource utilization
- Network performance
- Storage capacity and I/O
- Temperature monitoring
- Service availability

## 🔒 Security Architecture

### Defense in Depth
| Layer | Implementation |
|-------|---------------|
| **Perimeter** | pfSense firewall with VLAN segmentation |
| **DNS Security** | Pi-hole blocking 489K+ domains |
| **VPN Access** | Tailscale mesh network |
| **External Auth** | Cloudflare Access with Email OTP |
| **Secrets** | HashiCorp Vault + Vaultwarden |
| **Backup** | Automated daily with 7/4/2 retention |
| **Network Isolation** | 5 VLANs with enforced firewall rules |

### External Access
All public services secured through:
- Cloudflare Tunnel (no exposed ports)
- Cloudflare Access authentication
- Let's Encrypt SSL certificates
- Email OTP verification

## 💾 Backup Strategy

### Automated Backup Jobs
- **Small Containers** — Daily 02:00 to NAS (SMB/CIFS)
- **Large Containers** — Daily 02:30 to local storage (7.3TB HDD)
- **Retention Policy** — 7 daily, 4 weekly, 2 monthly snapshots

### Storage Architecture
- **Primary** — NVMe SSDs (137GB + 899GB ZFS)
- **Backup** — 3.6TB NAS + 7.3TB local HDD
- **Future** — Backblaze B2 off-site backup (planned)

## 📈 Project Status

### Completed Phases (34 total)
- ✅ Core Infrastructure (Proxmox, pfSense, VLANs)
- ✅ Monitoring & Alerting Stack
- ✅ Gaming Platform (Pterodactyl + servers)
- ✅ Backup Strategy & Implementation
- ✅ Secrets Management (Vault + Vaultwarden)
- ✅ AI Agent (Gilgamesh) with Hybrid Routing
- ✅ Local LLM (Ollama + ROCm on GPU)
- ✅ Documentation Pipeline Automation
- ✅ Knowledge Management (Obsidian + Nextcloud)

### In Progress
- 🔄 EMIYA Infrastructure Agent (CTO automation)
- 🔄 Midas Cost Tracking Agent (CFO automation)
- 🔄 Enhanced observability and alerting

### Planned Features
- 🔜 Advanced AI agents (MERLIN, Guardian, Mash)
- 🔜 Gaming Discord bot integration
- 🔜 Homepage dashboard enhancements
- 🔜 Price tracking and budget management
- 🔜 Career development portfolio
- 🔜 Advanced security monitoring

## 💡 Skills Demonstrated

### Infrastructure & DevOps
- **Virtualization** — Proxmox VE with LXC containers and KVM VMs
- **Networking** — VLANs, firewall rules, DNS, VPN mesh networks
- **Monitoring** — Prometheus ecosystem, log aggregation, alerting
- **Automation** — n8n workflow automation, Infrastructure as Code practices
- **Security** — Defense in depth, secrets management, access control

### Cloud Technologies
- **Containerization** — Docker, container orchestration
- **Storage** — ZFS, LVM, distributed storage concepts
- **Backup & Recovery** — Automated backup strategies, disaster recovery
- **CI/CD** — Automated documentation pipeline, version control integration

### Programming & Integration
- **API Integration** — REST APIs, webhooks, authentication
- **Scripting** — Bash, system administration automation
- **Database** — Vector databases, data persistence
- **AI/ML** — Local LLM deployment, GPU acceleration (ROCm)

## 📚 Documentation Structure

```
homelab-infrastructure/
├── README.md              # This file - project overview
├── roadmap.md             # Detailed phase planning and status
├── current-state.md       # Live infrastructure snapshot
├── service-catalog.md     # Complete service directory
├── changelog.md           # Session-by-session changes
├── troubleshooting.md     # Common issues and solutions
└── AI-CONTEXT.md         # Master context for AI assistants
```

## 🚀 Long-term Vision

### Phase 1: Foundation (Complete)
Reliable infrastructure with monitoring, backup, and basic automation.

### Phase 2: Intelligence (In Progress)
AI-powered infrastructure management with cost optimization and predictive maintenance.

### Phase 3: Platform (Planned)
Self-service infrastructure platform with advanced automation and integration.

### Phase 4: Portfolio (Future)
Professional showcase demonstrating enterprise-ready cloud engineering skills.

## 🔗 Connect

- **GitHub:** [muzakkir97/homelab-infrastructure](https://github.com/muzakkir97/homelab-infrastructure)
- **LinkedIn:** Regular project updates and technical articles
- **Email:** Available via GitHub profile

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

*Infrastructure operational since January 2026 • Last updated: April 26, 2026*