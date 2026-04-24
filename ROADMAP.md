# 📋 Project Roadmap — Homelab Infrastructure

> **Purpose:** Complete phase timeline with dependencies and status tracking
> **Last Updated:** April 24, 2026
> **Total Phases:** 64+ planned (22 completed, 1 in progress)

---

## 📊 Summary Statistics

| Category | Count | Status |
|----------|-------|--------|
| **Completed Phases** | 22 | ✅ |
| **In Progress** | 1 | 🔄 |
| **Near Term (Next 3-6 months)** | 8 | 📋 |
| **Planned Phases** | 40+ | 📋 |
| **Total Phases** | 64+ | — |

---

## ✅ Completed Phases

| Phase   | Title                                                     | Completed    | Dependencies |
|---------|-----------------------------------------------------------|--------------|--------------|
| 1       | Proxmox VE Installation                                   | Jan 2026     | Hardware |
| 2       | pfSense Firewall & VLAN Setup                             | Jan 2026     | Phase 1 |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)             | Jan 2026     | Phase 2 |
| 4       | External Access & SSL                                     | Feb 2026     | Phase 3 |
| 5       | Monitoring Stack                                          | Feb 2026     | Phase 4 |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)        | Feb 2026     | Phase 5 |
| 6E      | Homepage Dashboard                                        | Mar 2026     | Phase 6D |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening | Mar 9, 2026  | Phase 6E |
| 7       | Nextcloud Deployment                                      | Mar 8, 2026  | Phase 6F |
| 7A      | Backup Strategy                                           | Mar 13, 2026 | Phase 7 |
| 7B      | n8n Workflow Automation                                   | Apr 2, 2026  | Phase 7A |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration               | Apr 2, 2026  | Phase 7B |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)      | Apr 6, 2026  | Phase 7C |
| 7D-Sec  | Cloudflare Access for n8n                                 | Apr 7, 2026  | Phase 7D |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                            | Apr 24, 2026 | Phase 7D |
| 9       | NAS Deployment (Kinmoon)                                  | Mar 3, 2026  | Hardware |
| 13      | HashiCorp Vault — Secrets Manager                         | Apr 18, 2026 | Phase 7B |
| 14      | Secrets Management & Integration                          | Apr 24, 2026 | Phase 13 |
| 15      | Gilgamesh Additional Slash Commands                       | Apr 24, 2026 | Phase 7D-Menu |
| 16.1    | Documentation Pipeline - Update Workflow                  | Apr 19, 2026 | Phase 7C |
| 16.2    | Documentation Pipeline - Sync Docs Workflow               | Apr 19, 2026 | Phase 16.1 |
| 22      | Obsidian Knowledge Base                                   | Apr 24, 2026 | Phase 7 |
| 23      | Vaultwarden + Secrets Audit & Cleanup                     | Apr 18, 2026 | Phase 13 |
| 58      | Windrose Server Deployment                                | Apr 19, 2026 | Phase 6D |

---

## 🔄 In Progress

| Phase | Title | Started | Dependencies | Status |
|-------|-------|---------|--------------|--------|
| 38 | Ollama + ROCm on Kuromoon RX 6700 XT | Next session | Hardware | Near Term #1 |

---

## 📋 Planned Phases by Category

### 🤖 Gilgamesh & Automation (Near Term)

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 38 | Ollama + ROCm on Kuromoon RX 6700 XT | #1 CRITICAL | Hardware | Next session |
| 24.1 | Service Update Manager (Docker + apt + Proxmox, approval-gated) | #3 | Phase 15 | 2-4 weeks |
| 41 | Hybrid AI Routing (local/API based on complexity) | #6 | Phase 38 | 4-6 weeks |
| 7E | Extended Memory (20+ message conversations via RAG) | #7 | Phase 38 | 6-8 weeks |
| 7F | File Generation (code, configs, docs) | #8 | Phase 7E | 8-10 weeks |
| 7G | Vision API (image analysis) | — | Phase 7F | 10-12 weeks |
| 7H | Document Upload (PDF/text processing) | — | Phase 7G | 12-14 weeks |
| 7I | Quality Assurance (compare outputs with Claude Pro) | — | Phase 7H | 14-16 weeks |
| 7J | Migration (full switch to Gilgamesh) | — | Phase 7I | 16+ weeks |
| 7K | AI News Scraper (RSS/Reddit → Ollama → Telegram) | — | Phase 38 | TBD |

### 🎮 Gaming Platform Pipeline

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 59 | Mash Discord Bot Foundation | High | Phase 58 | 2-3 months |
| 60 | Mash Server Control (!start, !stop, !status) | High | Phase 59 | 2-3 months |
| 61 | Mash Player Activity Announcements | Medium | Phase 60 | 3-4 months |
| 62 | Mash Scheduled Reminders & Auto-shutdown | Medium | Phase 61 | 3-4 months |
| 63 | Mash Game Update Notifications | Medium | Phase 62 | 4-5 months |
| 64 | Gaming Pipeline Integration Testing | Low | Phase 63 | 4-5 months |

### 👤 Personal & Knowledge Management

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 16.3 | Monthly Infrastructure Audit (automated) | High | Phase 16.2 | 1-2 months |
| MERLIN | Reminder Agent (SSL, backups, maintenance) | #2 URGENT | Phase 38 | ASAP |
| 17 | Personal Finance Tracking | Medium | Phase 22 | 3-4 months |
| 18 | Subscription Management Automation | Medium | Phase 17 | 4-5 months |
| 19 | Career Transition Portfolio | Medium | Phase 22 | 3-6 months |

### 🏗️ Infrastructure Cleanup & Optimization

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 8 | Infrastructure Cleanup & Optimization | Medium | Phase 7A | 2-3 months |
| 10 | Off-site Backup (Backblaze B2) | Medium | Phase 8 | 3-4 months |
| 11 | Container Migration to ZFS | Low | Phase 8 | 4-6 months |
| 12 | Hardware Refresh Planning | Low | Phase 11 | 6+ months |

### 🌐 Core Services & Network

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 20 | WiFi Access Point Setup (EAP610) | Medium | Hardware | 1-2 months |
| 21 | Advanced Network Monitoring | Medium | Phase 20 | 2-3 months |
| 25 | DNS over HTTPS/TLS | Low | Phase 21 | 3-4 months |
| 26 | Network Intrusion Detection | Low | Phase 25 | 4-5 months |

### 🤖 AI & Local LLM Development

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 39 | Model Performance Benchmarking | Medium | Phase 38 | 1-2 months |
| 40 | Custom Model Fine-tuning | Low | Phase 39 | 3-6 months |
| 42 | AI Agent Ecosystem Expansion | Medium | Phase 38 | 2-4 months |
| 43 | Voice Assistant Integration | Low | Phase 42 | 6+ months |

### 💼 Career Development

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 27 | Kubernetes Lab Environment | Medium | Phase 8 | 3-4 months |
| 28 | Terraform Infrastructure as Code | Medium | Phase 27 | 4-5 months |
| 29 | AWS/Azure Integration | Medium | Phase 28 | 5-6 months |
| 30 | CI/CD Pipeline Implementation | Medium | Phase 29 | 6+ months |

### 🔮 Long Term Vision

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 44-50 | Multi-Site Deployment | Low | Phase 30 | 12+ months |
| 51-57 | Business Consulting Platform | Low | Phase 50 | 18+ months |
| 65+ | Community & Open Source Contributions | Low | All phases | 24+ months |

### 🔧 Hardware & Physical Infrastructure

| Phase | Title | Priority | Dependencies | Timeline |
|-------|-------|----------|--------------|----------|
| 31 | Rack Mounting & Cable Management | Low | Phase 12 | 6+ months |
| 32 | Redundant Power & UPS | Low | Phase 31 | 6+ months |
| 33 | Environmental Monitoring | Low | Phase 32 | 9+ months |
| 34-37 | Hardware Expansion Planning | Low | Phase 33 | 12+ months |

---

## 🎯 Critical Path Analysis

### Near-Term Dependencies (Next 8 weeks)
1. **Phase 38** (Ollama + ROCm) — CRITICAL for cost control
2. **MERLIN Agent** — URGENT for memory/reminder issues
3. **Phase 24.1** (Update Manager) — Infrastructure maintenance
4. **Phase 16.3** (Monthly Audit) — Automated reporting

### Gilgamesh Evolution Critical Path (16 weeks)
**Goal:** Replace claude.ai by August 2026
1. Phase 38 (Foundation) → Weeks 1-4
2. Phase 7E + 41 (Intelligence) → Weeks 5-8  
3. Phase 7F + 7G (Capabilities) → Weeks 9-12
4. Phase 7I + 7J (Migration) → Weeks 13-16

### Gaming Platform Critical Path (12 weeks)
**Goal:** Discord bot with full server management
1. Phase 59 (Foundation) → Weeks 1-4
2. Phase 60 + 61 (Core Features) → Weeks 5-8
3. Phase 62 + 63 (Advanced Features) → Weeks 9-12

---

## 📈 Success Metrics

### Infrastructure Maturity
- [ ] 99.5% uptime across all services
- [ ] <5 minute mean time to recovery (MTTR)
- [ ] Zero data loss incidents
- [ ] 100% automated backup verification

### Cost Optimization
- [ ] Claude API costs <$10/month (currently $30-40)
- [ ] Infrastructure costs <$50/month total
- [ ] 80% workload running on local hardware

### Automation Coverage
- [ ] 90% routine tasks automated
- [ ] Zero manual secret management
- [ ] Automated documentation updates
- [ ] Proactive maintenance scheduling

### Knowledge Management
- [ ] 100% session summaries in Obsidian
- [ ] Real-time infrastructure documentation
- [ ] Automated learning progress tracking
- [ ] Public portfolio updates

---

## 🗓️ Recommended Next Session Order

### Immediate Priority (Next 3 sessions)
1. **Phase 38:** Ollama + ROCm setup (cost control)
2. **MERLIN Agent:** Reminder system (memory issues)
3. **Phase 24.1:** Service Update Manager (infrastructure maintenance)

### Following Sessions (Sessions 4-8)
4. **Phase 16.3:** Monthly audit automation
5. **Phase 41:** Hybrid AI routing
6. **Phase 7E:** Extended memory for Gilgamesh
7. **Phase 59:** Mash Discord bot foundation
8. **Phase 7F:** File generation capabilities

### Note on Dependencies
- All Gilgamesh evolution phases depend on Phase 38 (local LLM)
- Gaming platform phases can run parallel to AI development
- Infrastructure cleanup can be interleaved with other work

---

*Last updated: April 24, 2026 — Auto-generated via Documentation Pipeline*