# AI-CONTEXT.md — Homelab Knowledge Base
*Last Updated: April 24, 2026*

## Quick Summary
**Current Status:** Phase 16.3 Complete (Da Vinci Documentation Pipeline)  
**Active Phase:** Planning Phase 16.4 (MERLIN Reminder Agent)  
**Infrastructure:** 9 LXCs, 2 VMs on Proxmox + External services  
**AI Agents:** 1 Active (Gilgamesh), 2 Planned (Da Vinci Stage 2, EMIYA)

---

## Session Log

### Session Summary — April 24, 2026
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

## Phase History

| Phase | Name | Status | Completion Date | Notes |
|-------|------|--------|-----------------|-------|
| 16.3 | Da Vinci Documentation Pipeline | ✅ Complete | April 24, 2026 | Async documentation workflow deployed |
| 16.2 | Gilgamesh Telegram Bot | ✅ Complete | April 24, 2026 | Multi-agent command router |
| 16.1 | n8n Automation Hub | ✅ Complete | April 24, 2026 | 8 workflows active |
| 15 | Nextcloud Personal Cloud | ✅ Complete | April 24, 2026 | File sync + WebDAV |
| 14 | Vaultwarden Password Manager | ✅ Complete | April 24, 2026 | Self-hosted Bitwarden |
| 13 | Grafana Monitoring | ✅ Complete | April 24, 2026 | Dashboards + alerts |
| 12 | Prometheus Metrics | ✅ Complete | April 24, 2026 | System monitoring |
| 11 | Reverse Proxy (Nginx) | ✅ Complete | April 24, 2026 | SSL + domain routing |
| 10 | Docker Host | ✅ Complete | April 24, 2026 | Container platform |
| 9 | PostgreSQL Database | ✅ Complete | April 24, 2026 | Multi-service database |
| 8 | Proxmox Backup Server | ✅ Complete | April 24, 2026 | VM/LXC backups |
| 7 | Network Storage (NFS) | ✅ Complete | April 24, 2026 | Shared file system |

---

## Current Infrastructure

### Hardware Inventory
| Device | Type | CPU | RAM | Storage | Status | Purpose |
|--------|------|-----|-----|---------|--------|---------|
| Dell OptiPlex 7050 | Mini PC | i7-7700T | 32GB | 1TB NVMe | 🟢 Active | Proxmox Host |

### Container Inventory
| Container | ID | Type | RAM | CPU | Status | Purpose |
|-----------|----|----- |-----|-----|--------|---------|
| nginx-proxy | 100 | LXC | 1GB | 1 | 🟢 Running | Reverse proxy |
| docker-host | 101 | LXC | 8GB | 4 | 🟢 Running | Container platform |
| postgresql | 102 | LXC | 2GB | 2 | 🟢 Running | Database server |
| nfs-storage | 103 | LXC | 1GB | 1 | 🟢 Running | Network storage |
| backup-server | 104 | LXC | 2GB | 2 | 🟢 Running | Proxmox backup |
| prometheus | 105 | LXC | 2GB | 2 | 🟢 Running | Metrics collection |
| grafana | 106 | LXC | 1GB | 1 | 🟢 Running | Monitoring dashboards |
| vaultwarden | 107 | LXC | 1GB | 1 | 🟢 Running | Password manager |
| nextcloud | 108 | LXC | 4GB | 2 | 🟢 Running | Personal cloud |
| n8n-automation | 109 | LXC | 2GB | 2 | 🟢 Running | Workflow automation |

### AI Agent Ecosystem
| Agent | Status | Platform | Purpose | Pronouns |
|-------|--------|----------|---------|----------|
| Gilgamesh | 🟢 Active | Telegram Bot | Command router & task delegation | he/him |
| Da Vinci (Stage 1) | 🟢 Active | n8n Workflow | Documentation pipeline | she/her |
| Da Vinci (Stage 2) | 📋 Planned | TBD | Knowledge curation & RAG | she/her |
| EMIYA | 📋 Planned | TBD | Infrastructure monitoring | he/him |
| MERLIN | 📋 Next | TBD | Reminder & scheduling agent | they/them |

### n8n Workflows (8 Active)
| Workflow | Purpose | Trigger | Status |
|----------|---------|---------|--------|
| Homelab Health Check | Monitor all services | Cron (15min) | 🟢 Active |
| Backup Automation | Orchestrate PBS backups | Webhook | 🟢 Active |
| Certificate Renewal | Auto-renew SSL certs | Cron (weekly) | 🟢 Active |
| Security Alerts | Failed login notifications | Webhook | 🟢 Active |
| Resource Monitoring | CPU/RAM/Disk alerts | Prometheus | 🟢 Active |
| Documentation Sync | GitHub to Nextcloud sync | Webhook | 🟢 Active |
| Container Updates | Auto-update containers | Cron (daily) | 🟢 Active |
| Da Vinci Documentation Pipeline | AI-powered doc management | Webhook | 🟢 Active |

---

## Key Integrations

### External Services
- **GitHub**: Code repositories + documentation
- **Telegram**: Bot interface (@JhinGilgamesh_bot)
- **Claude API**: AI processing for documentation
- **Domain**: najhin-gaming.com (Cloudflare DNS)

### Internal Network
- **Subnet**: 192.168.1.0/24
- **Proxmox**: 192.168.1.100
- **Gateway**: 192.168.1.1
- **Services**: Auto-assigned DHCP

### Authentication & Security
- **SSL**: Let's Encrypt via Nginx
- **Passwords**: Stored in Vaultwarden
- **Backups**: Automated via PBS
- **Monitoring**: Prometheus + Grafana

---

## Pending Tasks

### Immediate (This Week)
- [ ] Store new Nextcloud app password in Vaultwarden
- [ ] Store GitHub token used in Da Vinci in Vaultwarden  
- [ ] Run /sync-docs to regenerate all documentation
- [ ] Plan Phase 16.4: MERLIN reminder agent

### Phase 7E Integration
- [ ] Plan Da Vinci Stage 2 (RAG/knowledge recall)
- [ ] Design EMIYA infrastructure monitoring agent
- [ ] Integrate AI agents with existing monitoring

### Future Phases
- [ ] Media server (Plex/Jellyfin)
- [ ] Home Assistant integration
- [ ] VPN server setup
- [ ] Container registry

---

## Important Notes

### Credentials & Access
- **Nextcloud WebDAV**: Username is `admin` (not muzakkir)
- **All passwords**: Stored in Vaultwarden
- **GitHub integration**: Personal access token
- **Telegram**: Bot token in environment variables

### Documentation Standards  
- **AI-CONTEXT.md**: Master knowledge base (this file)
- **AI-CONTEXT-staging.md**: Raw session summaries (append-only)
- **Roadmap**: Phase tracking and planning
- **Changelog**: Historical changes log
- **Da Vinci Pipeline**: Automatic merge from staging to master

### Troubleshooting References
- **Proxmox**: Check cluster status, backup jobs
- **n8n**: Workflow execution logs, webhook endpoints  
- **Networking**: DNS resolution, SSL certificate status
- **Containers**: Resource usage, log aggregation

---

*This document is maintained by Da Vinci (AI Documentation Curator)*