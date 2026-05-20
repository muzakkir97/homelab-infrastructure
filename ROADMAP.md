# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** May 20, 2026  
> **Total Phases:** 86 planned | 43 completed | 2 in progress | 41 future  
> **Next Session Priority:** Phase 22.8C (Homepage Widgets)

---

## 📊 Phase Summary

| Category                              | Total | Complete | In Progress | Planned |
|---------------------------------------|-------|----------|-------------|---------|
| **Core Infrastructure & Security**     | 15    | 11       | 1           | 3       |
| **Gaming Platform Pipeline**          | 12    | 6        | 0           | 6       |
| **AI & Automation (Gilgamesh)**       | 19    | 13       | 0           | 6       |
| **Personal & Knowledge Management**    | 12    | 6        | 1           | 5       |
| **Monitoring & Observability**        | 8     | 6        | 0           | 2       |
| **Infrastructure Cleanup**            | 6     | 2        | 0           | 4       |
| **Career Development**                | 4     | 2        | 0           | 2       |
| **Long Term Vision**                  | 7     | 0        | 0           | 7       |
| **Hardware & Upgrades**               | 3     | 1        | 0           | 2       |

---

## ✅ Completed Phases (43)

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
| 15      | Gilgamesh Additional Slash Commands             | Apr 24, 2026 | Phase 7D     |
| 16.1    | Documentation Pipeline — Update Workflow        | Apr 19, 2026 | Phase 7C     |
| 16.2    | Documentation Pipeline — Sync Docs Workflow     | Apr 19, 2026 | Phase 16.1   |
| 16.3    | Da Vinci Documentation Pipeline                 | Apr 25, 2026 | Phase 16.2   |
| 16.4    | Documentation Pipeline Expansion — 8 Files     | May 20, 2026 | Phase 16.3   |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT            | Apr 24, 2026 | Phase 1      |
| 39      | Open WebUI                                      | Apr 24, 2026 | Phase 38     |
| 41      | Gilgamesh + Ollama Hybrid Routing               | Apr 24, 2026 | Phase 38     |
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

---

## ⚡ In Progress (2)

| Phase | Title                           | Status                              | Dependencies | ETA     |
|-------|---------------------------------|-------------------------------------|--------------|---------|
| 22.8C | Homepage Widgets                | 🚧 Next session priority            | Phase 22.8B  | May     |
| Guardian | Security Monitoring Agent     | 📋 Design phase after 22.8C         | Phase 22.8B  | Jun     |

---

## 📋 Planned Phases by Category

### 🤖 Gilgamesh & Automation (Priority: High)

| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| 7E    | Extended Memory (20+ message conversations)     | Da Vinci Stage 2  | 8h     | RAG-powered conversation continuity      |
| 7F    | File Generation (code, configs, docs)           | Phase 7E          | 6h     | Artifact generation capabilities         |
| 7G    | Vision API (image analysis)                     | Phase 7F          | 4h     | Llama 3.2 Vision 11B integration         |
| 7H    | Document Upload (PDF/text processing)           | Phase 7G          | 6h     | Document analysis and Q&A                |
| 7I    | Quality Assurance (compare with Claude Pro)     | Phase 7H          | 4h     | A/B testing and quality metrics          |
| 7J    | Migration (full switch to Gilgamesh)            | Phase 7I          | 2h     | Replace Claude Pro subscription          |

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
| 22.8D  | MERLIN Medication Checks        | Phase 22.8C  | 4h     | Daily medication reminders via buttons  |
| 22.8E  | Life Ledger (Photo + Text)      | Phase 22.8D  | 8h     | Vision AI for expense tracking           |
| 22.15  | Price Database Tracking         | Phase 22.8E  | 6h     | Grocery price comparison and budgeting   |
| 22.16  | Homepage Settings Tab           | Phase 22.15  | 4h     | User-configurable budget percentages     |
| 22.11  | Nextcloud Calendar Integration  | Phase 22.16  | 3h     | CalDAV sync with Obsidian tasks          |

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

### 🔩 Hardware (Priority: Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 52    | DOCP Memory Optimization             | RAM Upgrade  | 2h     | Tune 128GB kit for optimal performance |
| 53    | Second Proxmox Node                  | Budget       | 16h    | High availability cluster setup         |

---

## 🎯 Recommended Next Session Order

### Phase 22.8C — Homepage Widgets (Next Session)
**Effort:** 4 hours  
**Goal:** Add health tracking and infrastructure widgets to Homepage dashboard  
**Deliverables:** Health tab, infrastructure tab, webhook integration with n8n

### Da Vinci Stage 2 — RAG System (Session After)
**Effort:** 8-10 hours  
**Goal:** Knowledge retrieval across all agent conversations  
**Deliverables:** Qdrant vector database, embedding pipeline, knowledge recall

### Guardian Agent (Third Session)
**Effort:** 6-8 hours  
**Goal:** Security monitoring and threat detection  
**Deliverables:** Alert translation, threat detection, security reporting

### Phase 7E — Extended Memory (Fourth Session)
**Effort:** 8 hours  
**Goal:** 20+ message conversation continuity via RAG  
**Deliverables:** Enhanced conversation memory, context preservation

---

## 🔗 Phase Dependencies

**Critical Path Analysis:**
1. **Gilgamesh Enhancement Track:** 22.8C → Da Vinci Stage 2 → Guardian → 7E → 7F-7J
2. **Gaming Platform Track:** Mash 59-64 (can run parallel to Gilgamesh)
3. **Infrastructure Track:** 25-29 (low priority, independent)
4. **Career Development Track:** 35-36 (depends on infrastructure completion)

**Parallel Development Opportunities:**
- Gaming platform phases can run alongside AI development
- Infrastructure cleanup can happen during slower development periods
- Hardware upgrades can be integrated into any session

---

*Last updated: May 20, 2026 — Next update after Phase 22.8C completion*