# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** July 14, 2026  
> **Total Phases:** 104 planned | 52 completed | 0 in progress | 52 future  
> **Next Session Priority:** Chaldea Rename Propagation OR Deck Sync Manual Backfill OR Jeanne Alter Web Search Quality Improvement

---

## 📊 Phase Summary

| Category                              | Total | Complete | In Progress | Planned |
|---------------------------------------|-------|----------|-------------|---------|
| **Core Infrastructure & Security**     | 16    | 11       | 0           | 5       |
| **Gaming Platform Pipeline**          | 12    | 6        | 0           | 6       |
| **AI & Automation (Chaldea)**         | 37    | 22       | 0           | 15      |
| **Personal & Knowledge Management**    | 12    | 7        | 0           | 5       |
| **Monitoring & Observability**        | 8     | 8        | 0           | 0       |
| **Infrastructure Cleanup**            | 8     | 2        | 0           | 6       |
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
| 24 DA Vinci S2 | Da Vinci Stage 2 (RAG System & Knowledge Retrieval) | May 25, 2026 | Phase 24.10 |

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

### 🤖 Chaldea Agents & Automation (Priority: High)

#### Architecture & Refactoring
| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| 16.6  | Chaldea Rename Propagation (Gilgamesh → Jeanne Alter) | Phase 24.10 | 8h     | Rename bot identity, system prompt, Telegram username (@JhinGilgamesh_bot), and propagate through all 8 documentation files |
| 16.7  | Da Vinci → Nextcloud Deck Sync Pipeline       | Phase 24.10       | 6h     | 9th pipeline step: sync documentation changes to Homelab board via Deck API; match-and-update via sync-id tags; status-keyword-based stack routing |
| 16.8  | Deck Sync Manual Backfill                     | Phase 16.7        | 2h     | Manually tag all ~30 existing Homelab Deck cards with sync-id footers before automation goes live |
| 24.11 | Ecosystem-wide Credential Store Migration     | Phase 14          | 10h    | Move all hardcoded n8n credentials (Da Vinci, MERLIN, Midas, Jeanne Alter) to n8n's built-in credential store; applies to every agent |
| 24.12 | Jeanne Alter Architecture Refactor            | Phase 24.11       | 15h    | Adopt n8n AI Agent node (replace hand-built If/branch logic), integrate MCP for tool execution, integrate Mem0 self-hosted (Qdrant-backed) for unified memory, restructure persona into personality file |
| 24.13 | Universal Time/Date Awareness                | Phase 24.12       | 1h     | Inject current date/time into every agent's system prompt (pattern: Da Vinci's {{date}} replacement), applied ecosystem-wide instead of single gateway |
| 24.14 | Jeanne Alter Web Search Quality Improvement  | Phase Web Search  | 4h     | Iterative multi-query search with synthesis step, closer to Gemini-style search; replace single Firecrawl + Haiku call pattern |
| 24.15 | Jeanne Alter Email Management Pipeline       | Phase 24.11       | 6-8h   | Design Complete (July 9, 2026). 4 personal email accounts (Gmail OAuth2 × 3, iCloud IMAP × 1) → shared Email Classifier sub-workflow → Telegram notification + permanent-category facts (bills/payments/subscriptions) to Da Vinci Personal Knowledge gateway. 3x daily schedule. Credentials in n8n store. qwen3:14b primary, Claude Haiku fallback. 6 rollout steps: credentials → classifier → account triggers → staging store → notification → verify writes. |

#### Agent Feature Development
| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| Cu Chulainn | Security Monitoring Agent                   | Phase 24.10       | 6-8h   | Renamed May 16, 2026 from "Guardian"; alert translation, threat detection, security reporting; 2nd build priority; funnel agent sourcing from Alertmanager once built |
| Smart Morning Briefing | Dynamic Briefing (weather, schedule, health, tasks) | Phase 24.10 | 2h | Real-time weather via Firecrawl, calendar awareness, health nudges, task integration |
| Midas v2 | Financial Intelligence (Firefly III + Receipt Capture) | Phase Midas | 10-12h | Expense tracking, receipt/PDF import, spending insights; cost source migration to Langfuse Metrics API pending (small follow-up task, not urgent) |
| Proactive Goal Nudges | Condition-based alerts via ntfy      | Phase 24.10       | 3-4h   | Goal tracking with Telegram notifications |
| Weekly Automated Review | Sunday digest (health, spending, homelab) | Phase 24.10 | 2-3h | Summarized weekly activity report |
| "What did I do?" Recall | Natural language query over life_log | Phase 24.10 | 2h     | Search personal activity history |
| URL/Article Summarizer | Send link, Jeanne Alter summarizes via Firecrawl | Phase Web Search | 1-2h | Link-based content summarization |
| Grocery/Task List Management | Telegram-based list sync          | Phase 24.10       | 2h     | Task and grocery list management |
| Calendar Awareness | Google Calendar integration           | Phase 24.10       | 3-4h   | Schedule-aware responses |
| News Digest | Curated tech/AI/Malaysia news on demand | Phase Web Search | 2h     | Daily news briefing |
| Voice Notes | Whisper transcription → Obsidian   | Phase 24.10       | 4h     | Voice message processing |
| Plan My Day | Schedule + tasks + energy = daily suggestion | Phase 24.10 | 4-5h | Daily planning assistant |
| Scathach | Career Growth & Research Agent (LangGraph) | Phase 24.12       | TBD    | 1st build priority; career research and job application workflows; LangGraph evaluation pending, sequenced after Phase 24.12 (Jeanne Alter refactor to n8n AI Agent node may address the same multi-step reasoning problem) |
| Nightingale | Health Pipeline Agent                       | Phase 24.10       | TBD    | Extract health tracking (food/BP/medication logging) into dedicated agent; concept only, not yet scoped |

#### Research & Future (No Priority Yet)
| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| Multi-Agent Discussion Protocol | CrewAI-based agent-to-agent communication | Phase 24.12 | Research | Complexity-based trigger (quick topics live via Telegram, complex topics run in background). Jeanne Alter presents both sides on disagreement, user arbitrates. Summary-only reporting by default. Explicitly deferred — research/keep-in-view only. |
| Solomon Agent | Overseer/growth agent (weekly review) | Phase 24.12 | Research | FGO-lore fit: Chaldea administrator. Weekly cron-triggered review of all agents' activity/logs/decisions; proposes fixes for user approval. Human-in-the-loop growth, not autonomous self-modification. |
| Voice Organ (Jeanne Alter) | Whisper STT + local TTS | Phase 24.12 | Research | Chosen over vision-first investment due to VRAM constraints (single RX 6700 XT 12GB cannot run vision + main chat model concurrently). |
| Shared MCP Tool Layer | Multi-agent tool execution framework | Phase 24.12 | Research | One shared tool layer + shared Mem0 instance across all agents instead of rebuilding per-agent. |
| EMILIA (Vision-First Agent) | Advanced vision/perception capabilities | Phase 43 | Research | Explicitly deferred due to VRAM contention risk on current single-GPU hardware. Llama 3.2 Vision model and live browser vision/control both pending GPU upgrade. |

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
| 25    | WiFi Access Point Deployment         | Hardware     | 4h     | EAP610 deployment COMPLETE (July 5, 2026) — AX1800 access point (SSID `A21-22A`) now live on VLAN20_MAIN via TL-SG108E port 7. Functioning correctly after VLAN misconfiguration fix. |
| 26    | Legacy Network Cleanup               | Phase 25     | 3h     | Partially addressed (July 5): TL-SG108E ports 7-8 VLAN migration from legacy VLAN 1 to VLAN20_MAIN complete. Remaining work: migrate switch management IP from 192.168.1.20 (legacy) to proper VLAN10_MGMT address. |
| 27    | Domain Migration & Infrastructure Audit | Phase 14 | 8h     | **27.1 (audit):** Cloudflare Access policies, Tunnel routes, SSL, NPM configs, DNS hygiene audit across existing setup. **27.2 (migration):** Nine homelab subdomains move from najhin-gaming.com to muzakkir.tech (grafana, n8n, vault, passwords/Vaultwarden, cloud/Nextcloud, finance/Firefly III, ntfy, langfuse, home/Pulse Dashboard). Game server subdomains (mc, terraria, enshrouded, panel) remain permanently on najhin-gaming.com. Cloudflare zone setup for muzakkir.tech directed to begin July 1, 2026 — completion status unconfirmed, verify next infrastructure session. |
| 28    | Storage Optimization                 | —            | 6h     | Move Nextcloud data, thin pool cleanup |
| 29    | Performance Monitoring Expansion     | Phase 5      | 5h     | Advanced metrics and alerting rules    |

### 🛡️ Core Services (Priority: Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 30    | n8n + Vault Integration              | Phase 14     | 3h     | Direct secret fetching in workflows    |
| 31    | Off-site Backup (Backblaze B2)       | Phase 7A     | 6h     | 3-2-1 backup strategy completion       |

### 🤖 AI & Local LLM (Priority: High)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 42    | Llama 3.2 Vision Model Deployment   | Phase 38     | 2h     | Pull and configure vision model (deferred due to VRAM constraints) |
| 43    | Vision API Integration               | Phase 42     | 4h     | Connect vision to Jeanne Alter workflows (deferred due to VRAM constraints) |

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

### MERLIN SSL Check Migration to Uptime Kuma (Urgent Priority)
**Effort:** 1-2 hours (precedes all other sessions)  
**Status:** Completed July 14, 2026  
**Summary:** SSL expiry monitoring re-sourced from hardcoded Cloudflare API (broken) to Uptime Kuma (CT 206, already deployed). Alert was legitimate in design intent; root cause was broken detection mechanism, not the rule. Confirmed fix via `curl localhost:9100/metrics`, Prometheus query, and Alertmanager alert auto-resolution.

### Chaldea Rename Propagation (Next Session)
**Effort:** 8 hours  
**Goal:** Full ecosystem rename from Gilgamesh → Jeanne Alter across bot identity, system prompt, Telegram username, and all 8 documentation files  
**Deliverables:** Consistent agent naming across all systems, updated Telegram bot handle

### Deck Sync Manual Backfill (Second Session)
**Effort:** 2 hours  
**Goal:** Manually tag all ~30 existing Homelab Deck cards with sync-id footers  
**Precondition for:** Phase 16.7 (Da Vinci → Nextcloud Deck Sync Pipeline)  
**Deliverables:** All cards ready for automated sync integration

### Jeanne Alter Web Search Quality Improvement (Third Session)
**Effort:** 4 hours  
**Goal:** Iterative multi-query search with synthesis, closer to Gemini-style search  
**Deliverables:** Improved search result relevance and comprehensiveness

### Jeanne Alter Email Management Pipeline (Fourth Session)
**Effort:** 6-8 hours  
**Goal:** Implement email read + notification pipeline for 4 personal accounts with permanent-fact routing to Da Vinci Personal Knowledge system  
**Precondition:** Credentials in n8n store (first concrete step of Phase 24.11); initial sender whitelist supplied  
**Deliverables:** Email classifier, 4× account triggers, staging store with auto-clear, Telegram notifications, Obsidian writes for bills/payments/subscriptions

### Midas v2 — Financial Intelligence (Fifth Session)
**Effort:** 10-12 hours  
**Goal:** Firefly III integration with receipt and PDF statement capture  
**Deliverables:** Expense tracking, receipt OCR, PDF import, spending insights via Jeanne Alter

---

## 🔗 Phase Dependencies

**Critical Path Analysis:**
1. **Chaldea Architecture Track:** Phase 24.12 (Jeanne Alter Refactor) → 24.13 (Universal Time/Date) → Multi-Agent Discussion Protocol
2. **Documentation & Integration Track:** Phase 16.6 (Rename) → 16.7 (Deck Sync) → 16.8 (Deck Backfill)
3. **Agent Feature Development Track:** Phase 24.12 → Scathach (build priority 1st, LangGraph evaluation sequenced after 24.12) → Cu Chulainn (build priority 2nd) → Goal Nudges → Plan My Day
4. **Email & Credential Track:** Phase 24.11 (Credential Store Migration) → Phase 24.15 (Email Management) → Phase 24.12 (unified memory in refactor)
5. **Gaming Platform Track:** Mash 59-64 (can run parallel to Chaldea development)
6. **Infrastructure Track:** 25-29 (low priority, mostly independent; Phases 25 & 26 progressing; Phase 27 newly prioritized)
7. **Career Development Track:** 35-36 (depends on infrastructure completion)

**Parallel Development Opportunities:**
- Gaming platform phases can run alongside Chaldea development
- Infrastructure cleanup can happen during slower development periods
- Agent feature development can be integrated into Jeanne Alter refactor
- Deck sync backfill can occur while other sessions are in progress
- Domain migration audit (Phase 27.1) can run independently of Scathach/Cu Chulainn builds
- Email Management Pipeline (24.15) can proceed in parallel with Rename Propagation (16.6) after credentials are in n8n store

---

## 📌 July 9, 2026 Documentation Audit Session — Key Findings

### Agent Design Principles
A design principle now guides all future agent classification: **Funnel Agents**. An agent is a "funnel" if there is existing infrastructure it can sit on top of and translate into a daily digest with a recommended action, rather than re-implementing the underlying check itself. 

- **Funnel agents** (July 9, 2026): MERLIN (sources from Uptime Kuma), Midas (sources from Langfuse), Cu Chulainn (sources from Alertmanager, once built)
- **Not funnels — Interface layer:** Jeanne Alter (she is the interface itself; nothing underneath to translate)
- **Not funnels — Knowledge system:** Da Vinci (creates and maintains state; doesn't aggregate someone else's data)
- **Not funnels — Generative:** Scathach (career research and reasoning; nothing existing to funnel from)
- **Hybrid — Funnel-Plus:** EMIYA (reads infra state like a funnel, but also executes changes on approval)

**Why this matters:** Prevents agents from duplicating effort already solved by dedicated tools (e.g., MERLIN re-implementing SSL checks that Uptime Kuma already does, Midas re-calculating costs that Langfuse already tracks). Reduces drift risk between duplicate data sources and keeps agent build time focused on the actual differentiator — aggregation and recommendation — rather than plumbing.

### MERLIN Status: Partial Deployment with Critical Urgency → RESOLVED (July 14, 2026)
- **Deployed:** April 27, 2026, v1
- **Prior Status:** Partially functional — SSL check non-functional (hardcoded date, broken Cloudflare API token dependency)
- **Urgency (July 9):** SSL certificate expiration July 14, 2026 (5 days from audit date)
- **Resolution (July 14, 2026):** Re-sourced SSL check from Uptime Kuma (CT 206, already deployed) which has native HTTPS certificate monitoring. Removed Cloudflare token dependency entirely. 1-2 hour fix completed. Alert integrity verified — rule logic was sound, detection mechanism was broken.
- **Remaining design gap:** Currently reports raw numbers ("X days until expiry") without a recommended action. Per Funnel Agent principle, MERLIN should output two things: status + action recommendation.

### Midas Status: Partial Deployment with Cost Duplication Issue
- **Deployed:** April 27, 2026, v1
- **Status:** Functional but has a cost-tracking duplication problem
- **Decision:** Haiku/Claude cost data should source from Langfuse Metrics API (CT 223 already deployed and wired to Da Vinci + Jeanne Alter) instead of the separate `gilgamesh_costs` Data Table. Both currently track the same Haiku calls independently, risking drift. Ollama cost/savings tracking stays in Data Table since Ollama is $0 and outside Langfuse's automatic cost-inference model.
- **Design gap:** Currently reports totals without a recommended action. Per Funnel Agent principle, when spend trends toward the $10 monthly limit, Midas should suggest a specific action (e.g., "route more queries to Ollama" or name the highest-cost command_type).

### EMIYA Status: Concept Stage with Infrastructure Underneath
- **Status:** 3 of 8 sub-phases complete (24.1, 24.7, 24.8), but no unified running agent workflow yet
- **Completed sub-phases:** 
  - 24.1 (App Management) ✅ — Firefly III, ntfy, Langfuse containers deployed
  - 24.7 (Universal Notifications) ✅ — ntfy hub for all agent communications
  - 24.8 (Langfuse Observability) ✅ — LLM performance tracking wired
- **Incomplete sub-phases:** 24.2-24.6 (Alert Translation, Container Updates, Knowledge Ingestion, Proactive Monitoring, Performance Optimization)
- **Gap:** EMIYA is documented as a roadmap category with completed infrastructure underneath it, not yet a unified running workflow that reasons over that infrastructure. Once scoped, the agent translation layer can be built on top of these existing systems.
- **Overlap flagged:** EMIYA's planned Alert Translation (24.2) and Cu Chulainn's core responsibility both source from Alertmanager. Worth deciding later whether these merge into one workflow with two lenses (general ops vs. security) or remain separate.

### Cu Chulainn Status: Renamed Concept, Build Priority 2nd
- **Renamed:** May 16, 2026 from "Guardian"
- **Build priority:** 2nd (behind Scathach)
- **Status:** Concept stage — no implementation work begun
- **Role:** Security monitoring and threat detection agent; funnel pattern (translates Alertmanager alerts into readable, prioritized security reporting)
- **Gap:** All n8n workflow references still use old "Guardian" name — name propagation pending alongside broader Jeanne Alter rename.
- **Design note:** In FGO canon, Cu Chulainn is Scathach's student, reflected in build order.

### Scathach Status: Named Concept, Build Priority 1st, LangGraph Evaluation Pending
- **Prioritized:** May 16, 2026 as 1st build priority in entire agent roster
- **Status:** Concept stage — no implementation work begun
- **Role:** Career growth and job application workflows; **not** a funnel agent (generative reasoning work, nothing existing to translate)
- **Blocking prerequisite (updated July 9):** LangGraph evaluation should happen *after* Phase 24.12 (Jeanne Alter refactor to n8n's native AI Agent node), not standalone. Phase 24.12 tests whether n8n's native AI Agent node can handle multi-step autonomous reasoning — if it can, that same node may cover Scathach's needs too, avoiding the need for a second orchestration framework. Original decision (LangGraph must be evaluated before implementation scope) still holds, but now sequenced behind 24.12.
- **Design note:** In FGO canon, Scathach is Cu Chulainn's teacher, master of combat and strategy.

### Jeanne Alter Email Management Pipeline: Design Complete (July 9, 2026)
- **Status:** Design Complete, not yet implemented; 0/6 rollout steps deployed
- **Scope:** 4 personal email accounts (muzakkir.kholil06@gmail.com, muzakkirkholil97@i