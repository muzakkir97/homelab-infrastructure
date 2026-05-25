# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** May 25, 2026  
> **Total Phases:** 91 planned | 52 completed | 0 in progress | 39 future  
> **Next Session Priority:** Midas v2 (Financial Intelligence) OR Smart Morning Briefing OR Guardian Agent

---

## 📊 Phase Summary

| Category                              | Total | Complete | In Progress | Planned |
|---------------------------------------|-------|----------|-------------|---------|
| **Core Infrastructure & Security**     | 15    | 11       | 0           | 4       |
| **Gaming Platform Pipeline**          | 12    | 6        | 0           | 6       |
| **AI & Automation (Gilgamesh)**       | 26    | 22       | 0           | 4       |
| **Personal & Knowledge Management**    | 12    | 7        | 0           | 5       |
| **Monitoring & Observability**        | 8     | 8        | 0           | 0       |
| **Infrastructure Cleanup**            | 6     | 2        | 0           | 4       |
| **Career Development**                | 4     | 2        | 0           | 2       |
| **Long Term Vision**                  | 7     | 0        | 0           | 7       |
| **Hardware & Upgrades**               | 3     | 3        | 0           | 0       |

---

## ✅ Completed Phases (52)

### Core Infrastructure Foundation
| Phase | Title                                         | Completed    | Dependencies |
|-------|-----------------------------------------------|--------------|--------------|
| 1     | Proxmox VE Installation                       | Jan 2026     | —            |
| 2     | pfSense Firewall & VLAN Setup                 | Jan 2026     | Phase 1      |
| 3     | Core Services (Pi-hole, NPM, Tailscale, DDNS) | Jan 2026     | Phase 2      |
| 4     | External Access & SSL                         | Feb 2026     | Phase 3      |
| 5     | Monitoring Stack                              | Feb 2026     | Phase 4      |
| 6F    | Infrastructure Audit & Firewall Hardening    | Mar 9, 2026  | Phase 5      |
| 7A    | Backup Strategy                               | Mar 13, 2026 | Phase 7      |
| 9     | NAS Deployment (Kinmoon)                      | Mar 3, 2026  | —            |
| 13    | HashiCorp Vault — Secrets Manager             | Apr 18, 2026 | Phase 5      |
| 14    | Secrets Management & Integration              | Apr 24, 2026 | Phase 13     |
| 23    | Vaultwarden + Secrets Audit & Cleanup         | Apr 18, 2026 | Phase 13     |

### Gaming Platform
| Phase | Title                                    | Completed   | Dependencies |
|-------|------------------------------------------|-------------|--------------|
| 6A-6D | Gaming Platform (Pterodactyl, Servers)  | Feb 2026    | Phase 5      |
| 6E    | Homepage Dashboard                       | Mar 2026    | Phase 6D     |
| 58    | Windrose Server Deployment               | Apr 19, 2026| Phase 6D     |

### Storage & Applications  
| Phase | Title                | Completed    | Dependencies |
|-------|----------------------|--------------|--------------|
| 7     | Nextcloud Deployment | Mar 8, 2026  | Phase 5      |

### Automation & AI
| Phase   | Title                                           | Completed    | Dependencies |
|---------|-------------------------------------------------|--------------|--------------|
| 7B      | n8n Workflow Automation                         | Apr 2, 2026  | Phase 7      |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration     | Apr 2, 2026  | Phase 7B     |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Search)| Apr 6, 2026  | Phase 7C     |
| 7D-Sec  | Cloudflare Access for n8n                       | Apr 7, 2026  | Phase 7D     |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                  | Apr 24, 2026 | Phase 7D     |
| 7E      | Extended Memory (Conversation Archival)        | May 15, 2026 | Phase 7D     |
| 15      | Gilgamesh Additional Slash Commands             | Apr 24, 2026 | Phase 7D     |
| 16.1    | Documentation Pipeline — Update Workflow        | Apr 19, 2026 | Phase 7C     |
| 16.2    | Documentation Pipeline — Sync Docs Workflow     | Apr 19, 2026 | Phase 16.1   |
| 16.3    | Da Vinci Documentation Pipeline                 | Apr 25, 2026 | Phase 16.2   |
| 16.4    | Documentation Pipeline Expansion — 8 Files     | May 21, 2026 | Phase 16.3   |
| 16.5    | Da Vinci Update Pipeline Rebuild                | May 19, 2026 | Phase 16.4   |
| 24.8    | Langfuse Wiring (Da Vinci)                      | May 21, 2026 | Phase 16.5   |
| 24.9    | Personal Knowledge System (Gil → Da Vinci → Obsidian) | May 22, 2026 | Phase 24.8 |
| 24.10   | Triggered Qdrant Re-indexing                    | May 25, 2026 | Phase 24.9   |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT            | Apr 24, 2026 | Phase 1      |
| 39      | Open WebUI                                      | Apr 24, 2026 | Phase 38     |
| 41      | Gilgamesh + Ollama Hybrid Routing               | Apr 24, 2026 | Phase 38     |
| Web Search | Gilgamesh Web Search (Firecrawl Integration)  | May 25, 2026 | Phase 24.10  |
| Midas   | Midas CFO Agent                                 | Apr 27, 2026 | Phase 7D     |
| MERLIN  | MERLIN Reminders Agent                          | Apr 27, 2026 | Phase 7D     |

### Knowledge Management
| Phase | Title                                      | Completed    | Dependencies |
|-------|--------------------------------------------|--------------|--------------|
| 22    | Obsidian Knowledge Base                    | Apr 24, 2026 | Phase 7      |
| 22.1  | Obsidian Vault Structure Expansion        | Apr 27, 2026 | Phase 22     |
| 22.2  | Obsidian Daily Notes + Morning Briefing   | Apr 27, 2026 | Phase 22.1   |
| 22.8A | Button Menu System + Community Nodes      | Apr 27, 2026 | Phase 7D-Menu|
| 22.8B | Health Tracking (Food/BP/Medication Logging) | Apr 28, 2026 | Phase 22.8A |

### Hardware
| Phase | Title                                      | Completed    | Dependencies |
|-------|--------------------------------------------|--------------|--------------|
| 52    | DOCP Memory Optimization                   | May 2026     | —            |

---

## ⚡ In Progress (0)

No phases currently in progress.

---

## 📋 Planned Phases by Category

### 🤖 Gilgamesh & Automation (Priority: High)

| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| Da Vinci Stage 2 | RAG System & Knowledge Retrieval     | Phase 24.10       | —      | COMPLETE — RAG pipeline fully operational via Phase 24.9. Qdrant embeddings, Knowledge Indexer, and Gilgamesh RAG queries all active. No separate deployment required. |
| Guardian | Security Monitoring Agent                   | Phase 24.10       | 6-8h   | Alert translation, threat detection, security reporting |
| Smart Morning Briefing | Dynamic Briefing (weather, schedule, health, tasks) | Phase 24.10 | 2h | Real-time weather via Firecrawl, calendar awareness, health nudges, task integration |
| Midas v2 | Financial Intelligence (Firefly III + Receipt Capture) | Phase Midas | 10-12h | Expense tracking, receipt/PDF import, spending insights |
| Proactive Goal Nudges | Condition-based alerts via ntfy      | Phase 24.10       | 3-4h   | Goal tracking with Telegram notifications |
| Weekly Automated Review | Sunday digest (health, spending, homelab) | Phase 24.10 | 2-3h | Summarized weekly activity report |
| "What did I do?" Recall | Natural language query over life_log | Phase 24.10 | 2h     | Search personal activity history |
| URL/Article Summarizer | Send link, Gil summarizes via Firecrawl | Phase Web Search | 1-2h | Link-based content summarization |
| Grocery/Task List Management | Telegram-based list sync          | Phase 24.10       | 2h     | Task and grocery list management |
| Calendar Awareness | Google Calendar integration           | Phase 24.10       | 3-4h   | Schedule-aware responses |
| News Digest | Curated tech/AI/Malaysia news on demand | Phase Web Search | 2h     | Daily news briefing |
| Voice Notes | Whisper transcription → Obsidian   | Phase 24.10       | 4h     | Voice message processing |
| Plan My Day | Schedule + tasks + energy = daily suggestion | Phase 24.10 | 4-5h | Daily planning assistant |

### 🎮 Gaming Platform Pipeline (Priority: Medium)

| Phase | Title                              | Dependencies | Effort | Notes                                    |
|-------|------------------------------------|--------------|--------|------------------------------------------|
| 59    | Mash Discord Bot Foundation        | Phase 58     | 6h     | Basic Discord integration and commands   |
| 60    | Mash Game Server Control           | Phase 59     | 4h     | Start/stop server commands               |
| 61    | Mash Player Notifications          | Phase 60     | 3h     | Join/leave announcements                 |
| 62    | Mash Auto-shutdown                 | Phase 61     | 3h     | Idle server management                   |
| 63    | Mash Game Night Scheduling         | Phase 62     | 4h     | Automated reminders and coordination     |
| 64    | Mash Update Notifications          | Phase 63     | 2h     | Game version tracking and alerts         |

### 📊 Personal & Knowledge (Priority: Medium)

| Phase  | Title                           | Dependencies | Effort | Notes                                    |
|--------|---------------------------------|--------------|--------|------------------------------------------|
| 22.8D  | MERLIN Medication Checks        | ARCHIVED     | —      | Archived May 10, 2026 — Homepage retired. Medication reminders already implemented in Phase 22.8B. |
| 22.8E  | Life Ledger (Photo + Text)      | ARCHIVED     | —      | Archived May 10, 2026 — Homepage chain dead. Vision expense tracking moved to Phase 7G (Vision API). |
| 22.15  | Price Database Tracking         | ARCHIVED     | —      | Archived May 10, 2026 — Grocery tracking handled by Firefly III (CT 221). |
| 22.16  | Homepage Settings Tab           | ARCHIVED     | —      | Archived May 10, 2026 — Homepage replaced by Pulse Dashboard. |

### 🔧 Infrastructure Cleanup (Priority: Low-Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 25    | WiFi Access Point Deployment         | Hardware     | 4h     | EAP610 configuration and VLAN setup    |
| 26    | Legacy Network Cleanup               | Phase 25     | 3h     | Remove 192.168.1.0/24, switch IP update|
| 28    | Storage Optimization                 | —            | 6h     | Move Nextcloud data, thin pool cleanup |
| 29    | Performance Monitoring Expansion     | Phase 5      | 5h     | Advanced metrics and alerting rules    |

### 🛡️ Core Services (Priority: Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 27    | n8n + Vault Integration              | Phase 14     | 3h     | Direct secret fetching in workflows    |
| 30    | Off-site Backup (Backblaze B2)       | Phase 7A     | 6h     | 3-2-1 backup strategy completion       |

### 🤖 AI & Local LLM (Priority: High)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 42    | Llama 3.2 Vision Model Deployment   | Phase 38     | 2h     | Pull and configure vision model         |
| 43    | Vision API Integration               | Phase 42     | 4h     | Connect vision to Gilgamesh workflows  |

### 💼 Career Development (Priority: Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 35    | Infrastructure as Code (Terraform)   | Phase 29     | 12h    | Automate infrastructure deployment      |
| 36    | CI/CD Pipeline                       | Phase 35     | 10h    | GitLab/GitHub Actions for homelab       |

### 🔮 Long Term (Priority: Low)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 45    | Kubernetes Cluster                   | Phase 36     | 20h    | K3s or full Kubernetes deployment      |
| 46    | Service Mesh (Istio)                 | Phase 45     | 15h    | Advanced networking and security        |
| 47    | GitOps (ArgoCD)                      | Phase 45     | 8h     | Automated application deployment        |
| 48    | Multi-node Expansion                 | Phase 45     | 24h    | Scale beyond single Proxmox host       |
| 49    | Edge Computing Integration           | Phase 48     | 16h    | Raspberry Pi cluster integration        |
| 50    | Advanced AI Workloads                | Phase 43     | 20h    | Large model training and inference      |
| 51    | Open Source Contributions            | Phase 46     | Ongoing| Give back to projects used              |

---

## 🎯 Recommended Next Session Order

### Midas v2 — Financial Intelligence (Next Session)
**Effort:** 10-12 hours  
**Goal:** Firefly III integration with receipt and PDF statement capture  
**Deliverables:** Expense tracking, receipt OCR, PDF import, spending insights via Gil

### Smart Morning Briefing (Alternative Next Session)
**Effort:** 2 hours  
**Goal:** Dynamic briefing with real-time weather, schedule, health nudges, tasks  
**Deliverables:** Morning routine automation, Firecrawl weather integration, task awareness

### Guardian Agent (Third Session)
**Effort:** 6-8 hours  
**Goal:** Security monitoring and threat detection agent  
**Deliverables:** Alert translation, threat detection, Telegram security reporting

### Proactive Goal Nudges (Fourth Session)
**Effort:** 3-4 hours  
**Goal:** Condition-based alerts via ntfy integration  
**Deliverables:** Goal tracking notifications, habit reminders

---

## 🔗 Phase Dependencies

**Critical Path Analysis:**
1. **Gilgamesh Enhancement Track:** Phase 24.10 → Guardian → Goal Nudges → Plan My Day
2. **Knowledge & Tracking Track:** Smart Morning Briefing → Midas v2 → Weekly Review
3. **Gaming Platform Track:** Mash 59-64 (can run parallel to Gilgamesh)
4. **Infrastructure Track:** 25-29 (low priority, independent)
5. **Career Development Track:** 35-36 (depends on infrastructure completion)

**Parallel Development Opportunities:**
- Gaming platform phases can run alongside AI development
- Infrastructure cleanup can happen during slower development periods
- Agent feature development can be integrated into existing workflows
- Gilgamesh feature wishlist (16 items) can be prioritized into dedicated sessions

---

*Last updated: May 25, 2026 — Phase 24.10 (Triggered Qdrant Re-indexing) complete and deployed. Phase 52 (DOCP Memory Optimization) confirmed complete. Web Search (Gilgamesh) deployed with Firecrawl integration. Da Vinci Stage 2 marked complete — RAG pipeline fully operational via Phase 24.9. Phase 7E duplicate removed from Planned table (already in Completed, conversation buffer already operational). Gilgamesh feature wishlist (16 items) added to Planned phases. Next session priority: Midas v2 (financial intelligence) OR Smart Morning Briefing OR Guardian Agent.*