# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** April 24, 2026  
> **Total Phases:** 69 phases (24 completed, 0 in progress, 45 planned)

---

## 📊 Phase Summary

| Status | Count | Percentage |
|--------|-------|------------|
| ✅ Completed | 24 | 35% |
| 🚧 In Progress | 0 | 0% |
| 📋 Planned | 45 | 65% |
| **Total** | **69** | **100%** |

---

## ✅ Completed Phases

| Phase   | Title                                                     | Completed        | Dependencies |
|---------|-----------------------------------------------------------|------------------|--------------|
| 1       | Proxmox VE Installation                                   | Jan 2026         | Hardware     |
| 2       | pfSense Firewall & VLAN Setup                             | Jan 2026         | Phase 1      |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)             | Jan 2026         | Phase 2      |
| 4       | External Access & SSL                                     | Feb 2026         | Phase 3      |
| 5       | Monitoring Stack                                          | Feb 2026         | Phase 3      |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)        | Feb 2026         | Phase 3      |
| 6E      | Homepage Dashboard                                        | Mar 2026         | Phase 5      |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening | Mar 9, 2026      | Phase 6E     |
| 7       | Nextcloud Deployment                                      | Mar 8, 2026      | Phase 5      |
| 7A      | Backup Strategy                                           | Mar 13, 2026     | Phase 7, 9   |
| 7B      | n8n Workflow Automation                                   | Apr 2, 2026      | Phase 7      |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration               | Apr 2, 2026      | Phase 7B     |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)      | Apr 6, 2026      | Phase 7C     |
| 7D-Sec  | Cloudflare Access for n8n                                 | Apr 7, 2026      | Phase 7D     |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                            | Apr 24, 2026     | Phase 7D     |
| 9       | NAS Deployment (Kinmoon)                                  | Mar 3, 2026      | Hardware     |
| 13      | HashiCorp Vault — Secrets Manager                         | Apr 18, 2026     | Phase 7B     |
| 14      | Secrets Management & Integration                          | Apr 24, 2026     | Phase 13     |
| 15      | Gilgamesh Additional Slash Commands                       | Apr 24, 2026     | Phase 7D-Menu|
| 16.1    | Documentation Pipeline - Update Workflow                  | Apr 19, 2026     | Phase 7C     |
| 16.2    | Documentation Pipeline - Sync Docs Workflow               | Apr 19, 2026     | Phase 16.1   |
| 22      | Obsidian Knowledge Base                                   | Apr 24, 2026     | Phase 7      |
| 23      | Vaultwarden + Secrets Audit & Cleanup                     | Apr 18, 2026     | Phase 13     |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                      | Apr 24, 2026     | Hardware     |
| 39      | Open WebUI                                                | Apr 24, 2026     | Phase 38     |
| 58      | Windrose Server Deployment                                | Apr 19, 2026     | Phase 6D     |

---

## 🚧 In Progress Phases

*No phases currently in progress.*

---

## 📋 Planned Phases

### Gilgamesh & Automation (High Priority)

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 41      | Hybrid Routing (local/API complexity)   | Critical | Phase 38     |
| 7E      | Extended Memory (20+ message RAG)       | High     | Phase 41     |
| 7F      | File Generation (code, configs, docs)   | High     | Phase 7E     |
| 7G      | Vision API (image analysis)             | High     | Phase 7F     |
| 7H      | Document Upload (PDF/text processing)   | Medium   | Phase 7G     |
| 7I      | Quality Assurance (compare Claude Pro)  | Medium   | Phase 7H     |
| 7J      | Migration (full switch to Gilgamesh)    | Medium   | Phase 7I     |
| 7K      | AI News Scraper (RSS/Reddit)            | Low      | Phase 7J     |
| MERLIN  | Reminder Agent (SSL, backups, health)   | Critical | Phase 7B     |
| 16.3    | Monthly Infrastructure Audit (cron)     | Medium   | Phase 16.2   |
| 24.1    | Service Update Manager (approval-gated) | High     | Phase 15     |

### Gaming Platform Pipeline (Medium Priority)

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 59      | Mash Discord Bot (game server manager)  | Medium   | Phase 58     |
| 60      | Game Server Auto-scaling                | Medium   | Phase 59     |
| 61      | Automated Game Updates                  | Medium   | Phase 60     |
| 62      | Player Analytics Dashboard              | Low      | Phase 61     |
| 63      | Discord Integration Enhancements        | Low      | Phase 62     |
| 64      | Gaming Community Features               | Low      | Phase 63     |

### Personal & Knowledge Management

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 40      | Advanced Obsidian Automation            | Medium   | Phase 22     |
| 42      | Personal Finance Dashboard              | Low      | Phase 40     |
| 43      | Health & Fitness Tracking               | Low      | Phase 42     |
| 44      | Recipe & Meal Planning System           | Low      | Phase 43     |
| 45      | Travel Planning & Documentation         | Low      | Phase 44     |

### Infrastructure Cleanup & Optimization

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 17      | Infrastructure Cleanup & Optimization   | Medium   | Phase 16.2   |
| 18      | Advanced Monitoring (APM, Tracing)      | Medium   | Phase 17     |
| 19      | Service Mesh Implementation             | Low      | Phase 18     |
| 20      | GitOps & Infrastructure as Code         | Low      | Phase 19     |
| 21      | Disaster Recovery Testing               | Low      | Phase 20     |

### Core Services Expansion

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 25      | Advanced Backup (Offsite, Encryption)  | Medium   | Phase 7A     |
| 26      | Network Optimization (QoS, Traffic)    | Medium   | Phase 25     |
| 27      | DNS Improvements (DoH, Filtering)       | Low      | Phase 26     |
| 28      | VPN Server (OpenVPN/WireGuard)         | Low      | Phase 27     |
| 29      | Certificate Management (Let's Encrypt)  | Low      | Phase 28     |

### AI & Local LLM Development

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 30      | Model Optimization & Fine-tuning       | Medium   | Phase 39     |
| 31      | RAG Implementation (Document Search)    | Medium   | Phase 30     |
| 32      | Voice Assistant Integration             | Low      | Phase 31     |
| 33      | Image Generation (Stable Diffusion)    | Low      | Phase 32     |
| 34      | Code Assistant (local GitHub Copilot)  | Low      | Phase 33     |

### Career Development

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 35      | AWS Certification Lab Environment      | Medium   | Phase 17     |
| 36      | Kubernetes Cluster Deployment          | Medium   | Phase 35     |
| 37      | CI/CD Pipeline Implementation          | Medium   | Phase 36     |
| 46      | DevOps Portfolio Website               | Low      | Phase 37     |
| 47      | LinkedIn Content Automation            | Low      | Phase 46     |

### Long Term & Experimental

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 48      | IoT Integration (Home Automation)       | Low      | Phase 20     |
| 49      | Blockchain & Cryptocurrency Tracking   | Low      | Phase 48     |
| 50      | Home Security Camera System            | Low      | Phase 49     |
| 51      | Weather Station & Environmental Mon.   | Low      | Phase 50     |
| 52      | 3D Printing Queue Management           | Low      | Phase 51     |

### Hardware Expansion

| Phase   | Title                                    | Priority | Dependencies |
|---------|------------------------------------------|----------|--------------|
| 53      | WiFi Access Point Setup (EAP610)       | Medium   | Hardware     |
| 54      | Second Proxmox Node (Cluster)           | Low      | Hardware     |
| 55      | Network Storage Expansion              | Low      | Hardware     |
| 56      | Dedicated AI Inference Server          | Low      | Hardware     |
| 57      | Network Upgrade (10GbE)                | Low      | Hardware     |

---

## 🎯 Critical Path Analysis

### Immediate Priority (Next 2-4 Sessions)
1. **Phase 41** — Hybrid Routing (enables cost savings)
2. **MERLIN Agent** — Reminder system (user memory issues)
3. **Phase 24.1** — Service Update Manager (security)

### Short Term (Next 1-2 Months)
1. **Phase 7E-7J** — Complete Gilgamesh Evolution
2. **Phase 17** — Infrastructure cleanup
3. **Phase 25** — Offsite backup

### Medium Term (3-6 Months)
1. **Phase 35-37** — AWS certification environment
2. **Phase 59-64** — Gaming platform pipeline
3. **Phase 18-21** — Advanced infrastructure

### Long Term (6+ Months)
1. **Phase 46-47** — Career development automation
2. **Phase 48-52** — Experimental features
3. **Phase 53-57** — Hardware expansion

---

## 🏆 Milestone Targets

### August 2026: Gilgamesh Complete
- **Goal:** Replace Claude Pro ($30-40/month → $5-10/month)
- **Phases:** 41, 7E-7J complete
- **Success:** 20+ message memory, vision, file generation

### December 2026: Career Ready
- **Goal:** Job-ready DevOps portfolio
- **Phases:** 35-37, 46-47 complete
- **Success:** AWS certified, Kubernetes experience, automated content

### June 2027: Advanced Infrastructure
- **Goal:** Enterprise-grade homelab
- **Phases:** 18-21, 48-52 complete
- **Success:** Service mesh, GitOps, IoT integration

---

## 📋 Recommended Next Session Order

1. **Session Focus:** Phase 41 (Hybrid Routing)
   - Implement local/API complexity routing
   - Test cost savings with Ollama
   - Measure response quality vs Claude

2. **Session Focus:** MERLIN Agent Development
   - Build reminder system for SSL renewals
   - Backup restore test notifications
   - Infrastructure health proactive alerts

3. **Session Focus:** Phase 24.1 (Service Updates)
   - Docker container updates
   - Proxmox/OS updates
   - Telegram approval gate integration

4. **Session Focus:** Phase 7E (Extended Memory)
   - RAG implementation for 20+ messages
   - Knowledge base integration
   - Context retention testing

---

*This roadmap is auto-generated from the master AI context and updated after each session.*