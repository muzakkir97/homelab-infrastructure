# 📝 Changelog

All notable changes to the homelab infrastructure project.

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