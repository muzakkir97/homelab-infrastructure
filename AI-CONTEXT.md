# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** May 10, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. 17 LXC containers + 1 KVM VM planned. Hardware upgraded with 128GB DDR4 ordered. Pulse monitoring dashboard deployed on CT 208.

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

## 🏗️ 7-Layer Architecture (v2.0)

### Layer 1: Input (Data Collection)
- **Telegram** → Gilgamesh (primary interface)
- **Health sensors** → Samsung Health/Lepulse scale → Health Connect bridge
- **Voice memos** → Claude API transcription
- **Photos** → Llama 3.2 Vision analysis
- **Web content** → Firecrawl scraping

### Layer 2: Brain (Intelligence)
- **Primary:** Ollama qwen3:14b (local, RX 6700 XT)
- **Fallback:** Claude Haiku (API)
- **Complex:** Claude Sonnet (API)
- **Vision:** Llama 3.2 Vision 11B (VM 400)
- **Routing:** Smart complexity detection

### Layer 3: App (Services)
- **Firefly III** → Personal finance tracking (CT 221)
- **ntfy** → Universal notification hub (CT 222)
- **Langfuse** → LLM observability (CT 223)
- **Nextcloud** → File sync + WebDAV
- **Vault** → Secrets management

### Layer 4: Knowledge (Storage + Recall)
- **Obsidian** → Human-readable notes
- **Qdrant** → Vector search (RAG)
- **n8n Data Tables** → Fast agent queries
- **Proactive recall** → Context-aware knowledge injection

### Layer 5: Memory (Agent State)
- **Conversation history** → 20 messages (gilgamesh_memory)
- **Session modes** → health_logging, etc.
- **Goal tracking** → progress + blockers
- **Agent coordination** → shared state

### Layer 6: Observability (Monitoring)
- **Langfuse** → LLM performance tracking
- **Midas** → Cost analysis + optimization
- **Grafana** → Infrastructure metrics
- **Da Vinci** → Agent behavior logs

### Layer 7: Notification (Output)
- **ntfy** → Universal push notifications
- **Telegram** → Interactive responses
- **Obsidian writes** → Structured logging
- **Discord** → Gaming server management

---

## 🖥️ Hardware Inventory

| Device           | Hostname | Specs                                    | IP Address     | Role                              |
|------------------|----------|------------------------------------------|----------------|-----------------------------------|
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 128GB DDR4, RX 6700 XT 12GB | 192.168.10.5   | Hypervisor                        |
| pfSense Firewall | —        | AC8F Mini PC, Intel N100                 | 192.168.10.1   | Router, Firewall                  |
| Managed Switch   | —        | TP-Link TL-SG108E                        | 192.168.1.20   | Layer 2, VLANs                    |
| NAS              | Kinmoon  | UGREEN DXP2800, 5.4TB RAID 1            | 192.168.10.100 | Backups (RAID 1, ext4)           |
| DNS Server       | —        | Raspberry Pi 4                           | 192.168.30.10  | Pi-hole (~489K domains blocked)   |
| Gaming PC        | Minimoon | Ryzen 7 7800X3D, RX 9070 XT 16GB         | 192.168.20.101 | **Gaming only — never homelab**   |
| WiFi AP          | —        | TP-Link EAP610                           | TBD            | Purchased, pending setup          |

### GPU Notes (Kuromoon)

- RX 6700 XT passed through to VM 400 (ollama-gpu) for Ollama/ROCm AI inference
- PCIe addresses: GPU (0d:00.0), Audio (0d:00.1) — updated after motherboard change
- ASUS TUF B550M-E motherboard with AMD-V/SVM and IOMMU enabled
- Idle baseline: CPU 48.5°C, GPU edge 46°C, GPU 5W with zero-RPM fan
- HSA_OVERRIDE_GFX_VERSION=10.3.0 set via systemd override for gfx1031 compatibility

### RAM Upgrade (Ordered May 9, 2026)

- **Current:** 32GB DDR4 (2x16GB)
- **Ordered:** 4x32GB Corsair Vengeance LPX DDR4-3200 from Taobao
- **Future Total:** 128GB DDR4 (32GB per DIMM slot on ASUS TUF B550M-E)
- **Purpose:** VM/LXC density, large AI models, multiple workloads

### Health Sensors

- **Lepulse scale** → Fitdays app → Samsung Health → Health Connect bridge
- **Samsung Watch** → Samsung Health → Health Connect bridge
- **Data flow:** Sensors → Samsung Health → Health Connect webhook bridge → n8n → Obsidian + Data Tables

### Disk Health Monitoring

- **sda (WD Purple 8TB):** Running at 60°C — high but functional
- **sdb (Seagate 2TB):** 8 pending sectors detected — requires weekly monitoring

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

| ID  | Type | Name                    | IP             | Subdomain                     | Autostart | Status    |
|-----|------|-------------------------|----------------|-------------------------------|-----------|-----------|
| 201 | LXC  | nginx-proxy-manager     | 192.168.30.201 | —                             | ✅         | ✅ Running |
| 202 | LXC  | monitoring-prometheus   | 192.168.30.202 | —                             | ✅         | ✅ Running |
| 203 | LXC  | monitoring-grafana      | 192.168.30.203 | grafana.najhin-gaming.com     | ✅         | ✅ Running |
| 204 | LXC  | monitoring-loki         | 192.168.30.204 | —                             | ✅         | ✅ Running |
| 205 | LXC  | monitoring-alertmanager | 192.168.30.205 | —                             | ✅         | ✅ Running |
| 206 | LXC  | monitoring-uptime       | 192.168.30.206 | —                             | ✅         | ✅ Running |
| 207 | LXC  | network-ddns            | 192.168.30.207 | —                             | ✅         | ✅ Running |
| 208 | LXC  | dashboard-pulse         | 192.168.30.208 | home.najhin-gaming.com        | ✅         | ✅ Running |
| 211 | LXC  | automation-n8n          | 192.168.30.211 | n8n.najhin-gaming.com         | ✅         | ✅ Running |
| 213 | LXC  | vault                   | 192.168.30.213 | vault.najhin-gaming.com       | ✅         | ✅ Running |
| 214 | LXC  | password-vaultwarden    | 192.168.30.214 | passwords.najhin-gaming.com   | ✅         | ✅ Running |
| 220 | LXC  | nextcloud-hub           | 192.168.30.220 | cloud.najhin-gaming.com       | ✅         | ✅ Running |
| 221 | LXC  | finance-firefly         | 192.168.30.221 | finance.najhin-gaming.com     | 📋        | 📋 Planned |
| 222 | LXC  | notification-ntfy       | 192.168.30.222 | ntfy.najhin-gaming.com        | 📋        | 📋 Planned |
| 223 | LXC  | observability-langfuse  | 192.168.30.223 | langfuse.najhin-gaming.com    | 📋        | 📋 Planned |
| 300 | LXC  | gaming-panel            | 192.168.30.210 | —                             | ✅         | ✅ Running |
| 302 | LXC  | gaming-wings-1          | 192.168.30.212 | terraria/mc.najhin-gaming.com | ✅         | ✅ Running |
| 400 | VM   | ollama-gpu              | 192.168.30.224 | ollama.najhin-gaming.com      | ✅         | ✅ Running |

**Current Total: 14 LXC containers + 1 KVM VM**
**Planned Total: 17 LXC containers + 1 KVM VM (all on VLAN 30, all autostart enabled)**

### Pulse Dashboard (CT 208)

- **Container:** rcourtman/pulse:latest on port 7655 (resized to 8GB storage)
- **Purpose:** Proxmox-native monitoring dashboard replacing Homepage
- **Features:** Real-time metrics, container status, zero configuration required
- **Agents:** Installed on Kuromoon (Proxmox host) and CT 302 (Docker monitoring)
- **Access:** home.najhin-gaming.com (Websockets Support enabled in NPM)
- **Network:** pfSense rules added for CT 208 → Proxmox API (8006), Kuromoon → CT 208 (7655)

### Nextcloud Hub (CT 220)

- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Version:** 33.0.2
- **Data location:** Currently on /var/lib/nextcloud (root disk)
- **Planned migration:** Move data directory to /mnt/data-storage (7.3TB HDD) during future phase
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (0d:00.0) + audio (0d:00.1) passed through
- **PCIe config:** hostpci0=0000:0d:00.0,pcie=1,rombar=0 hostpci1=0000:0d:00.1,pcie=1
- **OS:** Ubuntu 22.04
- **SSH:** `ssh muzakkir@192.168.30.224` — use `muzakkir` user, not root
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

### Key Features (Phase 22.1 + 22.2 + 22.8B)

- **Subscription tracker:** 6 subscriptions tracked in 04-personal/subscriptions/
- **Dashboard:** Dataview queries at 04-personal/dashboard showing costs and totals
- **Daily notes:** Auto-created at midnight via n8n (Phase 22.2)
- **Morning briefing:** 7am Telegram summary via MERLIN
- **Health tracking:** Food log, BP log, medication log via Gilgamesh buttons (Phase 22.8B)
- **/daily command:** Gilgamesh creates immediate daily notes

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

| Servant      | Class  | Role                                                                          | Platform                      | Status                     |
|--------------|--------|-------------------------------------------------------------------------------|-------------------------------|----------------------------|
| Gilgamesh 👑 | Archer | Life Interface & Personal AI Assistant                                        | Telegram (@JhinGilgamesh_bot) | ✅ Active                   |
| Da Vinci 🎨  | Caster | Sync + Goals + Review (Stage 1 doc pipeline active; Stage 2 RAG planned)     | n8n/Nextcloud                 | ⚡ Partial — Stage 1 active |
| Midas 💰     | Caster | CFO — Cost Tracking & Optimization                                            | n8n                           | ✅ Active                   |
| MERLIN 🔮    | Caster | Proactive Nudges & Scheduler                                                  | n8n                           | ✅ Active                   |

### Final 6-Agent Roster (Redesigned)

| Servant             | Class    | Role                                          | Platform      | Build Order | Status     |
|---------------------|----------|-----------------------------------------------|---------------|-------------|------------|
| Gilgamesh 👑        | Archer   | Life Interface & Personal AI Assistant        | Telegram      | —           | ✅ Active   |
| Da Vinci 🎨         | Caster   | Sync + Goals + Weekly Review                  | n8n/Nextcloud | —           | ⚡ Partial  |
| Midas 💰            | Caster   | CFO — Cost Tracking & Optimization            | n8n           | 1st         | ✅ Active   |
| MERLIN 🔮           | Caster   | Proactive Nudges & Health Scheduler           | n8n           | 2nd         | ✅ Active   |
| EMIYA 🏹            | Archer   | CTO — Infrastructure + Agent Spawning         | n8n           | 3rd         | 📋 Planned |
| Guardian 🛡         | —        | Security Monitoring & Threat Detection        | n8n           | 4th         | 📋 Planned |

**Notes:**

- Roster reduced from 9 to 6 agents for focused deployment
- Build order is firm: Midas complete, MERLIN complete, then EMIYA (24.1-24.8), Guardian

### Da Vinci — Sync + Goals + Weekly Review

**Role scope:**

- Documentation: maintains AI-CONTEXT.md, changelog.md, troubleshoot.md via /update and /sync-docs
- Obsidian writes: session summaries written to vault via Nextcloud WebDAV
- Goal tracking: monitors progress, identifies blockers, suggests next steps
- Weekly review: Sunday analysis of week's progress, agent performance, system health
- RAG retrieval (Stage 2): queries Qdrant vector database for knowledge recall across all agents
- Observability logs: all agents log activity to Da Vinci

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

### MERLIN — Proactive Nudges & Health Scheduler

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
- Health nudges: medication reminders, BP readings, exercise prompts

**Current Implementation:**

- Daily 8am checks: SSL expiry (hardcoded July 14, 2026), Backup restore test (baseline 2026-01-01), Prometheus memory usage, Vault seal status
- Cloudflare API integration pending (token truncated issue in Vault kv/cloudflare)

### EMIYA — CTO + Infrastructure + Agent Spawning

**8 core features (Phases 24.1-24.8):**

1. **App Management** — Firefly III, ntfy, Langfuse lifecycle (24.1)
2. **Alert Translation** — Alertmanager alerts → plain English via ntfy (24.2)
3. **Container Updates** — Docker image updates, apt upgrades (approval-gated) (24.3)
4. **Knowledge Ingestion** — URLs → Firecrawl → triple-write pipeline (24.4)
5. **Proactive Monitoring** — anomaly detection, trend alerts before things break (24.5)
6. **Performance + Goal Optimization** — resource reallocation, goal tracking (24.6)
7. **Universal Notifications** — ntfy hub for all agent communications (24.7)
8. **Agent Spawning** — Level 3 templates, contextual agent creation (24.8)

**Design rule:** EMIYA proposes → Muzakkir approves → EMIYA executes. No autonomous destructive actions.

**Deployment:** 8 sub-phases (24.1–24.8), estimated 24–32 hours total. All phases are critical path.

### Guardian — Security Monitoring & Threat Detection

**Role:** Dedicated security monitoring replacing basic alert forwarding.
**Sources:** Alertmanager, log analysis, threat feeds, anomaly detection.
**Output:** ntfy alerts for threats, Obsidian storage for security logs.

### Design Principles

- Gilgamesh = Telegram-only (life interface)
- All agents communicate via ntfy (universal notification hub)
- All agents log activity to Da Vinci
- All agents use Fate/GO servant theming for consistency
- Universal data flow: Input → processing → triple-write (Data Tables + Obsidian + Qdrant)

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
- **Slash commands:** 11 commands for direct actions
- **Health tracking:** Food log, BP log, medication log via interactive button prompts (Phase 22.8B)

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

| Component     | Value                                                     |
|---------------|-----------------------------------------------------------|
| Telegram Bot  | @JhinGilgamesh_bot                                        |
| Chat ID       | 518832696                                                 |
| Ollama Model  | qwen3:14b (primary local model, 9.3GB, 12GB VRAM, VM 400) |
| Haiku Model   | claude-haiku-4-5-20251001 (Ollama fallback)               |
| Sonnet Model  | claude-sonnet-4-20250514 (complex queries)                |
| n8n Container | CT 211, 192.168.30.211                                    |
| Proxmox API   | root@pam!gilgamesh token                                  |
| Proxmox node  | muzakkir (not kuromoon)                                   |

### Cost Tracking Note

- cost_usd calculated from token rates: Sonnet $3/$15 per 1M input/output tokens, Haiku $0.80/$1 per 1M tokens
- Ollama tokens captured: data.input_tokens + data.output_tokens from Call Ollama node response
- Ollama queries cost $0 (local inference)
- command_type derived from Telegram message text directly

### n8n Workflows (Count: 12)

| Workflow                           | Purpose                                  | Nodes | Trigger  |
|------------------------------------|------------------------------------------|-------|----------|
| Telegram Agent                     | Main bot, menu, commands, hybrid routing | 15+   | Telegram |
| Documentation Pipeline — Update    | Session summary → 3 files                | 7     | Webhook  |
| Documentation Pipeline — Sync Docs | Full doc regeneration → 7 files          | 7     | Webhook  |
| Da Vinci Documentation Pipeline    | Raw staging summaries → formatted docs   | 11    | Webhook  |
| Midas — CFO Report                 | /midas command cost analysis             | 6     | Webhook  |
| Midas — Daily Brief                | 9am scheduled cost summary               | 4     | Schedule |
| MERLIN — Reminders                 | 8am daily infrastructure checks          | 8     | Schedule |
| Daily Note Creator                 | Midnight Obsidian daily notes            | 4     | Schedule |
| Morning Briefing                   | 7am daily summary to Telegram            | 5     | Schedule |
| Update Nextcloud File              | Legacy (unpublished)                     | 5     | Webhook  |
| Push to GitHub                     | Legacy (unpublished)                     | 4     | Webhook  |

> Note: Hybrid routing (Phase 41) is integrated into the Telegram Agent workflow — it is not a separate workflow. Health tracking (Phase 22.8B) is also integrated into the Telegram Agent workflow.

### Inline Keyboard Menu Status

| Menu Item         | Status    |
|-------------------|-----------|
| Main Menu         | ✅ Working |
| Homelab → Status  | ✅ Working |
| Homelab → Metrics | ✅ Working |
| Homelab → Temps   | ⚠️ SSH Bug |
| Homelab → Storage | ✅ Working |
| Homelab → Alerts  | ✅ Working |
| Homelab → Backup  | ✅ Working |
| Gaming submenu    | ✅ Working |
| Gilgamesh submenu | ✅ Working |
| Tools submenu     | ✅ Working |
| Health submenu    | ✅ Working |
| Help              | ✅ Working |

### n8n Data Tables

| Table Name               | Purpose                                          | Key Columns                          |
|--------------------------|--------------------------------------------------|--------------------------------------|
| gilgamesh_memory         | Conversation history (last 20 messages)         | id, timestamp, user, message, response |
| gilgamesh_costs          | Token usage and cost tracking                   | id, timestamp, model, tokens, cost_usd, command_type |
| gilgamesh_session_state  | Mode switching for health tracking              | chat_id, mode, updated_at            |
| health_food_log          | Food logging entries                            | id, logged_at, food_item, notes, chat_id |
| health_bp_log            | Blood pressure readings                         | id, logged_at, systolic, diastolic, notes, chat_id |
| health_med_log           | Medication tracking                             | id, logged_at, medication, dosage, notes, chat_id |

### SSH & API Access

- **SSH to Kuromoon:** CT 211 → 192.168.10.5:22 (pfSense rule added)
- **SSH to VM 400:** `ssh muzakkir@192.168.30.224` — use muzakkir user, not root
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

| Component    | Details                                             |
|--------------|-----------------------------------------------------|
| Webhooks     | doc-update, doc-sync, da-vinci                      |
| Telegram     | Routes /update and /sync-docs                       |
| Claude API   | Documentation merging intelligence                  |
| Nextcloud    | File storage via admin user                         |
| GitHub       | Version control via API push                        |
| Da Vinci     | Async processing (she/her pronouns)                 |
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

| Bot                | Platform         | Source              | Status   | Purpose                                                        |
|--------------------|------------------|---------------------|----------|----------------------------------------------------------------|
| @JhinGilgamesh_bot | Telegram         | n8n CT 211          | ✅ Active | Personal AI agent — chat, homelab control, /update, /sync-docs |
| Midas              | Telegram         | n8n CT 211          | ✅ Active | CFO cost tracking — /midas reports, 9am daily briefs           |
| MERLIN             | Telegram         | n8n CT 211          | ✅ Active | Reminders — 8am daily infrastructure checks                    |
| Daily Note Creator | Nextcloud WebDAV | n8n CT 211          | ✅ Active | Midnight daily note creation in Obsidian vault                 |
| Morning Briefing   | Telegram         | n8n CT 211          | ✅ Active | 7am daily summary to Telegram                                  |
| Health Tracking    | Obsidian WebDAV  | n8n CT 211          | ✅ Active | Food/BP/medication logging via Gilgamesh buttons               |
| Homelab Alerts     | Telegram         | Alertmanager CT 205 | ✅ Active | Critical alerts (host down, high CPU/memory/disk)              |
| Homelab Alerts     | Discord webhook  | Alertmanager CT 205 | ✅ Active | Warning-level alerts to #alerts channel                        |

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
| kinmoon-smb  | NAS RAID 1 | SMB/CIFS | 2.6 TB   | Small container backups |
| kinmoon-nfs  | NAS RAID 1 | NFS      | 2.6 TB   | Disk images only        |
| data-storage | Local HDD  | Direct   | 7.2 TB   | Large container backups |
| local-lvm    | Local NVMe | LVM-Thin | 137 GB   | Container root disks    |
| vmpool-fast  | Local NVMe | ZFS      | 899 GB   | Fast container storage  |

### NAS Configuration

- **Model:** UGREEN DXP2800 (2-bay)
- **RAID:** RAID 1 (mirrored drives for redundancy)
- **Filesystem:** ext4
- **SMB share:** proxmox-backups (username: Muzakkir)
- **NFS export:** /volume1/proxmox-backups
- **Status:** Normal (healthy, drives synced)
- **Network:** 192.168.10.100 (changed from 192.168.10.15 after factory reset)

---

## 🔒 Security Architecture

| Layer           | Implementation                                                                                                                                                                                     |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                                                                                                                                      |
| Segmentation    | 5 VLANs with enforced firewall rules                                                                                                                                                               |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                                                                                                                                        |
| VPN             | Tailscale (subnet router on pfSense, primary access)                                                                                                                                               |
| External Auth   | Cloudflare Access (Email OTP, muzakkir.kholil06@gmail.com only) for Grafana, n8n, Vault, Vaultwarden, Ollama, Homepage, Nextcloud (7 apps total)                                                   |
| External Access | Cloudflare Tunnel for all external services                                                                                                                                                        |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                                                                                                                                      |
| Backup          | Automated daily backups with 7/4/2 retention                                                                                                                                                       |
| Secrets         | HashiCorp Vault (CT 213, vault.najhin-gaming.com). KV engine at kv/. Secrets: kv/gilgamesh, kv/cloudflare, kv/proxmox, kv/alertmanager, kv/github, kv/nextcloud, kv/n8n, kv/pihole (8 paths total) |
| Passwords       | Vaultwarden (CT 214, passwords.najhin-gaming.com). Personal password manager with Bitwarden clients. API bypassed in Cloudflare Access (/api/, /identity/ paths).                                  |

### Vault Notes

- **Reseals on reboot:** Manual unseal required: `pct exec 213 -- vault operator unseal`
- **All credentials stored:** kv/nextcloud, kv/github, VM 400, Proxmox, CT 302 passwords

---

## 📋 Project Phase History

| Phase   | Title                                                            | Status     | Completed    |
|---------|------------------------------------------------------------------|------------|--------------|
| 1       | Proxmox VE Installation                                          | ✅ Complete | Jan 2026     |
| 2       | pfSense Firewall & VLAN Setup                                    | ✅ Complete | Jan 2026     |
| 3       | Core Services (Pi-hole, NPM, Tailscale, DDNS)                    | ✅ Complete | Jan 2026     |
| 4       | External Access & SSL                                            | ✅ Complete | Feb 2026     |
| 5       | Monitoring Stack                                                 | ✅ Complete | Feb 2026     |
| 6A-6D   | Gaming Platform (Pterodactyl, Terraria, Minecraft)               | ✅ Complete | Feb 2026     |
| 6E      | Homepage Dashboard                                               | ✅ Complete | Mar 2026     |
| 6F      | Infrastructure Audit, VLAN Migration & Firewall Hardening        | ✅ Complete | Mar 9, 2026  |
| 7       | Nextcloud Deployment                                             | ✅ Complete | Mar 8, 2026  |
| 7A      | Backup Strategy                                                  | ✅ Complete | Mar 13, 2026 |
| 7B      | n8n Workflow Automation                                          | ✅ Complete | Apr 2, 2026  |
| 7C      | Gilgamesh Telegram Bot + GitHub Integration                      | ✅ Complete | Apr 2, 2026  |
| 7D      | Gilgamesh Enhancements (Memory, Routing, Web Search)             | ✅ Complete | Apr 6, 2026  |
| 7D-Sec  | Cloudflare Access for n8n                                        | ✅ Complete | Apr 7, 2026  |
| 7D-Menu | Gilgamesh Inline Keyboard Menu                                   | ✅ Complete | Apr 24, 2026 |
| 9       | NAS Deployment (Kinmoon)                                         | ✅ Complete | Mar 3, 2026  |
| 13      | HashiCorp Vault — Secrets Manager                                | ✅ Complete | Apr 18, 2026 |
| 14      | Secrets Management & Integration                                 | ✅ Complete | Apr 24, 2026 |
| 15      | Gilgamesh Additional Slash Commands                              | ✅ Complete | Apr 24, 2026 |
| 16.1    | Documentation Pipeline — Update Workflow                         | ✅ Complete | Apr 19, 2026 |
| 16.2    | Documentation Pipeline — Sync Docs Workflow                      | ✅ Complete | Apr 19, 2026 |
| 16.3    | Da Vinci Documentation Pipeline                                  | ✅ Complete | Apr 25, 2026 |
| 22      | Obsidian Knowledge Base                                          | ✅ Complete | Apr 24, 2026 |
| 22.1    | Obsidian Vault Structure Expansion                               | ✅ Complete | Apr 27, 2026 |
| 22.2    | Obsidian Daily Notes + Morning Briefing                          | ✅ Complete | Apr 27, 2026 |
| 22.8A   | Button Menu System + Community Nodes                             | ✅ Complete | Apr 27, 2026 |
| 22.8B   | Health Tracking (Food/BP/Medication Logging)                     | ✅ Complete | Apr 28, 2026 |
| 23      | Vaultwarden + Secrets Audit & Cleanup                            | ✅ Complete | Apr 18, 2026 |
| 24.1    | App Management — Firefly III, ntfy, Langfuse deployment          | 📋 Planned | —            |
| 24.2    | Alert Translation — Alertmanager → ntfy plain English            | 📋 Planned | —            |
| 24.3    | Container Updates — Docker + apt updates (approval-gated)        | 📋 Planned | —            |
| 24.4    | Knowledge Ingestion — URLs → Firecrawl → triple-write pipeline   | 📋 Planned | —            |
| 24.5    | Proactive Monitoring — anomaly detection + proactive nudges      | 📋 Planned | —            |
| 24.6    | Performance + Goal Optimization — resource + goal tracking       | 📋 Planned | —            |
| 24.7    | Universal Notifications — ntfy hub for all agents               | 📋 Planned | —            |
| 24.8    | Agent Spawning — Level 3 templates, contextual creation         | 📋 Planned | —            |
| 25.1    | Voice Interface — Claude API transcription + voice responses     | 📋 Planned | —            |
| 25.2    | Email Integration — IMAP monitoring + smart filtering            | 📋 Planned | —            |
| 25.3    | Self-Evolving Skills — agent capability learning                | 📋 Planned | —            |
| 25.4    | Content Creation — automated reports, social posts              | 📋 Planned | —            |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                             | ✅ Complete | Apr 24, 2026 |
| 39      | Open WebUI                                                       | ✅ Complete | Apr 24, 2026 |
| 41      | Gilgamesh + Ollama Hybrid Routing                                | ✅ Complete | Apr 24, 2026 |
| 58      | Windrose Server Deployment                                       | ✅ Complete | Apr 19, 2026 |
| Midas   | Midas CFO Agent                                                  | ✅ Complete | Apr 27, 2026 |
| MERLIN  | MERLIN Reminders Agent                                           | ✅ Complete | Apr 27, 2026 |
| Pulse   | Pulse Dashboard Deployment                                       | ✅ Complete | May 10, 2026 |

---

## 🐛 Key Lessons Learned

### Hardware & Recovery

| Issue                                   | Resolution                                                  |
|-----------------------------------------|-------------------------------------------------------------|
| GPU PCIe address change after motherboard | Update VM config: qm set 400 --hostpci0 0000:0d:00.0,pcie=1 |
| KVM virtualization not available       | Enable AMD-V (SVM Mode) and IOMMU in UEFI settings         |
| Boot timeout after BIOS changes        | Disable systemd-networkd-wait-online.service               |
| RAID conversion with active mount       | Unmount NFS share before NAS factory reset                 |

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
| NFS cannot do LXC backups    | NFS + LXC UID namespace mapping incompatible — use SMB |

### n8n & Gilgamesh

| Issue                                 | Resolution                                                                                                         |
|---------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| n8n Data Tables schema caching bug    | Delete and recreate the table entirely                                                                             |
| Claude API 404 model not found        | Use exact model ID: `claude-haiku-4-5-20251001`                                                                    |
| Web search partial response capture   | Add Extract Response Code node, filter by `type === "text"`                                                        |
| Expression scoping issues             | Use Code node with `this.helpers.httpRequest`, reference `$json`                                                   |
| NFS fails for LXC backups             | UID namespace mapping (100000-165536); use SMB instead                                                             |
| pfSense rule not working              | Place new rules BEFORE RFC1918 block rules                                                                         |
| n8n GitHub credential 401             | User field (`muzakkir97`) must be populated                                                                        |
| Telegram single webhook constraint    | All update types (message + callback) must use one trigger                                                         |
| Answer Callback node loses data       | Bypass it; handle callback acknowledgment separately                                                               |
| Da Vinci grounding bug                | Use direct node references; increase max_tokens to 32000                                                           |
| Open WebUI JSON parse error           | Add `proxy_buffering off` to NPM advanced config for streaming                                                     |
| input.first()/input.last() unreliable | After Merge node, use direct node references like `$('Extract Response').first().json`                             |
| Ollama tokens always 0                | Call Ollama node already maps prompt_eval_count → input_tokens — read data.input_tokens not data.prompt_eval_count |
| Loki not being scraped                | Add scrape job to Prometheus config for CT 204                                                                     |
| Pulse deployment needs websockets     | Enable Websockets Support in NPM for real-time dashboard updates                                                   |
| Pulse agent tokens are one-time       | Generate new token per host — tokens cannot be reused                                                              |

### HashiCorp Vault (Phase 13)

| Issue                            | Resolution                                                     |
|----------------------------------|----------------------------------------------------------------|
| apt fails in new LXC             | Run `echo "nameserver 192.168.30.10" > /etc/resolv.conf` first |
| HashiCorp repo setup fails       | Install `lsb-release` before adding repo                       |
| Wrong repo architecture/codename | Hardcode `amd64` and `bookworm` in repo entry                  |
| Vault fails to start in LXC      | Add `disable_mlock = true` to vault.hcl                        |
| Token auth issues in shell       | Use `vault login <token>` not `export VAULT_TOKEN=`            |
| Vault sealed after reboot        | `pct exec 213 -- vault operator unseal`                        |
| Cloudflare token truncated       | kv/cloudflare only stores 7 chars — re-store full token        |

### Obsidian & Knowledge Management

| Issue                       | Resolution                                                         |
|-----------------------------|--------------------------------------------------------------------|
| Nextcloud quota exceeded    | Resize CT 220 from 20GB to 100GB with `pct resize 220 rootfs +80G` |
| Dataview queries no results | Queries must target folder with individual notes, not same file    |

### Cloudflare Access & Security

| Issue                         | Resolution                                                      |
|-------------------------------|-----------------------------------------------------------------|
| Bitwarden HTTP 525 from phone | Cloudflare Access blocking API — bypass /api/, /identity/ paths |
| n8n Access OTP not triggering | Cached session — test in incognito mode                         |

### Ollama & GPU

| Issue                                   | Resolution                                                              |
|-----------------------------------------|-------------------------------------------------------------------------|
| ROCm full SDK ran out of disk space     | Use minimal runtime only; extend LVM with `lvextend -l +100%FREE`       |
| amdgpu still bound after blacklist      | Add `softdep amdgpu pre: vfio-pci` to vfio.conf, rebuild initramfs      |
| Ollama running on CPU not GPU           | Add `HSA_OVERRIDE_GFX_VERSION=10.3.0` and render/video group membership |
| Open WebUI no models available          | Set `OLLAMA_HOST=0.0.0.0`; point container to 172.17.0.1:11434          |
| SSH to VM 400 permission denied as root | Use `muzakkir` user with sudo — root SSH is disabled                    |

### Monitoring & Alerts

| Issue                      | Resolution                                             |
|----------------------------|--------------------------------------------------------|
| High memory alerts at 80%  | Raise threshold to 85% (Windrose baseline ~9GB RAM)    |
| Prometheus HighMemoryUsage | Updated rule from 80% to 85% in alerting configuration |

### n8n Community Nodes

| Issue                           | Resolution                                                      |
|---------------------------------|-----------------------------------------------------------------|
| Dockerfile npm install breaks   | n8n uses pnpm catalog — use Settings → Community Nodes UI only  |
| chat_id undefined on callbacks  | Use callback_query?.message?.chat?.id \|\| message?.chat?.id     |
| Inline keyboard not showing     | Use HTTP Request node, not Telegram node for reply_markup       |

### Health Tracking & Obsidian Integration

| Issue                                   | Resolution                                                    |
|-----------------------------------------|---------------------------------------------------------------|
| [object Object] in Obsidian            | Fetch Daily Note Response Format must be Text, use .data field |
| UTC time in health logs                 | Add 8 * 60 * 60 * 1000ms offset for MYT timezone              |
| Duplicate appendText declaration        | Replace entire Code node with clean version                    |
| Fetch Daily Note URL not resolving     | Must use {{ }} expression wrapper in WebDAV URL               |
| JSON body invalid in health summary     | Switch to Using Fields Below mode in HTTP Request              |

### Pulse Dashboard Deployment

| Issue                              | Resolution                                                                   |
|------------------------------------|------------------------------------------------------------------------------|
| Pulse crash loop on CT 208         | No space left on device — resized container from 4GB to 8GB                  |
| Pulse agent 401 on CT 302          | Token already consumed by Kuromoon — generated new token from Pulse UI       |
| Pulse "Connection lost" through NPM | Websockets Support not enabled — toggled on in NPM proxy host                |

---

## ❓ Pending Tasks

### Immediate

| Task                                                                                        | Priority |
|---------------------------------------------------------------------------------------------|----------|
| Schedule backup restore test (CT 207 recommended), update lastTestDate in MERLIN            | High     |
| Fix Cloudflare API token in Vault (get full token from dashboard, re-store)                 | High     |
| Activate MERLIN workflow (toggle on)                                                        | High     |
| Monitor Windrose RAM usage — stop Docker container when not playing                         | High     |
| Update subscription costs in Obsidian when known (YouTube Premium, Cloudflare Domain, TIME) | Medium   |

### Infrastructure

| Task                                                                         | Priority |
|------------------------------------------------------------------------------|----------|
| Address local-lvm thin pool overprovisioning (during infrastructure cleanup) | Medium   |
| Begin Phase 24.1 (Firefly III) + 24.7 (ntfy) next session                   | High     |
| Create muzakkir97/homelab-private repo                                       | Medium   |
| Set up Backblaze B2 account                                                  | Medium   |
| Phase 27 (Vault + n8n) enables n8n to fetch secrets directly                 | Medium   |
| Install 128GB DDR4 RAM upgrade when Taobao delivery arrives                  | Medium   |
| Clean up temp backups (saves 6.9GB local storage) - optional                 | Low      |
| Add sdb monitoring to MERLIN daily checks                                    | Medium   |
| Improve NAS airflow — sda at 60°C is near thermal limit for HDDs            | Medium   |

### Gilgamesh & Documentation

| Task                                                                                            | Priority |
|-------------------------------------------------------------------------------------------------|----------|
| Build Monthly Infrastructure Audit cron workflow (assign new phase number — NOT 16.3)           | High     |
| Build Guardian security monitoring agent (after Phase 24.8)                                     | High     |
| Review and finalize ROADMAP-v2-draft.md                                                        | High     |
| Confirm Lepulse → Fitdays → Samsung Health sync works on phone                                  | Medium   |
| /update redesign — file attachment via Telegram, push to GitHub + Nextcloud                     | Medium   |
| Homepage embedded Gilgamesh chat UI (web frontend, shared memory with Telegram)                 | Medium   |
| Integrate Vault secrets into n8n Gilgamesh workflow (Phase 27)                                  | Medium   |
| Build Da Vinci Stage 2 (RAG) alongside Phase 7E                                                 | Medium   |

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
| Fix Homelab → Temps SSH bug                                                | Deferred |
| DOCP RAM optimization for 128GB kit                                         | Deferred |

---

## 📝 Session Log (Recent)

### May 10, 2026

Date: May 10, 2026
Phase: Architecture Redesign Planning (Pre-Phase 24.x)

Topics Discussed
- Complete architecture redesign: 7-layer model (Input → Brain → App → Knowledge → Memory → Observability → Notification)
- App selection: Firefly III, Health Connect webhook bridge, ntfy, Langfuse. Skipped Vikunja and Open Wearables.
- Knowledge ingestion pipeline: URLs → Firecrawl → Ollama → triple-write (Obsidian + Data Table + Qdrant) with proactive recall + source citation + media
- Agent spawning (Level 3 templates) merged into EMIYA scope
- Three essential capabilities added: goal tracking (→ 24.6), proactive nudges (→ 24.5), weekly review (→ 24.6)
- Future phases added: 25.1 Voice, 25.2 Email, 25.3 Self-Evolving Skills, 25.4 Content Creation

Decisions Made
- Deploy Firefly III (CT 221), ntfy (CT 222), Langfuse (CT 223)
- Health data: dual-write (Data Tables for agents + Obsidian for human)
- Merge 3 morning messages into one Gilgamesh daily digest at 7am
- Knowledge recall: proactive with source link + media
- Lepulse scale syncs via Fitdays → Samsung Health → Health Connect → same bridge as watch
- 5 phases retired: 22.8C, 22.8D, 22.8E, 22.15, 22.16
- New phases: 24.1–24.8 (core) + 25.1–25.4 (future)

Changes to AI-CONTEXT.md
- Replace architecture with 7-layer model
- Add CT 221/222/223 to container inventory (total: 17 LXC + 1 VM)
- Update all 6 agent roles (Gil=life interface, Da Vinci=sync+goals+review, MERLIN=nudges, EMIYA=infra+spawning)
- Replace roadmap with v2 build order: 24.1-24.8 → 7E → EMIYA → 25.1-25.4
- Add Lepulse scale to hardware notes
- Document universal data flow pattern

Action Items
- [ ] Review and finalize ROADMAP-v2-draft.md
- [ ] Update AI-CONTEXT.md + ROADMAP.md + push to GitHub
- [ ] Begin Phase 24.1 (Firefly III) + 24.7 (ntfy) next session
- [ ] Confirm Lepulse → Fitdays → Samsung Health sync works on phone

### May 10, 2026

Date: May 10, 2026
Phase: Pulse Dashboard Deployment + Backup Fix

Topics Discussed
- Researched homelab dashboard alternatives: Dashy, Homarr, Pulse, Homepage comparison
- Deployed Pulse monitoring dashboard on CT 208, replacing Homepage
- Fixed small container backups (kinmoon-smb created, NFS UID mapping issue resolved)
- Discovered sdb (Seagate 2TB) has 8 pending sectors and sda (WD Purple 8TB) running at 60°C
- Explained *arr media ecosystem (Sonarr, Radarr, Prowlarr, Jellyfin) — informational only, no deployment planned
- Kinmoon NAS IP changed to 192.168.10.100, username is Muzakkir (not admin)

Decisions Made
- Pulse chosen over Dashy/Homarr/custom dashboard — best Proxmox-native monitoring with zero config
- Homepage container removed from CT 208, Pulse runs on port 7655
- kinmoon-smb (SMB/CIFS) created for backups — NFS cannot handle LXC UID namespace mapping for backups
- kinmoon-nfs kept for disk images only, Backup content type removed
- Small container backup job (02:00) switched from kinmoon-nfs to kinmoon-smb
- Pulse Telegram alerts skipped — existing Alertmanager + MERLIN already covers alerting
- sdb monitoring: manual weekly smartctl check for now, add to MERLIN in future session
- *arr ecosystem not planned for deployment

Changes to AI-CONTEXT.md
- Hardware Inventory: Kinmoon NAS IP updated from 192.168.10.15 to 192.168.10.100
- Infrastructure Inventory: CT 208 now runs Pulse (rcourtman/pulse:latest) on port 7655, Homepage removed
- CT 208 storage: resized from 4GB to 8GB
- Backup Strategy: Small containers storage changed from kinmoon-nfs to kinmoon-smb (SMB/CIFS protocol)
- Storage section: Add kinmoon-smb (SMB/CIFS, 192.168.10.100, share: proxmox-backups, content: backup)
- Storage section: kinmoon-nfs content is disk images only (not backups)
- NAS credentials: username is Muzakkir (not admin)
- Pulse agent installed on Kuromoon (Proxmox host) and CT 302 (gaming-wings-1 Docker monitoring)
- pfSense rules added: CT 208 → Proxmox API (8006), Kuromoon → CT 208 (7655)
- NPM: home.najhin-gaming.com proxied to CT 208:7655 with Websockets Support enabled
- Disk health: sda (WD Purple 8TB) 60°C — high but functional; sdb (Seagate 2TB) 8 pending sectors — monitor weekly
- Pending Tasks: Add "Add sdb monitoring to MERLIN daily checks". Add "Fix NAS SMB password in Vault kv/nas if exists"
- n8n workflows count remains 12 (no new workflows added)
- Key Lessons: Add "Pulse deployment requires websocket support in NPM for real-time updates". Add "NFS cannot do LXC backups due to UID namespace mapping — use SMB". Add "Pulse agent tokens are one-time — generate new token per host"
- Small container backups were missing May 5-9 due to NAS reset — now restored

Errors & Resolutions
- Pulse crash loop on CT 208: No space left on device — resized CT 208 from 4GB to 8GB
- Pulse agent 401 on CT 302: Token already consumed by Kuromoon — generated new token from Pulse UI
- vzdump to kinmoon-nfs permission denied: NFS + LXC UID namespace mapping incompatible — created kinmoon-smb (SMB/CIFS) storage instead
- smbclient login failure with admin user: NAS username is Muzakkir not admin, password needed command-line format (user%password) to work
- Pulse "Connection lost" through NPM: Websockets Support not enabled — toggled on in NPM proxy host
- kinmoon-nfs backup content type error: Backup content type was missing — added then removed again since NFS can't do LXC backups

Action Items
- [ ] Add sdb SMART monitoring to MERLIN daily checks (Current_Pending_Sector threshold)
- [ ] Improve NAS airflow — sda at 60°C is near thermal limit for HDDs
- [ ] Add Pulse credentials to Vaultwarden
- [ ] Add management URLs for remaining containers in Pulse (CT 204 Loki if needed)
- [ ] Remove Backup content type from kinmoon-nfs if not already done
- [ ] Consider replacing sdb (Seagate 2TB, 5.7 years, 8 pending sectors) in future hardware planning
- [ ] Update Cloudflare Access if needed for home.najhin-gaming.com (Pulse vs Homepage)

### May 9, 2026

Date: May 9, 2026
Session Type: Hardware Recovery + Infrastructure Upgrade

Topics Covered
- Kuromoon hardware recovery after shop cleaning (thermal paste residue removal)
- VM 400 GPU passthrough fix (PCIe address update after motherboard change)
- BIOS virtualization configuration (AMD-V/SVM enable)
- Boot timeout fix (systemd-networkd-wait-online.service)
- RAID 1 setup on Kinmoon NAS (UGREEN DXP2800)
- NFS share configuration and Proxmox mount
- Backup restoration (May 4 backups to new RAID array)
- RAM upgrade purchase (4x32GB Corsair Vengeance DDR4 from Taobao)

Decisions Made
- Keep existing backup retention policy: 7 daily, 4 weekly, 2 monthly (not simplified to 7-day only)
- Purchased 4x32GB Corsair Vengeance DDR4 from Taobao → total 128GB when installed (massive upgrade from current 32GB)
- DOCP RAM optimization deferred to future session
- Temp backups cleanup optional (saves 6.9GB local storage)

Hardware Changes
Kuromoon (Proxmox):
- Motherboard: ASUS TUF B550M-E (mATX, 4 DIMM slots, confirmed operational)
- GPU PCIe address changed: 2d:00.0 → 0d:00.0 (RX 6700 XT)
- BIOS settings updated: AMD-V enabled, IOMMU enabled
- Boot services: Disabled systemd-networkd-wait-online.service
- VM 400 hostpci configuration updated to new PCIe addresses

Kinmoon NAS (UGREEN DXP2800):
- RAID configuration: RAID 1 (2-drive mirror)
- Storage Pool 1: 2.7TB total, 2.6TB usable
- Volume 1: ext4 filesystem
- NFS share: /volume1/proxmox-backups
- NAS IP: 192.168.10.100 (changed from 192.168.10.15 after factory reset)
- Status: Normal (healthy, sync complete)

Configuration Changes
VM 400 GPU Passthrough:
```
qm set 400 --delete hostpci0
qm set 400 --delete hostpci1
qm set 400 --hostpci0 0000:0d:00.0,pcie=1,rombar=0
qm set 400 --hostpci1 0000:0d:00.1,pcie=1
```

Boot Timeout Fix:
```
systemctl disable systemd-networkd-wait-online.service
```

NFS Mount (Proxmox):
- Storage ID: kinmoon-nfs
- Server: 192.168.10.100
- Export: /volume1/proxmox-backups
- Content: VZDump backup file
- Mount point: /mnt/pve/kinmoon-nfs

Backup Strategy:
- Schedule: Daily at 02:00
- Retention: 7 daily, 4 weekly, 2 monthly
- Storage: kinmoon-nfs (RAID 1 mirrored)
- Containers: 201,202,203,204,205,206,207,214,300
- Backup size: 6.9GB (May 4 backups restored)

Verification Results
✅ All 14 LXC containers running
✅ VM 400 running with GPU passthrough (RX 6700 XT recognized by ROCm)
✅ Ollama models intact (qwen3:14b, llama3.2)
✅ Gilgamesh Telegram bot responding
✅ ZFS pool vmpool healthy (no errors)
✅ RAID 1 status: Normal (2 drives mirrored)
✅ NFS mount operational (write test successful)
✅ May 4 backups (6.9GB) restored to NAS

Errors & Resolutions
Issue 1: VM 400 failed to start after motherboard change
- Error: GPU passthrough using old PCIe address (2d:00.0)
- Resolution: Updated hostpci0/1 to new addresses (0d:00.0, 0d:00.1)

Issue 2: "KVM virtualisation configured, but not available"
- Cause: New ASUS TUF B550M-E motherboard had virtualization disabled
- Resolution: Enabled AMD-V (SVM Mode) and IOMMU in UEFI settings

Issue 3: System hung at boot after BIOS changes
- Symptom: "clean, recovering journal" screen, 90-120 second wait
- Cause: systemd-networkd-wait-online.service waiting for network
- Resolution: Disabled the service, boot time optimized

Issue 4: NFS volume locked during RAID conversion
- Cause: Proxmox had NFS mounted while trying to delete volume
- Resolution: umount /mnt/pve/kinmoon-nfs before NAS factory reset

Changes to AI-CONTEXT.md
Update Hardware fleet section:
- Kuromoon: 32GB → 128GB DDR4 (4x32GB Corsair Vengeance ordered from Taobao)
- Kuromoon motherboard: ASUS TUF B550M-E (mATX, 4 DIMM slots)
- GPU PCIe addresses updated: 0d:00.0 (GPU), 0d:00.1 (audio)
- Kinmoon: IP address changed 192.168.10.15 → 192.168.10.100
- Kinmoon: 3.6TB WD Purple → 5.4TB RAID 1 configuration
- Kinmoon: Updated purpose "Backups (RAID 1, ext4)"

Update Infrastructure section:
- VM 400 PCIe config updated to new addresses
- ollama-gpu SSH note unchanged (muzakkir user)

Update Backup Strategy:
- kinmoon-nfs Storage: "NAS RAID 1" type, "NFS" protocol, "2.6 TB" capacity
- NAS Configuration subsection added with RAID 1 details

Update Key Lessons Learned:
- Add Hardware & Recovery section with 4 new entries
- GPU PCIe address fix, virtualization enable, boot timeout, RAID mount

Update Pending Tasks:
- Add "Install 128GB DDR4 RAM upgrade when Taobao delivery arrives" (Medium priority)
- Add "Clean up temp backups (saves 6.9GB local storage) - optional" (Low priority)
- Add "DOCP RAM optimization for 128GB kit" to Deferred section

Action Items
- [ ] Install RAM upgrade when delivery arrives from Taobao
- [ ] Monitor system stability with new motherboard
- [ ] Test backup restore from RAID 1 NFS share
- [ ] Consider DOCP optimization after RAM upgrade
- [ ] Clean up temp backups when storage space needed

### April 28, 2026 (Post-deployment Review)

Date: April 28, 2026
Phase: 22.8B complete + Post-deployment review

Topics Discussed
- Phase 22.8B full health tracking deployed and tested
- Post-deployment review established as standard practice
- Medication flow redesigned: MERLIN owns daily logging, Health menu owns management
- health_med_list Data Table design confirmed (medication + active)
- Karpathy concepts mapped to homelab
- Vision API options researched — Llama 3.2 Vision 11B confirmed free, local, runs on RX 6700 XT via Ollama
- Broader life ledger concept designed replacing narrow food log
- Multi-agent communication design flagged for dedicated session

Decisions Made
- Medication daily logging moves to MERLIN (8am dynamic buttons)
- Health menu Medication button = management only (add/remove/view)
- health_med_list table: medication + active columns, managed via Gilgamesh not n8n UI
- Vision model: Llama 3.2 Vision 11B via Ollama on VM 400 (free, local, no API cost)
- Qwen2.5-VL-7B as alternative vision model option
- Replace health_food_log with broader life_log table (single table for all personal logging)
- life_log columns: log_date, category, item, amount_myr, notes, photo_path, logged_at
- Phase 22.8E = Life Ledger (replaces narrow food expense design)
- Midas expanded scope: personal spending by category + API costs
- Da Vinci writes structured life_log entries to Obsidian daily note
- Post-deployment review session is now standard after each phase

Changes to AI-CONTEXT.md
- Phase 22.8B marked complete April 28, 2026
- Phase 22.8C: homepage health + homelab widgets (next)
- Phase 22.8D: MERLIN medication check + Medication management menu
- Phase 22.8E: Life Ledger (photo + text logging, life_log table, Llama 3.2 Vision, multi-agent routing)
- New planned table: health_med_list (medication, active)
- New planned table: life_log (replaces health_food_log for expenses)
- Vision model planned: Llama 3.2 Vision 11B on VM 400 via Ollama
- Midas Phase 2: personal spending reports by category
- MERLIN expanded: daily medication check with dynamic inline buttons
- New session topic queued: Multi-Agent communication design

Action Items
- [ ] Update ROADMAP.md: 22.8B complete, add 22.8C/D/E, fix Da Vinci Stage 2 placement
- [ ] Clean up 2026-04-28.md in Nextcloud (remove UTC test entries)
- [ ] Create health_med_list Data Table when ready to add meds
- [ ] Pull Llama 3.2 Vision on VM 400: ollama pull llama3.2-vision
- [ ] New session: Multi-Agent communication design
- [ ] Next deployment session: Phase 22.8C homepage widgets

### April 28, 2026

Date: April 28, 2026
Phase: 22.8B — Health Tracking Build (COMPLETE)

Topics Discussed
- Karpathy's LLM Wiki, Agentic Engineering, Dobby concept, Context Rot, System Prompt Learning applied to homelab
- Da Vinci as Obsidian librarian/gatekeeper — confirmed architecture
- Phase priority reorganisation — Da Vinci Stage 2 before 7E and EMIYA
- Phase 22.8B: full health tracking system built and tested

Decisions Made
- Health data writes to both n8n Data Tables (fast queries) and Obsidian daily notes (long-term)
- Da Vinci Stage 2 must be built before more agents write to Obsidian
- Mode switching via gilgamesh_session_state Data Table
- Cancel button on all health prompts via health_cancel callback
- MYT time via UTC+8 offset in Code nodes
- WebDAV GET then PUT pattern for Obsidian append
- Fetch Daily Note Response Format set to Text, body concatenated via .data field
- Format Today Summary uses plain text (no emojis) to avoid JSON issues

Changes to AI-CONTEXT.md
- Add 4 new Data Tables: gilgamesh_session_state, health_food_log, health_bp_log, health_med_log
- n8n workflows count: add health tracking nodes to Telegram Agent workflow (not a separate workflow)
- Phase 22.8B marked complete April 28, 2026
- Pending: Phase 22.8C (homepage widgets), Guardian agent, Da Vinci Stage 2
- Add Karpathy concepts to project context: LLM Wiki (Da Vinci Stage 2), Agentic Engineering (EMIYA design), Dobby (future home automation phase), Context Rot (justifies Da Vinci Stage 2 before 7E), System Prompt Learning (/update pipeline)
- Phase priority order updated: 22.8C → Guardian → Da Vinci Stage 2 → 7E → EMIYA 24.1

Errors & Resolutions
- [object Object] in Obsidian: Fetch Daily Note Response Format must be Text, use .data field in PUT body
- appendText duplicate declaration: replaced entire code block with clean version
- UTC time in Obsidian: add 8 * 60 * 60 * 1000ms offset to loggedAt
- Clear Mode type error: wrap chatId with Number() for numeric column filter
- Fetch Daily Note URL not resolving: must use {{ }} expression wrapper
- Format Today Summary illegal return: code was truncated, replaced with complete version
- JSON body invalid in Send Today Summary: switch to Using Fields Below mode

Action Items
- [ ] Update ROADMAP.md: add 22.8B (complete), 22.8C, fix Da Vinci Stage 2 placement
- [ ] Clean up 2026-04-28.md in Nextcloud (remove old UTC test entries)
- [ ] Begin Phase 22.8C (homepage health + homelab widgets) next session
- [ ] Guardian agent after 22.8C

### April 27, 2026

Date: April 27, 2026
Phase: 22.8A — Complete Button Menu + Community Nodes Install

Topics Discussed
- Community nodes research and installation
- n8n node audit (built-in vs community)
- Workflow canvas clutter discussion and refactor plan
- Phase 22.8A: converted all slash commands to buttons
- Mode switching system design for food/BP logging
- Phase 22.8B, 22.8C design planning

Decisions Made
- Ollama and Qdrant are built-in to n8n — no community nodes needed
- Community nodes install method: Settings → Community Nodes UI only. Dockerfile npm install breaks due to n8n pnpm catalog internals
- 3 community nodes installed: @mendable/n8n-nodes-firecrawl v2.1.1, n8n-nodes-puppeteer v1.5.0, n8n-nodes-tesseractjs v1.5.1
- Mode switching system: food_logging mode and bp_logging mode via gilgamesh_session_state Data Table
- /update command mode deferred — will implement later
- All parameterless slash commands converted to inline keyboard buttons
- Health submenu added to main menu with 4 placeholder buttons
- Homelab submenu: Alerts and Backup buttons added
- Gilgamesh submenu: Memory, Cost, Sync Docs, Clear buttons added
- chat_id bug fixed across Format Memory, Format Cost, Format Alerts, Format Backup, Get Chat ID, Trigger Sync Docs — all now handle both message and callback_query contexts using: $('Telegram Trigger').first().json.callback_query?.message?.chat?.id || $('Telegram Trigger').first().json.message?.chat?.id
- Temps button broken — pre-existing SSH bug, deferred
- Send Health node pattern: must use HTTP Request node (same as Send Gilgamesh) not Telegram node, because inline keyboard requires JSON.stringify reply_markup via direct Telegram API call

Changes to AI-CONTEXT.md
- n8n community nodes section: update installed nodes to include @mendable/n8n-nodes-firecrawl v2.1.1, n8n-nodes-puppeteer v1.5.0, n8n-nodes-tesseractjs v1.5.1. Install method: UI only.
- Inline Keyboard Menu Status: all submenus now fully working including Health (placeholder), Gilgamesh (Memory, Cost, Sync Docs, Clear), Homelab (Alerts, Backup added)
- Known bug: Homelab → Temps SSH command returns no output — deferred
- n8n built-in nodes note: Ollama Chat Model, Ollama Embeddings, Qdrant Vector Store are all built-in — no community node needed
- Phase 22.8A marked complete April 27, 2026
- Pending: Phase 22.8B (health tracking build), Phase 22.8C (homepage widgets)
- Add Deferred task: Fix Homelab → Temps SSH bug

Errors & Resolutions
- Dockerfile npm install -g: EUNSUPPORTEDPROTOCOL — n8n uses pnpm catalog internally. Fix: use UI installer instead
- npm install into /usr/local/lib/node_modules/n8n: same error. Fix: UI only
- Send Health node showing no keyboard: used Telegram node instead of HTTP Request. Fix: duplicate Send Gilgamesh (HTTP Request pattern) instead
- chat_id undefined on button callbacks: all Format/handler nodes used $json.message.chat.id which fails for callback_query. Fix: use callback_query?.message?.chat?.id || message?.chat?.id pattern

Action Items
- [ ] Fix Homelab → Temps SSH bug (deferred, separate session)
- [ ] Begin Phase 22.8B: health tracking (food log, BP, medication, daily summary)
- [ ] Create gilgamesh_session_state Data Table for mode switching
- [ ] Create health Data Tables: health_food_log, health_bp_log, health_medication_log
- [ ] Phase 22.8C: homepage health + homelab widgets via n8n webhooks (after 22.8B)

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
- Completed Obsidian 22.1: 10 folders, HOME note, \_index files, subfolders finance/grocery/health/subscriptions
- Completed Obsidian 22.2: Daily Note Creator midnight, Morning Briefing 7am, /daily command
- Planned Phase 22.9: reminders + voice notes Claude API + notebook photo vision
- Planned Phase 22.11: Nextcloud Calendar CalDAV integration

Decisions Made
- Memory alert threshold: 80% → 85%
- Ollama tokens: read data.input_tokens / data.output_tokens (already mapped by Call Ollama)
- command_type: derived from Telegram message text directly
- Midas webhook: midas-report, USD to MYR: 4.7, spend limit: $10
- SSL check: hardcoded July 14 2026 expiry — revisit in MERLIN v2
- Backup restore test baseline: 2026-01-01 (never tested)
- MERLIN reads nodes directly by name ($('Check Prometheus Memory')) not merged items
- Midas v2: revisit in future session for missing design discussion
- Obsidian vault: 10 folders + HOME master index + 7 \_index files
- Daily notes: midnight creation via n8n, 7am morning briefing, /daily command
- Phase 22.9 and 22.11 planned for future expansion

Changes to AI-CONTEXT.md
- n8n workflows: add Midas CFO Report (webhook midas-report), Midas Daily Brief (9am), MERLIN Reminders (8am), Daily Note Creator (midnight), Morning Briefing (7am) — total now 12 workflows
- gilgamesh_costs schema: command_type column added (Text)
- Prometheus: HighMemoryUsage threshold now 85%
- Key Lessons: add Loki scrape job fix, Ollama token field names fix
- Windrose: ~9GB RAM baseline — stop container when not playing
- Agent roster: Midas ✅ Active, MERLIN ✅ Active
- Obsidian vault structure updated — 10 folders, HOME note, \_index files
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

---

*Last updated: May 10, 2026 — Update this file at the end of each session before pushing to GitHub*