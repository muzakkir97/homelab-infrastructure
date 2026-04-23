# 🏠 Homelab Infrastructure Project

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Proxmox](https://img.shields.io/badge/Proxmox-VE-orange)](https://www.proxmox.com/en/proxmox-ve)
[![pfSense](https://img.shields.io/badge/pfSense-2.7-blue)](https://www.pfsense.org/)
[![Containers](https://img.shields.io/badge/LXC_Containers-14-green)](https://linuxcontainers.org/)
[![Uptime](https://img.shields.io/badge/Uptime-99.9%25-brightgreen)](https://grafana.najhin-gaming.com)

> **Enterprise-grade homelab for career transition from Customer Service Engineer to Cloud Engineering/DevOps**

## 🎯 Project Goals

This homelab serves as both a **learning environment** and **professional portfolio**, demonstrating enterprise-grade infrastructure practices while building practical DevOps skills. The project is publicly documented on GitHub and professionally showcased on LinkedIn as evidence of hands-on cloud engineering capabilities.

**Primary Objectives:**
- Build production-ready infrastructure with monitoring, alerting, and automation
- Demonstrate enterprise security practices with network segmentation and zero-trust principles
- Showcase Infrastructure as Code (IaC) and GitOps workflows
- Create AI-powered automation agents for infrastructure management
- Develop gaming platform capabilities for community engagement
- Document everything for knowledge sharing and career development

## 👤 About Me

**Muzakkir Kholil** | Customer Service Engineer @ F-Secure (Cybersecurity)  
📍 Petaling Jaya, Selangor, Malaysia  
🎯 **Target Role:** Cloud Engineering / DevOps  
🌐 **Domain:** najhin-gaming.com  
🔗 **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)

Currently transitioning from cybersecurity customer service to cloud engineering, leveraging my technical background and passion for infrastructure automation.

## 🏗️ Architecture Overview

```
                    Internet
                       ↓
              ISP Router (192.168.x.x)
                       ↓
                pfSense Firewall
              (WAN: DHCP, LAN: Trunk)
                       ↓
               TP-Link TL-SG108E Switch
                   (802.1Q VLANs)
                       ↓
        ┌──────┬──────┬──────┬──────┬──────┐
     VLAN10  VLAN20  VLAN30  VLAN40  VLAN50
      Mgmt    Main  Services   DMZ   Malware
        ↓       ↓       ↓       ↓       ↓
    Proxmox  Clients  14 LXC  Future  Isolated
      NAS    Gaming   Containers      Lab
    Pi-hole
```

### Network Design Philosophy

The network implements **router-on-a-stick** topology with strict VLAN segmentation:

| VLAN | Name | Subnet | Purpose | Access Control |
|------|------|--------|---------|----------------|
| 10 | MGMT | 192.168.x.x/24 | Infrastructure | Admin-only via Tailscale |
| 20 | MAIN | 192.168.x.x/24 | Client devices | Blocked from MGMT |
| 30 | SERVICES | 192.168.x.x/24 | All service containers | Controlled external access |
| 40 | DMZ | 192.168.x.x/24 | Public-facing services | Future use |
| 50 | MALWARE | 192.168.x.x/24 | Security lab | Air-gapped |

## 💻 Hardware Inventory

| Device | Hostname | Specifications | Role |
|--------|----------|----------------|------|
| **Proxmox Server** | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | Hypervisor |
| **pfSense Firewall** | — | AC8F Mini PC, Intel N100 | Router, Firewall |
| **Managed Switch** | — | TP-Link TL-SG108E | Layer 2, VLANs |
| **NAS Storage** | Kinmoon | UGREEN DXP2800, 3.6TB WD Purple | Backup target |
| **DNS Server** | — | Raspberry Pi 4 | Pi-hole (489K+ blocked domains) |
| **Gaming PC** | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB | Gaming only |
| **WiFi AP** | — | TP-Link EAP610 | Wireless (pending setup) |

### GPU Acceleration Ready
- RX 6700 XT earmarked for Ollama/ROCm AI inference
- IOMMU Group 18 (GPU), Group 19 (Audio)
- Idle baseline: CPU 48.5°C, GPU 46°C @ 5W

## 📦 Container Infrastructure

**14 LXC Containers** running on Proxmox, all with autostart enabled:

### Core Infrastructure
| Container | IP | Purpose | External Access |
|-----------|----|---------|-|
| nginx-proxy-manager | 192.168.x.201 | Reverse proxy | Internal |
| monitoring-prometheus | 192.168.x.202 | Metrics collection | Internal |
| monitoring-grafana | 192.168.x.203 | Dashboards | grafana.najhin-gaming.com |
| monitoring-loki | 192.168.x.204 | Log aggregation | Internal |
| monitoring-alertmanager | 192.168.x.205 | Alert routing | Internal |
| monitoring-uptime | 192.168.x.206 | Uptime monitoring | Internal |

### Automation & Management
| Container | IP | Purpose | External Access |
|-----------|----|---------|-|
| automation-n8n | 192.168.x.211 | Workflow automation | n8n.najhin-gaming.com |
| network-ddns | 192.168.x.207 | Dynamic DNS | Internal |
| dashboard-homepage | 192.168.x.208 | Service catalog | home.najhin-gaming.com |

### Security & Storage
| Container | IP | Purpose | External Access |
|-----------|----|---------|-|
| vault | 192.168.x.213 | Secrets management | vault.najhin-gaming.com |
| password-vaultwarden | 192.168.x.214 | Password manager | passwords.najhin-gaming.com |
| nextcloud-hub | 192.168.x.220 | File sync/collaboration | cloud.najhin-gaming.com |

### Gaming Platform
| Container | IP | Purpose | External Access |
|-----------|----|---------|-|
| gaming-panel | 192.168.x.210 | Pterodactyl panel | Internal |
| gaming-wings-1 | 192.168.x.212 | Game server node | terraria/mc.najhin-gaming.com |

## 🤖 Gilgamesh: AI Infrastructure Assistant

**Gilgamesh** is a sophisticated Telegram bot powered by Claude API and n8n workflows, serving as my personal AI assistant for homelab management.

### Core Features
- **Conversation Memory:** 20-message context retention via n8n Data Tables
- **Smart Routing:** Simple queries → Haiku 4, Complex → Sonnet 4
- **Web Search:** Real-time information via Claude's web search
- **Cost Tracking:** Token usage monitoring with spend alerts
- **Infrastructure Control:** Server status, metrics, backups, alerts
- **Documentation Automation:** Session summaries → GitHub via /update

### Available Commands
```
/help     - Command reference
/clear    - Reset conversation memory
/memory   - View recent messages
/cost     - Token usage and costs
/alerts   - Active system alerts
/backup   - Backup status
/update   - Merge session to docs
/sync-docs - Full doc regeneration
```

### Gilgamesh Evolution Roadmap
**Goal:** Replace Claude Pro subscription by August 2026, saving $300-360/year

**Phase 1 (Weeks 1-4):** Local LLM foundation  
**Phase 2 (Weeks 5-8):** Extended memory & hybrid routing  
**Phase 3 (Weeks 9-12):** File generation & vision capabilities  
**Phase 4 (Weeks 13-16):** Quality assurance & migration

## 📊 Monitoring Stack

### Observability Platform
- **Prometheus:** Metrics collection from all services and hardware
- **Grafana:** Real-time dashboards with custom visualizations
- **Loki:** Centralized log aggregation and analysis
- **Alertmanager:** Multi-channel alerting (Telegram, Discord)
- **Uptime Kuma:** Service availability monitoring

### Key Metrics Monitored
- Hardware: CPU, RAM, disk, network, temperatures
- Containers: Resource usage, health checks, log patterns
- Network: VLAN traffic, firewall blocks, DNS queries
- Applications: Response times, error rates, user sessions
- Business: Cost tracking, backup success rates, security events

## 🔒 Security Architecture

### Defense in Depth
1. **Perimeter Security:** ISP Router → pfSense with IDS/IPS
2. **Network Segmentation:** 5 VLANs with enforced firewall rules
3. **DNS Filtering:** Pi-hole blocking 489K+ malicious/ad domains
4. **VPN Access:** Tailscale mesh network for secure remote access
5. **Zero Trust:** Cloudflare Access with email OTP for public services
6. **Secrets Management:** HashiCorp Vault for API keys, certificates
7. **Password Security:** Vaultwarden for personal credential storage

### External Access Strategy
- **Admin Access:** Tailscale VPN only (VLAN 20 blocked from VLAN 10)
- **Public Services:** Cloudflare Tunnel with Access control
- **Gaming Services:** Direct exposure through pfSense (controlled)

## 💾 Backup Strategy

### Automated Daily Backups
| Schedule | Storage | Containers | Retention |
|----------|---------|------------|-----------|
| 02:00 | NAS (SMB) | Small containers (9) | 7 daily, 4 weekly, 2 monthly |
| 02:30 | Local HDD | Large containers (2) | 7 daily, 4 weekly, 2 monthly |

### Storage Architecture
- **Primary:** Local NVMe (LVM-Thin + ZFS) for container root disks
- **Backup:** NAS (3.6TB) + Local HDD (7.2TB) for redundancy
- **Planned:** Off-site backup via Backblaze B2

## 📈 Project Status

### Completed Phases (17 total)
- ✅ **Core Infrastructure:** Proxmox, pfSense, VLANs, monitoring
- ✅ **Gaming Platform:** Pterodactyl, Terraria, Minecraft, Windrose
- ✅ **AI Automation:** Gilgamesh bot, n8n workflows, documentation pipeline
- ✅ **Security:** Vault, Vaultwarden, Cloudflare Access, backup strategy
- ✅ **Storage & Sync:** Nextcloud, NAS integration, automated backups

### In Progress
- 📋 **Obsidian Knowledge Base:** Personal knowledge management
- 📋 **Ollama + ROCm:** Local AI inference with GPU acceleration

### Planned (40+ phases)
- 🎯 **AI Enhancement:** Extended memory, vision, hybrid routing
- 🎯 **Gaming Pipeline:** Discord bot, auto-scaling, status dashboards
- 🎯 **Business Automation:** n8n workflow marketplace
- 🎯 **Career Development:** Kubernetes, Ansible, Terraform

## 🛠️ Skills Demonstrated

### Infrastructure & Operations
- Hypervisor management (Proxmox VE)
- Network design & security (VLANs, firewalls, VPN)
- Container orchestration (LXC, Docker)
- Monitoring & observability (Prometheus, Grafana, Loki)
- Backup & disaster recovery strategies

### DevOps & Automation
- Infrastructure as Code practices
- CI/CD pipeline design (GitHub Actions)
- Configuration management
- Workflow automation (n8n)
- API integration and development

### Security & Compliance
- Zero-trust network architecture
- Secrets management (HashiCorp Vault)
- Multi-factor authentication
- Network segmentation & access control
- Security monitoring & incident response

### Cloud & Modern Practices
- GitOps workflows
- Containerization strategies
- API-first design principles
- Documentation as Code
- Cost optimization & monitoring

## 📚 Documentation Structure

### Public Repository
- `README.md` — Project overview (this file)
- `roadmap.md` — Complete phase roadmap with status
- `current-state.md` — Live infrastructure snapshot
- `service-catalog.md` — Service directory with access info
- `changelog.md` — Phase completion history
- `troubleshoot.md` — Common issues and solutions

### Private Documentation
- Sensitive configuration details
- API keys and credentials
- Internal IP addresses and network topology
- Security procedures and incident response

## 🔮 Long-term Vision

### Year 1 Goals (2026)
- Complete Gilgamesh AI assistant with local LLM capabilities
- Deploy full gaming platform with Discord integration
- Launch n8n workflow marketplace for revenue diversification
- Achieve 99.9% uptime with comprehensive monitoring

### Year 2+ Goals
- Kubernetes cluster migration from LXC containers
- Multi-site deployment with disaster recovery
- Open source contributions to homelab community
- Speaking engagements and technical blog

### Career Transition Targets
- **Q2 2026:** Junior Cloud Engineer role
- **Q4 2026:** Mid-level DevOps Engineer
- **2027:** Senior Infrastructure Engineer with team leadership

## 🤝 Community & Contact

This project is built with transparency and knowledge sharing in mind. I believe in documenting everything, sharing failures alongside successes, and contributing back to the homelab community.

**Connect with me:**
- 📧 Email: [Contact via GitHub]
- 💼 LinkedIn: [Professional updates and project showcases]
- 🐙 GitHub: [github.com/muzakkir97](https://github.com/muzakkir97)
- 🌐 Website: najhin-gaming.com

**Project Updates:** Follow along on LinkedIn for regular progress updates, technical deep-dives, and lessons learned.

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Last updated: April 24, 2026*