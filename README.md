# 🏗️ Homelab Infrastructure Project

[![Infrastructure](https://img.shields.io/badge/Infrastructure-Proxmox%20VE-orange)](https://www.proxmox.com/)
[![Monitoring](https://img.shields.io/badge/Monitoring-Prometheus%20%2B%20Grafana-blue)](https://prometheus.io/)
[![Automation](https://img.shields.io/badge/Automation-n8n-ff6d5a)](https://n8n.io/)
[![AI](https://img.shields.io/badge/AI-Ollama%20%2B%20ROCm-green)](https://ollama.ai/)
[![Gaming](https://img.shields.io/badge/Gaming-Pterodactyl-yellow)](https://pterodactyl.io/)
[![License](https://img.shields.io/badge/License-MIT-brightgreen.svg)](LICENSE)

> **Enterprise-grade homelab for career transition from Customer Service Engineer (F-Secure, cybersecurity) to Cloud Engineering / DevOps**

---

## 🎯 Project Goals

This homelab serves as both a **learning environment** and **professional portfolio** for my transition to Cloud Engineering/DevOps. The infrastructure demonstrates enterprise-grade practices, automation, monitoring, and security implementations suitable for production environments.

**Current Status:** 14 LXC containers + 1 KVM VM running across 5 VLANs with complete monitoring, automation, and AI integration.

## 👤 About Me

| Field            | Details                                              |
|------------------|------------------------------------------------------|
| **Name**         | Muzakkir Kholil                                      |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location**     | Petaling Jaya, Selangor, Malaysia                    |
| **Target Role**  | Cloud Engineering / DevOps                           |
| **Domain**       | najhin-gaming.com (Cloudflare)                       |
| **GitHub**       | github.com/muzakkir97                                |

## 🏗️ Architecture Overview

```
Internet → ISP Router → pfSense Firewall
                            ↓
                    802.1Q Trunk to Switch
                            ↓
        ┌─────────┬─────────┬─────────┬─────────┬─────────┐
      VLAN10    VLAN20    VLAN30    VLAN40    VLAN50
      Mgmt      Main     Services    DMZ     Malware
   (Proxmox)  (Clients) (Containers) (Future) (Isolated)
```

### VLAN Design

| VLAN ID | Name            | Subnet              | Purpose                                |
|---------|-----------------|---------------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.x.x/24     | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.x.x/24     | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.x.x/24     | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.x.x/24     | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.x.x/24     | Isolated security lab (air-gapped)     |

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                    | Role                              |
|------------------|----------|------------------------------------------|-----------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | Hypervisor                        |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                 | Router, Firewall                  |
| Managed Switch   | —        | TP-Link TL-SG108E                        | Layer 2, VLANs                    |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple          | Backups (SMB target)              |
| DNS Server       | —        | Raspberry Pi 4                           | Pi-hole (~489K domains blocked)   |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB         | **Gaming only — never homelab**   |
| WiFi AP          | —        | TP-Link EAP610                           | Purchased, pending setup          |

### GPU Passthrough Notes
- RX 6700 XT passed through to VM 400 (ollama-gpu) for AI inference
- IOMMU isolation with zero-RPM fan control when idle
- HSA_OVERRIDE_GFX_VERSION=10.3.0 for gfx1031 compatibility

## 📦 Container Inventory

**Total: 14 LXC containers + 1 KVM VM, all autostart enabled**

| ID   | Type | Name                    | Subdomain                     | Purpose                    |
|------|------|-------------------------|-------------------------------|----------------------------|
| 201  | LXC  | nginx-proxy-manager     | —                             | Reverse proxy, SSL         |
| 202  | LXC  | monitoring-prometheus   | —                             | Metrics collection         |
| 203  | LXC  | monitoring-grafana      | grafana.[DOMAIN]              | Metrics visualization      |
| 204  | LXC  | monitoring-loki         | —                             | Log aggregation            |
| 205  | LXC  | monitoring-alertmanager | —                             | Alert routing              |
| 206  | LXC  | monitoring-uptime       | —                             | Uptime monitoring          |
| 207  | LXC  | network-ddns            | —                             | Dynamic DNS updates        |
| 208  | LXC  | dashboard-homepage      | home.[DOMAIN]                 | Infrastructure dashboard   |
| 211  | LXC  | automation-n8n          | n8n.[DOMAIN]                  | Workflow automation        |
| 213  | LXC  | vault                   | vault.[DOMAIN]                | Secrets management         |
| 214  | LXC  | password-vaultwarden    | passwords.[DOMAIN]            | Password manager           |
| 220  | LXC  | nextcloud-hub           | cloud.[DOMAIN]                | File sync, knowledge base  |
| 300  | LXC  | gaming-panel            | —                             | Pterodactyl panel          |
| 302  | LXC  | gaming-wings-1          | terraria/mc.[DOMAIN]          | Game servers (Docker)      |
| 400  | VM   | ollama-gpu              | ollama.[DOMAIN]               | Local LLM with GPU         |

## 🤖 Gilgamesh AI Agent Ecosystem

**Theme:** Fate/Grand Order servants as homelab agents

### Active Agents

| Agent          | Role                                | Platform | Status              |
|----------------|-------------------------------------|----------|---------------------|
| Gilgamesh 👑   | Personal AI Assistant               | Telegram | ✅ Active            |
| Da Vinci 🎨    | Chief Intelligence Officer          | n8n      | ⚡ Partial (Stage 1) |
| Midas 💰       | CFO — Cost Tracking & Optimization | n8n      | ✅ Active            |
| MERLIN 🔮      | Reminders & Scheduler               | n8n      | ✅ Active            |

### Gilgamesh Features

- **Smart routing:** Ollama qwen3:14b (local) → Claude Haiku (fallback) → Claude Sonnet (complex)
- **Conversation memory:** Last 20 messages with cost tracking
- **Web search:** Real-time information via Claude's web_search tool
- **Infrastructure control:** Container status, metrics, alerts, backups
- **Documentation pipeline:** Session summaries → GitHub via /update and /sync-docs
- **Slash commands:** 10 commands for direct homelab management

### Cost Optimization Goal
**Replace Claude.ai Pro ($30-40/month) with Gilgamesh by August 2026**
- Target cost: $5-10/month (local LLM + occasional API)
- Projected annual savings: $300-360

## 📊 Monitoring Stack

| Component    | Purpose                   | Container |
|--------------|---------------------------|-----------|
| Prometheus   | Metrics collection        | CT 202    |
| Grafana      | Visualization dashboards  | CT 203    |
| Loki         | Log aggregation           | CT 204    |
| Alertmanager | Alert routing & silencing | CT 205    |
| Uptime Kuma  | Service availability      | CT 206    |

**Alerting:** Telegram (critical) + Discord (warnings) with plain English translations

## 🔒 Security Architecture

| Layer           | Implementation                                                                          |
|-----------------|-----------------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall with VLAN segmentation                                   |
| DNS Security    | Pi-hole blocking ~489K malicious/tracking domains                                       |
| VPN Access      | Tailscale subnet router for secure remote access                                        |
| External Auth   | Cloudflare Access with Email OTP for 7 public services                                  |
| Secrets Mgmt    | HashiCorp Vault (8 secret paths) + Vaultwarden password manager                        |
| Network Segm.   | 5 VLANs with enforced firewall rules and isolated malware lab                          |
| Backup Security | Automated daily backups with 7/4/2 retention to NAS and local storage                  |

## 💾 Backup Strategy

| Job              | Schedule | Storage                  | Retention                     |
|------------------|----------|--------------------------|-------------------------------|
| Small Containers | 02:00    | NAS (SMB)                | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | Local HDD (7.3TB)        | 7 daily, 4 weekly, 2 monthly |

**Coverage:** All containers backed up nightly with automated monitoring and alerting

## 📚 Knowledge Management (Obsidian)

- **Vault:** Auto-synced via Nextcloud to Gaming PC
- **Structure:** 10 organized folders with templates and automation
- **Features:** Daily notes (auto-created), subscription tracking, health logs
- **Integration:** n8n creates daily notes, morning briefings via Telegram
- **Plugins:** Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban

## 📋 Project Phases

### ✅ Completed (25 phases)
- **Phase 1-6F:** Core infrastructure, monitoring, gaming platform
- **Phase 7-7D:** Nextcloud, backups, AI agent foundation  
- **Phase 9:** NAS deployment
- **Phase 13-15:** Vault secrets management, Gilgamesh enhancements
- **Phase 16.1-16.3:** Documentation pipeline with Da Vinci
- **Phase 22-22.2:** Obsidian knowledge base with daily automation
- **Phase 23:** Vaultwarden password manager
- **Phase 38-41:** Local LLM with GPU acceleration and hybrid routing
- **Phase 58:** Windrose game server
- **Midas & MERLIN:** Active cost tracking and reminder agents

### 🚧 In Progress
- **Guardian Agent:** Security monitoring (next agent to build)
- **Phase 7E:** Extended memory for 20+ message conversations
- **Da Vinci Stage 2:** RAG retrieval from knowledge base

### 📋 Planned (45+ phases)
- **EMIYA Agent:** Infrastructure automation with approval gates
- **Gaming Platform:** Discord bot, automated tournaments
- **AI Evolution:** Vision API, document processing, quality assurance
- **Infrastructure:** WiFi deployment, off-site backups, cleanup

## 🚀 Skills Demonstrated

### Infrastructure & DevOps
- **Virtualization:** Proxmox VE with LXC containers and KVM VMs
- **Networking:** VLANs, firewall rules, DNS management, VPN
- **Monitoring:** Prometheus, Grafana, Loki, Alertmanager stack
- **Backup & DR:** Automated backup strategies with retention policies
- **Security:** Network segmentation, secrets management, access controls

### Automation & Development  
- **Workflow Automation:** n8n with 12 active workflows
- **AI Integration:** Local LLMs, API routing, cost optimization
- **Documentation:** Automated pipeline with Git integration
- **Scripting:** Bash, systemd services, configuration management

### Cloud & Modern Practices
- **Containerization:** Docker, container orchestration
- **Infrastructure as Code:** Configuration management, version control
- **GitOps:** Automated documentation, change tracking
- **Monitoring & Observability:** Metrics, logs, alerts, dashboards
- **Cost Optimization:** Resource allocation, usage tracking

## 📁 Documentation Structure

```
homelab-infrastructure/
├── README.md              # This overview (public)
├── roadmap.md             # Complete phase roadmap
├── current-state.md       # Live infrastructure snapshot
├── service-catalog.md     # All services with access details
├── docs/
│   ├── troubleshoot.md    # Common issues and solutions
│   └── changelog.md       # Recent changes and updates
└── LICENSE                # MIT License
```

## 🔮 Long-term Vision

**Goal:** Enterprise-grade homelab demonstrating production-ready skills for Cloud Engineering/DevOps roles.

**Target State by August 2026:**
- Complete AI agent ecosystem replacing external services
- Advanced automation with infrastructure self-healing
- Comprehensive gaming platform with tournament automation
- Professional portfolio showcasing modern DevOps practices

**Career Impact:**
- Hands-on experience with enterprise tools and practices
- Demonstrable automation and monitoring expertise  
- Cost optimization and resource management skills
- Documentation and process improvement experience

## 📞 Contact

- **GitHub:** [@muzakkir97](https://github.com/muzakkir97)
- **LinkedIn:** [muzakkir-kholil](https://linkedin.com/in/muzakkir-kholil)
- **Email:** muzakkir.kholil06@gmail.com
- **Domain:** najhin-gaming.com

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.