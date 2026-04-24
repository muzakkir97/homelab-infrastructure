# 🏠 Homelab Infrastructure Project

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Proxmox](https://img.shields.io/badge/Hypervisor-Proxmox_VE-E57000?logo=proxmox)](https://www.proxmox.com/)
[![pfSense](https://img.shields.io/badge/Firewall-pfSense-212121?logo=pfsense)](https://www.pfsense.org/)
[![Grafana](https://img.shields.io/badge/Monitoring-Grafana-F46800?logo=grafana&logoColor=white)](https://grafana.com/)
[![Telegram](https://img.shields.io/badge/Bot-Telegram-26A5E4?logo=telegram&logoColor=white)](https://telegram.org/)
[![Documentation](https://img.shields.io/badge/Docs-Auto_Generated-brightgreen)](https://github.com/muzakkir97/homelab-infrastructure)

> **Enterprise-grade homelab for career transition from Customer Service Engineer (F-Secure, cybersecurity) to Cloud Engineering / DevOps**

---

## 📋 Project Overview

This project showcases an **enterprise-grade homelab infrastructure** built for learning, experimentation, and career development. Currently running **15 containers** across **4 VLANs** with comprehensive monitoring, automation, and security implementations.

**Current Status:** Phase 39 complete (Open WebUI). 15 containers running across gaming platform, monitoring stack, automation workflows, and local AI inference.

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

## 🌐 VLAN Design

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.x.x/24  | 192.168.x.1  | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.x.x/24  | 192.168.x.1  | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.x.x/24  | 192.168.x.1  | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.x.x/24  | 192.168.x.1  | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.x.x/24  | 192.168.x.1  | Isolated security lab (air-gapped)     |

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                     | Role                                 |
|------------------|----------|-------------------------------------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB  | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                  | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                         | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple           | Backups (SMB target)                 |
| DNS Server       | —        | Raspberry Pi 4                            | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB          | Gaming only — never homelab          |
| WiFi AP          | —        | TP-Link EAP610                            | Purchased, pending setup             |

## 📦 Container Inventory (15 Total)

| CTID | Name                    | Subdomain                     | Status     |
|------|-------------------------|-------------------------------|------------|
| 201  | nginx-proxy-manager     | —                             | ✅ Running |
| 202  | monitoring-prometheus   | —                             | ✅ Running |
| 203  | monitoring-grafana      | grafana.najhin-gaming.com     | ✅ Running |
| 204  | monitoring-loki         | —                             | ✅ Running |
| 205  | monitoring-alertmanager | —                             | ✅ Running |
| 206  | monitoring-uptime       | —                             | ✅ Running |
| 207  | network-ddns            | —                             | ✅ Running |
| 208  | dashboard-homepage      | home.najhin-gaming.com        | ✅ Running |
| 211  | automation-n8n          | n8n.najhin-gaming.com         | ✅ Running |
| 213  | vault                   | vault.najhin-gaming.com       | ✅ Running |
| 214  | password-vaultwarden    | passwords.najhin-gaming.com   | ✅ Running |
| 220  | nextcloud-hub           | cloud.najhin-gaming.com       | ✅ Running |
| 300  | gaming-panel            | —                             | ✅ Running |
| 302  | gaming-wings-1          | terraria/mc.najhin-gaming.com | ✅ Running |
| 400  | ollama-gpu              | ollama.najhin-gaming.com      | ✅ Running |

All containers run on **VLAN 30** with **autostart enabled**.

## 🤖 Gilgamesh AI Agent

**Personal AI assistant** via Telegram with advanced features:
- **Conversation memory:** Last 20 messages stored in n8n Data Tables
- **Smart routing:** Simple inputs → Haiku 4.5, complex queries → Sonnet 4
- **Web search:** Real-time information via Claude's web_search tool
- **Infrastructure control:** Server status, metrics, alerts, backup status
- **Documentation pipeline:** Auto-updates GitHub docs with session summaries
- **Slash commands:** 8 commands for direct homelab management
- **Inline menu:** Full interactive menu system with submenus

### Gilgamesh Evolution Goal
**Replace claude.ai Pro subscription by August 2026** — transition from $30-40/month to $5-10/month using local Ollama + selective API usage.

## 🎮 Gaming Platform

| Service    | Container | Subdomain                     | Status     |
|------------|-----------|-------------------------------|------------|
| Pterodactyl Panel | CT 300 | —                      | ✅ Running |
| Wings Node | CT 302    | —                             | ✅ Running |
| Terraria   | CT 302    | terraria.najhin-gaming.com    | ✅ Running |
| Minecraft  | CT 302    | mc.najhin-gaming.com          | ✅ Running |
| Windrose   | CT 302    | —                             | ✅ Running |

**Windrose Server:** 4 max players, Medium difficulty, invite code NAJHINWINDROSE.

## 📊 Monitoring Stack

| Service      | Purpose                  | Container | Subdomain                |
|--------------|--------------------------|-----------|--------------------------|
| Prometheus   | Metrics collection       | CT 202    | —                        |
| Grafana      | Visualization dashboard  | CT 203    | grafana.najhin-gaming.com |
| Loki         | Log aggregation          | CT 204    | —                        |
| Alertmanager | Alert routing            | CT 205    | —                        |
| Uptime Kuma  | Service monitoring       | CT 206    | —                        |

**Alert Routing:** Critical alerts → Telegram, Warning alerts → Discord

## 🔒 Security Architecture

| Layer           | Implementation                                                                    |
|-----------------|-----------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                     |
| Segmentation    | 5 VLANs with enforced firewall rules                                              |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                       |
| VPN             | Tailscale (subnet router on pfSense, primary access)                              |
| External Auth   | Cloudflare Access (email OTP) for 7 applications                                  |
| External Access | Cloudflare Tunnel for Nextcloud                                                   |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                    |
| Backup          | Automated daily backups with 7/4/2 retention                                     |
| Secrets         | HashiCorp Vault (8 secret paths) + Vaultwarden password manager                  |

## 💾 Backup Strategy

| Job              | Schedule | Storage                  | Containers                              | Retention                    |
|------------------|----------|--------------------------|----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)        | 201, 202, 203, 204, 205, 206, 207, 214, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302, 400                         | 7 daily, 4 weekly, 2 monthly |

## 📈 Project Progress

### Phase Summary
- **Completed:** 24 phases
- **In Progress:** 0 phases  
- **Planned:** 45+ phases

### Recent Completions
- **Phase 38 (Apr 24):** Ollama + ROCm Local LLM
- **Phase 39 (Apr 24):** Open WebUI
- **Phase 22 (Apr 24):** Obsidian Knowledge Base
- **Phase 15 (Apr 24):** Gilgamesh Additional Slash Commands
- **Phase 14 (Apr 24):** Secrets Management & Integration

### Next Priority
- **Phase 41:** Hybrid Routing (local/API based on complexity)
- **MERLIN Agent:** Reminder system (highest priority due to memory issues)
- **Phase 24.1:** Service Update Manager

## 🎯 Skills Demonstrated

### Infrastructure & Networking
- **Hypervisor:** Proxmox VE with LXC containers
- **Networking:** VLAN segmentation, router-on-a-stick topology
- **Security:** pfSense firewall, Tailscale VPN, DNS filtering
- **Storage:** ZFS, LVM-thin, NFS/SMB protocols

### Monitoring & Observability
- **Metrics:** Prometheus, Grafana dashboards
- **Logging:** Loki, centralized log aggregation
- **Alerting:** Alertmanager with Telegram/Discord routing
- **Uptime:** Kuma service monitoring

### Automation & DevOps
- **Workflow Automation:** n8n with 7+ workflows
- **CI/CD:** Automated documentation pipeline
- **Infrastructure as Code:** Systematic deployment processes
- **Backup Automation:** Proxmox scheduled backups

### AI & Machine Learning
- **Local LLM:** Ollama with ROCm GPU acceleration
- **AI Integration:** Telegram bot with Claude API
- **Knowledge Management:** RAG with conversation memory
- **Cost Optimization:** Hybrid local/cloud AI strategy

### Development & Integration
- **APIs:** Proxmox, Cloudflare, GitHub, Telegram
- **Containers:** Docker, LXC management
- **Secrets Management:** HashiCorp Vault, Vaultwarden
- **Documentation:** Auto-generated from AI context

## 📚 Documentation Structure

| File                 | Purpose                                      | Update Method    |
|----------------------|----------------------------------------------|------------------|
| `README.md`          | Public project overview                      | Auto-generated   |
| `roadmap.md`         | Phase tracking and planning                  | Auto-generated   |
| `current-state.md`   | Live infrastructure snapshot                 | Auto-generated   |
| `service-catalog.md` | Complete service inventory                   | Auto-generated   |
| `changelog.md`       | Change history and session logs             | Auto-generated   |
| `troubleshoot.md`    | Error resolutions and lessons learned       | Auto-generated   |
| `AI-CONTEXT.md`      | Master context (private repo)               | Session summaries |

**Documentation Pipeline:** Gilgamesh `/update` and `/sync-docs` commands auto-generate all public documentation from master AI context.

## 🎯 Long-term Vision

### Career Transition Goals
- **Target Role:** Cloud Engineer / DevOps Engineer
- **Portfolio:** Public GitHub showcase of enterprise skills
- **Networking:** LinkedIn content and professional engagement
- **Certifications:** AWS/Azure certifications planned

### Technical Roadmap
- **Local AI:** Replace $30-40/month Claude Pro with $5-10/month local inference
- **Gaming Platform:** Full game server automation pipeline
- **Enterprise Features:** Service mesh, GitOps, advanced monitoring
- **Cost Optimization:** Sub-$50/month total infrastructure cost

### Knowledge Management
- **Obsidian Vault:** Comprehensive second brain with 6 plugins
- **Learning Logs:** Systematic documentation of all discoveries
- **Best Practices:** Curated knowledge base for future reference

## 📞 Contact & Connect

- **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)
- **LinkedIn:** [Connect for career transition updates]
- **Domain:** najhin-gaming.com
- **Location:** Petaling Jaya, Selangor, Malaysia

---

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

*This README is auto-generated from the master AI context. Last updated: April 24, 2026*