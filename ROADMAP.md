# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** April 26, 2026  
> **Total Phases:** 87 planned (34 complete, 2 in progress, 51 planned)

---

## 📊 Summary

| Status | Count | Percentage |
|--------|-------|------------|
| ✅ Complete | 34 | 39% |
| ⚡ In Progress | 2 | 2% |
| 📋 Planned | 51 | 59% |
| **Total** | **87** | **100%** |

---

## ✅ Completed Phases

| Phase | Title | Completion Date | Dependencies |
|-------|--------|----------------|-------------|
| 1 | Proxmox VE Installation | Jan 2026 | Hardware |
| 2 | pfSense Firewall & VLAN Setup | Jan 2026 | Phase 1 |
| 3 | Core Services (Pi-hole, NPM, Tailscale, DDNS) | Jan 2026 | Phase 2 |
| 4 | External Access & SSL | Feb 2026 | Phase 3 |
| 5 | Monitoring Stack | Feb 2026 | Phase 4 |
| 6A-6D | Gaming Platform (Pterodactyl, Terraria, Minecraft) | Feb 2026 | Phase 5 |
| 6E | Homepage Dashboard | Mar 2026 | Phase 6A-6D |
| 6F | Infrastructure Audit, VLAN Migration & Firewall Hardening | Mar 9, 2026 | Phase 6E |
| 7 | Nextcloud Deployment | Mar 8, 2026 | Phase 6F |
| 7A | Backup Strategy | Mar 13, 2026 | Phase 7, 9 |
| 7B | n8n Workflow Automation | Apr 2, 2026 | Phase 7A |
| 7C | Gilgamesh Telegram Bot + GitHub Integration | Apr 2, 2026 | Phase 7B |
| 7D | Gilgamesh Enhancements (Memory, Routing, Web Search) | Apr 6, 2026 | Phase 7C |
| 7D-Sec | Cloudflare Access for n8n | Apr 7, 2026 | Phase 7D |
| 7D-Menu | Gilgamesh Inline Keyboard Menu | Apr 24, 2026 | Phase 7D |
| 9 | NAS Deployment (Kinmoon) | Mar 3, 2026 | Hardware |
| 13 | HashiCorp Vault — Secrets Manager | Apr 18, 2026 | Phase 7B |
| 14 | Secrets Management & Integration | Apr 24, 2026 | Phase 13 |
| 15 | Gilgamesh Additional Slash Commands | Apr 24, 2026 | Phase 14 |
| 16.1 | Documentation Pipeline — Update Workflow | Apr 19, 2026 | Phase 15 |
| 16.2 | Documentation Pipeline — Sync Docs Workflow | Apr 19, 2026 | Phase 16.1 |
| 16.3 | Da Vinci Documentation Pipeline | Apr 25, 2026 | Phase 16.2 |
| 22 | Obsidian Knowledge Base | Apr 24, 2026 | Phase 7 |
| 23 | Vaultwarden + Secrets Audit & Cleanup | Apr 18, 2026 | Phase 13 |
| 38 | Ollama + ROCm on Kuromoon RX 6700 XT | Apr 24, 2026 | Hardware |
| 39 | Open WebUI | Apr 24, 2026 | Phase 38 |
| 41 | Gilgamesh + Ollama Hybrid Routing | Apr 24, 2026 | Phase 38, 39 |
| 58 | Windrose Server Deployment | Apr 19, 2026 | Phase 6A-6D |

---

## ⚡ In Progress

| Phase | Title | Progress | Dependencies | Notes |
|-------|-------|----------|-------------|-------|
| 22.15 | Price Database Tracking | Planning | Phase 22 | Budget-aware grocery system |
| 22.16 | Homepage Settings Tab | Planning | Phase 22.15 | User-adjustable budget percentages |

---

## 📋 Planned Phases

### 🤖 Gilgamesh & Automation (Priority 1)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 7E | Extended Memory (20+ message conversations via RAG) | Phase 41 | 8-12 | Vector database integration |
| 7F | File Generation (code, configs, docs) | Phase 7E | 6-8 | Artifact generation capabilities |
| 7G | Vision API (image analysis) | Phase 7F | 4-6 | Screenshot and diagram analysis |
| 7H | Document Upload (PDF/text processing) | Phase 7G | 6-8 | File processing and indexing |
| 7I | Quality Assurance (compare outputs with Claude Pro) | Phase 7H | 4-6 | Output quality validation |
| 7J | Migration (full switch to Gilgamesh) | Phase 7I | 2-4 | Complete Claude Pro replacement |
| 24.1 | EMIYA Foundation (Proxmox API + SSH + approval gate) | Phase 14 | 4-6 | CTO agent foundation |
| 24.2 | EMIYA Alert Translation (Alertmanager to plain English) | Phase 24.1 | 3-4 | Intelligent alert processing |
| 24.3 | EMIYA Container Updates (Docker + apt + Proxmox, approval-gated) | Phase 24.2 | 4-6 | Automated update management |
| 24.4 | EMIYA Proactive Monitoring (anomaly detection) | Phase 24.3 | 6-8 | Predictive infrastructure monitoring |
| 24.5 | EMIYA Security Management (threat detection) | Phase 24.4 | 6-8 | Security automation |
| 24.6 | EMIYA Performance Optimization | Phase 24.5 | 4-6 | Resource optimization suggestions |
| 24.7 | EMIYA Backup Verification | Phase 24.6 | 3-4 | Automated backup integrity checks |
| 24.8 | EMIYA Change Management | Phase 24.7 | 4-6 | Infrastructure change tracking |
| 25 | Midas Agent (CFO — Cost Tracking & Optimization) | Phase 24.1 | 6-8 | Financial monitoring automation |
| 26 | MERLIN Agent (Reminders & Scheduler) | Phase 25 | 4-6 | Proactive maintenance scheduling |
| 27 | Vault + n8n Integration | Phase 26 | 3-4 | Secrets management for workflows |
| 28 | Guardian Agent (Security Monitoring) | Phase 27 | 8-10 | Security event automation |
| 29 | Nexus Agent (Cross-platform Automation) | Phase 28 | 6-8 | Universal automation hub |
| 30 | Oracle Agent (Predictive Intelligence) | Phase 29 | 10-12 | Advanced analytics and prediction |

### 🎮 Gaming Platform Pipeline (Priority 2)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 59 | Mash Discord Bot Foundation | Phase 6A-6D | 4-6 | Discord bot infrastructure |
| 60 | Mash Server Management (!start, !stop, !status) | Phase 59 | 3-4 | Basic server control commands |
| 61 | Mash Player Notifications | Phase 60 | 2-3 | Join/leave announcements |
| 62 | Mash Scheduled Reminders | Phase 61 | 3-4 | Game night scheduling |
| 63 | Mash Auto-shutdown & Monitoring | Phase 62 | 4-6 | Resource management |
| 64 | Mash Game Updates & Personality | Phase 63 | 3-4 | Update notifications and character |

### 📚 Personal & Knowledge (Priority 3)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 16 | /update File Upload Redesign | Phase 16.3 | 4-6 | Telegram file attachment workflow |
| 17 | Monthly Infrastructure Audit Cron | Phase 16 | 3-4 | Automated audit reports |
| 18 | Backup Restore Testing Automation | Phase 7A | 4-6 | Automated disaster recovery testing |
| 19 | Homelab Embedded Gilgamesh Chat | Phase 7J | 8-10 | Web frontend for Homepage |
| 20 | Claude Project Auto-sync | Phase 16 | 6-8 | Project files synchronization |
| 21 | Sherlock Holmes Agent (Web Scraper & Research) | Phase 30 | 8-10 | Dedicated web scraping and research |

### 🧹 Infrastructure Cleanup (Priority 4)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 31 | Switch Management IP Migration | Phase 30 | 2-3 | 192.168.1.20 → 192.168.10.20 |
| 32 | Legacy LAN Removal | Phase 31 | 3-4 | Remove 192.168.1.0/24 network |
| 33 | Local-lvm Thin Pool Optimization | Phase 32 | 4-6 | Fix overprovisioning issues |
| 34 | WiFi Access Point Setup | Hardware | 3-4 | TP-Link EAP610 configuration |
| 35 | Alertmanager → n8n Migration | Phase 34 | 4-6 | Central notification hub |

### 🔧 Core Services (Priority 5)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 36 | Homepage Cloudflare Access Security | Phase 35 | 2-3 | Secure dashboard access |
| 37 | Nextcloud 2FA (TOTP) | Phase 36 | 2-3 | Enhanced authentication |
| 40 | Nextcloud Data Directory Migration | Phase 37 | 4-6 | Move to 7.3TB HDD |
| 42 | Off-site Backup (Backblaze B2) | Phase 40 | 6-8 | Cloud backup implementation |

### 🧠 AI & Local LLM (Priority 6)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 43 | Da Vinci Stage 2 (RAG) | Phase 7E | 8-10 | Vector database and retrieval |
| 44 | Agent Logging to Da Vinci | Phase 43 | 4-6 | Centralized agent observability |
| 45 | Da Vinci Stage 3 (Security Intelligence) | Phase 44 | 6-8 | Threat feed monitoring |
| 46 | Tier 2 Agents (Chiron, Medea, Waver) | Phase 45 | 12-16 | Career, QA, and PM agents |

### 💼 Career Development (Priority 7)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 47 | GitHub Portfolio Enhancement | Phase 46 | 4-6 | Professional documentation |
| 48 | LinkedIn Technical Content Creation | Phase 47 | 6-8 | Regular technical posts |
| 49 | Certification Study Lab | Phase 48 | 8-10 | AWS/Azure lab environment |
| 50 | Interview Preparation Environment | Phase 49 | 4-6 | Technical interview practice |

### 📈 Long Term (Priority 8)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 51 | Kubernetes Learning Cluster | Phase 50 | 16-20 | K8s on Proxmox |
| 52 | GitOps Implementation | Phase 51 | 12-16 | ArgoCD/Flux deployment |
| 53 | Service Mesh (Istio) | Phase 52 | 16-20 | Advanced networking |
| 54 | Observability Stack Enhancement | Phase 53 | 8-12 | OpenTelemetry integration |
| 55 | Infrastructure as Code (Terraform) | Phase 54 | 12-16 | IaC implementation |
| 56 | CI/CD Pipeline for Homelab | Phase 55 | 10-14 | Automated deployments |

### 🖥️ Hardware (Priority 9)

| Phase | Title | Dependencies | Estimated Hours | Description |
|-------|-------|-------------|----------------|-------------|
| 57 | P400S Repurpose Planning | Hardware Available | 6-8 | Z270E + i7-7700K build |
| 65 | Fedora Dual-boot on Minimoon | Hardware | 4-6 | Kernel 6.12+ for RDNA 4 |

---

## 🎯 Recommended Next Session Order

### Immediate Priority (Next 3 Sessions)
1. **Phase 22.15** — Price Database Tracking
2. **Phase 22.16** — Homepage Settings Tab  
3. **Phase 17** — Monthly Infrastructure Audit Cron

### Short Term (Weeks 1-4)
4. **Phase 25** — Midas Agent (CFO)
5. **Phase 26** — MERLIN Agent (Reminders)
6. **Phase 24.1** — EMIYA Foundation

### Medium Term (Weeks 5-8)  
7. **Phase 7E** — Extended Memory (RAG)
8. **Phase 24.2-24.3** — EMIYA Alert Translation & Updates
9. **Phase 28** — Guardian Agent (Security)

### Planning Dependencies
- **Midas must be built first** — No agent should spend tokens without cost visibility
- **Build order locked:** Midas → MERLIN → Guardian → Mash → Nexus → Oracle
- **EMIYA phases 24.1-24.5 are critical path** for infrastructure automation
- **Gaming pipeline (Phases 59-64)** can run in parallel with agent development

---

*This roadmap is a living document, updated after each session to reflect progress and changing priorities.*