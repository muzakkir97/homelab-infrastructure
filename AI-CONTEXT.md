# AI-CONTEXT.md — Muzakkir's Homelab Project

## Quick Summary
**Current Status:** Building AI ecosystem (Phase 16 active)  
**Latest Phase:** 16.3 — Da Vinci Documentation Pipeline (COMPLETE)  
**Active Services:** 34 containers across 6 LXCs on Proxmox cluster  
**Current Focus:** AI agent deployment and automation workflows

---

## Session Log

### Session Summary — 2026-04-25
25 april 2026 10.15pm - Short Second Test

---

### Session Summary — 2026-04-25
25 april 2026 10.15pm - Short test

---

### Session Summary — 2026-04-24
Date: April 24-25, 2026
Phase: 16.3 — Da Vinci Documentation Pipeline (COMPLETE)

Topics Discussed
- Reviewed pending tasks: VM 400 passwords, Bitwarden phone
  setup, cost_usd bug, Obsidian subscriptions, Cloudflare icons
- Test SQLite workflow confirmed deleted
- Generated full homelab roadmap as Obsidian Kanban board
  (homelab-roadmap-kanban.md, 7 columns, all phases)
- Reviewed planned AI agent roster (Fate/GO ecosystem)
- Clarified Da Vinci and EMIYA are NOT deployed — Gilgamesh
  is the only active agent currently
- Confirmed Da Vinci is canonically female (she/her)
- All agents route through Gilgamesh (@JhinGilgamesh_bot)
- Researched documentation agent patterns online
- Designed Da Vinci Stage 1 (async /update fix) and
  Stage 2 (RAG/knowledge recall, planned with Phase 7E)
- Built Phase 16.3 — Da Vinci Documentation Pipeline
- Created AI-CONTEXT-staging.md in Nextcloud (rolling append)
- Fixed Nextcloud WebDAV username (admin not muzakkir)
- Built 11-node Da Vinci n8n workflow
- Fixed multiple issues during build
- Full end-to-end test passed
- AI-CONTEXT.md accidentally overwritten by test — restored
  from backup, resynced to GitHub
- Stored Nextcloud app password and GitHub token task deferred
  to Phase 27 (Vault + n8n integration)

Decisions Made
- Da Vinci is an n8n workflow, not a separate LXC
- All agents route through Gilgamesh bot
- Staging file is rolling append — raw summaries preserved
- Da Vinci Stage 2 (RAG) planned alongside Phase 7E
- Da Vinci and EMIYA status corrected to Planned/Not deployed
- Nextcloud WebDAV username is admin throughout
- Download link in Da Vinci notification uses Nextcloud web UI
  URL, not raw WebDAV (Cloudflare Access blocks raw WebDAV)
- Nextcloud app password and GitHub token to be stored in
  Vault kv/nextcloud and kv/github (Phase 27)

Changes to AI-CONTEXT.md
- Mark Phase 16.3 as Complete (April 25, 2026)
- Correct agent table: Da Vinci and EMIYA from Active to
  Planned (not yet deployed)
- Add Da Vinci pronouns: she/her
- Add AI-CONTEXT-staging.md to infrastructure notes
- Update Nextcloud WebDAV username to admin throughout
- WebDAV base URL: https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- Add Da Vinci Stage 2 (RAG) planned alongside Phase 7E
- Add new n8n workflow: Da Vinci Documentation Pipeline
  (8th workflow, 11 nodes)
- Update n8n workflow count from 7 to 8
- Add pending task: Store Nextcloud app password + GitHub
  token in Vault kv/nextcloud and kv/github
- Add pending task: Phase 27 (Vault + n8n) enables n8n to
  fetch secrets directly — eliminates manual copy-paste
- Update Cloudflare Access icons pending list to 7 apps
  (added Ollama)

Changes to Other Docs
- roadmap.md: Move Phase 16.3 to Completed (April 25, 2026)
- changelog.md: Add Phase 16.3 completion entry
- service-catalog.md: Add Da Vinci Documentation Pipeline

Errors & Resolutions
- Nextcloud 401: Username was muzakkir, should be admin
- n8n HTTP Request JSON invalid: Moved Claude API call to
  Code node using this.helpers.httpRequest
- Write AI-CONTEXT empty body: Use explicit node reference
  $('Extract Response').first().json.updatedDoc
- Push to GitHub auth failed: Replaced HTTP Request node
  with Code node
- URL typo: cloud.najhin.gaming.com → cloud.najhin-gaming.com
- Cloudflare Access blocks raw WebDAV download link: Use
  Nextcloud web UI URL instead
- AI-CONTEXT.md overwritten by test run: Restored from local
  backup on Minimoon, resynced to GitHub via curl from CT 211

Action Items
- [ ] Store Nextcloud app password in Vault kv/nextcloud
- [ ] Store GitHub token in Vault kv/github
- [ ] Store VM 400 root password in Vaultwarden
- [ ] Set Bitwarden on phone
- [ ] Add remaining subscriptions to Obsidian
- [ ] Fix Obsidian dashboard backtick query
- [ ] Update Cloudflare Access icons (7 apps incl. Ollama)
- [ ] Fix cost_usd column bug in Save Cost node
- [ ] Next phase: MERLIN reminder agent

---

### Session Summary — 2026-04-24
Date: April 24, 2026
Phase: 16.3 — Da Vinci Documentation Pipeline (COMPLETE)

Topics Discussed
- Reviewed pending tasks from earlier today
- Generated Obsidian kanban board for full roadmap
- Clarified Da Vinci and EMIYA deployment status (neither deployed yet)
- Researched documentation agent patterns
- Designed and built Phase 16.3 — Da Vinci async documentation pipeline
- Confirmed Da Vinci uses she/her pronouns (canonically female in Fate/GO)
- Created AI-CONTEXT-staging.md in Nextcloud (append-only raw summaries)
- Built Da Vinci n8n workflow: 11 nodes — webhook, dual Nextcloud reads,
  merge, Claude API formatting, Nextcloud write, GitHub push, Telegram notify
- Fixed Nextcloud WebDAV username (admin not muzakkir)
- Fixed multiple n8n issues: JSON serialization, HTTP node validation,
  empty body expression, GitHub auth

Decisions Made
- Da Vinci is an n8n workflow (not a separate LXC)
- All agents route through Gilgamesh bot (@JhinGilgamesh_bot)
- Staging file is rolling append — raw summaries preserved indefinitely
- Da Vinci Stage 2 (RAG/knowledge recall) planned alongside Phase 7E
- Da Vinci and EMIYA corrected to NOT active — Gilgamesh is the only
  active agent currently
- Nextcloud WebDAV username is admin not muzakkir

Changes to AI-CONTEXT.md
- Mark Phase 16.3 as Complete (April 24, 2026)
- Correct agent table: Da Vinci and EMIYA status from Active to Planned
- Add AI-CONTEXT-staging.md to infrastructure notes
- Update Nextcloud WebDAV username to admin throughout
- Update Da Vinci description: she/her pronouns
- Add Da Vinci Stage 2 (RAG) as planned alongside Phase 7E
- Add new n8n workflow: Da Vinci Documentation Pipeline
- Update n8n workflow count from 7 to 8

Changes to Other Docs
- roadmap.md: Move Phase 16.3 to Completed (April 24, 2026)
- changelog.md: Add Phase 16.3 completion entry

Errors & Resolutions
- Nextcloud 401: Username was muzakkir, should be admin
- n8n HTTP Request JSON body invalid: Moved Claude API call to Code node
  using this.helpers.httpRequest to bypass n8n validation
- Write AI-CONTEXT empty body: Use explicit node reference
  $('Extract Response').first().json.updatedDoc instead of $json
- Push to GitHub auth failed: Replaced HTTP Request node with Code node
- URL typo: cloud.najhin.gaming.com should be cloud.najhin-gaming.com

Action Items
- Save and store new Nextcloud app password in Vaultwarden
- Store GitHub token used in Da Vinci in Vaultwarden
- Run /sync-docs to regenerate all docs
- Plan Da Vinci Stage 2 (RAG) alongside Phase 7E
- Next phase: MERLIN reminder agent

---

### Session Summary — 2026-04-24
Phase 16.3 test complete. Da Vinci documentation pipeline deployed and working. All nodes green — staging file append, Claude API formatting, Nextcloud write, GitHub push, and Telegram notification all successful.

---

### Session Summary — 2026-04-24
Test summary from Phase 16.3

---

### Session Summary — 2026-04-24
Test summary from Phase 16.3

---

## Phase History

| Phase | Description | Status | Date Completed |
|-------|------------|--------|----------------|
| 1.0 | Initial Proxmox cluster setup | Complete | March 15, 2026 |
| 2.1 | LXC container deployment | Complete | March 18, 2026 |
| 2.2 | Docker service deployment | Complete | March 20, 2026 |
| 3.1 | Cloudflare integration | Complete | March 22, 2026 |
| 4.1 | Monitoring stack (Grafana + Prometheus) | Complete | March 25, 2026 |
| 5.1 | Backup automation | Complete | March 28, 2026 |
| 6.1 | n8n workflow automation | Complete | April 2, 2026 |
| 7.1 | Vault secrets management | Complete | April 8, 2026 |
| 8.1 | Obsidian knowledge base | Complete | April 10, 2026 |
| 9.1 | Nextcloud file sync | Complete | April 12, 2026 |
| 10.1 | Media stack optimization | Complete | April 15, 2026 |
| 11.1 | Network security hardening | Complete | April 17, 2026 |
| 12.1 | Database optimization | Complete | April 19, 2026 |
| 13.1 | Ollama LLM deployment | Complete | April 20, 2026 |
| 14.1 | Open WebUI integration | Complete | April 21, 2026 |
| 15.1 | Telegram bot framework | Complete | April 22, 2026 |
| 16.1 | Gilgamesh agent deployment | Complete | April 23, 2026 |
| 16.2 | Documentation system | Complete | April 23, 2026 |
| 16.3 | Da Vinci Documentation Pipeline | Complete | April 25, 2026 |

---

## Pending Tasks

### High Priority
- [ ] Store Nextcloud app password in Vault kv/nextcloud
- [ ] Store GitHub token in Vault kv/github
- [ ] Store VM 400 root password in Vaultwarden
- [ ] Set Bitwarden on phone
- [ ] Fix cost_usd column bug in Save Cost node
- [ ] Next phase: MERLIN reminder agent

### Medium Priority
- [ ] Add remaining subscriptions to Obsidian
- [ ] Fix Obsidian dashboard backtick query
- [ ] Update Cloudflare Access icons (7 apps incl. Ollama)

### Low Priority
- [ ] Plan Da Vinci Stage 2 (RAG) alongside Phase 7E
- [ ] Phase 27 (Vault + n8n) enables n8n to fetch secrets directly

---

## Infrastructure Overview

### Physical Hardware
- **Primary Node (Minimoon):** Dell OptiPlex 3090, i5-10500T, 32GB RAM, 1TB NVMe
- **Secondary Node (Titan):** Custom build, AMD Ryzen 5600G, 32GB RAM, 2TB NVMe  
- **Storage:** TrueNAS VM with 4TB HDD pool for media/backups
- **Network:** Ubiquiti Dream Machine + 8-port switch, Cloudflare tunnels

### Container Inventory (34 services)

**CT 200 - Management Stack (Minimoon)**
- Grafana, Prometheus, Uptime Kuma, Portainer
- Node Exporter, cAdvisor

**CT 201 - Core Services (Minimoon)**  
- Traefik, n8n, Vault, Nextcloud
- PostgreSQL, Redis

**CT 202 - Media Stack (Titan)**
- Plex, Sonarr, Radarr, Prowlarr  
- Transmission, Overseerr

**CT 203 - Productivity (Minimoon)**
- Obsidian Sync, Vaultwarden
- Invoice Ninja, Stirling PDF

**CT 210 - AI/ML Stack (Titan)**
- Ollama, Open WebUI
- Jupyter, TensorFlow (planned)

**CT 211 - Development (Minimoon)**
- GitLab CE, GitLab Runner
- Code Server, Docker Registry

### AI Agent Ecosystem (Fate/Grand Order Theme)

| Agent | Role | Status | Platform | Pronouns |
|-------|------|--------|----------|----------|
| Gilgamesh | Master coordinator, /update handler | Active | Telegram Bot | he/him |
| Da Vinci | Knowledge curator, documentation | Planned | n8n workflow | she/her |
| EMIYA | Infrastructure guardian, monitoring | Planned | n8n + Grafana | he/him |
| MERLIN | Reminder agent, task scheduling | Planned | n8n workflow | he/him |
| Mash | User support, help system | Planned | Telegram Bot | she/her |

**Agent Communication:** All agents route through @JhinGilgamesh_bot on Telegram

### n8n Workflows (8 active)

1. **Homelab Monitor** - System health checks and alerting
2. **Media Automation** - Plex library management and notifications  
3. **Backup Orchestrator** - Automated backup scheduling and verification
4. **Cost Tracker** - Monthly infrastructure cost monitoring
5. **Document Sync** - GitHub repository synchronization
6. **Security Scanner** - Vulnerability monitoring and reporting
7. **Service Health** - Container status monitoring and restart automation
8. **Da Vinci Documentation Pipeline** - Session summary processing (11 nodes)

### Key Infrastructure Notes
- **Secrets Management:** Vault (kv-v2) + Vaultwarden for personal passwords
- **Monitoring:** Grafana + Prometheus + custom n8n workflows
- **Backups:** Daily container snapshots, weekly full system backups to TrueNAS
- **DNS/SSL:** Cloudflare tunnels + automatic SSL certificate management
- **Documentation:** AI-CONTEXT.md (formatted) + AI-CONTEXT-staging.md (raw summaries)
- **Nextcloud WebDAV:** Base URL https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **GitHub Integration:** Automated doc sync via curl from CT 211

---

## Current Sprint Focus

**Phase 16 - AI Ecosystem Deployment**
- ✅ 16.1: Gilgamesh master agent (Telegram bot)
- ✅ 16.2: Documentation system (/update command) 
- ✅ 16.3: Da Vinci async documentation pipeline
- 🎯 **Next:** 16.4: MERLIN reminder agent deployment

**Technical Debt**
- Cloudflare Access icon updates needed (7 apps including Ollama)
- VM 400 password management (store in Vaultwarden)
- n8n cost tracker bug fix (cost_usd column)
- Obsidian subscription management completion

---

*Last updated: April 25, 2026 via Da Vinci Documentation Pipeline*