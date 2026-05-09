# 🏠 Homelab Infrastructure Project

[![Documentation](https://img.shields.io/badge/docs-gitHub-blue)](https://github.com/muzakkir97/homelab-infrastructure)
[![License](https://img.shields.io/badge/license-MIT-green)](LICENSE)
[![Cloudflare](https://img.shields.io/badge/domain-najhin--gaming.com-orange)](https://najhin-gaming.com)
[![Proxmox](https://img.shields.io/badge/hypervisor-Proxmox%20VE-red)](https://www.proxmox.com/)
[![Containers](https://img.shields.io/badge/containers-14%20LXC%20%2B%201%20VM-brightgreen)](#infrastructure-inventory)

> **Enterprise-grade homelab for career transition from cybersecurity to Cloud Engineering/DevOps**  
> Built with modern infrastructure practices, full automation, and professional documentation

---

## 🎯 Project Goals

This homelab serves as both a **learning environment** and **professional portfolio** for my career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering/DevOps**. Every component demonstrates enterprise-grade practices, automation, and security.

**Key Objectives:**
- Build production-ready infrastructure with proper monitoring, backup, and security
- Develop expertise in containerization, automation, and infrastructure-as-code
- Create a comprehensive knowledge base and documentation system
- Demonstrate skills through real-world implementations
- Replace external SaaS dependencies with self-hosted solutions

---

## 👤 About Me

**Muzakkir Kholil** | Customer Service Engineer @ F-Secure (Cybersecurity)  
**Location:** Petaling Jaya, Selangor, Malaysia  
**Target Role:** Cloud Engineering / DevOps  
**Domain:** [najhin-gaming.com](https://najhin-gaming.com)  
**GitHub:** [@muzakkir97](https://github.com/muzakkir97)

---

## 🏗️ Architecture Overview

```
                 Internet
                    ↓
             ISP Router (DHCP)
                    ↓
               pfSense Firewall
                    ↓
              802.1Q Trunk (igc2)
                    ↓
             TP-Link TL-SG108E
                    ↓
     ┌──────┬──────┬──────┬──────┬──────┐
  VLAN10  VLAN20  VLAN30  VLAN40  VLAN50
   Mgmt    Main  Services  DMZ   Malware
     ↓       ↓       ↓      ↓       ↓
Proxmox  Clients  14 LXC   Future  Security
  NAS    Gaming    1 VM    Public    Lab
Pi-hole                   Services
```

**Architecture Pattern:** Router-on-a-Stick with VLAN segmentation  
**Hypervisor:** Proxmox VE with LXC containers for efficiency  
**Network:** pfSense firewall with 5 VLANs for security segmentation  
**Storage:** Hybrid approach - local NVMe + NAS RAID 1 for backups

---

## 🌐 VLAN Design

| VLAN ID | Name            | Subnet           | Purpose                                |
|---------|-----------------|------------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24  | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24  | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24  | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24  | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24  | Isolated security lab (air-gapped)     |

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                              | Role                              |
|------------------|----------|------------------------------------|-----------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 128GB DDR4, RX 6700 XT | Hypervisor                        |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100           | Router, Firewall                  |
| Managed Switch   | —        | TP-Link TL-SG108E                  | Layer 2, VLANs                    |
| NAS              | Kinmoon  | UGREEN DXP2800, 5.4TB RAID 1      | Backups                           |
| DNS Server       | —        | Raspberry Pi 4                     | Pi-hole (~489K domains blocked)   |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB  | **Gaming only — never homelab**   |
| WiFi AP          | —        | TP-Link EAP610                     | Purchased, pending setup          |

**GPU Passthrough:** RX 6700 XT passed through to VM 400 for AI inference with Ollama/ROCm

---

## 📦 Container Inventory

**Total: 14 LXC Containers + 1 KVM VM**  
*All containers on VLAN 30 (192.168.30.0/24) with autostart enabled*

### Core Infrastructure
- **201** - nginx-proxy-manager (reverse proxy)
- **202** - monitoring-prometheus (metrics collection)
- **203** - monitoring-grafana (dashboards)
- **204** - monitoring-loki (log aggregation)
- **205** - monitoring-alertmanager (alert routing)
- **206** - monitoring-uptime (service monitoring)

### Services & Automation
- **207** - network-ddns (dynamic DNS)
- **208** - dashboard-homepage (service dashboard)
- **211** - automation-n8n (workflow automation)
- **213** - vault (secrets management)
- **214** - password-vaultwarden (password manager)
- **220** - nextcloud-hub (file sync & collaboration)

### Gaming Platform
- **300** - gaming-panel (Pterodactyl control panel)
- **302** - gaming-wings-1 (game servers: Terraria, Minecraft, Windrose)

### AI Infrastructure
- **400** - ollama-gpu (KVM VM with GPU passthrough for local AI)

---

## 🤖 Gilgamesh AI Ecosystem

**Theme:** Homelab agents named after Fate/Grand Order servants

### Active Agents

| Servant      | Role                           | Platform                      | Status    |
|--------------|--------------------------------|-------------------------------|-----------|
| Gilgamesh 👑 | Personal AI Assistant          | Telegram (@JhinGilgamesh_bot) | ✅ Active |
| Da Vinci 🎨  | Chief Intelligence Officer     | n8n/Nextcloud                 | ⚡ Partial |
| Midas 💰     | CFO — Cost Tracking            | n8n                           | ✅ Active |
| MERLIN 🔮    | Reminders & Scheduler          | n8n                           | ✅ Active |

**Gilgamesh Features:**
- Smart routing: Ollama (local) → Haiku (fallback) → Sonnet (complex)
- 20-message conversation memory
- Real-time web search capabilities  
- Infrastructure monitoring and control
- Health tracking and daily notes
- Cost optimization and usage analytics

**Planned Agents:** Guardian (security), Mash (gaming Discord bot), EMIYA (infrastructure automation), and 5 more specialized agents

---

## 📊 Monitoring Stack

**Comprehensive observability with professional-grade tools:**

- **Prometheus** - Metrics collection and storage
- **Grafana** - Visualization dashboards and alerting
- **Loki** - Centralized log aggregation
- **Alertmanager** - Alert routing to Telegram/Discord
- **Uptime Kuma** - Service availability monitoring
- **Node Exporter** - System metrics collection

**Alert Channels:**
- Critical alerts → Telegram (immediate response)
- Warning alerts → Discord (team awareness)
- Infrastructure health → Daily briefings via MERLIN agent

---

## 🔒 Security Architecture

| Layer           | Implementation                                                    |
|-----------------|-------------------------------------------------------------------|
| Perimeter       | pfSense firewall with VLAN segmentation                          |
| DNS Security    | Pi-hole blocking ~489K malicious/ad domains                      |
| External Access | Cloudflare Tunnel + Access (Email OTP, whitelisted email only)   |
| VPN Access      | Tailscale mesh network for administrative access                  |
| Secrets         | HashiCorp Vault with KV engine for all credentials               |
| Passwords       | Vaultwarden (Bitwarden-compatible) for personal use              |
| Backup          | Daily automated backups with 7/4/2 retention                     |
| Network Isolation | 5 VLANs with strict firewall rules                              |

**Zero Trust Principles:** No service exposed without authentication, minimal attack surface, encrypted communication

---

## 💾 Backup Strategy

**Automated daily backups with professional retention:**

| Job              | Schedule | Storage                  | Containers                 | Retention                    |
|------------------|----------|--------------------------|----------------------------|------------------------------|
| Small Containers | 02:00    | NAS RAID 1 (NFS)         | 9 core infrastructure      | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | Local HDD                | Nextcloud, Gaming servers  | 7 daily, 4 weekly, 2 monthly |

**Storage Redundancy:** RAID 1 mirrored drives on NAS for critical backups

---

## ✅ Completed Phases

**Core Infrastructure (6 phases)** - Proxmox, pfSense, VLANs, monitoring, SSL  
**Gaming Platform (4 phases)** - Pterodactyl, game servers, dashboard  
**Storage & Backup (2 phases)** - Nextcloud, NAS deployment, backup automation  
**Security (3 phases)** - Vault, Vaultwarden, Cloudflare Access  
**Automation (8 phases)** - n8n workflows, Gilgamesh AI, documentation pipeline  
**Knowledge Management (4 phases)** - Obsidian vault, daily notes, health tracking  
**AI Infrastructure (3 phases)** - Ollama deployment, GPU passthrough, hybrid routing  

**Total: 41 completed phases**

---

## 🚧 In Progress

- **Phase 22.8C** - Homepage dashboard widgets for health and infrastructure
- **RAM Upgrade** - 128GB DDR4 (4x32GB) ordered from Taobao
- **Guardian Agent** - Security monitoring and threat detection
- **Da Vinci Stage 2** - RAG system for knowledge retrieval

---

## 📋 Planned Phases

**Next 16 weeks - Replace Claude.ai with local AI by August 2026**

### Gilgamesh & Automation (Priority 1)
- **Guardian Agent** - Security monitoring and alerting
- **EMIYA CTO** - Infrastructure automation with approval gates (8 sub-phases)
- **Da Vinci Stage 2** - RAG system with Qdrant vector database
- **Multi-agent Communication** - Inter-agent messaging and coordination

### Gaming Platform Pipeline (Priority 2)  
- **Mash Discord Bot** - Gaming server management via Discord (6 phases)
- **Advanced Game Management** - Auto-scaling, update management, player analytics

### Personal & Knowledge Management (Priority 3)
- **Life Ledger** - Comprehensive personal data tracking with vision AI
- **Homepage Dashboards** - Financial, health, and infrastructure widgets
- **Advanced Analytics** - Spending analysis, health trends, infrastructure optimization

### Infrastructure Cleanup & Optimization (Priority 4)
- **Network Modernization** - WiFi AP deployment, legacy network cleanup
- **Storage Optimization** - Data directory migration, thin pool management
- **Performance Tuning** - Resource optimization, monitoring improvements

---

## 🎓 Skills Demonstrated

**Infrastructure & DevOps:**
- Hypervisor management (Proxmox VE)
- Container orchestration (LXC, Docker)
- Network design & security (VLANs, firewalls, VPN)
- Monitoring & observability (Prometheus, Grafana, Loki)
- Infrastructure as Code practices
- Backup & disaster recovery
- GPU passthrough for AI workloads

**Automation & Development:**
- Workflow automation (n8n)
- AI agent development
- API integration & webhooks
- Documentation automation
- Cost optimization systems
- Health & performance monitoring

**Security:**
- Zero-trust network architecture
- Secrets management (Vault)
- Access control & authentication
- Network segmentation
- Threat monitoring & alerting

---

## 📚 Documentation Structure

| Document           | Purpose                                          |
|--------------------|--------------------------------------------------|
| README.md          | Project overview and public documentation        |
| roadmap.md         | Complete phase roadmap with dependencies         |
| current-state.md   | Live infrastructure snapshot                     |
| service-catalog.md | Complete service inventory with access details   |
| changelog.md       | Detailed change history                          |
| troubleshoot.md    | Common issues and solutions                      |
| AI-CONTEXT.md      | Master context for AI assistants (private)       |

**Update Process:** Automated documentation pipeline via Gilgamesh AI agent with manual review

---

## 🔮 Long-term Vision

**Year 1 Goals:**
- Complete transition from external SaaS to self-hosted solutions
- Deploy production-ready CI/CD pipeline
- Implement Infrastructure as Code (Terraform/Ansible)
- Build comprehensive security monitoring

**Year 2-3 Goals:**
- Kubernetes cluster deployment
- Multi-node infrastructure expansion
- Advanced AI/ML workload deployment
- Open source contributions

**Career Impact:**
- Demonstrate enterprise-grade infrastructure skills
- Build portfolio for Cloud Engineering roles
- Develop expertise in modern DevOps practices
- Create knowledge base for continuous learning

---

## 📞 Contact & Collaboration

**GitHub:** [@muzakkir97](https://github.com/muzakkir97)  
**LinkedIn:** Professional updates posted after each major phase  
**Domain:** [najhin-gaming.com](https://najhin-gaming.com) (external services)

**Collaboration Welcome:**
- Infrastructure architecture discussions
- Automation and AI agent development
- Security best practices
- Career transition insights

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Building the future, one container at a time* 🚀