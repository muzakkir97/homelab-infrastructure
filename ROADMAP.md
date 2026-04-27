# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** April 27, 2026
> **Total Phases:** 70+ phases across 9 categories
> **Status:** 25 completed, 3 in progress, 45+ planned

---

## 📊 Phase Summary

| Status        | Count | Description                               |
|---------------|-------|-------------------------------------------|
| ✅ Complete    | 25    | Infrastructure foundation & core services |
| 🚧 In Progress | 3     | AI agents & automation expansion         |
| 📋 Planned     | 45+   | Advanced features & platform completion  |

---

## ✅ Completed Phases

| Phase   | Title                                                            | Completed    |
|---------|------------------------------------------------------------------|--------------|
| 1       | Proxmox VE Installation                                          | Jan 2026     |
| 2       | pfSense Firewall & VLAN Setup                                    | Jan 2026     |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)                    | Jan 2026     |
| 4       | External Access & SSL                                            | Feb 2026     |
| 5       | Monitoring Stack                                                 | Feb 2026     |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)               | Feb 2026     |
| 6E      | Homepage Dashboard                                               | Mar 2026     |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening        | Mar 9, 2026  |
| 7       | Nextcloud Deployment                                             | Mar 8, 2026  |
| 7A      | Backup Strategy                                                  | Mar 13, 2026 |
| 7B      | n8n Workflow Automation                                          | Apr 2, 2026  |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration                      | Apr 2, 2026  |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)             | Apr 6, 2026  |
| 7D-Sec  | Cloudflare Access for n8n                                        | Apr 7, 2026  |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                                   | Apr 24, 2026 |
| 9       | NAS Deployment (Kinmoon)                                         | Mar 3, 2026  |
| 13      | HashiCorp Vault — Secrets Manager                                | Apr 18, 2026 |
| 14      | Secrets Management & Integration                                 | Apr 24, 2026 |
| 15      | Gilgamesh Additional Slash Commands                              | Apr 24, 2026 |
| 16.1    | Documentation Pipeline — Update Workflow                         | Apr 19, 2026 |
| 16.2    | Documentation Pipeline — Sync Docs Workflow                      | Apr 19, 2026 |
| 16.3    | Da Vinci Documentation Pipeline                                  | Apr 25, 2026 |
| 22      | Obsidian Knowledge Base                                          | Apr 24, 2026 |
| 22.1    | Obsidian Vault Structure Expansion                               | Apr 27, 2026 |
| 22.2    | Obsidian Daily Notes + Morning Briefing                         | Apr 27, 2026 |
| 23      | Vaultwarden + Secrets Audit & Cleanup                            | Apr 18, 2026 |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                             | Apr 24, 2026 |
| 39      | Open WebUI                                                       | Apr 24, 2026 |
| 41      | Gilgamesh + Ollama Hybrid Routing                                | Apr 24, 2026 |
| 58      | Windrose Server Deployment                                       | Apr 19, 2026 |
| Midas   | Midas CFO Agent                                                  | Apr 27, 2026 |
| MERLIN  | MERLIN Reminders Agent                                           | Apr 27, 2026 |

---

## 🚧 In Progress

| Phase | Title                                | Dependencies | Next Steps                                    |
|-------|--------------------------------------|--------------|-----------------------------------------------|
| 7E    | Extended Memory (20+ conversations) | Phase 41     | RAG integration with Da Vinci Stage 2        |
| Da Vinci Stage 2 | RAG Retrieval System | Phase 22.1  | Qdrant + nomic-embed-text on VM 400          |
| Guardian | Security Monitoring Agent         | MERLIN       | Next agent in build order after MERLIN       |

---

## 📋 Planned Phases

### 🔮 Gilgamesh & Automation (Priority 1)

| Phase | Title                                          | Dependencies     | Effort | Description                                         |
|-------|------------------------------------------------|------------------|--------|-----------------------------------------------------|
| 7F    | File Generation (code, configs, docs)         | Phase 7E         | 4h     | Gilgamesh creates and uploads files                |
| 7G    | Vision API (image analysis)                    | Phase 7F         | 3h     | Screenshot analysis and diagram processing          |
| 7H    | Document Upload (PDF/text processing)          | Phase 7G         | 4h     | File upload and content extraction                  |
| 7I    | Quality Assurance (vs Claude Pro)             | Phase 7H         | 6h     | Output comparison and quality metrics               |
| 7J    | Migration (full switch to Gilgamesh)          | Phase 7I         | 2h     | Complete transition from Claude Pro                 |
| 24.1  | EMIYA Foundation (Proxmox API + approval)     | Guardian         | 4h     | Infrastructure automation with safety gates        |
| 24.2  | EMIYA Alert Translation                        | Phase 24.1       | 3h     | Alertmanager to plain English                      |
| 24.3  | EMIYA Container Updates                        | Phase 24.2       | 4h     | Docker + apt updates with approval                 |
| 24.4  | EMIYA Proactive Monitoring                     | Phase 24.3       | 4h     | Anomaly detection and trend alerts                 |
| 24.5  | EMIYA Security Management                      | Phase 24.4       | 4h     | Threat detection and firewall suggestions          |
| 24.6  | EMIYA Performance Optimization                 | Phase 24.5       | 3h     | Resource reallocation recommendations              |
| 24.7  | EMIYA Backup Verification                      | Phase 24.6       | 3h     | Backup integrity checks and failure alerts         |
| 24.8  | EMIYA Change Management                        | Phase 24.7       | 3h     | Infrastructure change tracking and audit logs      |
| 27    | n8n + Vault Integration                        | Phase 14         | 3h     | Direct secret retrieval in workflows               |

### 🎮 Gaming Platform Pipeline (Priority 2)

| Phase | Title                                | Dependencies | Effort | Description                                         |
|-------|--------------------------------------|--------------|--------|-----------------------------------------------------|
| 59    | Mash Discord Bot Foundation          | Guardian     | 4h     | Basic Discord bot with game server commands        |
| 60    | Mash Player Management               | Phase 59     | 3h     | Join/leave announcements, player tracking          |
| 61    | Mash Auto-Shutdown                   | Phase 60     | 2h     | Idle server detection and shutdown                 |
| 62    | Mash Game Scheduling                 | Phase 61     | 3h     | Scheduled events and game night reminders          |
| 63    | Mash Tournament System               | Phase 62     | 6h     | Automated tournament brackets and scheduling       |
| 64    | Mash Advanced Features               | Phase 63     | 4h     | Game update notifications, server metrics          |
| 65    | Gaming Analytics                     | Phase 64     | 3h     | Player statistics and game usage reports           |
| 66    | Multi-Game Support                   | Phase 65     | 5h     | Support for additional game servers                |

### 📚 Personal & Knowledge Management (Priority 3)

| Phase  | Title                                | Dependencies | Effort | Description                                        |
|--------|--------------------------------------|--------------|--------|----------------------------------------------------|
| 22.3   | Obsidian Reminders Integration       | Phase 22.2   | 2h     | Task reminders via MERLIN                         |
| 22.4   | Health Data Automation               | Phase 22.3   | 3h     | Blood pressure tracking, food logs                |
| 22.5   | Finance Dashboard                    | Phase 22.4   | 4h     | Budget tracking with visual indicators             |
| 22.6   | Smart Grocery Lists                  | Phase 22.5   | 3h     | Budget-aware shopping with price comparisons      |
| 22.9   | Voice Notes + Photo Processing       | Phase 22.6   | 4h     | Voice-to-text and notebook photo OCR              |
| 22.11  | Calendar Integration                 | Phase 22.9   | 3h     | Nextcloud CalDAV sync with reminders              |
| 22.15  | Price Database Tracking              | Phase 22.11  | 4h     | Grocery price monitoring and comparison            |
| 22.16  | Homepage Settings Tab                | Phase 22.15  | 3h     | Budget configuration UI                           |

### 🛠️ Infrastructure Cleanup & Optimization (Priority 4)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 8     | Storage Optimization               | Phase 7A     | 4h     | Resolve local-lvm thin pool overprovisioning       |
| 10    | WiFi Access Point Deployment       | Phase 6F     | 3h     | TP-Link EAP610 setup and integration               |
| 11    | Off-site Backup (Backblaze B2)     | Phase 7A     | 4h     | Cloud backup for critical data                     |
| 12    | Network Cleanup                    | Phase 10     | 3h     | Remove legacy 192.168.1.0/24 network              |
| 17    | Monthly Infrastructure Audit       | Phase 16.3   | 3h     | Automated monthly health reports                    |
| 18    | Advanced Monitoring                | Phase 5      | 4h     | Custom dashboards, SLA monitoring                  |
| 19    | Security Hardening Phase 2         | Phase 13     | 5h     | 2FA, additional access controls                    |

### 🔧 Core Services Enhancement (Priority 5)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 20    | Nextcloud Performance Tuning       | Phase 7      | 3h     | Redis cache, database optimization                  |
| 21    | Advanced Backup Testing            | Phase 7A     | 4h     | Automated restore verification                      |
| 25    | DNS Infrastructure Upgrade         | Phase 3      | 3h     | Redundant DNS, advanced filtering                   |
| 26    | Certificate Management             | Phase 4      | 3h     | Automated renewal, wildcard certificates            |
| 28    | Container Registry                 | Phase 14     | 4h     | Private Docker registry for custom images          |
| 29    | GitOps Implementation              | Phase 16.3   | 5h     | Infrastructure as code with Git workflows          |

### 🤖 AI & Local LLM Enhancement (Priority 6)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 40    | Ollama Model Management            | Phase 38     | 3h     | Automated model updates, version control            |
| 42    | AI Model Benchmarking              | Phase 41     | 4h     | Performance comparison, quality metrics             |
| 43    | Sherlock Holmes Web Scraper        | Phase 24.8   | 4h     | RSS feeds, Reddit, news site monitoring            |
| 44    | Oracle Predictive Intelligence     | Sherlock     | 5h     | Internal + external trend analysis                  |
| 45    | Nexus Cross-platform Integration   | Oracle       | 4h     | Agent coordination and workflow orchestration      |
| 46    | Advanced AI Workflows              | Phase 45     | 6h     | Multi-step automation with AI decision making      |

### 💼 Career Development (Priority 7)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 30    | Portfolio Website                  | Phase 28     | 6h     | Professional showcase of homelab projects          |
| 31    | LinkedIn Integration               | Phase 30     | 3h     | Automated project updates and achievements          |
| 32    | Certification Tracking             | Phase 22.5   | 2h     | Progress monitoring for cloud certifications       |
| 33    | Resume Automation                  | Phase 32     | 3h     | Dynamic resume generation from project data        |
| 34    | Interview Preparation              | Phase 33     | 4h     | Technical question database and practice system    |

### 🔮 Long Term Vision (Priority 8)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 50    | Kubernetes Migration Planning       | Phase 29     | 8h     | Container orchestration strategy                    |
| 51    | Multi-Node Expansion               | Phase 50     | 12h    | Cluster deployment and management                   |
| 52    | Advanced Security Lab              | Phase 19     | 6h     | Malware analysis, threat hunting environment       |
| 53    | IoT Integration                    | Phase 10     | 5h     | Home automation and sensor monitoring              |
| 54    | Machine Learning Pipeline          | Phase 46     | 10h    | Data processing and model training infrastructure   |
| 55    | Disaster Recovery Testing          | Phase 21     | 6h     | Full site failover and recovery procedures         |

### 🖥️ Hardware & Infrastructure (Priority 9)

| Phase | Title                              | Dependencies | Effort | Description                                          |
|-------|------------------------------------|--------------|--------|------------------------------------------------------|
| 67    | Hardware Refresh Planning          | Phase 8      | 4h     | Next generation server specifications               |
| 68    | Power Management                   | Phase 67     | 3h     | UPS integration, power monitoring                  |
| 69    | Environmental Monitoring           | Phase 68     | 3h     | Temperature, humidity, power consumption tracking   |
| 70    | Legacy Hardware Repurpose          | Phase 69     | 4h     | P400S build integration or alternative use         |

---

## 🎯 Recommended Next Session Order

### Immediate Priority (Next 3 Sessions)
1. **Guardian Agent** — Complete agent build order (after MERLIN)
2. **Phase 7E** — Extended memory for Gilgamesh (20+ messages)
3. **Phase 24.1** — EMIYA Foundation (infrastructure automation)

### Medium Priority (Sessions 4-8)
4. **Phase 59** — Mash Discord Bot Foundation
5. **Phase 22.3** — Obsidian Reminders Integration  
6. **Phase 17** — Monthly Infrastructure Audit
7. **Phase 7F** — File Generation capabilities
8. **Phase 24.2** — EMIYA Alert Translation

### Strategic Priority (Sessions 9-12)
9. **Da Vinci Stage 2** — RAG retrieval system
10. **Phase 11** — Off-site Backup (Backblaze B2)
11. **Phase 7G** — Vision API integration
12. **Phase 60** — Mash Player Management

### Dependencies Overview
- **Agent Build Order:** Midas ✅ → MERLIN ✅ → Guardian → Mash → Nexus → Oracle → EMIYA
- **Gilgamesh Evolution:** Phases 7E → 7F → 7G → 7H → 7I → 7J (August 2026 target)
- **Gaming Platform:** Phases 59 → 60 → 61 → 62 → 63 → 64 (tournament system)
- **Infrastructure:** Guardian → Phase 24.1 → 24.2 → ... → 24.8 (EMIYA capabilities)

---

*Last updated: April 27, 2026*