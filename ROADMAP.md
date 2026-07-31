# 🗺️ Homelab Infrastructure Roadmap

> **Last Updated:** August 1, 2026  
> **Total Phases:** 105 planned | 54 completed | 0 in progress | 51 future  
> **Next Session Priority:** Chaldea Rename Propagation (Gilgamesh → Jeanne Alter) OR Jeanne Alter Email Management Pipeline OR Midas v2 — Financial Intelligence

---

## 📊 Phase Summary

| Category                              | Total | Complete | In Progress | Planned |
|---------------------------------------|-------|----------|-------------|---------|
| **Core Infrastructure & Security**     | 17    | 12       | 0           | 5       |
| **Gaming Platform Pipeline**          | 12    | 6        | 0           | 6       |
| **AI & Automation (Chaldea)**         | 37    | 23       | 0           | 14      |
| **Personal & Knowledge Management**    | 12    | 7        | 0           | 5       |
| **Monitoring & Observability**        | 8     | 8        | 0           | 0       |
| **Infrastructure Cleanup**            | 9     | 3        | 0           | 6       |
| **Career Development**                | 4     | 2        | 0           | 2       |
| **Long Term Vision**                  | 7     | 0        | 0           | 7       |
| **Hardware & Upgrades**               | 3     | 3        | 0           | 0       |

---

## ✅ Completed Phases (54)

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
| 34    | Kinmoon Hard Drive 1 Replacement              | Aug 1, 2026  | Phase 9      |
| MERLIN SSL Check | MERLIN SSL Check Migration to Uptime Kuma | Jul 14, 2026 | Phase 5 |

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
| 32    | DDNS Automation & WAN IP Stability Monitoring | Phase 3           | 4-6h   | **Elevated Priority (July 17, 2026)** due to repeated WAN IP changes within one week (3 changes recorded: 202.184.101.136 → 202.184.103.49 → 202.184.109.124 during session timeframe). Extend CT 207 ddclient automation to cover Palworld egg PublicIP variable in Pelican (currently manual sync required). Add real-time WAN IP monitoring to Uptime Kuma or ntfy to alert when IP changes detected. Consider DNS failover strategy if ISP instability continues. |

#### Agent Feature Development
| Phase | Title                                           | Dependencies      | Effort | Notes                                    |
|-------|-------------------------------------------------|-------------------|--------|------------------------------------------|
| Cu Chulainn | Security Monitoring Agent                   | Phase 24.10       | 6-8h   | Renamed May 16, 2026 from "Guardian"; alert translation, threat detection, security reporting; 2nd build priority; funnel agent sourcing from Alertmanager once built. Note: n8n workflow references still use old "Guardian" name — rename propagation pending alongside Phase 16.6. |
| Smart Morning Briefing | Dynamic Briefing (weather, schedule, health, tasks) | Phase 24.10 | 2h | Real-time weather via Firecrawl, calendar awareness, health nudges, task integration |
| Midas v2 | Financial Intelligence (Firefly III + Receipt Capture) | Phase Midas | 10-12h | Expense tracking, receipt/PDF import, spending insights; cost source migration to Langfuse Metrics API pending (small follow-up task, not urgent). Design gap (July 9, 2026): currently reports totals without recommended actions; per Funnel Agent principle, should suggest spending optimization when approaching $10 monthly limit. |
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
| 27    | Domain Migration & Infrastructure Audit | Phase 14 | 8h     | **27.1 (audit):** Cloudflare Access policies, Tunnel routes, SSL, NPM configs, DNS hygiene audit across existing setup. **27.2 (migration):** Nine homelab subdomains move from najhin-gaming.com to muzakkir.tech (grafana, n8n, vault, passwords/Vaultwarden, cloud/Nextcloud, finance/Firefly III, ntfy, langfuse, home/Pulse Dashboard). Game server subdomains (mc, terraria, enshrouded, panel) remain permanently on najhin-gaming.com. Cloudflare zone setup for muzakkir.tech directed to begin July 1, 2026 — completion status unconfirmed, verify next infrastructure session. **Action Item (July 17, 2026):** Update Cloudflare DNS records for Palworld, Terraria, Minecraft, and Enshrouded subdomains under najhin-gaming.com — currently pointing at stale IPs from before session changes. **Status (Aug 1, 2026):** Vaultwarden (passwords.najhin-gaming.com) tunnel route fixed during Kinmoon recovery session. Game server DNS records still need verification/update. |
| 28    | Storage Optimization                 | —            | 6h     | Move Nextcloud data, thin pool cleanup |
| 29    | Performance Monitoring Expansion     | Phase 5      | 5h     | Advanced metrics and alerting rules    |
| 33    | Maintenance Window Consolidation & Automation | Phase 25 | 4h | **New Phase (July 17, 2026).** Consolidate Palworld + Terraria restart schedules with nightly vzdump backup job into single off-peak maintenance block (exact timing pending confirmation of reliably dead low-usage hour, currently all events scattered). Per-server restart Schedules in Pelican to be set up once window finalized. Reschedule vzdump job (currently ~02:11 AM, conflicts with active gameplay) via Datacenter → Backup. |

### 🛡️ Core Services (Priority: Medium)

| Phase | Title                                | Dependencies | Effort | Notes                                   |
|-------|--------------------------------------|--------------|--------|-----------------------------------------|
| 30    | n8n + Vault Integration              | Phase 14     | 3h     | Direct secret fetching in workflows    |
| 31    | Off-site Backup (Backblaze B2)       | Phase 34     | 6h     | **Status (Aug 1, 2026):** Now unblocked — Kinmoon Storage Pool 1 rebuild completed successfully (Aug 1, 2026 01:10 UTC). All drives healthy, RAID 1 status Normal, array UUID 5bb187d0:b14f67a3:9f4d8ab9:16d079f8. Emergency backup copy on Kuromoon (1.2TB at /mnt/hdd-backup-2/kinmoon-emergency-backup/) still present as safety net, pending deletion once Kinmoon is trusted long-term. backup-daily vzdump job re-enabled and configured to run 2026-08-02 02:00. Ready to proceed with offsite expansion. |

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

### Phase 16.6: Chaldea Rename Propagation (Next Major Session)
**Effort:** 8 hours  
**Priority:** High  
**Goal:** Full ecosystem rename from Gilgamesh → Jeanne Alter across bot identity, system prompt, Telegram username, and all 8 documentation files  
**Deliverables:** 
- Update n8n workflow system prompts across all Chaldea agents
- Rename Telegram bot from @JhinGilgamesh_bot to new handle
- Update all 8 documentation files with new agent names
- Consistent agent naming across all systems
**Summary:** Rename bot identity to Jeanne Alter across n8n workflows, Telegram bot handle, and documentation. Requires updates to: Da Vinci, MERLIN, Midas, Cu Chulainn (currently still named "Guardian" in some workflow references), and documentation files.

### Phase 24.15: Jeanne Alter Email Management Pipeline (Second Major Session)
**Effort:** 6-8 hours  
**Priority:** High  
**Status:** Design complete (July 9, 2026)  
**Precondition:** Phase 24.11 (Credential Store Migration) — n8n credentials need to be in the built-in store first  
**Deliverables:** 
- Email classifier sub-workflow for 4 accounts
- 3× daily schedule triggers
- Telegram notifications
- Permanent-fact routing to Da Vinci Personal Knowledge gateway for bills/payments/subscriptions
- Support for Gmail OAuth2 (3 accounts) and iCloud IMAP (1 account)
**Summary:** Implement email read + notification pipeline for personal accounts with automatic categorization and integration into knowledge system. qwen3:14b primary model, Claude Haiku fallback.

### Phase 24.14: Jeanne Alter Web Search Quality Improvement (Third Major Session)
**Effort:** 4 hours  
**Priority:** High  
**Goal:** Iterative multi-query search with synthesis step  
**Deliverables:** Improved search result relevance and comprehensiveness closer to Gemini-style search
**Summary:** Replace single Firecrawl + Haiku call pattern with iterative multi-query approach and result synthesis.

### Phase Midas v2: Financial Intelligence (Fourth Major Session)
**Effort:** 10-12 hours  
**Priority:** High  
**Goal:** Firefly III integration with receipt and PDF statement capture  
**Deliverables:** 
- Expense tracking with receipt OCR
- PDF import workflow
- Spending insights via Jeanne Alter
- Suggested spending optimization when approaching $10 monthly limit
**Summary:** Enhance Midas agent with financial intelligence features including receipt capture, expense tracking, and proactive spending recommendations.

### Phase 31: Off-site Backup (Backblaze B2) (Ready to Start)
**Effort:** 6 hours  
**Priority:** High  
**Status:** NOW UNBLOCKED (Aug 1, 2026) — Kinmoon Storage Pool 1 rebuild complete and verified healthy. Emergency backup copy safe on Kuromoon. backup-daily job re-enabled.  
**Deliverables:** Offsite backup expansion, completing 3-2-1 backup strategy
**Summary:** With Kinmoon stable and verified, proceed with Backblaze B2 integration for offsite redundancy.

### Phase 16.8: Deck Sync Manual Backfill (Quick Session)
**Effort:** 2 hours  
**Priority:** Medium  
**Goal:** Manually tag all ~30 existing Homelab Deck cards with sync-id footers  
**Precondition for:** Phase 16.7 (Da Vinci → Nextcloud Deck Sync Pipeline)  
**Deliverables:** All cards ready for automated sync integration
**Summary:** One-time manual tagging of existing Nextcloud Deck cards to prepare for Phase 16.7 automation.

---

## 🔗 Phase Dependencies

**Critical Path Analysis:**
1. **Phase 34 (Kinmoon Recovery) → COMPLETE (Aug 1, 2026)** — Storage Pool 1 fully rebuilt, data restored, libata fix applied, backup-daily re-enabled
2. **Phase 34 COMPLETION → Phase 31 (Offsite Backup) UNBLOCKED** — Proceed with Backblaze B2 integration
3. **Chaldea Architecture Track:** Phase 24.11 (Credential Store) → Phase 24.12 (Jeanne Alter Refactor) → Phase 24.13 (Universal Time/Date) → Multi-Agent Discussion Protocol
4. **Chaldea Agent Feature Track:** Phase 24.11 → Phase 24.15 (Email Management) → remaining agent features
5. **Documentation & Integration Track:** Phase 16.6 (Rename) → 16.7 (Deck Sync) → 16.8 (Deck Backfill)
6. **Agent Feature Development Track:** Phase 24.12 → Scathach (build priority 1st, LangGraph evaluation) → Cu Chulainn (build priority 2nd, rename propagation pending) → Goal Nudges → Plan My Day

---

## 📝 Session Summary (Aug 1, 2026)

**Kinmoon NAS Recovery — Multi-day Hardware & Software Incident Resolution**

### Major Accomplishments
- **Storage Pool 1 Rebuild:** Completely rebuilt RAID 1 array from scratch (old array UUID `c4e2dde2:...` → new UUID `5bb187d0:b14f67a3:9f4d8ab9:16d079f8`). Full data restore from Kuromoon emergency backup copy (1.2TB, verified matching on both sides).
- **libata.force=3.0Gbps Fix Applied & Verified:** Kernel boot parameter appended to both `/boot/EFI/debian/grub.cfg` (live) and `/boot/EFI/debian/grub.am` (template). Zero `WRITE FPDMA QUEUED` errors observed post-fix across fresh boot and full data restore.
- **Emergency Backup Resync:** Relaunched rsync backup copy in `tmux` (original died from signal interruption). Handled multiple bad-sector read stalls on Hard Drive 2 (ST3000DM008) via file-by-file exclusion. Final backup count: 1.2TB on Kuromoon at `/mnt/hdd-backup-2/kinmoon-emergency-backup/`.
- **Vaultwarden External Access Fixed:** Diagnosed and resolved broken `passwords.najhin-gaming.com` Cloudflare Tunnel route (stray broken CNAME → deleted and recreated via Published Application Routes UI). Corrected Service Type from HTTPS to HTTP. Updated Vaultwarden Docker image from stale version to `latest` to resolve extension login flow incompatibility.
- **Proxmox Storage Credential Sync:** Updated `kinmoon-smb` CIFS credentials in Proxmox after Kinmoon account password reset.
- **Backup Job Re-enabled:** `backup-daily` vzdump job re-enabled with confirmed targets (CT 306 Enshrouded, CT 307 Palworld already included). Scheduled first run 2026-08-02 02:00.

### Issues Discovered & Resolved
- **rsync Backup Copy Died:** Original nohup session died from SIGINT/SIGTERM/SIGHUP. Relaunched in tmux for session-independence.
- **Multiple Bad Sectors on Hard Drive 2:** Drive reported `critical medium error` (failed READ on specific sectors). SMART data healthy. Resolved via file-by-file exclusion in rsync.
- **SSH Login Failure to Kinmoon:** Stale/incorrect password. Resolved via UGOS web UI password reset. Discovered username is case-sensitive