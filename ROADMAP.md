# 🗺️ Project Roadmap

> **Last Updated:** April 24, 2026  
> **Total Phases:** 64+ planned  
> **Completed:** 17 phases  
> **In Progress:** 2 phases  

## 📊 Summary Overview

| Status | Count | Percentage |
|--------|-------|------------|
| **✅ Completed** | 17 | 27% |
| **📋 In Progress** | 2 | 3% |
| **🔄 Planned** | 45+ | 70% |
| **🎯 Near Term** | 6 | Priority |

---

## ✅ Completed Phases

| Phase | Title | Completed | Dependencies |
|-------|-------|-----------|--------------|
| 1 | Proxmox VE Installation | Jan 2026 | Hardware setup |
| 2 | pfSense Firewall & VLAN Setup | Jan 2026 | Phase 1 |
| 3 | Core Services (Pi-hole, NPM, Tailscale, DDNS) | Jan 2026 | Phase 2 |
| 4 | External Access & SSL | Feb 2026 | Phase 3 |
| 5 | Monitoring Stack | Feb 2026 | Phase 4 |
| 6A-6D | Gaming Platform (Pterodactyl, Terraria, Minecraft) | Feb 2026 | Phase 5 |
| 6E | Homepage Dashboard | Mar 2026 | Phase 6D |
| 6F | Infrastructure Audit, VLAN Migration & Firewall Hardening | Mar 9, 2026 | Phase 6E |
| 7 | Nextcloud Deployment | Mar 8, 2026 | Phase 6F |
| 7A | Backup Strategy | Mar 13, 2026 | Phase 7 |
| 7B | n8n Workflow Automation | Apr 2, 2026 | Phase 7A |
| 7C | Gilgamesh Telegram Bot + GitHub Integration | Apr 2, 2026 | Phase 7B |
| 7D | Gilgamesh Enhancements (Memory, Routing, Web Search) | Apr 6, 2026 | Phase 7C |
| 7D-Sec | Cloudflare Access for n8n | Apr 7, 2026 | Phase 7D |
| 7D-Menu | Gilgamesh Inline Keyboard Menu | Apr 24, 2026 | Phase 7D |
| 9 | NAS Deployment (Kinmoon) | Mar 3, 2026 | Phase 7A |
| 13 | HashiCorp Vault — Secrets Manager | Apr 18, 2026 | Phase 9 |
| 14 | Secrets Management & Integration | Apr 24, 2026 | Phase 13 |
| 15 | Gilgamesh Additional Slash Commands | Apr 24, 2026 | Phase 14 |
| 16.1 | Documentation Pipeline - Update Workflow | Apr 19, 2026 | Phase 7C |
| 16.2 | Documentation Pipeline - Sync Docs Workflow | Apr 19, 2026 | Phase 16.1 |
| 23 | Vaultwarden + Secrets Audit & Cleanup | Apr 18, 2026 | Phase 13 |
| 58 | Windrose Server Deployment | Apr 19, 2026 | Phase 6D |

---

## 📋 In Progress

| Phase | Title | Started | Target | Dependencies |
|-------|-------|---------|--------|--------------|
| 22 | Obsidian Knowledge Base | Planning | Q2 2026 | Phase 15 |
| 38 | Ollama + ROCm on Kuromoon RX 6700 XT | Planning | Q2 2026 | Phase 22 |

---

## 🎯 Near Term Priority Queue

| Priority | Phase | Title | Rationale |
|----------|-------|-------|-----------|
| #1 | MERLIN Agent | Reminder & Scheduler Agent | User memory issues |
| #2 | 38 | Ollama + ROCm on Kuromoon RX 6700 XT | Stop API bleeding, local LLM |
| #3 | 22 | Obsidian Knowledge Base | Foundation for AI memory |
| #4 | 7E | Gilgamesh Extended Memory (20+ messages) | Core AI enhancement |
| #5 | 41 | Gilgamesh Hybrid Routing | Cost optimization |
| #6 | 16.3 | Monthly Infrastructure Audit (Cron) | Automated health checks |

---

## 🔮 Planned Phases by Category

### Gilgamesh & AI Automation
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 7E | Extended Memory (20+ message conversations via RAG) | Phase 22 |
| 7F | File Generation (code, configs, docs) | Phase 7E |
| 7G | Vision API (image analysis) | Phase 7F |
| 7H | Document Upload (PDF/text processing) | Phase 7G |
| 7I | Quality Assurance (compare outputs with Claude Pro) | Phase 7H |
| 7J | Migration (full switch to Gilgamesh) | Phase 7I |
| 7K | AI News Scraper (RSS/Reddit → Ollama evaluation) | Phase 38 |
| 41 | Hybrid Routing (local/API based on complexity) | Phase 38 |

### Gaming Platform Pipeline
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 59 | Gaming Platform Standardization & Documentation | Phase 58 |
| 60 | Mash Discord Bot (Gaming Server Management) | Phase 59 |
| 61 | Auto-scaling & Resource Management | Phase 60 |
| 62 | Game Server Status Dashboard (Grafana) | Phase 61 |
| 63 | Scheduled Game Nights & Notifications | Phase 62 |
| 64 | Game Update Automation | Phase 63 |

### Personal & Knowledge Management
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 24 | Expense Tracking System | Phase 22 |
| 25 | Time Tracking Integration | Phase 24 |
| 25.5 | Private Repository + Backblaze B2 Backup | Phase 25 |
| 26 | Homepage Upgrade (Stock/Price Trackers) | Phase 25.5 |
| 26.1 | Homepage Weather Widget | Phase 26 |
| 26.2 | Homepage Calendar Integration | Phase 26.1 |
| 26.3 | Homepage Embedded Gilgamesh Chat | Phase 26.2 |

### Infrastructure Cleanup & Optimization
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 8 | Infrastructure Cleanup & Optimization | Phase 7A |
| 10 | WiFi Access Point Setup | Phase 9 |
| 11 | Network Optimization | Phase 10 |
| 12 | Advanced Firewall Rules | Phase 11 |
| 16.3 | Monthly Infrastructure Audit (Cron Workflow) | Phase 16.2 |
| 17 | SSL Certificate Automation | Phase 12 |
| 18 | Advanced Monitoring | Phase 17 |
| 19 | Performance Tuning | Phase 18 |

### Core Services & Storage
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 20 | Service Discovery | Phase 19 |
| 21 | Load Balancing | Phase 20 |
| 65 | Database Cluster (PostgreSQL HA) | Phase 21 |
| 66 | Redis Cluster | Phase 65 |
| 67 | Object Storage (MinIO) | Phase 66 |
| 68 | Backup Automation Enhancement | Phase 67 |

### AI & Local LLM Infrastructure
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 39 | Local LLM Model Management | Phase 38 |
| 40 | AI Model Performance Monitoring | Phase 39 |
| 42 | Multi-Model Support | Phase 41 |
| 43 | AI Agent Framework | Phase 42 |
| 44 | Voice Interface (Whisper) | Phase 43 |

### n8n Business Automation
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 27 | Advanced n8n Workflows | Phase 14 |
| 28 | Workflow Marketplace Preparation | Phase 27 |
| 29 | Business Process Automation | Phase 28 |
| 30 | Revenue Tracking System | Phase 29 |

### Career Development & Skills
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 31 | Kubernetes Learning Lab | Phase 30 |
| 32 | Ansible Automation | Phase 31 |
| 33 | Terraform Infrastructure | Phase 32 |
| 34 | CI/CD Pipeline Enhancement | Phase 33 |
| 35 | Security Hardening | Phase 34 |
| 36 | Compliance & Auditing | Phase 35 |
| 37 | Documentation Website | Phase 36 |

### Long Term Expansion
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 45 | Multi-node Cluster | Phase 44 |
| 46 | Disaster Recovery Site | Phase 45 |
| 47 | High Availability Setup | Phase 46 |
| 48 | Performance Optimization | Phase 47 |
| 49 | Advanced Security | Phase 48 |
| 50 | Compliance Framework | Phase 49 |

### Hardware & Infrastructure
| Phase | Title | Dependencies |
|-------|-------|--------------|
| 51 | Second Proxmox Node | Phase 50 |
| 52 | Shared Storage Cluster | Phase 51 |
| 53 | Network Upgrade (10Gbps) | Phase 52 |
| 54 | UPS & Power Management | Phase 53 |
| 55 | Environmental Monitoring | Phase 54 |
| 56 | Hardware Refresh Planning | Phase 55 |

---

## 🎴 Fate Grand Order Agent Ecosystem Roadmap

### Phase Allocation for Servants

| Servant | Phase | Status | Purpose |
|---------|-------|--------|---------|
| **Gilgamesh** | 7C-7J | ✅ Active | Personal AI Assistant (Telegram) |
| **Da Vinci** | 27-29 | 📋 Planned | Knowledge Curator (n8n/Nextcloud) |
| **EMIYA** | 27-29 | ✅ Active | Infrastructure Translator (n8n) |
| **MERLIN** | Special | 🎯 Priority #1 | Reminders & Scheduler |
| **Mash** | 60 | 📋 Planned | Gaming Server Manager (Discord) |
| **Guardian** | 35-36 | 📋 Planned | Security Monitoring |
| **Nexus** | 43 | 📋 Planned | Cross-platform Automation |
| **Scribe** | 37 | 📋 Planned | Auto-documentation |
| **Midas** | 30 | 📋 Planned | Cost Tracking & Optimization |
| **Oracle** | 40 | 📋 Planned | Predictive Intelligence |

---

## 📋 Recommended Next Session Order

Based on current priorities and dependencies:

1. **Complete MERLIN Agent** — Address critical memory/reminder issues
2. **Phase 22 Session 1** — Install Obsidian on Gaming PC
3. **Phase 38 Planning** — Ollama + ROCm requirements analysis
4. **Phase 16.3** — Monthly Infrastructure Audit cron workflow
5. **Phase 7E** — Extended Memory for Gilgamesh (post-Obsidian)
6. **Phase 25.5** — Private repo + Backblaze B2 setup

### Weekly Rhythm Suggestion
- **Monday:** Infrastructure/DevOps phases (Core services, monitoring)
- **Wednesday:** Gilgamesh/AI development (7E-7J series)
- **Friday:** Gaming platform or business automation
- **Weekend:** Documentation, planning, and knowledge base updates

---

*This roadmap is living documentation, updated after each completed phase via the Gilgamesh /sync-docs pipeline.*