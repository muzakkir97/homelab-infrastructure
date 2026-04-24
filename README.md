# 🏠 Enterprise-Grade Homelab Infrastructure

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Infrastructure](https://img.shields.io/badge/Infrastructure-Proxmox-orange)](https://www.proxmox.com/)
[![Monitoring](https://img.shields.io/badge/Monitoring-Grafana-blue)](https://grafana.com/)
[![Automation](https://img.shields.io/badge/Automation-n8n-purple)](https://n8n.io/)
[![AI Assistant](https://img.shields.io/badge/AI-Gilgamesh-gold)](https://telegram.me/JhinGilgamesh_bot)

> **Enterprise-grade homelab built for career transition from Customer Service Engineer to Cloud Engineering/DevOps**
>
> **Current Status:** Phase 22 complete (Obsidian Knowledge Base) • 14 LXC containers running • Production-ready monitoring stack

---

## 🎯 Project Goals

This homelab serves as both a **learning environment** and **professional portfolio** for my career transition from Customer Service Engineer at F-Secure (cybersecurity) to Cloud Engineering/DevOps roles. The infrastructure demonstrates enterprise patterns, security best practices, and modern DevOps tooling at scale.

### Primary Objectives
- **Skills Development:** Hands-on experience with enterprise technologies
- **Career Transition:** Portfolio demonstrating cloud/DevOps capabilities
- **Professional Documentation:** GitHub-based knowledge sharing
- **Cost Optimization:** Replace $30-40/month Claude Pro with local AI by August 2026

---

## 👤 About Me

**Muzakkir Kholil** — Customer Service Engineer at F-Secure, transitioning to Cloud Engineering/DevOps

- **Current Role:** Customer Service Engineer @ F-Secure (cybersecurity)
- **Location:** Petaling Jaya, Selangor, Malaysia
- **Target Role:** Cloud Engineering / DevOps
- **Domain:** najhin-gaming.com (Cloudflare managed)
- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)

---

## 🏗️ Architecture Overview

### Network Topology: Router-on-a-Stick

```
Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
                                              ↓
                                    802.1Q Trunk (igc2)
                                              ↓
                                    TP-Link TL-SG108E
                                              ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

### VLAN Design

| VLAN ID | Name            | Subnet              | Gateway        | Purpose                                |
|---------|-----------------|---------------------|----------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.x.x/24      | 192.168.x.x    | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.x.x/24      | 192.168.x.x    | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.x.x/24      | 192.168.x.x    | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.x.x/24      | 192.168.x.x    | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.x.x/24      | 192.168.x.x    | Isolated security lab (air-gapped)     |

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                     | Role                                 |
|------------------|----------|-------------------------------------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB  | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                  | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                         | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple           | Backups                              |
| DNS Server       | —        | Raspberry Pi 4                            | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB          | **Gaming only — never homelab**      |
| WiFi AP          | —        | TP-Link EAP610                            | Purchased, pending setup             |

### GPU Notes
- **RX 6700 XT** in IOMMU Group 18 (earmarked for Ollama/ROCm AI inference)
- **Idle baseline:** CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan

---

## 📦 Container Inventory (14 LXC Containers)

All containers run on VLAN 30 (Services) with autostart enabled:

| CTID | Name                    | Purpose                           | External Access               |
|------|-------------------------|-----------------------------------|-------------------------------|
| 201  | nginx-proxy-manager     | Reverse proxy                     | —                             |
| 202  | monitoring-prometheus   | Metrics collection                | —                             |
| 203  | monitoring-grafana      | Metrics visualization             | grafana.najhin-gaming.com     |
| 204  | monitoring-loki         | Log aggregation                   | —                             |
| 205  | monitoring-alertmanager | Alert management                  | —                             |
| 206  | monitoring-uptime       | Uptime monitoring                 | —                             |
| 207  | network-ddns            | Dynamic DNS updates               | —                             |
| 208  | dashboard-homepage      | Infrastructure dashboard          | home.najhin-gaming.com        |
| 211  | automation-n8n          | Workflow automation               | n8n.najhin-gaming.com         |
| 213  | vault                   | Secrets management                | vault.najhin-gaming.com       |
| 214  | password-vaultwarden    | Password manager                  | passwords.najhin-gaming.com   |
| 220  | nextcloud-hub           | Cloud storage & collaboration     | cloud.najhin-gaming.com       |
| 300  | gaming-panel            | Pterodactyl panel                 | —                             |
| 302  | gaming-wings-1          | Game servers (Terraria, Minecraft, Windrose) | terraria/mc.najhin-gaming.com |

---

## 🤖 Gilgamesh AI Assistant

**Primary AI agent** built on Telegram (@JhinGilgamesh_bot) for homelab administration and personal assistance.

### Key Features
- **Conversation Memory:** Last 20 messages stored for context
- **Smart Routing:** Simple queries → Haiku 4.5, complex → Sonnet 4
- **Web Search:** Real-time information via Claude's web_search tool
- **Infrastructure Control:** Server status, metrics, temperatures, storage
- **Documentation Pipeline:** Auto-update project docs via /update and /sync-docs
- **Cost Tracking:** Token usage monitoring and optimization

### Slash Commands (8 total)
- `/help` — Command reference
- `/clear` — Reset conversation memory
- `/memory` — View conversation history
- `/cost` — Usage and cost tracking
- `/alerts` — Active Alertmanager alerts
- `/backup` — Backup status for all containers
- `/update` — Merge session summary into docs
- `/sync-docs` — Full documentation regeneration

### Gilgamesh Evolution Goal
**Replace claude.ai with Gilgamesh by August 2026 (16-week roadmap)**
- **Current Cost:** $30-40/month (Claude Pro + API)
- **Target Cost:** $5-10/month (local LLM + occasional API)
- **Projected Savings:** $300-360/year

---

## 📊 Monitoring Stack

Comprehensive infrastructure monitoring using modern observability tools:

- **Prometheus** (CT 202) — Metrics collection and storage
- **Grafana** (CT 203) — Visualization dashboards
- **Loki** (CT 204) — Log aggregation and search
- **Alertmanager** (CT 205) — Alert routing and notification
- **Uptime Kuma** (CT 206) — Service availability monitoring

### Alert Channels
- **Telegram:** @JhinGilgamesh_bot for critical alerts
- **Discord:** #alerts channel for warnings

---

## 🔒 Security Architecture

Multi-layered security approach following enterprise best practices:

| Layer           | Implementation                                                                    |
|-----------------|-----------------------------------------------------------------------------------|
| **Perimeter**   | ISP Router → pfSense firewall                                                     |
| **Segmentation** | 5 VLANs with enforced firewall rules                                              |
| **DNS Filtering** | Pi-hole ad/tracker blocking (~489K domains)                                       |
| **VPN Access**  | Tailscale (subnet router on pfSense)                                              |
| **External Auth** | Cloudflare Access (email OTP) for 7 public services                               |
| **Tunnel Access** | Cloudflare Tunnel for Nextcloud                                                   |
| **Secrets Management** | HashiCorp Vault (8 secret paths) + Vaultwarden (passwords)                        |
| **Backup Strategy** | Automated daily backups (7/4/2 retention)                                         |

---

## 💾 Backup Strategy

Automated backup system with hybrid storage approach:

### Backup Jobs
- **Small Containers:** 02:00 daily → NAS (SMB/CIFS, 3.6TB)
- **Large Containers:** 02:30 daily → Local HDD (7.2TB)
- **Retention:** 7 daily, 4 weekly, 2 monthly

### Storage Pools
- **kinmoon-smb:** NAS storage for small container backups
- **data-storage:** Local HDD for large container backups  
- **local-lvm:** Local NVMe for container root disks (137GB)
- **vmpool-fast:** ZFS pool for high-performance storage (899GB)

---

## 📝 Project Status

### Completed Phases (22 phases)
- **Infrastructure:** Proxmox, pfSense, VLANs, core services
- **Monitoring:** Complete observability stack with Grafana dashboards
- **Security:** HashiCorp Vault, Vaultwarden, Cloudflare Access
- **Automation:** n8n workflows, Gilgamesh AI assistant
- **Gaming Platform:** Pterodactyl panel with Terraria/Minecraft servers
- **Knowledge Management:** Obsidian vault with 6 plugins
- **Documentation:** Automated pipeline for GitHub integration

### In Progress
- **Phase 38:** Ollama + ROCm on Kuromoon RX 6700 XT (AI inference)

### Near-Term Planned (Next 3-6 months)
- **Phase 24.1:** Service Update Manager (approval-gated)
- **Phase 41:** Hybrid AI routing (local/cloud based on complexity)
- **Gilgamesh Extended Memory:** 20+ message conversations via RAG
- **MERLIN Reminder Agent:** SSL renewals, backup tests, maintenance
- **Gaming Platform Pipeline:** Discord bot, automated deployments

---

## 🎓 Skills Demonstrated

### Infrastructure & Virtualization
- **Proxmox VE:** LXC containers, storage management, clustering concepts
- **pfSense:** Advanced firewall rules, VLANs, VPN configuration
- **Networking:** 802.1Q trunking, VLAN segmentation, DNS management

### Monitoring & Observability  
- **Prometheus:** Metric collection, PromQL queries, alerting rules
- **Grafana:** Dashboard creation, visualization, alert management
- **Loki:** Log aggregation, LogQL queries, structured logging

### Security & Compliance
- **Zero Trust:** Network segmentation, principle of least privilege
- **Secrets Management:** HashiCorp Vault, credential rotation
- **External Access:** Cloudflare Access, tunnel-based connectivity

### Automation & DevOps
- **Infrastructure as Code:** Documented, repeatable deployments
- **CI/CD Concepts:** Automated testing, deployment pipelines
- **Workflow Automation:** n8n for complex business logic

### AI & Machine Learning
- **LLM Integration:** Claude API, conversation memory, cost optimization
- **Model Deployment:** Preparing for local Ollama inference
- **Intelligent Automation:** Context-aware decision making

---

## 📚 Documentation Structure

This project maintains comprehensive documentation across multiple formats:

### Public Documentation (GitHub)
- **README.md** — Project overview and architecture
- **roadmap.md** — Complete phase timeline with dependencies
- **current-state.md** — Live infrastructure snapshot
- **service-catalog.md** — Service directory with access details

### Private Documentation
- **AI-CONTEXT.md** — Complete technical context for AI assistants
- **changelog.md** — Detailed change history
- **troubleshoot.md** — Issue resolutions and lessons learned

### Knowledge Management
- **Obsidian Vault** — Personal knowledge base with automated sync
- **Session Summaries** — Detailed progress tracking per phase

---

## 🔮 Long-Term Vision

### Career Goals
- **Cloud Architect:** Design and implement large-scale cloud infrastructures
- **DevOps Engineering:** Build and maintain CI/CD pipelines at enterprise scale
- **Site Reliability Engineering:** Ensure high availability of critical systems

### Technical Evolution
- **AI-First Operations:** Intelligent automation for routine tasks
- **Multi-Cloud Deployment:** AWS, Azure, GCP integration
- **Kubernetes Migration:** Container orchestration at scale
- **Edge Computing:** Distributed processing capabilities

### Business Applications
- **Consulting Services:** Homelab expertise for SMB clients
- **Content Creation:** Technical blog, YouTube channel
- **Community Contribution:** Open-source tools and documentation

---

## 🤝 Contact & Connect

I'm always interested in connecting with fellow homelab enthusiasts, cloud engineers, and DevOps professionals!

- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)
- **LinkedIn:** Connect for career discussions and technical insights
- **Email:** Available through GitHub profile

### Current Focus
- **Seeking:** Cloud Engineering / DevOps opportunities in Malaysia/Remote
- **Learning:** Kubernetes, Terraform, AWS/Azure certifications
- **Building:** Advanced AI automation for infrastructure management

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*Last updated: April 24, 2026 — Infrastructure documentation auto-generated via Gilgamesh AI assistant*