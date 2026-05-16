# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** May 16, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. Phase 24.7 (ntfy), 24.1 (Firefly III), and 24.8 (Langfuse) complete. Nextcloud Deck integration complete with Da Vinci project management. Hardware upgraded to 128GB DDR4 with 3-tier storage architecture. 20 LXC containers + 1 KVM VM deployed. Da Vinci Stage 2 (RAG) complete with Qdrant + nomic embeddings. Phase 7E (Extended Memory) complete with conversation archival. Pelican panel migration complete with Minecraft/Terraria split.

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
- **Conversation history** → 15 messages (gilgamesh_memory)
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
| Proxmox Server   | Kuromoon | Ryzen 5 5600X, 128GB DDR4-3200, RX 6700 XT 12GB | 192.168.10.5   | Hypervisor                        |
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

### RAM Upgrade (Complete - May 16, 2026)

- **Installed:** 4x32GB Corsair Vengeance LPX DDR4-3200 (128GB total)
- **Speed:** DDR4-3200 MT/s confirmed (manual BIOS tuning from 2666 to rated speed)
- **Purpose:** VM/LXC density, large AI models, multiple workloads

### Storage Architecture (3-Tier, Complete - May 16, 2026)

| Tier | Storage    | Device              | Capacity | Mount Point       | Purpose                          |
|------|------------|---------------------|----------|-------------------|----------------------------------|
| 1    | local-zfs  | NVMe ZFS RAID1      | 770GB    | /                | OS, VM disks, fast containers     |
| 2    | ssd-storage| WD Green 1TB SATA   | 927GB    | /dev/sdd         | Rootdir/images, medium workloads  |
| 3    | hdd-backup | 2x8TB SATA HDD      | 14.6TB   | /mnt/hdd-backup-* | Backups, Nextcloud data          |

### Health Sensors

- **Lepulse scale** → Fitdays app → Samsung Health → Health Connect bridge
- **Samsung Watch** → Samsung Health → Health Connect bridge
- **Data flow:** Sensors → Samsung Health → Health Connect webhook bridge → n8n → Obsidian + Data Tables

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

> **Note:** CTID 201–305 are LXC containers. VMID 400 is a KVM virtual machine with PCIe GPU passthrough — it is NOT an LXC container.

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
| 221 | LXC  | finance-firefly         | 192.168.30.224 | finance.najhin-gaming.com     | ✅         | ✅ Running |
| 222 | LXC  | notification-ntfy       | 192.168.30.222 | ntfy.najhin-gaming.com        | ✅         | ✅ Running |
| 223 | LXC  | observability-langfuse  | 192.168.30.223 | langfuse.najhin-gaming.com    | ✅         | ✅ Running |
| 302 | LXC  | gaming-wings-1          | 192.168.30.212 | —                             | ✅         | ✅ Running |
| 303 | LXC  | minecraft-server        | 192.168.30.215 | —                             | ✅         | ✅ Running |
| 304 | LXC  | terraria-server         | 192.168.30.216 | —                             | ✅         | ✅ Running |
| 305 | LXC  | gaming-panel-pelican    | 192.168.30.217 | —                             | ✅         | ✅ Running |
| 400 | VM   | ollama-gpu              | 192.168.30.221 | ollama.najhin-gaming.com      | ✅         | ✅ Running |

**Current Total: 20 LXC containers + 1 KVM VM**
**Planned Total: 20 LXC containers + 1 KVM VM (all on VLAN 30, all autostart enabled)**

### Pulse Dashboard (CT 208)

- **Container:** rcourtman/pulse:latest on port 7655 (resized to 8GB storage)
- **Purpose:** Proxmox-native monitoring dashboard replacing Homepage
- **Features:** Real-time metrics, container status, zero configuration required
- **Agents:** Installed on Kuromoon (Proxmox host) and CT 302 (Docker monitoring)
- **Access:** home.najhin-gaming.com (Websockets Support enabled in NPM)
- **Network:** pfSense rules added for CT 208 → Proxmox API (8006), Kuromoon → CT 208 (7655)

### Nextcloud Hub (CT 220)

- **Root disk:** 100GB (resized from 20GB to resolve quota issues)
- **Data directory:** /mnt/ncdata (bind mount from /mnt/hdd-backup-1/nextcloud-data)
- **Version:** 33.0.2
- **Storage:** Data on Tier 3 HDD, app on NVMe (1.3GB after migration)
- **WebDAV base URL:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **Enabled apps:** Deck (project management), Tasks, Calendar
- **Backup:** rsync to Kinmoon NAS nightly at 03:00

### Firefly III (CT 221)

- **Container:** fireflyiii/core:latest + MariaDB backend
- **IP:** 192.168.30.224 (NOTE: not .221, which is VM 400)
- **Purpose:** Personal finance tracking (MYR currency)
- **Access:** finance.najhin-gaming.com (Cloudflare Access + Email OTP)
- **API:** Token stored in Vaultwarden (name: gilgamesh)

### ntfy (CT 222)

- **Container:** binwiederhier/ntfy:latest on port 2586
- **Purpose:** Universal notification hub for all agents
- **Auth:** Built-in username/password (not Cloudflare Access for phone app compatibility)
- **Access:** ntfy.najhin-gaming.com (Cloudflare Tunnel without Access)
- **Integration:** Uptime Kuma → ntfy alerts configured

### Langfuse (CT 223)

- **Stack:** Langfuse v3.174.1 (6-container stack: web, worker, ClickHouse, PostgreSQL, Redis, MinIO)
- **Container specs:** 2 cores, 4GB RAM, 16GB disk
- **Purpose:** LLM observability and performance tracking
- **Access:** langfuse.najhin-gaming.com (Cloudflare Access + Email OTP)
- **Organization:** Kuromoon, Project: Gilgamesh Agents
- **Internal URL:** http://192.168.30.223:3000
- **Health endpoint:** /api/public/health

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (0d:00.0) + audio (0d:00.1) passed through
- **PCIe config:** hostpci0=0000:0d:00.0,pcie=1,rombar=0 hostpci1=0000:0d:00.1,pcie=1
- **OS:** Ubuntu 22.04
- **SSH:** `ssh muzakkir@192.168.30.221` — use `muzakkir` user, not root
- **Ollama models:** qwen3:14b (primary, 9.3GB), llama3.2:latest (secondary, 3B), nomic-embed-text (768 dims)
- **Open WebUI:** Docker container, connects via 172.17.0.1:11434
- **Qdrant:** Port 6333 (REST), 6334 (gRPC), storage at /opt/qdrant/storage

### Gaming Servers

- **Pelican Panel (CT 305):** Next-gen game server panel (192.168.30.217), SQLite + Redis + PHP 8.3
- **Minecraft Server (CT 303):** Paper 1.21.4 on port 25570 (192.168.30.215), Pelican Wings
- **Terraria Server (CT 304):** tModLoader on port 7777 (192.168.30.216), Pelican Wings
- **Windrose (CT 302):** Deployed at /opt/windrose, 4 max players, Medium difficulty, invite code NAJHINWINDROSE

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
- **Phase 7E** — Extended Memory (20+ message conversations via RAG) ✅ COMPLETE

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

### Phase 25.3 — Self-Evolving Skills Learning Loop

Inspiration: OpenClaw skills system + Hermes Agent closed learning loop
Concept: Agents that get smarter the longer they run by capturing solutions as reusable knowledge

Loop: detect → extract → store → recall → refine
1. Detection — After multi-step resolution or user correction, agent flags "this was hard/new"
2. Skill extraction — Agent generates structured procedure doc (problem, context, solution steps, gotchas)
3. Storage — Da Vinci writes to Obsidian (03-knowledge/skills/) and embeds in Qdrant
4. Recall — On similar future queries, RAG retrieves skill doc and injects into agent context
5. Refinement — If skill gets corrected or improved, updated version overwrites original

Dependencies: Da Vinci Stage 2 (Qdrant) → Phase 7E (extended memory) → Phase 24.4 (knowledge ingestion)
Effort: 10-12h
Architecture: Knowledge documents via RAG, not executable code. Fits Da Vinci-as-sole-writer. Executable skills = future EMIYA extension.

Examples of auto-generated skills:
- n8n Data Table Upsert always inserting → Conditions section gotcha
- WebDAV append pattern → GET-then-PUT code template
- Mixed RAM validation → memtest86+ procedure
- New LXC SSH access → pfSense rule template

Note: Key Lessons section in AI-CONTEXT.md is the manual version of this — Phase 25.3 automates that capture.

---

## 🎴 Fate Grand Order Agent Ecosystem

Theme: Homelab agents named after Fate/Grand Order servants. Final roster locked April 25, 2026.

### Active Agents

| Servant      | Class  | Role                                                                          | Platform                      | Status                     |
|--------------|--------|-------------------------------------------------------------------------------|-------------------------------|----------------------------|
| Gilgamesh 👑 | Archer | Life Interface & Personal AI Assistant                                        | Telegram (@JhinGilgamesh_bot) | ✅ Active                   |
| Da Vinci 🎨  | Caster | Sync + Goals + Review (Stage 2 RAG active; Stage 1 doc pipeline active)     | n8n/Nextcloud                 | ⚡ Partial — Stage 2 active |
| Midas 💰     | Caster | CFO — Cost Tracking & Optimization                                            | n8n                           | ✅ Active                   |
| MERLIN 🔮    | Caster | Proactive Nudges & Scheduler                                                  | n8n                           | ✅ Active                   |

### Final 6-Agent Roster (Redesigned)

| Servant             | Class    | Role                                          | Platform      | Build Order | Status     |
|---------------------|----------|-----------------------------------------------|---------------|-------------|------------|
| Gilgamesh 👑        | Archer   | Life Interface & Personal AI Assistant        | Telegram      | —           | ✅ Active   |
| Da Vinci 🎨         | Caster   | Sync + Goals + Weekly Review + RAG           | n8n/Nextcloud | —           | ⚡ Partial  |
| Midas 💰            | Caster   | CFO — Cost Tracking & Optimization            | n8n           | 1st         | ✅ Active   |
| MERLIN 🔮           | Caster   | Proactive Nudges & Health Scheduler           | n8n           | 2nd         | ✅ Active   |
| EMIYA 🏹            | Archer   | CTO — Infrastructure + Agent Spawning         | n8n           | 3rd         | 📋 Planned |
| Guardian 🛡         | —        | Security Monitoring & Threat Detection        | n8n           | 4th         | 📋 Planned |

**Notes:**

- Roster reduced from 9 to 6 agents for focused deployment
- Build order is firm: Midas complete, MERLIN complete, then EMIYA (24.1-24.8), Guardian

### Da Vinci — Sync + Goals + Weekly Review + RAG

**Role scope:**

- Documentation: maintains AI-CONTEXT.md, changelog.md, troubleshoot.md via /update and /sync-docs
- Obsidian writes: session summaries written to vault via Nextcloud WebDAV
- Goal tracking: monitors progress, identifies blockers, suggests next steps
- Weekly review: Sunday analysis of week's progress, agent performance, system health
- RAG retrieval (Stage 2): Qdrant + nomic-embed-text for knowledge recall across all agents
- Observability logs: all agents log activity to Da Vinci
- Kanban management: Nextcloud Deck card creation and status updates

**Stage 2 stack:** Qdrant (VM 400, port 6333/6334) + nomic-embed-text + n8n Knowledge Indexer workflow

**Collections:**
- obsidian_knowledge: 1567 points (9 indexed folders)
- gilgamesh_conversations: 1 point (conversation archival)

**Pronouns:** she/her

**Technical notes:**

- Direct node references required: use `$('Extract Response').first().json.updatedDoc` instead of `$json` after Merge node
- max_tokens set to 32000 to prevent AI-CONTEXT.md being replaced with hallucinated content
- Grounding fix pending: add step to fetch current AI-CONTEXT.md from Nextcloud before Claude API call
- Deck integration complete with 6 additional nodes for kanban management
- Da Vinci workflow includes Limit node before Notify Complete to prevent notification spam
- Knowledge Indexer runs at 3am daily (full re-index v1)

### Nextcloud Deck Integration

Da Vinci integrates with Nextcloud Deck for kanban project management:

**Boards:** 3 boards (Homelab ID: 4, Career ID: 5, Personal ID: 6)
**Stacks:** Backlog → Up Next → Blocked → In Progress → Done
**Labels:** 22 on Homelab (category, priority, agent, effort), 9 on Career/Personal
**Behavior:** Auto-create/complete cards, advisory comments for movement
**API:** http://192.168.30.220/index.php/apps/deck/api/v1.0
**Auth:** Basic auth with app password (credential: NextCloud-Deck)

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

## 🤖 Gilgamesh AI Agent (Phase 7C/7D/7D-Menu/15/41/Da Vinci Stage 2/Phase 7E)

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
                                          RAG Retrieval (Qdrant → nomic-embed-text)
                                                    ↓
                                          Conversation Archival (30+ rows → Qdrant)
                                                    ↓
                                          /update → Da Vinci → Nextcloud → GitHub
                                          /sync-docs → Full Doc Pipeline
```

### Smart Routing + RAG + Extended Memory (Phase 41 + Da Vinci Stage 2 + Phase 7E — COMPLETE)

- **Ollama qwen3:14b** — Primary route for simple queries (local, free, fast)
- **Haiku 4.5** — Fallback if Ollama is down or unavailable
- **Sonnet 4** — Complex queries: triggered by complexity keywords or messages over 50 words
- **RAG Retrieval** — Qdrant vector search with nomic-embed-text embeddings (768 dims)
- **Extended Memory** — Conversation archival when history exceeds 30 rows
- Routing logic runs in a Route Check If node before calling any model
- RAG skipped for greetings (<10 chars or regex match)
- /update messages filtered from conversation history

### Features

- **Conversation memory:** Last 15 messages stored in n8n Data Tables (reduced from 20 for RAG integration)
- **Extended memory:** Conversations archived to Qdrant at 30+ rows
- **Smart routing:** Ollama (local, primary) → Haiku (fallback) → Sonnet (complex)
- **RAG knowledge recall:** Queries Qdrant for relevant Obsidian content + conversations
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
| /clear     | Memory reset      | Clears conversation memory (last 15 messages) |
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
| Embedding Model | nomic-embed-text (768 dims, VM 400)                    |
| Haiku Model   | claude-haiku-4-5-20251001 (Ollama fallback)               |
| Sonnet Model  | claude-sonnet-4-20250514 (complex queries)                |
| n8n Container | CT 211, 192.168.30.211                                    |
| Qdrant        | VM 400, ports 6333 (REST) + 6334 (gRPC)                  |
| Proxmox API   | root@pam!gilgamesh token                                  |
| Proxmox node  | muzakkir (not kuromoon)                                   |

### Cost Tracking Note

- cost_usd calculated from token rates: Sonnet $3/$15 per 1M input/output tokens, Haiku $0.80/$1 per 1M tokens
- Ollama tokens captured: data.input_tokens + data.output_tokens from Call Ollama node response
- Ollama queries cost $0 (local inference)
- command_type derived from Telegram message text directly

### n8n Workflows (Count: 14)

| Workflow                           | Purpose                                  | Nodes | Trigger  |
|------------------------------------|------------------------------------------|-------|----------|
| Telegram Agent                     | Main bot, menu, commands, hybrid routing, RAG, extended memory | 18+ | Telegram |
| Documentation Pipeline — Update    | Session summary → 3 files                | 7     | Webhook  |
| Documentation Pipeline — Sync Docs | Full doc regeneration → 7 files          | 7     | Webhook  |
| Da Vinci Documentation Pipeline    | Raw staging summaries → formatted docs + Deck | 17+ | Webhook |
| Da Vinci — Knowledge Indexer       | Obsidian → Qdrant indexing (3am daily)   | 8     | Schedule |
| Midas — CFO Report                 | /midas command cost analysis             | 6     | Webhook  |
| Midas — Daily Brief                | 9am scheduled cost summary               | 4     | Schedule |
| MERLIN — Reminders                 | 8am daily infrastructure checks          | 8     | Schedule |
| Daily Note Creator                 | Midnight Obsidian daily notes            | 4     | Schedule |
| Morning Briefing                   | 7am daily summary to Telegram            | 5     | Schedule |
| Update Nextcloud File              | Legacy (unpublished)                     | 5     | Webhook  |
| Push to GitHub                     | Legacy (unpublished)                     | 4     | Webhook  |

> Note: Hybrid routing (Phase 41), RAG retrieval (Da Vinci Stage 2), and Extended Memory (Phase 7E) are all integrated into the Telegram Agent workflow. Health tracking (Phase 22.8B) is also integrated.

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
| gilgamesh_memory         | Conversation history (last 15 messages)         | id, timestamp, user, message, response |
| gilgamesh_costs          | Token usage and cost tracking                   | id, timestamp, model, tokens, cost_usd, command_type |
| gilgamesh_session_state  | Mode switching for health tracking              | chat_id, mode, updated_at            |
| health_food_log          | Food logging entries                            | id, logged_at, food_item, notes, chat_id |
| health_bp_log            | Blood pressure readings                         | id, logged_at, systolic, diastolic, notes, chat_id |
| health_med_log           | Medication tracking                             | id, logged_at, medication, dosage, notes, chat_id |

### RAG System (Da Vinci Stage 2)

| Component | Details |
|-----------|---------|
| **Vector DB** | Qdrant on VM 400 (http://192.168.30.221:6333) |
| **Embeddings** | nomic-embed-text (768 dimensions) |
| **Collections** | obsidian_knowledge (1567 points), gilgamesh_conversations (1 point) |
| **Indexed Folders** | 00-inbox, 01-homelab, 02-career, 03-knowledge, 07-daily, 08-projects, 09-meetings, 10-reference, AI-Stuff/Homelab/homelab-infrastructure |
| **Excluded** | 04-personal/ (privacy), 05-templates/, 06-archive/ |
| **Indexing** | Daily 3am via "Da Vinci — Knowledge Indexer" workflow (full re-index v1) |
| **Retrieval** | Top 5 results, similarity threshold 0.7, injected into system prompt |

### SSH & API Access

- **SSH to Kuromoon:** CT 211 → 192.168.10.5:22 (pfSense rule added)
- **SSH to VM 400:** `ssh muzakkir@192.168.30.221` — use muzakkir user, not root
- **SSH to CT 302:** Password auth enabled (PermitRootLogin yes)
- **Proxmox API:** root@pam!gilgamesh token
- **Storage IDs:** kinmoon-nfs (not kinmoon-smb in Proxmox)

### Known Constraints

- Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body
- Progress bars use ASCII (= and -) — Unicode block chars don't render on Telegram mobile
- RAG payload field is "content" (not "text" or "pageContent")

---

## 🔧 Documentation Pipeline (Phase 16.1/16.2/16.3)

### Architecture

```
Session Summary → /update → Da Vinci workflow → Claude API → AI-CONTEXT.md + changelog.md + troubleshoot.md
                                                                     ↓
                                                              Nextcloud + GitHub + Deck
```

#### Da Vinci Documentation Pipeline (Phase 16.3)

```
Raw summaries → AI-CONTEXT-staging.md (rolling append)
                         ↓
               Da Vinci n8n workflow → Claude API (max_tokens: 32000)
                         ↓
               Formatted AI-CONTEXT.md → Nextcloud + GitHub + Deck kanban
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
| Deck         | Kanban cards created from action items              |

### File Coverage (sync-docs)

- AI-CONTEXT.md, changelog.md, troubleshoot.md (from /update)
- README.md, roadmap.md, current-state.md, service-catalog.md

### Da Vinci Technical Notes

- **Direct node references:** Use `$('Extract Response').first().json.updatedDoc` — do NOT rely on `$json` after Merge node
- **max_tokens:** 32000 (prevents AI-CONTEXT.md being replaced with hallucinated content)
- **Grounding fix pending:** Add step to fetch current AI-CONTEXT.md from Nextcloud before Claude API call
- **Deck integration:** Extended workflow with 6 additional nodes for Deck kanban management (Parse Deck Actions, If, Fetch Homelab Stacks, Build Deck Requests, Loop Deck Actions, Execute Deck Action)
- **Notification fix:** Limit node (max 1) added before Notify Complete to prevent notification spam

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
| Homelab-Ntfy       | ntfy CT 222      | Uptime Kuma CT 206  | ✅ Active | Service down alerts via ntfy                                   |
| Nextcloud Deck     | Deck CT 220      | n8n CT 211          | ✅ Active | Kanban project management via Da Vinci                         |

**Planned:** Migrate Alertmanager alerts to route through n8n first (central hub). Game server notifications to Discord via n8n.

### Nextcloud Deck Integration

| Component | Details |
|-----------|---------|
| **App** | Deck on CT 220 (cloud.najhin-gaming.com) |
| **Boards** | 3 boards: Homelab (ID: 4), Career (ID: 5), Personal (ID: 6) |
| **Stacks** | 5 stacks per board: Backlog, Up Next, Blocked, In Progress, Done |
| **Labels** | 22 labels on Homelab (category, priority, agent, effort), 9 on Career/Personal |
| **API Base** | http://192.168.30.220/index.php/apps/deck/api/v1.0 |
| **Auth** | Basic auth with app password (credential: NextCloud-Deck) |

---

## 💾 Backup Strategy (Phase 7A + Hardware Upgrade)

### Backup Jobs

| Job              | Schedule | Storage                  | Containers                                  | Retention                    |
|------------------|----------|--------------------------|---------------------------------------------|------------------------------|
| Nightly Backup   | 02:00    | hdd-backup-1 (Local HDD) | All 20 LXC containers + VM 400            | 7 daily, 4 weekly           |
| Nextcloud Data   | 03:00    | Kinmoon NAS (rsync)      | /mnt/hdd-backup-1/nextcloud-data           | Mirror sync                  |

### Storage

| Name         | Type       | Protocol | Capacity | Purpose                 |
|--------------|------------|----------|----------|-------------------------|
| hdd-backup-1 | Local HDD  | Direct   | 7.3 TB   | Proxmox backups + Nextcloud data |
| hdd-backup-2 | Local HDD  | Direct   | 7.3 TB   | Future backup expansion |
| kinmoon-smb  | NAS RAID 1 | SMB/CIFS | 2.6 TB   | Off-site Nextcloud data |
| kinmoon-nfs  | NAS RAID 1 | NFS      | 2.6 TB   | Legacy storage         |
| local-zfs    | NVMe RAID1 | ZFS      | 770 GB   | OS, VM disks, fast containers |
| ssd-storage  | SATA SSD   | LVM-Thin | 927 GB   | Medium workload storage |

### Backup Scripts

- **Proxmox vzdump:** Built-in backup job to /mnt/hdd-backup-1
- **Nextcloud rsync:** /usr/local/bin/backup-nextcloud.sh, logs to /var/log/nextcloud-backup.log

### NAS Configuration

- **Model:** UGREEN DXP2800 (2-bay)
- **RAID:** RAID 1 (mirrored drives for redundancy)
- **Filesystem:** ext4
- **SMB share:** proxmox-backups (username: Muzakkir, password rotated May 16)
- **NFS export:** /volume1/proxmox-backups
- **Status:** Normal (healthy, drives synced)
- **Network:** 192.168.10.100

---

## 🔒 Security Architecture

| Layer           | Implementation                                                                                                                                                                                     |
|-----------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Perimeter       | ISP Router → pfSense firewall                                                                                                                                                                      |
| Segmentation    | 5 VLANs with enforced firewall rules                                                                                                                                                               |
| DNS             | Pi-hole ad/tracker blocking (~489K domains)                                                                                                                                                        |
| VPN             | Tailscale (subnet router on pfSense, primary access)                                                                                                                                               |
| External Auth   | Cloudflare Access (Email OTP, muzakkir.kholil06@gmail.com only) for Grafana, n8n, Vault, Vaultwarden, Ollama, Nextcloud, Firefly III, Langfuse (8 apps total)                                           |
| External Access | Cloudflare Tunnel for all external services                                                                                                                                                        |
| Admin Access    | Tailscale only (VLAN 20 blocked from VLAN 10)                                                                                                                                                      |
| Backup          | Automated daily backups with 7/4 retention                                                                                                                                                         |
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
| 7E      | Extended Memory (Conversation Archival)                          | ✅ Complete | May 15, 2026 |
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
| 24.1    | App Management — Firefly III deployment                          | ✅ Complete | May 13, 2026 |
| 24.2    | Alert Translation — Alertmanager → ntfy plain English            | 📋 Planned | —            |
| 24.3    | Container Updates — Docker + apt updates (approval-gated)        | 📋 Planned | —            |
| 24.4    | Knowledge Ingestion — URLs → Firecrawl → triple-write pipeline   | 📋 Planned | —            |
| 24.5    | Proactive Monitoring — anomaly detection + proactive nudges      | 📋 Planned | —            |
| 24.6    | Performance + Goal Optimization — resource + goal tracking       | 📋 Planned | —            |
| 24.7    | Universal Notifications — ntfy hub deployment                    | ✅ Complete | May 13, 2026 |
| 24.8    | Langfuse AI Observability                                        | ✅ Complete | May 14, 2026 |
| 25.1    | Voice Interface — Claude API transcription + voice responses     | 📋 Planned | —            |
| 25.2    | Email Integration — IMAP monitoring + smart filtering            | 📋 Planned | —            |
| 25.3    | Self-Evolving Skills — Hermes-style learning loop (knowledge docs + RAG recall) | 📋 Planned | —            |
| 25.4    | Content Creation — automated reports, social posts              | 📋 Planned | —            |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                             | ✅ Complete | Apr 24, 2026 |
| 39      | Open WebUI                                                       | ✅ Complete | Apr 24, 2026 |
| 41      | Gilgamesh + Ollama Hybrid Routing                                | ✅ Complete | Apr 24, 2026 |
| 58      | Windrose Server Deployment                                       | ✅ Complete | Apr 19, 2026 |
| Midas   | Midas CFO Agent                                                  | ✅ Complete | Apr 27, 2026 |
| MERLIN  | MERLIN Reminders Agent                                           | ✅ Complete | Apr 27, 2026 |
| Pulse   | Pulse Dashboard Deployment                                       | ✅ Complete | May 10, 2026 |
| Deck    | Nextcloud Deck + Da Vinci Integration                            | ✅ Complete | May 14, 2026 |
| DV-S2   | Da Vinci Stage 2 (RAG System)                                   | ✅ Complete | May 15, 2026 |
| HW-128  | Hardware Upgrade (128GB RAM + 3-Tier Storage)                    | ✅ Complete | May 16, 2026 |
| Pelican | Pelican Panel Migration                                          | ✅ Complete | May 16, 2026 |

---

## 🐛 Key Lessons Learned

### Hardware & Recovery

| Issue                                   | Resolution                                                  |
|-----------------------------------------|-------------------------------------------------------------|
| GPU PCIe address change after motherboard | Update VM config: qm set 400 --hostpci0 0000:0d:00.0,pcie=1 |
| KVM virtualization not available       | Enable AMD-V (SVM Mode) and IOMMU in UEFI settings         |
| Boot timeout after BIOS changes        | Disable systemd-networkd-wait-online.service               |
| RAID conversion with active mount       | Unmount NFS share before NAS factory reset                 |
| DOCP 3600 failed POST                  | Manual DDR4-2666 setting negotiated to rated DDR4-3200     |
| Enterprise repo 401 errors             | Disable pve-enterprise.list/sources — add pve-no-subscription.list |

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
| CT 220 startup failed after mp0 | Mount directory didn't exist — fixed with mkdir -p  |
| rsync Permission denied      | LXC UID mapping — www-data (UID 33) maps to UID 100033 |

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
| Cloudflare Access blocks apps         | Apps needing persistent connections (ntfy, websockets) should use native auth not Cloudflare Access                |
| Deck API assignLabel                  | PUT /assignLabel (no label ID in URL), body {"labelId": X}                                                         |
| Deck API card update                  | requires owner field or returns 400; omitting description wipes it                                                |
| Deck API headers                      | OCS-APIRequest and Accept headers required on all calls                                                            |
| Nextcloud API auth                    | app password required, regular password returns "not logged in"                                                    |
| Internal URL bypasses Cloudflare     | http://192.168.30.220 (port 80 only, no SSL)                                                                      |
| Loop Over Items done output spam      | "done" output emits ALL processed items, not summary — add Limit node before notifications                        |
| PROPFIND not in n8n HTTP Request     | Use Code node with this.helpers.httpRequest() instead                                                              |
| Information Extractor JSON parsing    | Graceful error handling with Continue on Error, null checks in Add Timestamps                                      |
| RAG empty results                     | Payload field is "content" not "text/pageContent" — check Qdrant schema                                            |
| search_document/query prefixes        | Remove prefixes — incompatible with chunked content retrieval                                                      |

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
| Proxmox unreachable alerts | node exporter missing after reinstall — installed prometheus-node-exporter |

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

### Langfuse Deployment

| Issue                               | Resolution                                                                   |
|-------------------------------------|------------------------------------------------------------------------------|
| Langfuse auth redirect to localhost:3000 after signup | NEXTAUTH_URL was set to http://localhost:3000. Fix: change to https://langfuse.najhin-gaming.com in docker-compose.yml, docker compose down && up -d. |

### Qdrant & RAG

| Issue                               | Resolution                                                                   |
|-------------------------------------|------------------------------------------------------------------------------|
| PROPFIND not available in n8n HTTP Request | Use Code node with this.helpers.httpRequest() for WebDAV operations |
| 401 on Nextcloud WebDAV            | Create Nextcloud app password, store in Vaultwarden                         |
| Information Extractor JSON parsing failure | Set On Error to Continue, handle null gracefully in Add Timestamps |
| RAG returning empty results         | Payload field was "content" not "text/pageContent" — check Qdrant collection schema |
| search_document/query prefixes      | Removed both — incompatible with chunked content retrieval                  |
| crypto.randomUUID() unavailable     | Used Date.now() * 100 + i for Qdrant point IDs                             |
| Qdrant 400 on insert               | Point IDs must be UUID or unsigned integer                                  |
| /update messages in history         | Filter with !startsWith('/update') to prevent command pollution             |

### Pelican Panel Migration

| Issue                               | Resolution                                                                   |
|-------------------------------------|------------------------------------------------------------------------------|
| CT create with local-lvm failed     | Use local-zfs storage instead (updated after hardware migration)            |
| Sury PHP repo 418 error             | Add sury repo manually via apt.gpg key                                      |
| Wings binary wrong architecture     | Use pelican-dev/wings, not pterodactyl/wings — different config paths       |
| Wings token typo from screenshot    | Use Auto Deploy Command instead of manual token entry                       |
| Egg import 500 error                | chown -R www-data on storage/framework/cache for write permissions          |
| Queue worker not processing jobs    | Pelican uses 'default' queue, not 'high,standard,low'                       |
| Double APP_INSTALLED in .env        | Use grep -v to remove duplicates before appending                           |

---

## ❓ Pending Tasks

### Immediate

| Task                                                                                        | Priority |
|---------------------------------------------------------------------------------------------|----------|
| Wire Langfuse into n8n Gilgamesh workflow using built-in Langfuse node                     | High     |
| Wire Langfuse into Da Vinci, Midas, MERLIN workflows                                       | High     |
| Store Langfuse API keys (public + secret) in Vaultwarden                                   | High     |
| Add Uptime Kuma monitor for Langfuse (http://192.168.30.223:3000/api/public/health)        | High     |
| Add Uptime Kuma monitor for Qdrant (port 6333)                                             | High     |
| Update morning briefing container count (18 → 20)                                          | High     |
| Add NPM proxy host for Pelican panel (gaming.najhin-gaming.com or panel.najhin-gaming.com) | High     |
| Add Uptime Kuma monitors for CT 303, CT 304, CT 305                                         | High     |
| Update vzdump backup jobs to include CT 303, CT 304, CT 305                                 | High     |
| Schedule backup restore test (CT 207 recommended), update lastTestDate in MERLIN            | High     |
| Fix Cloudflare API token in Vault (get full token from dashboard, re-store)                 | High     |
| Activate MERLIN workflow (toggle on)                                                        | High     |
| Monitor vzdump backup run tonight at 02:00 — verify in Proxmox backup log                   | High     |
| Monitor nextcloud rsync run tonight at 03:00 — check /var/log/nextcloud-backup.log          | High     |
| Replace Kinmoon NAS drive (~RM 400-500)                                                     | High     |
| Monitor Windrose RAM usage — stop Docker container when not playing                         | High     |
| Update subscription costs in Obsidian when known (YouTube Premium, Cloudflare Domain, TIME) | Medium   |
| Fix /update Telegram message too long (shorten Da Vinci notification)                       | Medium   |

### Infrastructure

| Task                                                                         | Priority |
|------------------------------------------------------------------------------|----------|
| Migrate mc.najhin-gaming.com DNS/port forward from old CT 302 IP to CT 303 IP | High   |
| Migrate terraria.najhin-gaming.com DNS/port forward to CT 304 IP             | High     |
| Install Calamity mods on CT 304 (currently fresh tModLoader)                 | Medium   |
| Fix morning briefing 0/0 container count (wrong node name in Proxmox API call) | Medium |
| Remove Samsung 750 EVO after 1 week of stability (around May 22)             | Medium   |
| Address local-zfs thin pool overprovisioning (during infrastructure cleanup) | Medium   |
| Fix container-Inventory.md 404 (delete and re-upload with lowercase name)    | Medium   |
| Fix Information Extractor backtick JSON parsing properly                     | Medium   |
| Begin Phase 24.2 (Alert Translation) next session                           | High     |
| Create muzakkir97/homelab-private repo                                       | Medium   |
| Set up Backblaze B2 account                                                  | Medium   |
| Phase 27 (Vault + n8n) enables n8n to fetch secrets directly                 | Medium   |
| Consider moving VM 400 boot disk to ssd-storage                             | Medium   |
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
| Create Anthropic Admin API key for Midas cost tracking (future session)                         | Medium   |
| Wire Midas → Firefly III API (Phase 24.2, future session)                                       | Medium   |
| Add Qdrant conversation search to Format Messages (for "what did we discuss?" queries)          | Medium   |
| Investigate assistant messages saving as Null in gilgamesh_conversations                        | Medium   |
| Increase Top K or tune RAG for broad queries                                                    | Medium   |
| Update AI-CONTEXT.md, current-state.md, service-catalog.md                                      | Medium   |

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

### May 16, 2026 — Game Server Split — Pelican Panel Migration (Complete)

Date: May 16, 2026
Phase: Game Server Split — Pelican Panel Migration (Complete)

Topics Discussed
- Game server audit on CT 302: Wings managing Minecraft + Terraria, Windrose standalone
- Migration from Pterodactyl to Pelican game server panel
- CT 303 (Minecraft), CT 304 (Terraria), CT 305 (Pelican panel) created
- Pelican Wings deployed on CT 303 and CT 304
- Minecraft 1.21.4 Paper world migrated from CT 302 to CT 303
- Terraria tModLoader + Calamity mods + world migrated from CT 302 to CT 304
- CT 302 cleaned up — Wings removed, only Windrose remains
- CT 300 (Pterodactyl) decommissioned

Decisions Made
- Storage name: local-lvm → local-zfs after hardware migration
- Pelican replaces Pterodactyl as game server panel
- CT 302: Windrose only, downsized to 10GB RAM / 2 cores
- CT 303: Minecraft server, 6GB RAM / 2 cores / 192.168.30.215:25570
- CT 304: Terraria server, 12GB RAM / 2 cores / 192.168.30.216:7777
- CT 305: Pelican panel, 2GB RAM / 2 cores / 192.168.30.217
- Pelican uses SQLite (default), QUEUE_CONNECTION=database
- Queue worker must listen on default queue (not high,standard,low)
- Pelican Wings binary from pelican-dev/wings (not pterodactyl/wings)
- Wings config path: /etc/pelican/config.yml

Changes to AI-CONTEXT.md
- Storage: local-lvm → local-zfs (ZFS pool, NVMe)
- Remove CT 300 (gaming-panel, Pterodactyl) — decommissioned
- Add CT 303: minecraft-server, 192.168.30.215, Pelican Wings, 2c/6GB/30GB
- Add CT 304: terraria-server, 192.168.30.216, Pelican Wings, 2c/12GB/30GB
- Add CT 305: gaming-panel-pelican, 192.168.30.217, Pelican v1, 2c/2GB/30GB
- Update CT 302: Windrose only, 10GB RAM / 2 cores
- Gaming servers: Minecraft at .215:25570, Terraria at .216:7777, Windrose at .212:15777
- Total containers: 20 LXC + 1 VM (added 3, removed 1)
- Pelican panel: http://192.168.30.217, SQLite, queue=database
- Key Lessons: Pelican queue uses 'default' not 'high,standard,low'; Wings binary is pelican-dev/wings not pterodactyl/wings; config path is /etc/pelican/ not /etc/pterodactyl/; APP_INSTALLED must be true in .env; Auto Deploy Command more reliable than manual config

Errors & Resolutions
- local-lvm not found: use local-zfs after hardware migration
- Template not found: run pveam download local first
- PHP repo 418: add sury repo manually via gpg key
- Pelican Wings config not found: pelican-dev/wings uses /etc/pelican/ not /etc/pterodactyl/
- Wings token typo from screenshot: use Auto Deploy Command instead of manual copy
- APP_INSTALLED=false blocking API 404: append APP_INSTALLED=true to .env
- Egg import 500: chown -R www-data on storage directory
- Queue worker not processing: jobs on 'default' queue, add to worker command
- Double .env entries: use grep -v + rebuild file
- Wings restarting removed containers: stop Wings service first before removing containers

Action Items
- [ ] Add NPM proxy host for Pelican panel (gaming.najhin-gaming.com or panel.najhin-gaming.com)
- [ ] Add Uptime Kuma monitors for CT 303, CT 304, CT 305
- [ ] Update vzdump backup jobs to include CT 303, CT 304, CT 305
- [ ] Fix morning briefing 0/0 container count
- [ ] Fix /update Telegram message too long
- [ ] Migrate mc.najhin-gaming.com DNS/port forward from old CT 302 IP to CT 303 IP
- [ ] Migrate terraria.najhin-gaming.com DNS/port forward to CT 304 IP
- [ ] Install Calamity mods on CT 304 (currently fresh tModLoader)
- [ ] Update AI-CONTEXT.md via /update

### May 16, 2026 — Hardware Upgrade — Post-Migration Cleanup + Nextcloud Storage Migration

Date: May 16, 2026
Phase: Hardware Upgrade — Post-Migration Cleanup + Nextcloud Storage Migration

Topics Discussed
- SATA SSD (WD Green 1TB) installation as Tier 2 storage (ssd-storage)
- Two 8TB HDD configuration as local backup storage (hdd-backup-1, hdd-backup-2)
- Nightly backup job covering all 17 LXC + VM 400
- RAM speed confirmed DDR4-3200 MT/s after manual BIOS tuning
- Prometheus node exporter installation on Proxmox host
- NAS password rotation
- Nextcloud data directory migrated from NVMe to HDD
- Nextcloud data backup to Kinmoon via nightly rsync

Decisions Made
- ssd-storage: LVM-Thin on WD Green 1TB SATA SSD (sdd), 927GB, content: rootdir,images
- hdd-backup-1: ext4 on WD Purple 8TB (sdb), mounted at /mnt/hdd-backup-1, content: backup
- hdd-backup-2: ext4 on Seagate 8TB (sdc), mounted at /mnt/hdd-backup-2, content: backup
- Nightly vzdump backup at 02:00 to hdd-backup-1, retention: keep-daily=7, keep-weekly=4
- RAM confirmed DDR4-3200 MT/s (manual 2666 BIOS setting negotiated to rated speed)
- Proxmox enterprise repos disabled, pve-no-subscription repo added
- Nextcloud datadirectory moved from /var/www/nextcloud/data to /mnt/ncdata (HDD bind mount)
- Nextcloud data backed up to Kinmoon NAS via rsync, nightly at 03:00
- hdd-backup-1 used for both vzdump backups AND Nextcloud data — two separate directories

Changes to AI-CONTEXT.md
- Hardware → Storage: ssd-storage LVM-Thin active on /dev/sdd (WD Green 2.5" 1TB SATA)
- Hardware → Storage: hdd-backup-1 at /mnt/hdd-backup-1 (WD Purple 8TB, sdb, ext4)
- Hardware → Storage: hdd-backup-2 at /mnt/hdd-backup-2 (Seagate 8TB, sdc, ext4)
- Hardware → RAM: Confirmed DDR4-3200 MT/s on all 4x32GB sticks
- Proxmox → Repos: Enterprise repos disabled, pve-no-subscription repo active
- Proxmox → Monitoring: prometheus-node-exporter installed on host, port 9100
- Backup → Schedule: vzdump job backup-daily, 02:00, hdd-backup-1, all containers + VM 400
- Backup → Nextcloud: rsync /mnt/hdd-backup-1/nextcloud-data → Kinmoon NAS nightly at 03:00
- Backup → Script: /usr/local/bin/backup-nextcloud.sh, log: /var/log/nextcloud-backup.log
- Nextcloud → datadirectory: /mnt/ncdata (bind mount from /mnt/hdd-backup-1/nextcloud-data)
- Nextcloud → NVMe usage: 1.3GB (down from 32GB after data migration)
- Kinmoon → Password: Rotated, kinmoon-smb reconnected with new credentials

Errors & Resolutions
- Enterprise repo 401: Disabled pve-enterprise.list/sources, ceph.list/sources — added pve-no-subscription.list
- CT 220 startup failed after mp0 added: Mount directory didn't exist — fixed with mkdir -p /mnt/hdd-backup-1/nextcloud-data
- rsync Permission denied: LXC UID mapping — www-data (UID 33) maps to UID 100033 on host — fixed with chown -R 100033:100033

Action Items
- [ ] Remove Samsung 750 EVO after 1 week of stability (around May 22)
- [ ] Monitor vzdump backup run tonight at 02:00 — verify in Proxmox backup log
- [ ] Monitor nextcloud rsync run tonight at 03:00 — check /var/log/nextcloud-backup.log
- [ ] Consider moving VM 400 boot disk to ssd-storage
- [ ] Update AI-CONTEXT.md, current-state.md, service-catalog.md

### May 15, 2026 — Da Vinci Stage 2 + Phase 7E

Date: May 15, 2026
Phase: Da Vinci Stage 2 + Phase 7E

Topics Discussed
- Qdrant deployment on VM 400 (Docker)
- nomic-embed-text embeddings (768 dimensions, local, zero cost)
- Da Vinci Knowledge Indexer workflow (scheduled 3am)
- Gilgamesh RAG integration via Format Messages Code node
- Container inventory note for Obsidian
- Conversation archival to Qdrant
- RAG skip logic for greetings
- /update history pollution fix

Decisions Made
- Qdrant runs on VM 400 alongside Ollama (not separate LXC)
- Two collections: obsidian_knowledge (1567 points), gilgamesh_conversations
- Scheduled batch indexing at 3am (full re-index v1, no incremental yet)
- RAG retrieval via direct HTTP calls in Format Messages Code node
- search_document/search_query prefixes removed (incompatible with chunking)
- History reduced from 20 to 15 messages
- RAG skipped for greetings (<10 chars or regex match)
- /update messages filtered from conversation history
- Conversation archival threshold: 30 rows

Changes to AI-CONTEXT.md
- Qdrant deployed on VM 400: port 6333 (REST), 6334 (gRPC), storage at /opt/qdrant/storage
- nomic-embed-text pulled into Ollama on VM 400
- Collections: obsidian_knowledge (1567 points), gilgamesh_conversations (1 point)
- New workflow: "Da Vinci — Knowledge Indexer" — Schedule 3am, full re-index
- New n8n credential: QdrantApi account (http://192.168.30.221:6333, no API key)
- New n8n credential: n8n-rag-indexer (Nextcloud app password for WebDAV PROPFIND)
- Indexed folders: 00-inbox, 01-homelab, 02-career, 03-knowledge, 07-daily, 08-projects, 09-meetings, 10-reference, AI-Stuff/Homelab/homelab-infrastructure
- Telegram Agent modified: Format Messages now includes RAG retrieval (embed via Ollama → search Qdrant → inject into system prompt)
- Telegram Agent modified: Add Timestamps handles null gracefully when Information Extractor fails
- Telegram Agent modified: /update messages excluded from conversation history
- Telegram Agent modified: conversation archival nodes added (Count → Archive → Delete)
- Qdrant payload field is "content" (not "text" or "pageContent")
- Container inventory corrections: Firefly III = CT 221 (NPM proxy at 192.168.30.224:8080), Vaultwarden = CT 214 (192.168.30.214:8080), HashiCorp Vault = CT 213 (192.168.30.213:8200), Pterodactyl Panel = CT 300, Wings = CT 302, Pi-hole on Raspberry Pi 4 (192.168.30.10, not Proxmox)
- container-inventory.md created in Obsidian 01-homelab

Errors & Resolutions
- PROPFIND not in n8n HTTP Request: used Code node with this.helpers.httpRequest()
- 401 on Nextcloud WebDAV: created Nextcloud app password (n8n-rag-indexer)
- Information Extractor JSON backticks: set On Error to Continue, Add Timestamps handles null
- RAG returning empty: payload field was "content" not "text/pageContent"
- search_document/search_query prefixes caused mismatch: removed both
- crypto.randomUUID() unavailable in n8n Code: used Date.now() * 100 + i
- Qdrant 400 on insert: point IDs must be UUID or unsigned integer
- "hi" triggering container responses: /update messages in history, filtered with !startsWith('/update')

Action Items
- [ ] Fix container-Inventory.md download 404 (verify after next 3am indexer run)
- [ ] Fix Information Extractor JSON backtick parsing properly
- [ ] Add Qdrant conversation search to Format Messages (for "what did we discuss?" queries)
- [ ] Investigate assistant messages saving as Null in gilgamesh_conversations
- [ ] Add Uptime Kuma monitor for Qdrant (port 6333)
- [ ] Increase Top K or tune RAG for broad queries
- [ ] Add Langfuse tracing to RAG queries (optional)

### May 14, 2026 — Phase 24.8 — Langfuse (AI Observability)

Date: May 14, 2026
Phase: Phase 24.8 — Langfuse (AI Observability)

Topics Discussed
- Reviewed roadmap and AI-CONTEXT.md pending tasks to determine next phase
- Resolved roadmap vs AI-CONTEXT conflict: AI-CONTEXT is source of truth, Phase 24.8 (Langfuse) is next
- Langfuse v3 architecture: 6 Docker containers (langfuse-web, langfuse-worker, ClickHouse, PostgreSQL, Redis, MinIO)
- Full deployment of Langfuse on CT 223 (observability-langfuse, 192.168.30.223)
- Cloudflare Tunnel + Access setup for langfuse.najhin-gaming.com
- NEXTAUTH_URL fix: localhost:3000 → https://langfuse.najhin-gaming.com (required for auth redirect)
- n8n → Langfuse connectivity verified

Decisions Made
- CT 223 specs: 2 cores, 4096MB RAM, 512MB swap, 16GB disk (heavier than ntfy/Firefly III due to 6-container stack)
- Telemetry disabled (TELEMETRY_ENABLED: false) for data sovereignty
- Cloudflare Access with Email OTP enabled for langfuse.najhin-gaming.com
- Organization name: Kuromoon, Project name: Gilgamesh Agents
- Langfuse v3.174.1 deployed (latest stable)
- Batch export disabled (LANGFUSE_S3_BATCH_EXPORT_ENABLED: false) — not needed for homelab scale

Changes to AI-CONTEXT.md
- Infrastructure Inventory: CT 223 (observability-langfuse, 192.168.30.223, Langfuse v3 Docker stack, 2 cores, 4GB RAM, 16GB disk, Phase 24.8) status changed from 📋 Planned to ✅ Running. Total: 18 LXC + 1 VM.
- Phase Status: Phase 24.8 (Langfuse) COMPLETED 2026-05-14
- Services/URLs: Langfuse: langfuse.najhin-gaming.com (Cloudflare Tunnel + Access email OTP)
- Langfuse internal URL: http://192.168.30.223:3000 (for n8n and other agents)
- Langfuse health endpoint: /api/public/health
- Langfuse Docker stack: langfuse-web (port 3000), langfuse-worker (port 3030), ClickHouse (8123/9000), MinIO (9090), Redis (6379), PostgreSQL (5432) — all except web and minio bound to localhost
- Key Lessons: Langfuse v3 NEXTAUTH_URL must match external URL or auth redirects break to localhost. Langfuse v3 requires 6 containers (web, worker, ClickHouse, PostgreSQL, Redis, MinIO) — significantly heavier than v2.
- Pending Tasks: Add "Wire Langfuse to n8n agent workflows (Gilgamesh, Da Vinci, Midas, MERLIN)" as High priority. Add "Add Uptime Kuma monitor for Langfuse" as High priority. Add "Store Langfuse API keys in Vaultwarden" as High priority.

Errors & Resolutions
- Langfuse auth redirect to localhost:3000 after signup: NEXTAUTH_URL was set to http://localhost:3000. Fix: change to https://langfuse.najhin-gaming.com in docker-compose.yml, docker compose down && up -d.

Action Items
- [ ] Store Langfuse API keys (public + secret) in Vaultwarden
- [ ] Add Uptime Kuma monitor for Langfuse (http://192.168.30.223:3000/api/public/health)
- [ ] Wire Langfuse into n8n Gilgamesh workflow using built-in Langfuse node
- [ ] Wire Langfuse into Da Vinci, Midas, MERLIN workflows
- [ ] Update morning briefing container count (17 → 18)

### May 13, 2026 — Nextcloud Deck + Da Vinci Integration

Date: May 14, 2026
Phase: Nextcloud Deck + Da Vinci Integration

Topics Discussed
- Nextcloud Deck one-time setup: 3 boards (Homelab, Career, Personal), 5 stacks each, 40 labels, 74 phase cards
- Deck API authentication: regular password rejected, app password required for API access
- Deck API endpoint debugging: assignLabel route is PUT /assignLabel with {"labelId": X} in body (no ID in URL)
- Card update API requires owner field in body or returns 400
- Internal URL http://192.168.30.220 required (Cloudflare blocks Basic Auth on external URL)
- Label assignment and emoji prefix script for visual card differentiation
- Da Vinci workflow extended with 6 new nodes for Deck kanban management
- First successful card creation via n8n Deck integration

Decisions Made
- Nextcloud Deck app password created for API access (credential name: NextCloud-Deck in n8n)
- Homelab board ID: 4, Career board ID: 5, Personal board ID: 6
- Stack IDs fetched dynamically via GET /boards/4/stacks
- Labels assigned via PUT /boards/{id}/stacks/{sid}/cards/{cid}/assignLabel with body {"labelId": X}
- Card updates require title, type, owner, and description fields to avoid field wiping
- Status emojis on card titles: ✅ Done, 🔧 In Progress, 📋 Backlog, 🚫 Blocked
- Da Vinci Deck nodes appended after Push to GitHub in existing workflow
- AI-CONTEXT-staging.md cleared to prevent 53 stale action items from creating duplicate cards

Changes to AI-CONTEXT.md
- Add Nextcloud Deck section under Bots & Integrations:
  - Deck app on CT 220 (cloud.najhin-gaming.com)
  - 3 boards: Homelab (ID 4), Career (ID 5), Personal (ID 6)
  - 5 stacks per board: Backlog, Up Next, Blocked, In Progress, Done
  - 22 labels on Homelab (category, priority, agent, effort), 9 on Career, 9 on Personal
  - API base: http://192.168.30.220/index.php/apps/deck/api/v1.0
  - Auth: Basic auth with app password (credential: NextCloud-Deck)
- Add to Key Lessons / n8n & Gilgamesh:
  - Deck API assignLabel: PUT /assignLabel (no label ID in URL), body {"labelId": X}
  - Deck API card update: requires owner field or returns 400; omitting description wipes it
  - Deck API: OCS-APIRequest and Accept headers required on all calls
  - Nextcloud API auth: app password required, regular password returns "not logged in"
  - Internal URL http://192.168.30.220 bypasses Cloudflare (port 80 only, no SSL)
- Update n8n Workflows table: Da Vinci workflow now includes Deck management nodes (Parse Deck Actions, If, Fetch Homelab Stacks, Build Deck Requests, Loop Deck Actions, Execute Deck Action)
- Update Da Vinci Documentation Pipeline section: add Deck as third output target alongside Obsidian and GitHub

Errors & Resolutions
- Nextcloud API "Current user is not logged in": use app password instead of regular password
- Deck API 405 on assignLabel: route is /assignLabel without /{labelId} suffix; labelId goes in request body only
- Deck API 400 on card update: must include owner field in PUT body
- Card description wiped on update: must include current description in PUT body
- n8n Code node "Referenced node doesn't exist": webhook node named "Da Vinci Webhook" not "Webhook"
- n8n Code node "summaryText.split is not a function": webhook body is trigger-only; session text is in Read Staging File node
- n8n HTTP Request "Invalid URL =http://": URL field was in Fixed mode with = prefix; switch to Expression mode

Action Items
- [ ] Test full /update pipeline end-to-end with this session summary
- [ ] Verify Da Vinci creates Deck card from action items in this summary
- [ ] Connect Loop done output and If false output to Notify Complete Telegram node
- [ ] Update deck-davinci-n8n-guide.md with correct API endpoints discovered during debugging

---

*Last updated: May 16, 2026 — Update this file at the end of each session before pushing to GitHub*