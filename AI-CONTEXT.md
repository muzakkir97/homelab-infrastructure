# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** April 25, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Phase 16.3 complete (Da Vinci Documentation Pipeline). 14 LXC containers running.

---

## 👤 About Me

| Field            | Details                                              |
|------------------|------------------------------------------------------|
| **Name**         | Muzakkir Kholil                                      |
| **Current Role** | Customer Service Engineer @ F-Secure (cybersecurity) |
| **Location**     | Petaling Jaya, Selangor, Malaysia                    |
| **Target Role**  | Cloud Engineering / DevOps                           |
| **Domain**       | najhin-gaming.com (Cloudflare)                       |
| **GitHub**       | github.com/muzakkir97                                |

---

## 🎯 My Preferences (IMPORTANT)

### Learning Style
- **Prefer complex over simple** — I want to understand the "why", not just copy-paste commands
- **Explain like you're teaching** — Don't assume I know; explain concepts before implementation
- **No shortcuts** — I'd rather learn the hard way if it teaches more

### Documentation Requirements
- **GitHub docs must be sanitized** — Replace real IPs with `192.168.x.x` or `[PLACEHOLDER]`
- **LinkedIn posts for each phase** — Professional updates showing progress
- **Dual documentation** — Public (sanitized) + Private (full details)

### Communication
- **Don't use multiple-choice widgets during deployment** — Just give me the instructions
- **Be direct** — Tell me if my idea is bad and suggest alternatives
- **It's okay to say "I don't know"** — I prefer honesty over guessing
- **Make recommendations directly** — No re-asking confirmed decisions, single confirmation then proceed

### Naming Conventions
- **Function-based names:** `monitoring-prometheus`, `gaming-panel`, `network-ddns`
- **Moon theme for hardware:** Kuromoon (Proxmox), Kinmoon (NAS), Minimoon (Gaming PC)

### Priority Framework
- **Health → Mental → Work → Gaming → n8n business → Homelab**
- Gaming pipeline is self-care, not distraction
- n8n business priority sits below gaming (health/mental focus)

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                     | IP Address     | Role                                 |
|------------------|----------|-------------------------------------------|----------------|--------------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB  | 192.168.10.5   | Hypervisor                           |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                  | 192.168.10.1   | Router, Firewall                     |
| Managed Switch   | —        | TP-Link TL-SG108E                         | 192.168.1.20   | Layer 2, VLANs                       |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple           | 192.168.10.15  | Backups (SMB target: kinmoon-nfs)    |
| DNS Server       | —        | Raspberry Pi 4                            | 192.168.30.10  | Pi-hole (~489K domains blocked)      |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB          | 192.168.20.101 | **Gaming only — never homelab**      |
| WiFi AP          | —        | TP-Link EAP610                            | TBD            | Purchased, pending setup             |

### GPU Notes (Kuromoon)
- RX 6700 XT in IOMMU Group 18, audio in Group 19
- Earmarked for Ollama/ROCm AI inference (Phase 38 — promoted to Near Term #2)
- Idle baseline: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan

---

## 🌐 Network Architecture

### Topology: Router-on-a-Stick

```
Internet → ISP Router (192.168.100.1) → pfSense (WAN: DHCP)
                                              ↓
                                    802.1Q Trunk (igc2)
                                              ↓
                                    TP-Link TL-SG108E
                                              ↓
                    ┌─────────┬─────────┬─────────┬─────────┐
                  VLAN10   VLAN20   VLAN30   VLAN40   VLAN50
                  Mgmt     Main    Services   DMZ    Malware
```

### VLAN Design (OPERATIONAL)

| VLAN ID | Name            | Subnet          | Gateway      | Purpose                                |
|---------|-----------------|-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

---

## 📦 Container Inventory (Proxmox LXC)

| CTID | Name                    | IP             | Subdomain                     | Autostart | Status     |
|------|-------------------------|----------------|-------------------------------|-----------|------------|
| 201  | nginx-proxy-manager     | 192.168.30.201 | —                             | ✅        | ✅ Running |
| 202  | monitoring-prometheus   | 192.168.30.202 | —                             | ✅        | ✅ Running |
| 203  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | ✅        | ✅ Running |
| 204  | monitoring-loki         | 192.168.30.204 | —                             | ✅        | ✅ Running |
| 205  | monitoring-alertmanager | 192.168.30.205 | —                             | ✅        | ✅ Running |
| 206  | monitoring-uptime       | 192.168.30.206 | —                             | ✅        | ✅ Running |
| 207  | network-ddns            | 192.168.30.207 | —                             | ✅        | ✅ Running |
| 208  | dashboard-homepage      | 192.168.30.208 | home.najhin-gaming.com        | ✅        | ✅ Running |
| 211  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | ✅        | ✅ Running |
| 213  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | ✅        | ✅ Running |
| 214  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | ✅        | ✅ Running |
| 220  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅        | ✅ Running |
| 300  | gaming-panel            | 192.168.30.210 | —                             | ✅        | ✅ Running |
| 302  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | ✅        | ✅ Running |
| 400  | ollama-gpu              | 192.168.30.221 | ollama.najhin-gaming.com      | ✅        | ✅ Running |

**Total: 14 LXC containers, all on VLAN 30, all autostart enabled**

### Nextcloud Hub (CT 220)
- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Data location:** Currently on /var/lib/nextcloud (root disk)
- **Planned migration:** Move data directory to /mnt/data-storage (7.3TB HDD) during future phase
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/

### Gaming Servers (Docker on CT 302)
- **Terraria** — terraria.najhin-gaming.com
- **Minecraft** — mc.najhin-gaming.com  
- **Windrose** — Deployed at /opt/windrose, 4 max players, Medium difficulty, invite code NAJHINWINDROSE

---

## 📚 Obsidian Knowledge Base (Phase 22)

### Vault Configuration
- **Path:** C:\Users\muzak\Nextcloud\Obsidian\second-brain (auto-syncs to CT 220)
- **Platform:** Obsidian on Minimoon (Gaming PC)
- **Sync:** Via Nextcloud - changes sync automatically between local vault and cloud

### Plugins Installed
1. **Dataview** — Database queries and dashboards
2. **Tasks** — Task management and tracking  
3. **Templater** — Note templates and automation
4. **Calendar** — Calendar view integration
5. **Excalidraw** — Diagrams and sketches
6. **Kanban** — Kanban boards for project management

### Folder Structure
```
second-brain/
├── 00-inbox/          # Quick capture, unsorted notes
├── 01-homelab/        # Infrastructure documentation
├── 02-career/         # Career transition materials
├── 03-knowledge/      # Learning notes, references
├── 04-personal/       # Personal management (subscriptions, finance)
├── 05-templates/      # Note templates
└── 06-archive/        # Completed or outdated notes
```

### Key Features
- **Subscription tracker:** Individual notes per subscription in 04-personal/subscriptions/
- **Dashboard:** Dataview queries showing subscription costs and totals at 04-personal/dashboard
- **Session summaries:** Template at 05-templates/session-summary.md for homelab documentation

---

## 🔮 Gilgamesh Evolution Roadmap

### PRIMARY GOAL: Replace claude.ai with Gilgamesh by August 2026 (16 weeks)

**Current Cost:** $30-40/month Claude Pro + API usage
**Target Cost:** $5-10/month (local LLM + occasional API)

### 4-Phase Plan

#### Phase 1: Foundation (Weeks 1-4)
- **Phase 38** — Ollama + ROCm on Kuromoon RX 6700 XT (CRITICAL — stop API bleeding)
- **Phase 22** — Obsidian Knowledge Base ✅ COMPLETE

#### Phase 2: Intelligence (Weeks 5-8)
- **Phase 7E** — Extended Memory (20+ message conversations via RAG)
- **Phase 41** — Hybrid Routing (local/API based on complexity)

#### Phase 3: Capabilities (Weeks 9-12)
- **Phase 7F** — File Generation (code, configs, docs)
- **Phase 7G** — Vision API (image analysis)
- **Phase 7H** — Document Upload (PDF/text processing)

#### Phase 4: Refinement (Weeks 13-16)
- **Phase 7I** — Quality Assurance (compare outputs with Claude Pro)
- **Phase 7J** — Migration (full switch to Gilgamesh)

### Success Criteria
- 20+ message conversation retention
- Knowledge base recall from Obsidian
- Vision capabilities for screenshots/diagrams
- Artifact generation (code, configs)
- Monthly cost under $10

### Cost Projection Table

| Component | Current | Target | Savings |
|-----------|---------|--------|---------|
| Claude Pro | $20/month | $0/month | $240/year |
| API Usage | $10-20/month | $5-10/month | $60-120/year |
| **Total** | **$30-40/month** | **$5-10/month** | **$300-360/year** |

### Backup Plan
Keep Claude Pro if quality insufficient, still save $10-15/month with hybrid approach

---

## 🎴 Fate Grand Order Agent Ecosystem

Theme: Homelab agents named after Fate/Grand Order servants, discussed April 21-22, 2026

### Active Agents

| Servant | Class | Role | Platform | Status |
|---------|-------|------|----------|--------|
| Gilgamesh 👑 | Archer | Personal AI Assistant | Telegram (@JhinGilgamesh_bot) | ✅ Active |

### Planned Agents

| Servant | Class | Role | Platform | Status |
|---------|-------|------|----------|--------|
| Caster (Da Vinci) 🎨 | Caster | Knowledge Curator | n8n/Nextcloud | 📋 Planned (Stage 2: RAG) |
| Archer (EMIYA) 🏹 | Archer | Infrastructure Translator | n8n | 📋 Planned |

### Planned Agents (Priority Order)

Phase 2: Safety Net (High Priority)

| Servant | Class | Role | Noble Phantasm | Priority |
|---------|-------|------|----------------|----------|
| MERLIN 🔮 | Caster | Reminders & Scheduler | *Garden of Avalon* | #1 (user keeps forgetting everything) |
| Mash Kyrielight 🛡️ | Shielder | Gaming Server Manager + Wellbeing | *Lord Camelot* | High (Phases 59-64) |
| Guardian | — | Security Monitoring | — | Medium |

Phase 3: Efficiency
- Nexus — Cross-platform Automation
- Scribe — Auto-documentation
- Midas — Cost Tracking & Optimization

Phase 4: Intelligence
- Oracle — Predictive Intelligence

### Merlin Details (Reminder Agent — #1 Priority)

Why Merlin:
- Clairvoyance (sees the future → knows what you'll forget)
- Garden of Avalon (creates safe space → maintenance windows)
- Illusion magic (reminds you gently, not annoyingly)

What Merlin Does:
- SSL certificate renewal reminders (expires in X days)
- Backup restore test reminders (overdue by X days)
- Loki/Prometheus memory limit warnings (OOM in X days)
- Scheduled maintenance windows
- Proactive infrastructure health checks

### Mash Details (Gaming Discord Bot)

Why Mash:
- Protects the team (gaming friends)
- Keeps everyone safe and organized
- Announces player activity

What Mash Does:
- Discord bot commands: !start, !stop, !status
- Announces player joins/leaves
- Scheduled game night reminders
- Auto-shutdown idle servers
- Game update notifications
- Personality: "Senpai, PlayerX just joined Windrose!"

Implementation: Phases 59-64 (Gaming Platform Pipeline)

### Design Principles

- Gilgamesh = Telegram-only (homelab admin)
- Mash = Discord-only (gaming with friends)
- Keep gaming separate from homelab admin
- All agents use Fate/GO servant theming for consistency

---

## 🤖 Gilgamesh AI Agent (Phase 7C/7D/7D-Menu/15)

### Architecture
```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Claude API → Response
                                     ↓
                              Memory (n8n Data Tables)
                                     ↓
                              /update → Nextcloud → GitHub
                              /sync-docs → Full Doc Pipeline
```

### Features
- **Conversation memory:** Last 20 messages stored in n8n Data Tables
- **Smart routing:** Simple inputs → Haiku 4.5, complex queries → Sonnet 4
- **Web search:** Real-time information via Claude's web_search tool
- **Cost tracking:** Token usage logged to gilgamesh_costs table
- **Inline keyboard menu:** Full menu system with all submenus working
- **Context sync:** /update command pushes session summaries to AI-CONTEXT.md via GitHub
- **Documentation pipeline:** /sync-docs triggers full documentation regeneration (7 files)
- **Slash commands:** 6 additional commands for direct actions

### Slash Commands

| Command | Purpose | Description |
|---------|---------|-------------|
| /help | Quick reference | Shows all available commands and menu options |
| /clear | Memory reset | Clears conversation memory (last 20 messages) |
| /memory | View history | Shows recent conversation messages |
| /cost | Usage tracking | Displays token usage and estimated costs |
| /alerts | System status | Shows active Alertmanager alerts |
| /backup | Backup status | Last backup times for all containers |
| /update | Session summary | Merges session into docs (3 files) |
| /sync-docs | Full regeneration | Regenerates all documentation (7 files) |

### Technical Details

| Component      | Value                       |
|----------------|-----------------------------|
| Telegram Bot   | @JhinGilgamesh_bot          |
| Chat ID        | 518832696                   |
| Haiku Model    | claude-haiku-4-5-20251001   |
| Sonnet Model   | claude-sonnet-4-20250514    |
| n8n Container  | CT 211, 192.168.30.211      |
| Proxmox API    | root@pam!gilgamesh token    |
| Proxmox node   | muzakkir (not kuromoon)     |

### Cost Tracking Note
- gilgamesh_costs.cost_usd column always returns 0 (pre-existing bug from earlier phases)
- Cost calculation uses estimated token rates (Sonnet $3/$15 per 1M input/output tokens)

### n8n Workflows (Count: 8)

| Workflow                    | Purpose                             | Nodes | Trigger         |
|-----------------------------|-------------------------------------|-------|-----------------|
| Telegram Agent              | Main bot, menu, commands            | 15    | Telegram        |
| Documentation Pipeline - Update | Session summary → 3 files     | 7     | Webhook         |
| Documentation Pipeline - Sync Docs | Full doc regeneration → 7 files | 7  | Webhook         |
| Da Vinci Documentation Pipeline | Raw staging summaries → formatted docs | 11 | Webhook |
| Update Nextcloud File       | Legacy (unpublished)                | 5     | Webhook         |
| Push to GitHub              | Legacy (unpublished)                | 4     | Webhook         |

### Inline Keyboard Menu Status

| Menu Item        | Status       |
|------------------|--------------|
| Main Menu        | ✅ Working   |
| Homelab → Status | ✅ Working   |
| Homelab → Metrics| ✅ Working   |
| Homelab → Temps  | ✅ Working   |
| Homelab → Storage| ✅ Working   |
| Gaming submenu   | ✅ Working   |
| Gilgamesh submenu| ✅ Working   |
| Tools submenu    | ✅ Working   |
| Help             | ✅ Working   |

### SSH & API Access
- **SSH to Kuromoon:** CT 211 → 192.168.10.5:22 (pfSense rule added)
- **SSH to CT 302:** Password auth enabled (PermitRootLogin yes)
- **Proxmox API:** root@pam!gilgamesh token
- **Storage IDs:** kinmoon-nfs (not kinmoon-smb in Proxmox)

### Known Constraints
- Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body
- Progress bars use ASCII (= and -) — Unicode block chars don't render on Telegram mobile

---

## 🔧 Documentation Pipeline (Phase 16.1/16.2/16.3)

### Architecture
```
Session Summary → Claude → AI-CONTEXT.md + changelog.md + troubleshoot.md
                    ↓
               Nextcloud + GitHub
```

#### Da Vinci Documentation Pipeline (Phase 16.3)
```
Raw summaries → AI-CONTEXT-staging.md → Da Vinci n8n workflow → Claude API → Formatted AI-CONTEXT.md
                                                    ↓
                                              Nextcloud + GitHub
```

### Commands

| Command     | Purpose                                     | Files Updated |
|-------------|---------------------------------------------|---------------|
| /update     | Every session - merge summary into docs    | 3 files       |
| /sync-docs  | Deployment sessions - regenerate all docs  | 7 files       |

### Pipeline Components

| Component    | Details                              |
|--------------|--------------------------------------|
| Webhooks     | doc-update, doc-sync, da-vinci       |
| Telegram     | Route /update and /sync-docs         |
| Claude API   | Documentation merging intelligence   |
| Nextcloud    | File storage via admin user          |
| GitHub       | Version control via API push         |
| Da Vinci     | Async processing (she/her pronouns)  |
| Staging file | AI-CONTEXT-staging.md (rolling append) |

### File Coverage (sync-docs)
- AI-CONTEXT.md, changelog.md, troubleshoot.md (from /update)
- README.md, roadmap.md, current-state.md, service-catalog.md

---

## 🤖 Bots & Integrations

| Bot | Platform | Source | Status | Purpose |
|-----|----------|--------|--------|---------|
| @JhinGilgamesh_bot | Telegram | n8n CT 211 | ✅ Active | Personal AI agent — chat, homelab control, /update, /sync-docs |
| Homelab Alerts | Telegram | Alertmanager CT 205 | ✅ Active | Critical alerts (host down, high CPU/memory/disk) |
| Homelab Alerts | Discord webhook | Alertmanager CT 205 | ✅ Active | Warning-level alerts to #alerts channel |

**Planned:** Migrate Alertmanager alerts to route through n8n first (central hub). Game server notifications to Discord via n8n.

---

## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                              | Retention                    |
|------------------|----------|--------------------------|----------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)        | 201, 202, 203, 204, 205, 206, 207, 214, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302                               | 7 daily, 4 weekly, 2 monthly |

### Storage

| Name         | Type       | Protocol | Capacity | Purpose                 |
|--------------|------------|----------|----------|-------------------------|
| kinmoon-smb  | NAS        | SMB/CIFS | 3.6 TB   | Small container backups |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  |

---

## 🔒 Security Architecture

| Layer           | Implementation                                                                    |
|-----------------|-----------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                     |
| Segmentation    | 5 VLANs with enforced firewall rules                                              |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                       |
| VPN             | Tailscale (subnet router on pfSense, primary access)                              |
| External Auth   | Cloudflare Access (email OTP) for Grafana, n8n, Vault, Vaultwarden (7 apps total) |
| External Access | Cloudflare Tunnel for Nextcloud                                                   |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                    |
| Backup          | Automated daily backups with 7/4/2 retention                                     |
| Secrets         | HashiCorp Vault (CT 213, vault.najhin-gaming.com). KV engine at kv/. Secrets: kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager, kv/github, kv/nextcloud, kv/n8n, kv/pihole (8 paths total) |
| Passwords       | Vaultwarden (CT 214, passwords.najhin-gaming.com). Personal password manager with Bitwarden clients. |

---

## 📋 Project Phase History

| Phase   | Title                                                     | Status         | Completed        |
|---------|-----------------------------------------------------------|----------------|------------------|
| 1       | Proxmox VE Installation                                   | ✅ Complete    | Jan 2026         |
| 2       | pfSense Firewall & VLAN Setup                             | ✅ Complete    | Jan 2026         |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)             | ✅ Complete    | Jan 2026         |
| 4       | External Access & SSL                                     | ✅ Complete    | Feb 2026         |
| 5       | Monitoring Stack                                          | ✅ Complete    | Feb 2026         |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)        | ✅ Complete    | Feb 2026         |
| 6E      | Homepage Dashboard                                        | ✅ Complete    | Mar 2026         |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening | ✅ Complete    | Mar 9, 2026      |
| 7       | Nextcloud Deployment                                      | ✅ Complete    | Mar 8, 2026      |
| 7A      | Backup Strategy                                           | ✅ Complete    | Mar 13, 2026     |
| 7B      | n8n Workflow Automation                                   | ✅ Complete    | Apr 2, 2026      |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration               | ✅ Complete    | Apr 2, 2026      |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)      | ✅ Complete    | Apr 6, 2026      |
| 7D-Sec  | Cloudflare Access for n8n                                 | ✅ Complete    | Apr 7, 2026      |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                            | ✅ Complete    | Apr 24, 2026     |
| 9       | NAS Deployment (Kinmoon)                                  | ✅ Complete    | Mar 3, 2026      |
| 13      | HashiCorp Vault — Secrets Manager                         | ✅ Complete    | Apr 18, 2026     |
| 14      | Secrets Management & Integration                          | ✅ Complete    | Apr 24, 2026     |
| 15      | Gilgamesh Additional Slash Commands                       | ✅ Complete    | Apr 24, 2026     |
| 16.1    | Documentation Pipeline - Update Workflow                  | ✅ Complete    | Apr 19, 2026     |
| 16.2    | Documentation Pipeline - Sync Docs Workflow               | ✅ Complete    | Apr 19, 2026     |
| 16.3    | Da Vinci Documentation Pipeline                           | ✅ Complete    | Apr 25, 2026     |
| 22      | Obsidian Knowledge Base                                   | ✅ Complete    | Apr 24, 2026     |
| 23      | Vaultwarden + Secrets Audit & Cleanup                     | ✅ Complete    | Apr 18, 2026     |
| 24.1    | Service Update Manager (Docker + apt + Proxmox, approval-gated) | 📋 Planned | —               |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                      | ✅ Complete    | Apr 24, 2026     |
| 39      | Open WebUI                                                | ✅ Complete    | Apr 24, 2026     |
| 58      | Windrose Server Deployment                                | ✅ Complete    | Apr 19, 2026     |

---

## 🐛 Key Lessons Learned

### Network & VLANs

| Issue                            | Resolution                                          |
|----------------------------------|-----------------------------------------------------|
| Proxmox LXC VLAN not working     | Add `bridge-vids 2-4094` to /etc/network/interfaces |
| Intel i226-V NICs failing        | FreeBSD driver issue; only igc2/igc3 work on AC8F   |
| Double NAT breaking port forward | Port forward on BOTH ISP router AND pfSense         |
| Inter-VLAN DNS not resolving     | Use pfSense DNS Resolver forwarding to Pi-hole      |

### Firewall Hardening

| Issue                            | Resolution                                    |
|----------------------------------|-----------------------------------------------|
| Internet loss after rule changes | Source must be "subnets" not "address"        |
| Tailscale ping failing           | Normal — use `tailscale ping` instead of ICMP |
| Blocked traffic still works      | Disconnect Tailscale to test local firewall   |

### Backup Strategy

| Issue                        | Resolution                                            |
|------------------------------|-------------------------------------------------------|
| SMB/CIFS kernel crashes      | Switch to NFS protocol (later switched back to SMB)   |
| NFS permission denied        | Change squash to "Map all users to admin"             |
| Large file I/O errors on NAS | Hybrid approach — large containers to local storage   |
| ZFS snapshot busy            | Kill stuck processes or reboot, then destroy snapshot |
| Container locked (backup)    | `pct unlock <CTID>`                                   |

### n8n & Gilgamesh

| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| n8n Data Tables schema caching bug   | Delete and recreate the table entirely                          |
| Claude API 404 model not found       | Use exact model ID: `claude-haiku-4-5-20251001`                 |
| Web search partial response capture  | Add Extract Response Code node, filter by `type === "text"`     |
| Expression scoping issues            | Use Code node with `this.helpers.httpRequest`, reference `$json`|
| NFS fails for LXC backups            | UID namespace mapping (100000-165536); use SMB instead          |
| pfSense rule not working             | Place new rules BEFORE RFC1918 block rules                      |
| n8n GitHub credential 401            | User field (`muzakkir97`) must be populated                     |
| Telegram single webhook constraint   | All update types (message + callback) must use one trigger      |
| Answer Callback node loses data      | Bypass it; handle callback acknowledgment separately            |

### HashiCorp Vault (Phase 13)

| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| apt fails in new LXC                 | Run `echo "nameserver 192.168.30.10" > /etc/resolv.conf` first |
| HashiCorp repo setup fails           | Install `lsb-release` before adding repo                       |
| Wrong repo architecture/codename    | Hardcode `amd64` and `bookworm` in repo entry                   |
| Vault fails to start in LXC          | Add `disable_mlock = true` to vault.hcl                        |
| Token auth issues in shell           | Use `vault login <token>` not `export VAULT_TOKEN=`            |

### Obsidian & Knowledge Management

| Issue                                | Resolution                                                      |
|--------------------------------------|-----------------------------------------------------------------|
| Nextcloud quota exceeded             | Resize CT 220 from 20GB to 100GB with `pct resize 220 rootfs +80G` |
| Dataview queries no results         | Queries must target folder with individual notes, not same file |

### Business & Learning

| Issue                              | Resolution                                             |
|------------------------------------|----------------------------------------------------|
| MVP = Minimum Viable Product (basic version for validation) |  |
| n8n business priority sits below gaming (health/mental focus) |  |
| Gaming pipeline is self-care, not distraction |  |

---

## ❓ Pending Tasks

### Immediate
| Task | Priority |
|------|----------|
| Build MERLIN reminder agent (highest priority due to memory issues) | High |
| Fix cost_usd column not being saved (investigate Save Cost node in main workflow) | High |
| Share Windrose invite code NAJHINWINDROSE with friends | High |
| Monitor RAM usage with Windrose running on CT 302 | High |
| Set Bitwarden app on phone with passwords.najhin-gaming.com server | High |
| Set $10 API limit in Anthropic Console (temporary) | High |
| Export homelab diagram from Claude Design (PNG for GitHub/LinkedIn) | High |
| Store VM 400 root password in Vaultwarden | High |
| Add remaining subscriptions to Obsidian vault | High |
| Fix Total by Category query in Obsidian dashboard (backtick issue) | High |
| Store Nextcloud app password in Vault kv/nextcloud | High |
| Store GitHub token in Vault kv/github | High |
| Update Cloudflare Access icons (7 apps including Ollama) | High |

### Infrastructure
| Task | Priority |
|------|----------|
| Update Nextcloud to 33.0.2 | High |
| Move Nextcloud data directory to /mnt/data-storage (long term) | Medium |
| Address local-lvm thin pool overprovisioning (during infrastructure cleanup) | Medium |
| Plan Phase 24.1 (Service Update Manager) in dedicated session | Medium |
| Delete old disconnected /update nodes from Telegram Agent workflow | Medium |
| Delete Test SQLite workflow from n8n | Medium |
| Retire Update Nextcloud File and Push to GitHub workflows (unpublish) | Medium |
| Create muzakkir97/homelab-private repo | Medium |
| Set up Backblaze B2 account | Medium |
| Phase 27 (Vault + n8n) enables n8n to fetch secrets directly | Medium |

### Gilgamesh & Documentation
| Task | Priority |
|------|----------|
| Build Phase 16.3 — Monthly Infrastructure Audit cron workflow | High |
| Build Mash Discord bot (Phases 59-64) | High |
| /update redesign — file attachment via Telegram, push to GitHub + Nextcloud | Medium |
| Homepage embedded Gilgamesh chat UI (web frontend, shared memory with Telegram) | Medium |
| Integrate Vault secrets into n8n Gilgamesh workflow | Medium |
| Document all future agent additions in Fate Agent section | Medium |
| Build Da Vinci Stage 2 (RAG) alongside Phase 7E | Medium |

### Infrastructure
| Task | Priority |
|------|----------|
| Migrate Alertmanager alerts through n8n (central notification hub) | Medium |
| Switch management IP (192.168.1.20 → 192.168.10.20) | Medium |
| Remove legacy LAN 192.168.1.0/24 (after switch migration) | Medium |
| WiFi Access Point setup (EAP610 purchased) | Medium |
| Homepage security (Cloudflare Access) | Low |
| Off-site backup (Backblaze B2) | Low |
| Nextcloud 2FA (TOTP) | Low |

### Deferred
| Task | Priority |
|------|----------|
| Fedora dual-boot on Minimoon (kernel 6.12+ for RDNA 4) | Deferred |
| Old P400S build repurpose (Z270E + i7-7700K) | Deferred |
| Claude Project auto-sync — revisit if Anthropic releases Project Files API | Deferred |

---

## 📝 Session Log (Recent)

### April 25, 2026
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

###Date: April 24, 2026
Phase: 38 — Ollama + ROCm Local LLM (COMPLETE)
        39 — Open WebUI (COMPLETE)

Topics Discussed

Deployed Ollama with ROCm GPU acceleration on RX 6700 XT
Created VM 400 (ollama-gpu) with PCIe passthrough on Proxmox
Installed Ubuntu 22.04, ROCm 6.1.3 runtime, Ollama
Pulled qwen2.5:14b (9GB, fits in 12GB VRAM) and llama3.2:latest
Deployed Open WebUI via Docker
Exposed via NPM at ollama.najhin-gaming.com with SSL + Cloudflare Access

Decisions Made

VM 400 named ollama-gpu, static IP 192.168.30.221, VLAN 30
GPU passthrough: 2d:00.0 (RX 6700 XT) + 2d:00.1 (audio)
ROCm minimal runtime only (not full SDK) to save disk space
HSA_OVERRIDE_GFX_VERSION=10.3.0 set via systemd override for gfx1031
OLLAMA_HOST=0.0.0.0 set so Docker container can reach Ollama API
Open WebUI connects via Docker bridge 172.17.0.1:11434
Primary model: qwen2.5:14b (14B fits in 12GB VRAM comfortably)
Secondary model: llama3.2 3B for fast/light tasks
VM autostart enabled

Changes to AI-CONTEXT.md

Add VM 400 (ollama-gpu) to container inventory:
  VMID 400, ollama-gpu, 192.168.30.221, ollama.najhin-gaming.com, autostart on, running
Mark Phase 38 as Complete (April 24, 2026)
Mark Phase 39 as Complete (April 24, 2026)
Add Ollama models: qwen2.5:14b, llama3.2:latest
Add new pending task: Store ollama-gpu VM password in Vaultwarden
Add new pending task: Add ollama.najhin-gaming.com to Cloudflare Access icons pending list

Changes to Other Docs

roadmap.md: Move Phase 38 and 39 to Completed (April 24, 2026)
changelog.md: Add Phase 38 + 39 completion entry
service-catalog.md: Add ollama-gpu VM and Open WebUI service

Errors & Resolutions

ROCm full SDK ran out of disk space: LVM only used 28GB of 60GB —
  fixed with lvextend -l +100%FREE and resize2fs
amdgpu still bound after blacklist: Added softdep amdgpu pre: vfio-pci
  to vfio.conf, rebuilt initramfs, rebooted
Ollama running 100% CPU not GPU: Added HSA_OVERRIDE_GFX_VERSION=10.3.0
  and group membership (render, video) via systemd override
Open WebUI no models available: Ollama bound to 127.0.0.1 only —
  set OLLAMA_HOST=0.0.0.0 and pointed container to 172.17.0.1:11434

Action Items

Store ollama-gpu VM root password in Vaultwarden
Add ollama.najhin-gaming.com Cloudflare Access icon
Begin Phase 41 (Gilgamesh + Ollama Hybrid routing) in future session

### April 24, 2026
Date: April 24, 2026
Phase: 22 — Obsidian Knowledge Base (COMPLETE)

Topics Discussed

Installed Obsidian on Minimoon, vault at Nextcloud/Obsidian/second-brain
Installed and enabled 6 plugins: Dataview, Tasks, Templater, Calendar, Excalidraw, Kanban
Created folder structure: 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 05-templates, 06-archive
Created session summary template in 05-templates
Built subscription tracker dashboard using Dataview (separate notes per subscription)
Fixed Nextcloud quota issue — root disk was 99% full (20GB), resized to 100GB
Discovered thin pool overprovisioning warning on local-lvm
Discussed long term fix: move Nextcloud data directory to /mnt/data-storage (7.3TB HDD)
Windrose ground loot discussion — no config option exists in Early Access to remove existing world loot; fresh world reset is the only clean option
Planned Phase 24.1 — Service Update Manager (Docker + apt + Proxmox, approval-gated via Gilgamesh)

Decisions Made

Vault location: C:\Users\muzak\Nextcloud\Obsidian\second-brain (auto-syncs to CT 220)
Subscription entries as individual notes inside 04-personal/subscriptions/
Dashboard note at 04-personal/dashboard
Nextcloud data dir migration deferred — handle per phase order
Phase 24.1 scope: all updates (Docker, apt, Proxmox) with Telegram approval gate
Phase 38 (Ollama + ROCm) to be done in next chat session

Changes to AI-CONTEXT.md

Mark Phase 22 as Complete (April 24, 2026)
Add Obsidian vault details: path C:\Users\muzak\Nextcloud\Obsidian\second-brain
Add 6 plugins list under Obsidian section
Add folder structure to Obsidian section
Add Phase 24.1 to planned phases: Service Update Manager (Docker + apt + Proxmox, approval-gated)
Add pending task: Move Nextcloud data directory to /mnt/data-storage (long term)
Add pending task: Nextcloud 33.0.2 update available
Add pending task: local-lvm thin pool overprovisioning — address during infrastructure cleanup
Update CT 220 disk size from 20GB to 100GB

Changes to Other Docs

roadmap.md: Move Phase 22 to Completed (April 24, 2026)
roadmap.md: Add Phase 24.1 — Service Update Manager to planned phases
changelog.md: Add Phase 22 completion entry

Errors & Resolutions

Nextcloud sync errors (quota exceeded): CT 220 root disk was 99% full — resized from 20GB to 100GB with pct resize 220 rootfs +80G
Dataview no results: queries must point to folder containing individual subscription notes, not the same file

Action Items

Add remaining subscriptions to 04-personal/subscriptions/
Fix Total by Category query (backtick issue in dashboard)
Update Nextcloud to 33.0.2
Move Nextcloud data dir to /mnt/data-storage (deferred, per phase)
Address local-lvm thin pool overprovisioning (deferred, infrastructure cleanup)
Begin Phase 38 (Ollama + ROCm) in next session
Plan Phase 24.1 (Service Update Manager) in dedicated session

### April 24, 2026
Date: April 24, 2026
Phase: 15 — Gilgamesh Additional Slash Commands (COMPLETE)

Topics Discussed

Built 6 new slash commands for Gilgamesh Telegram bot
Removed /temps and /storage from scope (redundant with menu)
Removed /logs from scope (not practical without proper filtering)

Decisions Made

/temps and /storage dropped — already in menu system
/logs dropped — defer to future dedicated menu action
Cost calculation uses estimated token rates (Sonnet $3/$15 per 1M) because cost_usd column in gilgamesh_costs table has always been 0 (pre-existing bug from earlier phases)
Always Output Data enabled on Get Alerts and Clear Memory DB nodes to handle empty responses

Changes to AI-CONTEXT.md

Mark Phase 15 as Complete (April 24, 2026)
Add 6 new commands to Gilgamesh features section: /help, /clear, /memory, /cost, /alerts, /backup
Note: gilgamesh_costs.cost_usd column always 0 — cost estimated from token counts

Changes to Other Docs

roadmap.md: Move Phase 15 to Completed
changelog.md: Add Phase 15 completion entry

Errors & Resolutions

Clear Memory DB "at least one condition required": Added role is not empty condition
Send Clear Confirmation wrong chat_id: Used $('Get Chat ID').first().json.chat_id instead of $json.message.chat.id
Clear Memory DB no output when table empty: Enabled Always Output Data
Format Cost SyntaxError: Removed toLocaleString() and emojis from Code node
Get Alerts no output on empty array: Enabled Always Output Data
Format Alerts "Active Alerts: undefined": Added Array.isArray() check before using alerts

Action Items

Save and Publish Telegram Agent workflow
Run /sync-docs to push updated docs to GitHub
Fix cost_usd column not being saved (investigate Save Cost node in main workflow)
Begin Phase 22 (Obsidian) next session

### April 24, 2026
Date: April 24, 2026
Phase: 7D-Menu + Phase 14 — Gilgamesh Inline Keyboard Menu (COMPLETE)

Topics Discussed

Built all remaining Gilgamesh menu actions
Homelab → Metrics (Proxmox API)
Homelab → Temps (SSH to Kuromoon, lm-sensors)
Homelab → Storage (Proxmox API)
Gaming servers status (SSH to CT 302, docker ps)
Gilgamesh info panel (static)
Tools quick links (static)
Help command reference (static)

Decisions Made

SSH key auth set up from CT 211 to Kuromoon (port 22 pfSense rule added)
SSH password auth set up from n8n to CT 302 (PermitRootLogin yes)
Proxmox root password set for SSH access from n8n
Progress bars use = and - ASCII characters (Unicode block chars don't render on Telegram mobile)
Gaming submenu shows direct status (no submenu layer)
kinmoon storage ID is kinmoon-nfs not kinmoon-smb
SSH credentials: "SSH Password account" (Kuromoon), "SSH CT302 Wings" (CT 302)

Changes to AI-CONTEXT.md

Mark Phase 7D-Menu as ✅ Complete (April 24, 2026)
Mark Phase 14 as ✅ Complete (April 24, 2026)
Update Inline Keyboard Menu Status table — all items ✅ Working
Add SSH credentials to n8n credential list
Add pfSense rule: TCP 22 from 192.168.30.211 to 192.168.10.5 (n8n SSH to Kuromoon)
Add PermitRootLogin yes on CT 302

Changes to Other Docs

roadmap.md: Move 7D-Menu and Phase 14 to Completed
changelog.md: Add Phase 14 completion entry

Errors & Resolutions

Format Metrics callback_query undefined: Pull inputData from $('Callback Router').first().json instead of $json
Send Metrics invalid JSON: Use "Using Fields Below" body mode instead of raw JSON
SSH hanging from CT 211 to Kuromoon: pfSense blocking port 22 VLAN30→VLAN10, added firewall rule
SSH password auth failing on CT 302: PermitRootLogin prohibit-password — changed to PermitRootLogin yes
Get Game Servers invalid syntax: {{.Names}} parsed as n8n expression — switch Command field to Fixed mode
kinmoon-smb showing N/A: Storage ID is kinmoon-nfs in Proxmox

Action Items

Push updated docs to GitHub via /sync-docs
Store Proxmox root password in Vaultwarden
Store CT 302 root password in Vaultwarden
Begin Phase 15 (additional slash commands) or Phase 22 (Obsidian) next session

### April 23, 2026
Date: April 23, 2026
Phase: Gilgamesh Evolution + Claude Design Discovery

Topics Discussed

Fate Grand Order AI agent ecosystem (Gilgamesh, Da Vinci, EMIYA active; MERLIN, Mash, Guardian, Nexus, Scribe, Midas, Oracle planned)
Claude API cost limit hit, need to control spending
Long-term goal: Replace claude.ai with Gilgamesh by August 2026 (16-week roadmap)
Target cost: $30-40/month → $5-10/month
Claude Design discovered (launched April 17, 2026) - new visual design tool
Created homelab infrastructure diagram using Claude Design
AI News Scraper idea for tracking new homelab-ready tools

Decisions Made

PRIMARY GOAL: Gilgamesh replaces Claude Web in 16 weeks, save $240-360/year
Phase 38 (Ollama + ROCm) promoted to Near Term #2 (CRITICAL)
Phase 41 (Hybrid Routing) promoted to Near Term #6
Six new Gilgamesh phases: 7E Extended Memory, 7F File Generation, 7G Vision API, 7H Document Upload, 7I Quality Assurance, 7J Migration
Phase 7K added: AI News Scraper (RSS/Reddit scraping, Ollama evaluation, Telegram alerts, Obsidian storage)
Claude Design strategy: Use sparingly (1-2 projects/week), high-value only (LinkedIn, GitHub, milestones), do NOT automate
Phase 7F scope: Automate text/code/Mermaid diagrams, keep marketing graphics manual via Claude Design

Changes to AI-CONTEXT.md

Add Gilgamesh Evolution Roadmap section (4 phases over 16 weeks, cost projection table, success criteria)
Add Fate Grand Order Agent Ecosystem section (active + planned servants with roles)
Phase 38 promoted to Near Term #2, Phase 41 to Near Term #6
Updated recent_updates with Fate agents, Gilgamesh Evolution goal, Phase 38 promotion, Claude Design discovery

Errors & Resolutions

Claude search missed Fate roster initially, used conversation_search with specific names (Merlin, Mash) to find it

Action Items

Download updated roadmap.md and push to GitHub
Send condensed summary via /update to Gilgamesh
Set $10 API spend limit in Anthropic Console (temporary safety)
Export homelab diagram from Claude Design (PNG for GitHub/LinkedIn)
Begin Phase 38 planning after Phase 14 complete

---

*Last updated: April 25, 2026 — Update this file at the end of each session before pushing to GitHub*