# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** April 27, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Midas CFO Agent, MERLIN Reminders, and Obsidian Daily Creator active. 14 LXC containers + 1 KVM VM running.

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

| Device           | Hostname | Specs                                    | IP Address     | Role                              |
|------------------|----------|------------------------------------------|----------------|-----------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 32GB RAM, RX 6700 XT 12GB | 192.168.10.5   | Hypervisor                        |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                 | 192.168.10.1   | Router, Firewall                  |
| Managed Switch   | —        | TP-Link TL-SG108E                        | 192.168.1.20   | Layer 2, VLANs                    |
| NAS              | Kinmoon  | UGREEN DXP2800, 3.6TB WD Purple          | 192.168.10.15  | Backups (SMB target: kinmoon-nfs) |
| DNS Server       | —        | Raspberry Pi 4                           | 192.168.30.10  | Pi-hole (~489K domains blocked)   |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB         | 192.168.20.101 | **Gaming only — never homelab**   |
| WiFi AP          | —        | TP-Link EAP610                           | TBD            | Purchased, pending setup          |

### GPU Notes (Kuromoon)

- RX 6700 XT passed through to VM 400 (ollama-gpu) for Ollama/ROCm AI inference
- IOMMU Group 18 (GPU), Group 19 (audio) — both passed through
- Idle baseline: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan
- HSA_OVERRIDE_GFX_VERSION=10.3.0 set via systemd override for gfx1031 compatibility

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

## 📦 Infrastructure Inventory (Proxmox)

> **Note:** CTID 201–302 are LXC containers. VMID 400 is a KVM virtual machine with PCIe GPU passthrough — it is NOT an LXC container.

| ID   | Type | Name                    | IP             | Subdomain                     | Autostart | Status     |
|------|------|-------------------------|----------------|-------------------------------|-----------|------------|
| 201  | LXC  | nginx-proxy-manager     | 192.168.30.201 | —                             | ✅         | ✅ Running |
| 202  | LXC  | monitoring-prometheus   | 192.168.30.202 | —                             | ✅         | ✅ Running |
| 203  | LXC  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | ✅         | ✅ Running |
| 204  | LXC  | monitoring-loki         | 192.168.30.204 | —                             | ✅         | ✅ Running |
| 205  | LXC  | monitoring-alertmanager | 192.168.30.205 | —                             | ✅         | ✅ Running |
| 206  | LXC  | monitoring-uptime       | 192.168.30.206 | —                             | ✅         | ✅ Running |
| 207  | LXC  | network-ddns            | 192.168.30.207 | —                             | ✅         | ✅ Running |
| 208  | LXC  | dashboard-homepage      | 192.168.30.208 | home.najhin-gaming.com        | ✅         | ✅ Running |
| 211  | LXC  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | ✅         | ✅ Running |
| 213  | LXC  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | ✅         | ✅ Running |
| 214  | LXC  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | ✅         | ✅ Running |
| 220  | LXC  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅         | ✅ Running |
| 300  | LXC  | gaming-panel            | 192.168.30.210 | —                             | ✅         | ✅ Running |
| 302  | LXC  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | ✅         | ✅ Running |
| 400  | VM   | ollama-gpu              | 192.168.30.221 | ollama.najhin-gaming.com      | ✅         | ✅ Running |

**Total: 14 LXC containers + 1 KVM VM (VM 400 ollama-gpu), all on VLAN 30, all autostart enabled**

### Nextcloud Hub (CT 220)

- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Version:** 33.0.2
- **Data location:** Currently on /var/lib/nextcloud (root disk)
- **Planned migration:** Move data directory to /mnt/data-storage (7.3TB HDD) during future phase
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (2d:00.0) + audio (2d:00.1) passed through
- **OS:** Ubuntu 22.04
- **SSH:** `ssh muzakkir@192.168.30.221` — use `muzakkir` user, not root
- **Ollama models:** qwen3:14b (primary, 9.3GB), llama3.2:latest (secondary, 3B)
- **Open WebUI:** Docker container, connects via 172.17.0.1:11434

### Gaming Servers (Docker on CT 302)

- **Terraria** — terraria.najhin-gaming.com
- **Minecraft** — mc.najhin-gaming.com
- **Windrose** — Deployed at /opt/windrose, 4 max players, Medium difficulty, invite code NAJHINWINDROSE, **consumes ~9GB RAM when running**

---

## 📚 Obsidian Knowledge Base (Phase 22)

### Vault Configuration

- **Path:** C:\\Users\\muzak\\Nextcloud\\Obsidian\\second-brain (auto-syncs to CT 220)
- **Platform:** Obsidian on Minimoon (Gaming PC)
- **Sync:** Via Nextcloud — changes sync automatically between local vault and cloud

### Plugins Installed

1. **Dataview** — Database queries and dashboards
2. **Tasks** — Task management and tracking
3. **Templater** — Note templates and automation
4. **Calendar** — Calendar view integration
5. **Excalidraw** — Diagrams and sketches
6. **Kanban** — Kanban boards for project management

### Folder Structure (Phase 22.1 Complete)

```
second-brain/
├── 00-inbox/          # Quick capture, unsorted notes
├── 01-homelab/        # Infrastructure documentation
├── 02-career/         # Career transition materials
├── 03-knowledge/      # Learning notes, references
├── 04-personal/       # Personal management, subscriptions, finance
│   ├── subscriptions/ # Individual notes per subscription
│   ├── finance/       # Budget tracking, grocery lists
│   ├── grocery/       # Shopping lists and meal planning
│   └── health/        # Blood pressure tracking, food logs
├── 05-templates/      # Note templates
├── 06-archive/        # Completed or outdated notes
├── 07-daily/          # Daily notes from Phase 22.2
├── 08-projects/       # Active projects and goals
├── 09-meetings/       # Meeting notes and agendas
└── 10-reference/      # Quick reference materials
```

### Key Features (Phase 22.1 + 22.2)

- **Subscription tracker:** 6 subscriptions tracked in 04-personal/subscriptions/
- **Dashboard:** Dataview queries at 04-personal/dashboard showing costs and totals
- **Daily notes:** Auto-created at midnight via n8n (Phase 22.2)
- **Morning briefing:** 7am Telegram summary via MERLIN
- **/daily command:** Gilgamesh creates immediate daily notes

### Homepage Dashboard Design (Planned — Phases 22.15/22.16)

4 separate tabs:
- **Homelab tab** — container status, metrics, alerts
- **Health tab** — BP tracking, food log, exercise
- **Finance tab** — budget overview, subscription costs, weekly spending
- **Settings tab** — user-adjustable budget percentages

### Budget Configuration (Planned)

- Monthly budget tracked as percentage of income
- Default allocation: 15% groceries (~RM 450/month, ~RM 112.50/week)
- All percentages user-adjustable in Settings tab
- Grocery lists are budget-aware: tight month triggers pasar mode with cheaper alternatives and price comparisons (pasar vs supermarket)
- Shopping checklist is Telegram-only — not on the homepage dashboard
- Phases 22.15 (Price Database) and 22.16 (Homepage Settings Tab) cover implementation

---

## 🔮 Gilgamesh Evolution Roadmap

### PRIMARY GOAL: Replace claude.ai with Gilgamesh by August 2026 (16 weeks)

**Current Cost:** $30-40/month Claude Pro + API usage
**Target Cost:** $5-10/month (local LLM + occasional API)

### 4-Phase Plan

#### Phase 1: Foundation (Weeks 1-4)

- **Phase 38** — Ollama + ROCm on Kuromoon RX 6700 XT ✅ COMPLETE
- **Phase 22** — Obsidian Knowledge Base ✅ COMPLETE

#### Phase 2: Intelligence (Weeks 5-8)

- **Phase 41** — Hybrid Routing (Ollama + Haiku + Sonnet) ✅ COMPLETE
- **Phase 7E** — Extended Memory (20+ message conversations via RAG)

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

| Component  | Current          | Target          | Savings           |
|------------|------------------|-----------------|-------------------|
| Claude Pro | $20/month        | $0/month        | $240/year         |
| API Usage  | $10-20/month     | $5-10/month     | $60-120/year      |
| **Total**  | **$30-40/month** | **$5-10/month** | **$300-360/year** |

### Backup Plan

Keep Claude Pro if quality insufficient, still save $10-15/month with hybrid approach.

---

## 🎴 Fate Grand Order Agent Ecosystem

Theme: Homelab agents named after Fate/Grand Order servants. Final roster locked April 25, 2026.

### Active Agents

| Servant          | Class  | Role                                                                 | Platform                      | Status              |
|------------------|--------|----------------------------------------------------------------------|-------------------------------|---------------------|
| Gilgamesh 👑     | Archer | Personal AI Assistant                                                | Telegram (@JhinGilgamesh_bot) | ✅ Active            |
| Da Vinci 🎨      | Caster | Chief Intelligence Officer (Stage 1 doc pipeline active; Stage 2 RAG planned) | n8n/Nextcloud        | ⚡ Partial — Stage 1 active |
| Midas 💰         | Caster | CFO — Cost Tracking & Optimization                                   | n8n                           | ✅ Active            |
| MERLIN 🔮        | Caster | Reminders & Scheduler                                                | n8n                           | ✅ Active            |

### Final 9-Agent Roster (Locked April 25, 2026)

| Servant              | Class    | Role                                   | Platform      | Build Order | Status    |
|----------------------|----------|----------------------------------------|---------------|-------------|-----------|
| Gilgamesh 👑         | Archer   | Personal AI Assistant                  | Telegram      | —           | ✅ Active  |
| Da Vinci 🎨          | Caster   | Chief Intelligence Officer             | n8n/Nextcloud | —           | ⚡ Partial |
| Midas 💰             | Caster   | CFO — Cost Tracking & Optimization     | n8n           | 1st         | ✅ Active  |
| MERLIN 🔮            | Caster   | Reminders & Scheduler                  | n8n           | 2nd         | ✅ Active  |
| Guardian 🛡          | —        | Security Monitoring                    | n8n           | 3rd         | 📋 Planned |
| Mash Kyrielight 🛡️  | Shielder | Gaming Server Manager + Wellbeing      | Discord       | 4th         | 📋 Planned |
| Nexus 🔗             | —        | Cross-platform Automation              | n8n           | 5th         | 📋 Planned |
| Oracle 🔮            | —        | Predictive Intelligence (internal + external) | n8n    | 6th         | 📋 Planned |
| EMIYA 🏹             | Archer   | CTO — Infrastructure Engineer          | n8n           | TBD         | 📋 Planned |
| Sherlock Holmes 🔍   | Ruler    | Web Scraper & Research Agent           | n8n           | TBD         | 📋 Planned |

**Notes:**
- Scribe absorbed into Da Vinci (documentation is Da Vinci's domain)
- Oracle absorbs Zhuge Liang
- Sherlock Holmes added April 26 as dedicated web scraper — web scraping removed from EMIYA scope
- **Build order is firm: Midas complete, MERLIN complete, then Guardian, Mash, Nexus, Oracle**

### Tier 2 Agents (Planned — Build After Core Roster)

| Servant | Role          | Notes                            |
|---------|---------------|----------------------------------|
| Chiron  | Career Coach  | Career transition support        |
| Medea   | QA Engineer   | Quality assurance for all agents |
| Waver   | Project Manager | Sprint planning, task tracking |

### Da Vinci — Chief Intelligence Officer

**Role scope:**
- Documentation: maintains AI-CONTEXT.md, changelog.md, troubleshoot.md via /update and /sync-docs
- Obsidian writes: session summaries written to vault via Nextcloud WebDAV
- RAG retrieval (Stage 2): queries Qdrant vector database for knowledge recall across all agents
- Observability logs: all agents log activity to Da Vinci
- Security intelligence (Stage 3, planned): threat feed monitoring
- Tech news morning brief (Stage 3, planned): homelab-relevant AI and infrastructure news

**Stage 2 stack:** Qdrant + nomic-embed-text + n8n native RAG nodes on VM 400. Folder `04-personal/` excluded from RAG indexing.

**Pronouns:** she/her

**Technical notes:**
- Direct node references required: use `$('Extract Response').first().json.updatedDoc` instead of `$json` after Merge node
- max_tokens set to 32000 to prevent AI-CONTEXT.md being replaced with hallucinated content
- Grounding fix pending: add step to fetch current AI-CONTEXT.md from Nextcloud before Claude API call

### Midas — CFO (Cost Tracking & Optimization)

**Two workflows:**
1. **CFO Report** — `/midas` command shows token usage, costs by model, savings from Ollama
2. **Daily Brief** — 9am scheduled summary to Telegram

**Features:**
- Tracks Gilgamesh costs (Sonnet, Haiku, Ollama)
- USD to MYR conversion (hardcoded rate: 4.7)
- Ollama savings calculated at Haiku equivalent rate
- $10 monthly API spend limit monitoring
- command_type tracking for detailed cost breakdown

**Why Midas:**
- Midas touch turns everything to gold → cost optimization
- King of wealth → perfect CFO role

### MERLIN — Reminders & Scheduler

**Why Merlin:**
- Clairvoyance (sees the future → knows what you'll forget)
- Garden of Avalon (creates safe space → maintenance windows)
- Illusion magic (reminds you gently, not annoyingly)

**What MERLIN Does:**
- SSL certificate renewal reminders (expires in X days)
- Backup restore test reminders (overdue by X days)
- Loki/Prometheus memory limit warnings (OOM in X days)
- Scheduled maintenance windows
- Proactive infrastructure health checks

**Current Implementation:**
- Daily 8am checks: SSL expiry (hardcoded July 14, 2026), Backup restore test (baseline 2026-01-01), Prometheus memory usage, Vault seal status
- Cloudflare API integration pending (token truncated issue in Vault kv/cloudflare)

### EMIYA — CTO / Infrastructure Engineer

**10 core features:**
1. Proxmox VM and LXC lifecycle management (create, start, stop, delete — approval-gated)
2. Alert translation (Alertmanager alerts converted to plain English via Telegram)
3. Container updates (Docker image updates, apt upgrades — approval-gated)
4. Proactive monitoring (anomaly detection, trend alerts before things break)
5. Security management (threat detection, firewall rule suggestions)
6. Performance optimization (identify bottlenecks, suggest resource reallocation)
7. Backup verification (confirm backup integrity, alert on failures)
8. Change management (track all infrastructure changes, generate audit log)
9. Service health reporting (daily infrastructure summary to Telegram)
10. Pre-flight checks via Da Vinci before executing any changes

**Design rule:** EMIYA proposes → Muzakkir approves → EMIYA executes. No autonomous destructive actions.

**Deployment:** 8 sub-phases (24.1–24.8), estimated 24–32 hours total. Phases 24.1–24.5 are critical path.

### Sherlock Holmes — Web Scraper & Research Agent

**Role:** Dedicated web scraper replacing web scraping from EMIYA scope.
**Sources:** RSS feeds, Reddit, news sites, homelab tool trackers.
**Output:** Telegram alerts for relevant findings, Obsidian storage for research notes.

### Mash — Gaming Discord Bot

**What Mash Does:**
- Discord bot commands: !start, !stop, !status
- Announces player joins/leaves
- Scheduled game night reminders
- Auto-shutdown idle servers
- Game update notifications
- Personality: "Senpai, PlayerX just joined Windrose!"

**Implementation:** Phases 59-64 (Gaming Platform Pipeline)

### Design Principles

- Gilgamesh = Telegram-only (homelab admin)
- Mash = Discord-only (gaming with friends)
- All agents log activity to Da Vinci
- All agents use Fate/GO servant theming for consistency
- Build Midas first — no agent should spend tokens without cost visibility

---

## 🤖 Gilgamesh AI Agent (Phase 7C/7D/7D-Menu/15/41)

### Architecture

```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Route Check
                                                    ↓
                               ┌────────────────────┼──────────────────┐
                               ▼                    ▼                  ▼
                         Ollama qwen3:14b      Haiku 4.5           Sonnet 4
                         (simple, local)    (Ollama fallback)    (complex queries)
                               └────────────────────┼──────────────────┘
                                                    ▼
                                          Memory (n8n Data Tables)
                                                    ↓
                                          /update → Da Vinci → Nextcloud → GitHub
                                          /sync-docs → Full Doc Pipeline
```

### Smart Routing (Phase 41 — COMPLETE)

- **Ollama qwen3:14b** — Primary route for simple queries (local, free, fast)
- **Haiku 4.5** — Fallback if Ollama is down or unavailable
- **Sonnet 4** — Complex queries: triggered by complexity keywords or messages over 50 words
- Routing logic runs in a Route Check If node before calling any model

### Features

- **Conversation memory:** Last 20 messages stored in n8n Data Tables
- **Smart routing:** Ollama (local, primary) → Haiku (fallback) → Sonnet (complex)
- **Web search:** Real-time information via Claude's web_search tool
- **Cost tracking:** Token usage logged to gilgamesh_costs table with command_type; cost_usd calculated from token rates
- **Inline keyboard menu:** Full menu system with all submenus working
- **Context sync:** /update command pushes session summaries to AI-CONTEXT.md via GitHub
- **Documentation pipeline:** /sync-docs triggers full documentation regeneration (7 files)
- **Slash commands:** 10 commands for direct actions

### Slash Commands

| Command    | Purpose           | Description                                   |
|------------|-------------------|-----------------------------------------------|
| /help      | Quick reference   | Shows all available commands and menu options |
| /clear     | Memory reset      | Clears conversation memory (last 20 messages) |
| /memory    | View history      | Shows recent conversation messages            |
| /cost      | Usage tracking    | Displays token usage and estimated costs      |
| /alerts    | System status     | Shows active Alertmanager alerts              |
| /backup    | Backup status     | Last backup times for all containers          |
| /update    | Session summary   | Merges session into docs (3 files)            |
| /sync-docs | Full regeneration | Regenerates all documentation (7 files)       |
| /midas     | CFO report        | Cost analysis and savings summary             |
| /daily     | Daily notes       | Creates Obsidian daily note (defaults today)  |

### Technical Details

| Component     | Value                                                              |
|---------------|--------------------------------------------------------------------|
| Telegram Bot  | @JhinGilgamesh_bot                                                 |
| Chat ID       | 518832696                                                          |
| Ollama Model  | qwen3:14b (primary local model, 9.3GB, 12GB VRAM, VM 400)         |
| Haiku Model   | claude-haiku-4-5-20251001 (Ollama fallback)                        |
| Sonnet Model  | claude-sonnet-4-20250514 (complex queries)                         |
| n8n Container | CT 211, 192.168.30.211                                             |
| Proxmox API   | root@pam!gilgamesh token                                           |
| Proxmox node  | muzakkir (not kuromoon)                                            |

### Cost Tracking Note

- cost_usd calculated from token rates: Sonnet $3/$15 per 1M input/output tokens, Haiku $0.80/$1 per 1M tokens
- Ollama tokens captured: data.input_tokens + data.output_tokens from Call Ollama node response
- Ollama queries cost $0 (local inference)
- command_type derived from Telegram message text directly

### n8n Workflows (Count: 12)

| Workflow                            | Purpose                                | Nodes | Trigger  |
|-------------------------------------|----------------------------------------|-------|----------|
| Telegram Agent                      | Main bot, menu, commands, hybrid routing | 15+  | Telegram |
| Documentation Pipeline — Update     | Session summary → 3 files              | 7     | Webhook  |
| Documentation Pipeline — Sync Docs  | Full doc regeneration → 7 files        | 7     | Webhook  |
| Da Vinci Documentation Pipeline     | Raw staging summaries → formatted docs | 11    | Webhook  |
| Midas — CFO Report                  | /midas command cost analysis           | 6     | Webhook  |
| Midas — Daily Brief                 | 9am scheduled cost summary             | 4     | Schedule |
| MERLIN — Reminders                  | 8am daily infrastructure checks        | 8     | Schedule |
| Daily Note Creator                  | Midnight Obsidian daily notes          | 4     | Schedule |
| Morning Briefing                    | 7am daily summary to Telegram          | 5     | Schedule |
| Update Nextcloud File               | Legacy (unpublished)                   | 5     | Webhook  |
| Push to GitHub                      | Legacy (unpublished)                   | 4     | Webhook  |

> Note: Hybrid routing (Phase 41) is integrated into the Telegram Agent workflow — it is not a separate workflow.

### Inline Keyboard Menu Status

| Menu Item         | Status     |
|-------------------|------------|
| Main Menu         | ✅ Working  |
| Homelab → Status  | ✅ Working  |
| Homelab → Metrics | ✅ Working  |
| Homelab → Temps   | ✅ Working  |
| Homelab → Storage | ✅ Working  |
| Gaming submenu    | ✅ Working  |
| Gilgamesh submenu | ✅ Working  |
| Tools submenu     | ✅ Working  |
| Help              | ✅ Working  |

### SSH & API Access

- **SSH to Kuromoon:** CT 211 → 192.168.10.5:22 (pfSense rule added)
- **SSH to VM 400:** `ssh muzakkir@192.168.30.221` — use muzakkir user, not root
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
Session Summary → /update → Da Vinci workflow → Claude API → AI-CONTEXT.md + changelog.md + troubleshoot.md
                                                                     ↓
                                                              Nextcloud + GitHub
```

#### Da Vinci Documentation Pipeline (Phase 16.3)

```
Raw summaries → AI-CONTEXT-staging.md (rolling append)
                         ↓
               Da Vinci n8n workflow → Claude API (max_tokens: 32000)
                         ↓
               Formatted AI-CONTEXT.md → Nextcloud + GitHub
```

### Commands

| Command    | Purpose                                   | Files Updated |
|------------|-------------------------------------------|---------------|
| /update    | Every session — merge summary into docs   | 3 files       |
| /sync-docs | Deployment sessions — regenerate all docs | 7 files       |

### Pipeline Components

| Component    | Details                                         |
|--------------|-------------------------------------------------|
| Webhooks     | doc-update, doc-sync, da-vinci                  |
| Telegram     | Routes /update and /sync-docs                   |
| Claude API   | Documentation merging intelligence              |
| Nextcloud    | File storage via admin user                     |
| GitHub       | Version control via API push                    |
| Da Vinci     | Async processing (she/her pronouns)             |
| Staging file | AI-CONTEXT-staging.md in Nextcloud (rolling append) |

### File Coverage (sync-docs)

- AI-CONTEXT.md, changelog.md, troubleshoot.md (from /update)
- README.md, roadmap.md, current-state.md, service-catalog.md

### Da Vinci Technical Notes

- **Direct node references:** Use `$('Extract Response').first().json.updatedDoc` — do NOT rely on `$json` after Merge node
- **max_tokens:** 32000 (prevents AI-CONTEXT.md being replaced with hallucinated content)
- **Grounding fix pending:** Add step to fetch current AI-CONTEXT.md from Nextcloud before Claude API call

---

## 🤖 Bots & Integrations

| Bot                | Platform        | Source              | Status    | Purpose                                                          |
|--------------------|-----------------|---------------------|-----------|------------------------------------------------------------------|
| @JhinGilgamesh_bot | Telegram        | n8n CT 211          | ✅ Active  | Personal AI agent — chat, homelab control, /update, /sync-docs   |
| Midas              | Telegram        | n8n CT 211          | ✅ Active  | CFO cost tracking — /midas reports, 9am daily briefs            |
| MERLIN             | Telegram        | n8n CT 211          | ✅ Active  | Reminders — 8am daily infrastructure checks                      |
| Daily Note Creator | Nextcloud WebDAV| n8n CT 211          | ✅ Active  | Midnight daily note creation in Obsidian vault                  |
| Morning Briefing   | Telegram        | n8n CT 211          | ✅ Active  | 7am daily summary to Telegram                                   |
| Homelab Alerts     | Telegram        | Alertmanager CT 205 | ✅ Active  | Critical alerts (host down, high CPU/memory/disk)                |
| Homelab Alerts     | Discord webhook | Alertmanager CT 205 | ✅ Active  | Warning-level alerts to #alerts channel                          |

**Planned:** Migrate Alertmanager alerts to route through n8n first (central hub). Game server notifications to Discord via n8n.

---

## 💾 Backup Strategy (Phase 7A)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                                  | Retention                    |
|------------------|----------|--------------------------|---------------------------------------------|------------------------------|
| Small Containers | 02:00    | kinmoon-smb (NAS)        | 201, 202, 203, 204, 205, 206, 207, 214, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30    | data-storage (Local HDD) | 220, 302                                    | 7 daily, 4 weekly, 2 monthly |

### Storage

| Name         | Type       | Protocol | Capacity | Purpose                 |
|--------------|------------|----------|----------|-------------------------|
| kinmoon-smb  | NAS        | SMB/CIFS | 3.6 TB   | Small container backups |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  |

---

## 🔒 Security Architecture

| Layer           | Implementation                                                                                                                                                                                       |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                                                                                                                                        |
| Segmentation    | 5 VLANs with enforced firewall rules                                                                                                                                                                 |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                                                                                                                                          |
| VPN             | Tailscale (subnet router on pfSense, primary access)                                                                                                                                                 |
| External Auth   | Cloudflare Access (Email OTP, muzakkir.kholil06@gmail.com only) for Grafana, n8n, Vault, Vaultwarden, Ollama, Homepage, Nextcloud (7 apps total)                                                     |
| External Access | Cloudflare Tunnel for all external services                                                                                                                                                          |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                                                                                                                                        |
| Backup          | Automated daily backups with 7/4/2 retention                                                                                                                                                         |
| Secrets         | HashiCorp Vault (CT 213, vault.najhin-gaming.com). KV engine at kv/. Secrets: kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager, kv/github, kv/nextcloud, kv/n8n, kv/pihole (8 paths total)  |
| Passwords       | Vaultwarden (CT 214, passwords.najhin-gaming.com). Personal password manager with Bitwarden clients. API bypassed in Cloudflare Access (/api/, /identity/ paths).                                    |

### Vault Notes

- **Reseals on reboot:** Manual unseal required: `pct exec 213 -- vault operator unseal`
- **All credentials stored:** kv/nextcloud, kv/github, VM 400, Proxmox, CT 302 passwords

---

## 📋 Project Phase History

| Phase   | Title                                                            | Status          | Completed    |
|---------|------------------------------------------------------------------|-----------------|--------------|
| 1       | Proxmox VE Installation                                          | ✅ Complete      | Jan 2026     |
| 2       | pfSense Firewall & VLAN Setup                                    | ✅ Complete      | Jan 2026     |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)                    | ✅ Complete      | Jan 2026     |
| 4       | External Access & SSL                                            | ✅ Complete      | Feb 2026     |
| 5       | Monitoring Stack                                                 | ✅ Complete      | Feb 2026     |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)               | ✅ Complete      | Feb 2026     |
| 6E      | Homepage Dashboard                                               | ✅ Complete      | Mar 2026     |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening        | ✅ Complete      | Mar 9, 2026  |
| 7       | Nextcloud Deployment                                             | ✅ Complete      | Mar 8, 2026  |
| 7A      | Backup Strategy                                                  | ✅ Complete      | Mar 13, 2026 |
| 7B      | n8n Workflow Automation                                          | ✅ Complete      | Apr 2, 2026  |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration                      | ✅ Complete      | Apr 2, 2026  |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)             | ✅ Complete      | Apr 6, 2026  |
| 7D-Sec  | Cloudflare Access for n8n                                        | ✅ Complete      | Apr 7, 2026  |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                                   | ✅ Complete      | Apr 24, 2026 |
| 9       | NAS Deployment (Kinmoon)                                         | ✅ Complete      | Mar 3, 2026  |
| 13      | HashiCorp Vault — Secrets Manager                                | ✅ Complete      | Apr 18, 2026 |
| 14      | Secrets Management & Integration                                 | ✅ Complete      | Apr 24, 2026 |
| 15      | Gilgamesh Additional Slash Commands                              | ✅ Complete      | Apr 24, 2026 |
| 16.1    | Documentation Pipeline — Update Workflow                         | ✅ Complete      | Apr 19, 2026 |
| 16.2    | Documentation Pipeline — Sync Docs Workflow                      | ✅ Complete      | Apr 19, 2026 |
| 16.3    | Da Vinci Documentation Pipeline                                  | ✅ Complete      | Apr 25, 2026 |
| 22      | Obsidian Knowledge Base                                          | ✅ Complete      | Apr 24, 2026 |
| 22.1    | Obsidian Vault Structure Expansion                               | ✅ Complete      | Apr 27, 2026 |
| 22.2    | Obsidian Daily Notes + Morning Briefing                         | ✅ Complete      | Apr 27, 2026 |
| 22.15   | Price Database Tracking                                          | 📋 Planned       | —            |
| 22.16   | Homepage Settings Tab                                            | 📋 Planned       | —            |
| 23      | Vaultwarden + Secrets Audit & Cleanup                            | ✅ Complete      | Apr 18, 2026 |
| 24.1    | EMIYA Foundation (Proxmox API + SSH + approval gate)             | 📋 Planned       | —            |
| 24.2    | EMIYA Alert Translation (Alertmanager to plain English)          | 📋 Planned       | —            |
| 24.3    | EMIYA Container Updates (Docker + apt + Proxmox, approval-gated) | 📋 Planned      | —            |
| 24.4    | EMIYA Proactive Monitoring (anomaly detection)                   | 📋 Planned       | —            |
| 24.5    | EMIYA Security Management (threat detection)                     | 📋 Planned       | —            |
| 24.6    | EMIYA Performance Optimization                                   | 📋 Planned       | —            |
| 24.7    | EMIYA Backup Verification                                        | 📋 Planned       | —            |
| 24.8    | EMIYA Change Management                                          | 📋 Planned       | —            |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                             | ✅ Complete      | Apr 24, 2026 |
| 39      | Open WebUI                                                       | ✅ Complete      | Apr 24, 2026 |
| 41      | Gilgamesh + Ollama Hybrid Routing                                | ✅ Complete      | Apr 24, 2026 |
| 58      | Windrose Server Deployment                                       | ✅ Complete      | Apr 19, 2026 |
| Midas   | Midas CFO Agent                                                  | ✅ Complete      | Apr 27, 2026 |
| MERLIN  | MERLIN Reminders Agent                                           | ✅ Complete      | Apr 27, 2026 |

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

| Issue                                | Resolution                                                       |
|--------------------------------------|------------------------------------------------------------------|
| n8n Data Tables schema caching bug   | Delete and recreate the table entirely                           |
| Claude API 404 model not found       | Use exact model ID: `claude-haiku-4-5-20251001`                  |
| Web search partial response capture  | Add Extract Response Code node, filter by `type === "text"`      |
| Expression scoping issues            | Use Code node with `this.helpers.httpRequest`, reference `$json` |
| NFS fails for LXC backups            | UID namespace mapping (100000-165536); use SMB instead           |
| pfSense rule not working             | Place new rules BEFORE RFC1918 block rules                       |
| n8n GitHub credential 401            | User field (`muzakkir97`) must be populated                      |
| Telegram single webhook constraint   | All update types (message + callback) must use one trigger       |
| Answer Callback node loses data      | Bypass it; handle callback acknowledgment separately             |
| Da Vinci grounding bug               | Use direct node references; increase max_tokens to 32000         |
| Open WebUI JSON parse error          | Add `proxy_buffering off` to NPM advanced config for streaming   |
| input.first()/input.last() unreliable | After Merge node, use direct node references like `$('Extract Response').first().json` |
| Ollama tokens always 0               | Call Ollama node already maps prompt_eval_count → input_tokens — read data.input_tokens not data.prompt_eval_count |
| Loki not being scraped               | Add scrape job to Prometheus config for CT 204                  |

### HashiCorp Vault (Phase 13)

| Issue                            | Resolution                                                      |
|----------------------------------|-----------------------------------------------------------------|
| apt fails in new LXC             | Run `echo "nameserver 192.168.30.10" > /etc/resolv.conf` first  |
| HashiCorp repo setup fails       | Install `lsb-release` before adding repo                        |
| Wrong repo architecture/codename | Hardcode `amd64` and `bookworm` in repo entry                   |
| Vault fails to start in LXC      | Add `disable_mlock = true` to vault.hcl                         |
| Token auth issues in shell       | Use `vault login <token>` not `export VAULT_TOKEN=`             |
| Vault sealed after reboot        | `pct exec 213 -- vault operator unseal`                         |
| Cloudflare token truncated       | kv/cloudflare only stores 7 chars — re-store full token         |

### Obsidian & Knowledge Management

| Issue                       | Resolution                                                          |
|-----------------------------|---------------------------------------------------------------------|
| Nextcloud quota exceeded    | Resize CT 220 from 20GB to 100GB with `pct resize 220 rootfs +80G` |
| Dataview queries no results | Queries must target folder with individual notes, not same file     |

### Cloudflare Access & Security

| Issue                         | Resolution                                                       |
|-------------------------------|------------------------------------------------------------------|
| Bitwarden HTTP 525 from phone | Cloudflare Access blocking API — bypass /api/, /identity/ paths  |
| n8n Access OTP not triggering | Cached session — test in incognito mode                          |

### Ollama & GPU

| Issue                                    | Resolution                                                              |
|------------------------------------------|-------------------------------------------------------------------------|
| ROCm full SDK ran out of disk space      | Use minimal runtime only; extend LVM with `lvextend -l +100%FREE`       |
| amdgpu still bound after blacklist       | Add `softdep amdgpu pre: vfio-pci` to vfio.conf, rebuild initramfs     |
| Ollama running on CPU not GPU            | Add `HSA_OVERRIDE_GFX_VERSION=10.3.0` and render/video group membership |
| Open WebUI no models available           | Set `OLLAMA_HOST=0.0.0.0`; point container to 172.17.0.1:11434          |
| SSH to VM 400 permission denied as root  | Use `muzakkir` user with sudo — root SSH is disabled                    |

### Monitoring & Alerts

| Issue                       | Resolution                                                |
|-----------------------------|---------------------------------------------------------|
| High memory alerts at 80%  | Raise threshold to 85% (Windrose baseline ~9GB RAM)      |
| Prometheus HighMemoryUsage  | Updated rule from 80% to 85% in alerting configuration   |

---

## ❓ Pending Tasks

### Immediate

| Task                                                                                        | Priority |
|---------------------------------------------------------------------------------------------|----------|
| Schedule backup restore test (CT 207 recommended), update lastTestDate in MERLIN            | High     |
| Fix Cloudflare API token in Vault (get full token from dashboard, re-store)                | High     |
| Activate MERLIN workflow (toggle on)                                                       | High     |
| Monitor Windrose RAM usage — stop Docker container when not playing                         | High     |
| Export homelab diagram from Claude Design (PNG for GitHub/LinkedIn)                         | High     |
| Update subscription costs in Obsidian when known (YouTube Premium, Cloudflare Domain, TIME) | Medium   |

### Infrastructure

| Task                                                                         | Priority |
|------------------------------------------------------------------------------|----------|
| Address local-lvm thin pool overprovisioning (during infrastructure cleanup) | Medium   |
| Plan Phase 24.1 (EMIYA Foundation) in dedicated session                      | Medium   |
| Create muzakkir97/homelab-private repo                                       | Medium   |
| Set up Backblaze B2 account                                                  | Medium   |
| Phase 27 (Vault + n8n) enables n8n to fetch secrets directly                 | Medium   |

### Gilgamesh & Documentation

| Task                                                                                        | Priority |
|---------------------------------------------------------------------------------------------|----------|
| Build Monthly Infrastructure Audit cron workflow (assign new phase number — NOT 16.3)      | High     |
| Build Guardian security monitoring agent (next after MERLIN)                               | High     |
| Build Mash Discord bot (Phases 59-64)                                                       | High     |
| Document Sherlock Holmes agent design (sources, scraping targets, output, Obsidian integration) | Medium |
| /update redesign — file attachment via Telegram, push to GitHub + Nextcloud                 | Medium   |
| Homepage embedded Gilgamesh chat UI (web frontend, shared memory with Telegram)             | Medium   |
| Integrate Vault secrets into n8n Gilgamesh workflow (Phase 27)                              | Medium   |
| Build Da Vinci Stage 2 (RAG) alongside Phase 7E                                             | Medium   |

### Infrastructure (Network & Services)

| Task                                                               | Priority |
|--------------------------------------------------------------------|----------|
| Migrate Alertmanager alerts through n8n (central notification hub) | Medium   |
| Switch management IP (192.168.1.20 → 192.168.10.20)                | Medium   |
| Remove legacy LAN 192.168.1.0/24 (after switch migration)          | Medium   |
| WiFi Access Point setup (EAP610 purchased)                         | Medium   |
| Homepage security (Cloudflare Access)                              | Low      |
| Off-site backup (Backblaze B2)                                     | Low      |
| Nextcloud 2FA (TOTP)                                               | Low      |

### Deferred

| Task                                                                       | Priority |
|----------------------------------------------------------------------------|----------|
| Fedora dual-boot on Minimoon (kernel 6.12+ for RDNA 4)                     | Deferred |
| Old P400S build repurpose (Z270E + i7-7700K)                               | Deferred |
| Claude Project auto-sync — revisit if Anthropic releases Project Files API | Deferred |

---

## 📝 Session Log (Recent)

### April 27, 2026

Date: April 27, 2026
Phase: Midas CFO Agent, MERLIN Reminders, and Obsidian Daily Creator active. 14 LXC containers + 1 KVM VM running.

Topics Discussed

- 80% RAM alert — Windrose 8.9GB + VM 400 7.9GB — normal baseline, raised Prometheus threshold 80% to 85%
- Fixed Ollama token capture (read data.input_tokens not prompt_eval_count)
- Added command_type column to gilgamesh_costs
- Built Midas CFO Report workflow (/midas, webhook midas-report, 6 nodes)
- Built Midas Daily Brief (9am scheduled, 4 nodes)
- Fixed Loki not scraped by Prometheus — added scrape job to prometheus.yml
- Built MERLIN Reminders (8am daily: SSL Jul 14 2026, Proxmox memory 85%, Vault seal, backup restore test)
- Completed Obsidian 22.1: 10 folders, HOME note, _index files, subfolders finance/grocery/health/subscriptions
- Completed Obsidian 22.2: Daily Note Creator midnight, Morning Briefing 7am, /daily command
- Planned Phase 22.9: reminders + voice notes Claude API + notebook photo vision
- Planned Phase 22.11: Nextcloud Calendar CalDAV integration

Decisions Made

- Memory alert threshold: 80% → 85%
- Ollama tokens: read data.input_tokens / data.output_tokens (already mapped by Call Ollama)
- command_type: derived from Telegram Trigger message text directly
- Midas webhook: midas-report, USD to MYR: 4.7, spend limit: $10
- SSL check: hardcoded July 14 2026 expiry — revisit in MERLIN v2
- Backup restore test baseline: 2026-01-01 (never tested)
- MERLIN reads nodes directly by name ($('Check Prometheus Memory')) not merged items
- Midas v2: revisit in future session for missing design discussion
- Obsidian vault: 10 folders + HOME master index + 7 _index files
- Daily notes: midnight creation via n8n, 7am morning briefing, /daily command
- Phase 22.9 and 22.11 planned for future expansion

Changes to AI-CONTEXT.md

- n8n workflows: add Midas CFO Report (webhook midas-report), Midas Daily Brief (9am), MERLIN Reminders (8am), Daily Note Creator (midnight), Morning Briefing (7am) — total now 12 workflows
- gilgamesh_costs schema: command_type column added (Text)
- Prometheus: HighMemoryUsage threshold now 85%
- Key Lessons: add Loki scrape job fix, Ollama token field names fix
- Windrose: ~9GB RAM baseline — stop container when not playing
- Agent roster: Midas ✅ Active, MERLIN ✅ Active
- Obsidian vault structure updated — 10 folders, HOME note, _index files
- Phases 22.1 and 22.2 marked complete April 27 2026
- Daily Note Creator and Morning Briefing active
- Pending: Schedule backup restore test CT 207, fix Cloudflare Vault token

Errors & Resolutions

- Ollama tokens always 0: read data.input_tokens/output_tokens not prompt_eval_count/eval_count
- Aggregate Alerts Prometheus not found: use $('Check Prometheus Memory').first().json directly
- SSL checker API returns empty: use hardcoded expiry date instead
- Cloudflare Vault token truncated: stored as 7 chars only — needs full token re-stored
- MERLIN Aggregate Alerts error.includes not a function: error field is object not string

Action Items

- Schedule backup restore test on CT 207, update lastTestDate in MERLIN
- Fix Cloudflare API token in Vault (get full token from dashboard, re-store)
- Activate MERLIN workflow (toggle on in n8n)
- Next session: Phase 22.3 or Guardian agent

### April 26, 2026

Date: April 26, 2026
Phase: AI-CONTEXT.md Audit & Correction Sprint

Topics Discussed

Cross-referenced AI-CONTEXT.md against all recent session logs and conversation history.
Found 15 errors: 4 critical (wrong data), 8 significant (missing content), 3 minor.
Generated corrected AI-CONTEXT.md manually.

Decisions Made

Phase 41 confirmed complete April 24, 2026 (Hybrid Routing).
Gilgamesh routing is Ollama qwen3:14b first, Haiku fallback, Sonnet for complex.
qwen3:14b is the primary local model (upgraded from qwen2.5:14b April 24).
VM 400 is KVM VM with PCIe passthrough, not an LXC container.
Final 9-agent roster locked: Gilgamesh, Da Vinci, Midas, MERLIN, Guardian, Mash, Nexus, Oracle, EMIYA.
Sherlock Holmes added as 10th agent (dedicated web scraper, Ruler class).
Scribe absorbed into Da Vinci. Oracle absorbs Zhuge Liang.
Build order: Midas first, then MERLIN, Guardian, Mash, Nexus, Oracle.
Da Vinci role: Chief Intelligence Officer (not Knowledge Curator).
EMIYA role: CTO / Infrastructure Engineer with 10 core features.
Tier 2 agents planned: Chiron (Career), Medea (QA), Waver (PM).
New phases: 22.15, 22.16, 24.1 through 24.8.
Monthly Infrastructure Audit cron workflow to get new phase number (not 16.3 — already taken).
n8n workflow count corrected to 6 (Hybrid Routing integrated into Telegram Agent, not separate).
Obsidian homepage 4-tab design and budget config (15% groceries) documented.

Changes to AI-CONTEXT.md

Full replacement with corrected version — all 15 audit errors resolved.

Errors & Resolutions

15 documentation errors found and corrected — see audit summary from April 26 session.

Action Items

Push corrected AI-CONTEXT.md to GitHub via /sync-docs or manual upload.
Run /sync-docs to regenerate all docs from corrected base.
Begin Phase 22.1 (Vault Structure Expansion) in next session.

### April 25, 2026

Date: April 24-25, 2026
Phase: 16.3 — Da Vinci Documentation Pipeline (COMPLETE) + Architecture Planning

Topics Discussed

Built Phase 16.3 Da Vinci Documentation Pipeline (11-node n8n workflow).
Created AI-CONTEXT-staging.md in Nextcloud (rolling append staging file).
Fixed Nextcloud WebDAV username (admin, not muzakkir).
Full end-to-end test passed.
Final 9-agent Fate ecosystem designed and locked.
Da Vinci Stage 2 stack decided: Qdrant + nomic-embed-text + n8n native RAG on VM 400.
Universal agent design rules established.
Build order locked: Midas first.

Decisions Made

Da Vinci is an n8n workflow, not a separate LXC.
All agents route through Gilgamesh bot.
Staging file is rolling append — raw summaries preserved.
Da Vinci Stage 2 (RAG) planned alongside Phase 7E.
Nextcloud WebDAV username is admin throughout.
Download link uses Nextcloud web UI URL, not raw WebDAV (Cloudflare Access blocks raw WebDAV).
Scribe absorbed into Da Vinci. Oracle absorbs Zhuge Liang.
Build order: Midas first, then MERLIN, Guardian, Mash, Nexus, Oracle.

Errors & Resolutions

Nextcloud 401: Username was muzakkir, should be admin.
n8n HTTP Request JSON invalid: Moved Claude API call to Code node.
Write AI-CONTEXT empty body: Use explicit node reference $('Extract Response').first().json.updatedDoc.
Push to GitHub auth failed: Replaced HTTP Request node with Code node.
Cloudflare Access blocks raw WebDAV download link: Use Nextcloud web UI URL instead.
AI-CONTEXT.md overwritten by test run: Restored from local backup on Minimoon.

### April 24, 2026

Date: April 24, 2026
Phase: 38 — Ollama + ROCm Local LLM (COMPLETE) / 39 — Open WebUI (COMPLETE) / 41 — Hybrid Routing (COMPLETE)

Topics Discussed

Deployed Ollama with ROCm GPU acceleration on RX 6700 XT (VM 400 ollama-gpu).
Installed Ubuntu 22.04, ROCm 6.1.3 runtime, Ollama.
Pulled qwen2.5:14b and llama3.2:latest, then upgraded to qwen3:14b.
Deployed Open WebUI via Docker.
Fixed Open WebUI JSON parse error (proxy_buffering off in NPM).
Fixed SSH to VM 400 (muzakkir user, not root).
Updated Open WebUI to latest version.
Built Phase 41 hybrid routing in Telegram Agent workflow.

Decisions Made

VM 400 named ollama-gpu, static IP 192.168.30.221, VLAN 30. KVM VM with PCIe passthrough.
GPU passthrough: 2d:00.0 (RX 6700 XT) + 2d:00.1 (audio).
HSA_OVERRIDE_GFX_VERSION=10.3.0 set via systemd override for gfx1031.
OLLAMA_HOST=0.0.0.0 set so Docker container can reach Ollama API.
Open WebUI connects via Docker bridge 172.17.0.1:11434.
Primary model: qwen3:14b (upgraded from qwen2.5:14b). Secondary: llama3.2:latest.
Routing logic: complex keywords or over 50 words = Sonnet, otherwise Ollama, Haiku as fallback if Ollama down.
Hybrid routing integrated into Telegram Agent — not a separate workflow.

Errors & Resolutions

ROCm full SDK ran out of disk space: Use minimal runtime; extend LVM.
amdgpu still bound after blacklist: Added softdep amdgpu pre: vfio-pci.
Ollama running 100% CPU not GPU: Added HSA_OVERRIDE_GFX_VERSION=10.3.0 and group membership.
Open WebUI no models available: Set OLLAMA_HOST=0.0.0.0.
SSH to VM 400 permission denied: VM uses muzakkir user not root.
Open WebUI JSON parse error: proxy_buffering off needed in NPM advanced config for streaming.

---

*Last updated: April 27, 2026 — Update this file at the end of each session before pushing to GitHub*