# 📝 Changelog

All notable changes to the homelab infrastructure project.

---

### [April 24, 2026] - Phase 38/39 Model Upgrade + Phase 41: Hybrid Routing (COMPLETE)

#### Added
- **Phase 41 Hybrid Routing** in Gilgamesh Telegram Agent with intelligent query classification
- **Ollama model upgrade** from qwen2.5:14b to qwen3:14b (9.3GB, optimized for 12GB VRAM)
- **Complex keyword detection** for routing: API, authentication, security, deployment, configuration, troubleshooting → Claude Sonnet
- **Word count routing** - queries >50 words automatically routed to Claude Sonnet for better handling
- **Fallback system** - Ollama primary → Haiku fallback if Ollama down → Sonnet for complex queries
- **Open WebUI updates** to latest version with improved JSON streaming support

#### Technical
- **Route Check If node** with Call Ollama (Code) and Call Claude (HTTP Request) branches
- **Merge node** combining responses from both routing paths before Extract Response
- **SSH access fix** - VM 400 uses muzakkir user with sudo access, not direct root login
- **NPM configuration** - proxy_buffering off in advanced config for streaming JSON responses

#### Fixed
- **SSH authentication to VM 400** - corrected user credentials (muzakkir vs root)
- **Open WebUI JSON parse error** - NPM proxy_buffering setting for streaming responses
- **httpRequestWithAuthentication limitation** - replaced with separate HTTP Request node with credentials
- **Extract Response node reference** - explicitly reference $('Merge').first().json for proper data flow

#### Architecture
- **Primary routing:** Simple queries → Ollama qwen3:14b (local, fast, cost-effective)
- **Complex routing:** Complex keywords OR >50 words → Claude Sonnet (cloud, advanced reasoning)
- **Fallback routing:** Ollama unavailable → Claude Haiku (cloud, basic queries)

#### Gilgamesh Evolution
- **Phase 1 Foundation** now complete with local LLM + hybrid routing
- **Cost optimization** achieved - most queries now processed locally on Ollama
- **Quality assurance** maintained - complex queries still use Claude's advanced capabilities

#### Planned Next
- **Phase 7E:** Extended Memory (20+ message conversations via RAG integration)
- **Phase 16.3:** Da Vinci Documentation Pipeline (append-only /update redesign)
- **MERLIN reminder agent** (highest priority for memory management)

---

### [April 24, 2026] - Phase 38 + 39: Ollama ROCm + Open WebUI (COMPLETE)

#### Added
- **VM 400 ollama-gpu** with Ubuntu 22.04, 6 CPU cores, 8GB RAM, 100GB disk on VLAN 30 (192.168.30.221)
- **RX 6700 XT PCIe passthrough** successful GPU acceleration for AI inference workloads
- **ROCm 6.1.3 runtime** installed and configured for AMD GPU compute support
- **Ollama service** running with 100% GPU utilization confirmed
- **AI models deployed:** qwen2.5:14b (8.7GB primary), llama3.2 (2.0GB backup)
- **Open WebUI** Docker container on port 3000 for web-based model interaction
- **Subdomain access:** ollama.najhin-gaming.com via NPM SSL termination
- **Cloudflare Access protection** (email OTP) for external security

#### Technical
- **GPU passthrough:** IOMMU Group 18 configured in Proxmox VM settings
- **ROCm compatibility:** HSA_OVERRIDE_GFX_VERSION=10.3.0 for RX 6700 XT support
- **External access:** OLLAMA_HOST=0.0.0.0 environment variable for network binding
- **vfio configuration:** Softdep modules loaded for GPU isolation

#### Fixed
- **VM disk space:** Resized root filesystem from 20GB to 100GB using lvextend + resize2fs
- **vfio module loading:** Added softdep configuration to /etc/modprobe.d/vfio.conf with initramfs update
- **ROCm GPU detection:** Environment override for RX 6700 XT compatibility (10.3.0 GFX version)
- **Open WebUI accessibility:** Network binding configuration for external container access

#### Milestone
- **Gilgamesh Evolution Phase 1 Foundation:** Ollama + Open WebUI infrastructure complete
- **Local AI infrastructure:** Ready for extended memory and hybrid routing development
- **Cost reduction path:** Foundation for reducing $30-40/month Claude dependency

#### Planned Next
- Phase 7E: Extended Memory (20+ message conversations via RAG)
- MERLIN reminder agent (highest priority for memory management)

---

### [April 24, 2026] - Phase 22: Obsidian Knowledge Base (COMPLETE)

#### Added
- **Obsidian installation** on Minimoon (Gaming PC) with vault at C:\Users\muzak\Nextcloud\Obsidian\second-brain
- **6 essential plugins:** Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban
- **Structured folder system:** 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 05-templates, 06-archive
- **Subscription tracker dashboard:** Individual notes per subscription using Dataview queries
- **Session summary template:** Standardized template for homelab session documentation

#### Technical
- **Auto-sync via Nextcloud:** Local vault syncs automatically to CT 220 cloud storage
- **Dashboard queries:** Dataview showing subscription costs and category totals
- **Template system:** 05-templates/session-summary.md for consistent documentation

#### Fixed
- **Nextcloud quota exceeded:** CT 220 root disk resized from 20GB to 100GB (99% full → 20% usage)
- **Dataview query errors:** Individual subscription notes required in separate folder structure

#### Infrastructure
- **CT 220 storage expansion:** Root disk 20GB → 100GB via `pct resize 220 rootfs +80G`
- **Thin pool overprovisioning:** Warning identified on local-lvm storage (deferred to infrastructure cleanup)

#### Planned
- **Phase 24.1:** Service Update Manager (Docker + apt + Proxmox updates with Telegram approval)
- **Data migration:** Move Nextcloud data directory to /mnt/data-storage (7.3TB HDD) in future phase

#### Action Items
- Add remaining subscriptions to Obsidian vault
- Fix Total by Category query (backtick parsing issue)
- Update Nextcloud to version 33.0.2
- Begin Phase 38 (Ollama + ROCm) next session

---

### [April 24, 2026] - Phase 15: Gilgamesh Additional Slash Commands (COMPLETE)

#### Completed
- **Phase 15:** Gilgamesh Additional Slash Commands deployed and tested

#### Added
- **6 new slash commands:** /help, /clear, /memory, /cost, /alerts, /backup
- **Command documentation:** Complete reference guide for all Gilgamesh commands
- **Memory management:** /clear command to reset conversation history
- **Cost tracking display:** /cost command shows token usage and estimated costs
- **Alert monitoring:** /alerts command displays active Alertmanager alerts
- **Backup status:** /backup command shows last backup times for all containers

#### Technical
- **Always Output Data enabled:** Get Alerts and Clear Memory DB nodes handle empty responses
- **Cost calculation:** Uses estimated token rates (Sonnet $3/$15 per 1M) due to cost_usd bug
- **Error handling:** Added proper array checks and chat ID references
- **Workflow optimization:** Removed redundant /temps and /storage commands (available in menu)

#### Fixed
- Clear Memory DB "at least one condition required": Added role not empty condition
- Send Clear Confirmation wrong chat_id: Use $('Get Chat ID').first().json.chat_id pattern
- Format Cost SyntaxError: Removed toLocaleString() and emojis from Code node
- Get Alerts/Format Alerts undefined handling: Added Array.isArray() checks

#### Known Issues
- **cost_usd column bug:** gilgamesh_costs.cost_usd always returns 0 (pre-existing from earlier phases)
- **Cost estimation:** Using token count approximation until Save Cost node investigation

#### Scope Changes
- **/logs command deferred:** Not practical without proper filtering, saved for future menu action
- **Command consolidation:** /temps and /storage removed (redundant with existing menu system)

---

### [April 24, 2026] - Phase 7D-Menu + Phase 14: Complete Gilgamesh Menu System

#### Completed
- **Phase 7D-Menu:** Complete inline keyboard menu system for Gilgamesh bot
- **Phase 14:** Secrets Management & Integration phase finalized

#### Added
- **Homelab → Metrics:** Real-time Proxmox system metrics (CPU, RAM, storage usage)
- **Homelab → Temps:** Live temperature monitoring via SSH to Kuromoon (lm-sensors)
- **Homelab → Storage:** Complete storage overview with usage percentages
- **Gaming submenu:** Direct Docker container status from CT 302
- **Gilgamesh info:** Bot capabilities and feature overview
- **Tools submenu:** Quick access links to all services
- **Help menu:** Complete command reference and usage guide

#### Technical
- **SSH access configured:** CT 211 → Kuromoon (pfSense rule TCP 22 added)
- **Container SSH enabled:** CT 302 root access with PermitRootLogin yes
- **Progress bars:** ASCII characters (= and -) for mobile compatibility
- **Storage fix:** kinmoon-nfs identified as correct Proxmox storage ID
- **Credentials added:** SSH Password account (Kuromoon), SSH CT302 Wings

#### Fixed
- Format Metrics callback_query undefined: Use inputData from Callback Router node
- Send Metrics invalid JSON: Switch to "Using Fields Below" body mode
- SSH hanging to Kuromoon: Added pfSense firewall rule VLAN30→VLAN10 port 22
- SSH auth failing CT 302: Changed PermitRootLogin from prohibit-password to yes
- Game server template parsing: Switch Command field to Fixed mode
- Storage display N/A: Correct storage ID is kinmamoon-nfs not kinmoon-smb

#### Security
- SSH key authentication between CT 211 and Kuromoon
- Password authentication enabled for CT 302 container management
- pfSense firewall rule for specific n8n SSH access

---

### [April 23, 2026] - Gilgamesh Evolution + Claude Design Discovery

#### Planning
- **PRIMARY GOAL:** Replace claude.ai with Gilgamesh by August 2026 (16-week roadmap)
- **Cost target:** Reduce from $30-40/month to $5-10/month using local LLM + hybrid routing
- **Phase 38 promoted:** Ollama + ROCm moved to Near Term #2 (CRITICAL — stop API bleeding)
- **Phase 41 promoted:** Hybrid Routing moved to Near Term #6

#### Added
- **Gilgamesh Evolution Roadmap:** 4-phase plan over 16 weeks (Foundation → Intelligence → Capabilities → Refinement)
- **Seven new Gilgamesh phases:** 7E Extended Memory, 7F File Generation, 7G Vision API, 7H Document Upload, 7I Quality Assurance, 7J Migration, 7K AI News Scraper
- **Fate agent ecosystem:** Recovered from April 21-22 discussions, active agents (Gilgamesh, Da Vinci, EMIYA) and planned (MERLIN #1 priority, Mash gaming bot)
- **Cost projection:** Target savings of $300-360/year with local LLM infrastructure
- **Claude Design integration:** Homelab infrastructure diagram created, strategy for high-value visual content only

#### Documentation
- **Agent separation strategy:** Gaming agents (Mash Discord) separate from admin agents (Gilgamesh Telegram)
- **Success criteria:** 20+ message conversations, RAG recall, Vision API, artifacts generation, under $10/month cost
- **Phase 7F scope defined:** Automate text/code/Mermaid diagrams, keep marketing graphics manual via Claude Design
- **AI News Scraper concept:** RSS/Reddit scraping, Ollama evaluation, Telegram alerts, Obsidian storage

#### Business
- **Backup plan:** Keep Claude Pro if quality insufficient, still save $10-15/month with hybrid approach
- **Claude Design strategy:** Use sparingly (1-2 projects/week), high-value only, do NOT automate

---

### [April 23, 2026] - Gilgamesh Evolution Roadmap Planning

#### Planning
- **PRIMARY GOAL:** Replace claude.ai with Gilgamesh by August 2026 (16-week roadmap)
- **Cost target:** Reduce from $30-40/month to $5-10/month using local LLM + hybrid routing
- **Phase 38 promoted:** Ollama + ROCm moved to Near Term #2 (CRITICAL — stop API bleeding)
- **Phase 41 promoted:** Hybrid Routing moved to Near Term #6

#### Added
- **Six new Gilgamesh phases:** 7E Extended Memory, 7F File Generation, 7G Vision API, 7H Document Upload, 7I Quality Assurance, 7J Migration
- **4-phase evolution plan:** Foundation (Ollama + Obsidian) → Intelligence (Memory + Routing) → Capabilities (Artifacts + Vision) → Refinement (QA + Migration)
- **Success criteria:** 20+ message conversations, RAG recall, Vision API, artifacts generation, under $10/month cost

#### Documentation
- **Fate agent ecosystem recovered:** Active agents (Gilgamesh, Da Vinci, EMIYA) and planned agents (MERLIN #1 priority, Mash gaming bot, Guardian security)
- **Roadmap restructured:** Complete rewrite with new priority order and cost projections
- **Agent separation:** Gaming agents (Mash Discord) separate from admin agents (Gilgamesh Telegram)

#### Business
- **Backup plan:** Keep Claude Pro if quality insufficient, still save $10-15/month with hybrid approach
- **API limit:** Temporary $10 limit set to control costs during development

---

### [April 23, 2026] - Documentation Update: Fate Agent Roster Recovery

#### Added
- **Fate Grand Order Agent Ecosystem** section in AI-CONTEXT.md
- Active agents documented: Gilgamesh (Telegram), Da Vinci (n8n/Nextcloud), EMIYA (n8n)
- Planned agent priority order: MERLIN #1 (reminders), Mash (gaming Discord bot), Guardian (security)
- Detailed specifications for MERLIN reminder agent and Mash Discord bot
- Agent design principles: Gilgamesh=Telegram admin, Mash=Discord gaming, separate domains

#### Planning
- MERLIN identified as highest priority agent due to user memory issues
- Mash Discord bot planned for Phases 59-64 (Gaming Platform Pipeline)
- Agent ecosystem theme: Fate/Grand Order servants for consistency

#### Documentation
- Recovered agent roster from April 21-22 conversation history
- Established pattern for documenting future agent additions
- Added specific character names for better conversation searchability

#### Lessons Learned
- Use specific character/servant names when searching past conversations ("Merlin Chaldea" vs "Fate themed agent")
- Important planning discussions can get lost without proper documentation structure

---

### [April 20, 2026] - Foundation Planning Session

#### Planning
- **Completed foundation planning** — prioritized health, mental, work, gaming, n8n business, homelab
- **Homepage requirements analysis** — identified 12 must-have widgets for post-foundation build
- **n8n business strategy** — validation framework, tiered pricing ($29/$99/$199), multi-channel marketing
- **Gaming pipeline priority** — confirmed Discord bot build BEFORE n8n scaling (Phases 60-64)
- **Learning roadmap** — deferred Ansible/k8s until curiosity-driven (not job pressure)
- **Backup strategy** — private repo + Backblaze B2 deployment in Week 3-4

#### Technical
- **Priority framework locked** — Health → Mental → Work → Gaming → n8n business → Homelab
- **Foundation execution order** — Sessions 1-2: Obsidian, 3: Commands, 4: Expense, 5: Backup, 6-8: Business
- **Cloudflare Access** — device trust, 30-day sessions for improved UX
- **Obsidian integration** — manual upload for now, automation later

#### Added (Planned)
- Phase 7E.1: Validation Framework (4 hours)
- Phase 7E.2: Marketing Playbook (4 hours)  
- Phase 25.5: Minimal Off-Site Backup (3-4 hours)
- Phase 26 expansion: Homepage Security + Personal/Business Widgets + Price/Stock Tracking + Calendar/Todos/RSS

#### Business Strategy
- Work Claude access: homelab-vault + business-vault (YES), personal-vault (NO)
- Burnout prevention: Obsidian daily energy tracking (1-10 scale)
- Business reframe: "Learning by building" not "must make money"
- Gaming as self-care, not distraction

---

### [April 19, 2026] - Phase 16.1, 16.2, 58: Documentation Pipeline + Windrose Server

#### Added
- **Documentation Pipeline - Update workflow** (Phase 16.1) — 7 nodes, webhook triggered
- **Documentation Pipeline - Sync Docs workflow** (Phase 16.2) — 7 nodes, webhook triggered  
- **Windrose dedicated server** deployed via Docker on CT 302
- Server running at `/opt/windrose`, 4 max players, Medium difficulty
- Invite code NAJHINWINDROSE set and tested successfully

#### Technical
- Pipeline routes: /update (3 files every session), /sync-docs (7 files deployment sessions)
- Used `indifferentbroccoli/windrose-server-docker` image
- Wired both pipelines into Telegram Agent via Switch node routes
- Updated Telegram Chat ID from 510832696 to 518832696 (correction)
- Created new Nextcloud app password: n8n-doc-pipeline

#### Fixed
- Nextcloud Push 401: Created new app password for doc pipeline
- Template literal syntax errors with backticks in AI-CONTEXT.md content
- Switch node routing for /sync-docs command

#### Removed
- session-summary.md from GitHub (legacy file no longer needed)

---

### [December 19, 2024] - Documentation Pipeline Test

#### Technical
- Tested documentation pipeline integration via Telegram /update command
- Verified end-to-end workflow from Telegram → n8n → Claude → GitHub  
- No infrastructure changes made - pipeline verification session only
- Documentation merging process confirmed operational

---

### [December 19, 2024] - Documentation Pipeline Test

#### Added
- Tested documentation pipeline integration via Telegram /update command
- Verified end-to-end workflow from Telegram → n8n → Claude → GitHub

#### Technical
- No infrastructure changes made during this session
- Pipeline verification successful - documentation merging process confirmed operational

---

### [April 19, 2026] - Phase 58: Windrose Server Deployment

#### Added
- **Windrose dedicated server** deployed via Docker on CT 302
- Server running at `/opt/windrose` on gaming-wings-1
- 4 max players, Medium difficulty setting
- Successfully tested connection with invite code

#### Technical
- Used `indifferentbroccoli/windrose-server-docker` image
- Deployed alongside existing Pterodactyl game servers
- Docker Compose deployment on CT 302

---

### [April 19, 2026] - Phase 58: Windrose Server Deployment

#### Added
- **Windrose dedicated server** deployed via Docker on CT 302
- Server running at `/opt/windrose` on gaming-wings-1
- 4 max players, Medium difficulty setting
- Successfully tested connection with invite code

#### Technical
- Used `indifferentbroccoli/windrose-server-docker` image
- Deployed alongside existing Pterodactyl game servers
- Docker Compose deployment on CT 302

---

## [April 14, 2026] - Documentation Update

### Changed
- Updated README.md with current project state (16 phases complete)
- Updated current-state.md with all 12 containers
- Added ROADMAP.md with comprehensive phase planning
- Updated changelog.md with missing entries

---

## [April 7, 2026] - Phase 7D-Sec: Cloudflare Access for n8n

### Added
- Cloudflare Access protection for n8n (email OTP)
- n8n now secured same as Grafana
- Updated AI-CONTEXT.md with consolidated state

### Security
- n8n.najhin-gaming.com now requires email OTP authentication
- Matches existing Grafana security model

---

## [April 6, 2026] - Phase 7D: Gilgamesh Enhancements Complete

### Added
- **Conversation memory** — Last 20 messages stored in n8n Data Tables
- **Smart routing** — Haiku 4.5 for simple queries, Sonnet 4 for complex
- **Web search** — Real-time information via Claude web_search tool
- **Cost tracking** — Token usage logged to gilgamesh_costs table

### Fixed
- Save Cost node — Recreated gilgamesh_costs table (n8n schema caching bug)
- Save Assistant Message — Added Extract Response node for web search multi-block responses

### Technical
- Model IDs: `claude-haiku-4-5-20251001`, `claude-sonnet-4-20250514`
- Memory retention: 20 messages per conversation
- Cost logging: Input tokens, output tokens, model used, timestamp

---

## [April 5, 2026] - Gilgamesh Memory & Web Search

### Added
- Conversation memory using n8n Data Tables
- Web search capability via Claude API
- Memory stores role, content, timestamp per message

### Fixed
- HTTP Request JSON formatting issues
- If node routing for model selection
- Send text message node configuration

---

## [April 2, 2026] - Phase 7C: Gilgamesh Telegram Bot

### Added
- **Gilgamesh AI Agent** — Telegram bot @JhinGilgamesh_bot
- n8n workflow: Telegram → Claude API → Response
- `/update` command for context synchronization
- GitHub integration for AI-CONTEXT.md updates
- Nextcloud integration for backup storage

### Technical
- Chat ID: 510832696
- Workflow: Telegram Agent on CT 211
- Claude API integration with system prompt

---

## [April 2, 2026] - Phase 7B: n8n Workflow Automation

### Added
- **CT 211** (automation-n8n) — n8n workflow automation platform
- Subdomain: n8n.najhin-gaming.com
- Nginx Proxy Manager configuration
- Initial workflow templates

### Technical
- Container: LXC on VLAN 30
- IP: 192.168.30.211
- Storage: 20GB on vmpool-fast

---

## [March 16, 2026] - Phase 7A Completion & Pi-hole Enhancement

### Completed
- Phase 7A fully complete (pfSense + Pi-hole backups added)
- Enhanced Pi-hole blocklists: 81K → 489K domains blocked

### Added
- Purchased TP-Link EAP610 WiFi AP (pending setup)

---

## [March 13, 2026] - Phase 7A: Backup Strategy

### Added
- Automated backup jobs for all containers
- Two-tier backup strategy:
  - Small containers → kinmoon-smb (NAS)
  - Large containers → data-storage (local HDD)
- Retention policy: 7 daily, 4 weekly, 2 monthly

### Technical
- Schedule: 02:00 (small), 02:30 (large)
- Protocol: SMB/CIFS for NAS (NFS had UID mapping issues)

---

## [March 10, 2026] - Documentation Update

### Changed
- Updated roadmap.md with Phase 7A planning
- Updated service-catalog.md
- Updated current-state.md

---

## [March 9, 2026] - Phase 6F: Infrastructure Audit & Hardening

### Security
- Comprehensive firewall rule audit
- VLAN isolation verification
- pfSense rule ordering corrected (before RFC1918 block)
- SERVICE_PORTS alias updated with monitoring ports

### Fixed
- VLAN10_MGMT rule for pfSense WebUI access (TCP 443)
- Inter-VLAN routing verification

---

## [March 8, 2026] - Phase 7: Nextcloud Deployment

### Added
- **CT 220** (nextcloud-hub) — Nextcloud file sync & storage
- Subdomain: cloud.najhin-gaming.com
- Cloudflare Tunnel for external access
- SSL via Nginx Proxy Manager

### Technical
- Container: LXC on VLAN 30
- IP: 192.168.30.220
- Storage: 100GB on vmpool-fast

---

## [March 3, 2026] - Phase 9: NAS Deployment

### Added
- **Kinmoon** — UGREEN DXP2800 NAS deployed
- 3.6TB WD Purple storage
- SMB/CIFS share for Proxmox backups
- Storage ID: kinmoon-smb

### Technical
- IP: 192.168.10.15 (VLAN 10)
- Protocol: SMB/CIFS (NFS had LXC UID mapping issues)

---

## [March 2026] - Phase 6E: Homepage Dashboard

### Added
- **CT 208** (dashboard-homepage) — gethomepage.dev dashboard
- Subdomain: home.najhin-gaming.com
- Widgets: Proxmox, Pi-hole, Uptime Kuma, Prometheus, weather, game servers

### Technical
- Container: LXC on VLAN 30
- IP: 192.168.30.208

---

## [February 2026] - Phase 6A-6D: Gaming Platform

### Added
- **CT 300** (gaming-panel) — Pterodactyl Panel
- **CT 302** (gaming-wings-1) — Pterodactyl Wings node
- Terraria server (terraria.najhin-gaming.com)
- Minecraft server (mc.najhin-gaming.com)

### Technical
- DNS: Cloudflare DNS-only mode (not proxied) for game servers
- Double NAT port forwarding configured

---

## [February 2026] - Phase 5: Monitoring Stack

### Added
- **CT 202** (monitoring-prometheus) — Metrics collection
- **CT 203** (monitoring-grafana) — Dashboards & visualization
- **CT 204** (monitoring-loki) — Log aggregation
- **CT 205** (monitoring-alertmanager) — Alert routing
- **CT 206** (monitoring-uptime) — Uptime Kuma
- Cloudflare Access for Grafana (email OTP)
- 13 alert rules configured
- Telegram + Discord alert routing

---

## [February 2026] - Phase 4: External Access & SSL

### Added
- Cloudflare Tunnel configuration
- SSL certificates via Let's Encrypt (NPM)
- Cloudflare Access (email OTP) setup

---

## [January 2026] - Phase 3: Core Services

### Added
- **CT 201** (nginx-proxy-manager) — Reverse proxy
- **CT 207** (network-ddns) — Dynamic DNS
- Pi-hole deployment on Raspberry Pi 4
- Tailscale subnet router on pfSense

---

## [January 2026] - Phase 2: pfSense & VLANs

### Added
- pfSense firewall on AC8F Mini PC
- 5 VLAN configuration (10, 20, 30, 40, 50)
- 802.1Q trunk to TP-Link switch
- Inter-VLAN routing with firewall rules

### Lessons Learned
- Proxmox needs `bridge-vids 2-4094` for VLAN passing
- Pi-hole requires "ALL" listening mode for multi-VLAN
- Place rules BEFORE RFC1918 block in pfSense

---

## [January 2026] - Phase 1: Proxmox VE Installation

### Added
- Proxmox VE 8.x installation on Kuromoon
- ZFS mirror on NVMe drives (vmpool-fast)
- LVM-thin storage (local-lvm)
- First Ubuntu Server VM for testing

### Hardware
- Ryzen 5 5600X, 32GB DDR4
- 2x 1TB NVMe (ZFS mirror)
- RX 6700 XT 12GB (for future AI workloads)

---

## Legend

- **Added** — New features or components
- **Changed** — Changes to existing functionality
- **Fixed** — Bug fixes
- **Security** — Security-related changes
- **Technical** — Technical details and specifications
- **Lessons Learned** — Knowledge gained from troubleshooting