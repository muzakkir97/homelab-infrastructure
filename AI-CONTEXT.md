# Muzakkir's Homelab AI-CONTEXT
*Last Updated: January 2025 | Knowledge Curator: Da Vinci*

---

## Quick Summary
**Current Status:** Phase 16.3 complete — Da Vinci documentation pipeline deployed and operational

**Active Project:** MERLIN reminder agent (next phase)

**Infrastructure:** 3 nodes, 45 containers, 8 n8n workflows, 1 active AI agent (Gilgamesh)

---

## Session Log

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

## Phase History
| Phase | Description | Status | Completed | Notes |
|-------|-------------|--------|-----------|--------|
| 16.3 | Da Vinci Documentation Pipeline | ✅ Complete | April 25, 2026 | 11-node n8n workflow, async docs |
| 16.2 | Cost Tracking Dashboard | ✅ Complete | April 24, 2026 | Grafana + InfluxDB |
| 16.1 | Infrastructure Documentation | ✅ Complete | April 23, 2026 | Full homelab catalog |
| 15.3 | Cloudflare Integration | ✅ Complete | April 22, 2026 | Access + tunnels |
| 15.2 | External Access Setup | ✅ Complete | April 21, 2026 | VPN + security |

---

## AI Agent Ecosystem

### Agent Roster (Fate/GO Theme)
| Agent | Role | Status | Pronouns | Platform | Purpose |
|-------|------|--------|----------|----------|---------|
| **Gilgamesh** | Master Control | 🟢 Active | he/him | @JhinGilgamesh_bot | Primary interface, routing |
| **Da Vinci** | Knowledge Curator | 🟡 Planned | she/her | n8n workflow | Documentation, context |
| **EMIYA** | System Guardian | 🟡 Planned | he/him | TBD | Monitoring, alerts |
| **MERLIN** | Reminder Mage | 🟡 Planned | he/him | TBD | Scheduled tasks, reminders |

### Current Capabilities
- **Gilgamesh Bot**: Command routing, basic queries
- **Da Vinci Pipeline**: Async documentation updates (Stage 1 complete)
- **Planned Features**: RAG knowledge recall, system monitoring, task scheduling

---

## Infrastructure Overview

### Physical Hardware
| Device | Role | CPU | RAM | Storage | OS | Status |
|--------|------|-----|-----|---------|----|----|
| **Minimoon** | Main Node | Ryzen 7 7700 | 64GB | 2TB NVMe + 16TB HDD | Proxmox | 🟢 Active |
| **Newmoon** | Secondary | Intel i5-8400 | 32GB | 1TB SSD | Proxmox | 🟢 Active |
| **VM-400** | Services Hub | Virtual | 8GB | 100GB | Ubuntu 24.04 | 🟢 Active |

### Container Inventory (45 total)
**Core Services (12)**
- Traefik, Authelia, Nextcloud, Vaultwarden
- Uptime Kuma, Grafana, InfluxDB, Prometheus
- Postgres, Redis, Nginx, Portainer

**Media & Content (8)**
- Jellyfin, Sonarr, Radarr, Prowlarr
- Transmission, Tautulli, Overseerr, Kavita

**Development (10)**
- GitLab, n8n, Ollama, Open WebUI
- Code Server, Jupyter, Node-RED, API Gateway
- Jenkins, Harbor Registry

**Monitoring (8)**
- Node Exporter (3x), cAdvisor (3x)
- Loki, Promtail

**Network & Utilities (7)**
- Pi-hole, Cloudflare DDNS, Speedtest
- File Browser, Duplicati, Watchtower, Dozzle

---

## n8n Workflows (8 active)

1. **Cost Tracking Pipeline** - Proxmox API → InfluxDB
2. **System Health Monitor** - Multi-node health checks
3. **Media Automation** - Arr stack orchestration
4. **Backup Orchestrator** - Automated backup chains
5. **Alert Manager** - Telegram notifications
6. **DDNS Updater** - Cloudflare DNS automation
7. **Service Monitor** - Container health tracking
8. **Da Vinci Documentation Pipeline** - Async docs (11 nodes)

---

## Network Architecture

### Core Network (192.168.1.0/24)
- **Gateway**: 192.168.1.1 (Router)
- **Minimoon**: 192.168.1.100
- **Newmoon**: 192.168.1.200
- **VM-400**: 192.168.1.50

### Container Networks
- **Traefik Network**: Reverse proxy mesh
- **Database Network**: Isolated backend services
- **Monitoring Network**: Metrics collection overlay
- **Media Network**: Arr stack communication

### External Access
- **Cloudflare Tunnels**: Zero Trust access
- **Authelia SSO**: Unified authentication
- **VPN Access**: Secure remote management

---

## Key Integrations

### Authentication Stack
- **Authelia** → LDAP/OIDC provider
- **Vaultwarden** → Password management
- **Cloudflare Access** → Zero Trust gateway

### Monitoring Stack
- **Prometheus** → Metrics collection
- **Grafana** → Visualization
- **InfluxDB** → Time series storage
- **Uptime Kuma** → Service availability

### Automation Stack
- **n8n** → Workflow orchestration
- **Telegram Bot** → Command interface
- **Webhook System** → Event-driven actions

---

## Documentation System

### File Structure
```
docs/
├── AI-CONTEXT.md         # This file (master context)
├── AI-CONTEXT-staging.md # Raw session summaries (Nextcloud)
├── service-catalog.md    # Complete service inventory
├── network-diagram.md    # Network topology
├── roadmap.md           # Development timeline
├── changelog.md         # Version history
└── troubleshooting.md   # Known issues & solutions
```

### Documentation Workflow
1. **Session End** → Append to AI-CONTEXT-staging.md
2. **Da Vinci Webhook** → Merge to AI-CONTEXT.md
3. **GitHub Push** → Sync to repository
4. **Telegram Notify** → Update confirmation

### Key URLs
- **Nextcloud WebDAV**: `https://cloud.najhin-gaming.com/remote.php/dav/files/admin/`
- **GitHub Repository**: `https://github.com/najhin/homelab-docs`
- **Documentation Portal**: `https://docs.najhin-gaming.com`

---

## Pending Tasks

### High Priority
- [ ] Store Nextcloud app password in Vault kv/nextcloud
- [ ] Store GitHub token in Vault kv/github
- [ ] Store VM 400 root password in Vaultwarden
- [ ] Fix cost_usd column bug in Save Cost node
- [ ] Next phase: MERLIN reminder agent

### Medium Priority
- [ ] Set Bitwarden on phone
- [ ] Add remaining subscriptions to Obsidian
- [ ] Fix Obsidian dashboard backtick query
- [ ] Update Cloudflare Access icons (7 apps incl. Ollama)

### Future Planning
- [ ] Da Vinci Stage 2 (RAG) alongside Phase 7E
- [ ] Phase 27 (Vault + n8n) for secret management
- [ ] EMIYA monitoring agent deployment
- [ ] Advanced alerting rules

---

## Quick Reference

### Essential Commands
```bash
# Container management
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
docker compose -f /opt/homelab/docker-compose.yml up -d

# System monitoring
htop
df -h
systemctl status

# Network diagnostics
ping 8.8.8.8
curl -I https://najhin-gaming.com
```

### Critical Endpoints
- **Traefik Dashboard**: `https://traefik.najhin-gaming.com`
- **Grafana**: `https://grafana.najhin-gaming.com`
- **n8n**: `https://n8n.najhin-gaming.com`
- **Nextcloud**: `https://cloud.najhin-gaming.com`
- **Portainer**: `https://portainer.najhin-gaming.com`

### Emergency Contacts
- **Telegram Bot**: @JhinGilgamesh_bot
- **Admin Email**: admin@najhin-gaming.com
- **Status Page**: `https://status.najhin-gaming.com`

---

*This document is automatically maintained by Da Vinci, the Knowledge Curator.*