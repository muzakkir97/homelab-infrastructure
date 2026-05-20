# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** May 20, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. Phase 24.7 (ntfy), 24.1 (Firefly III), and 24.8 (Langfuse) complete. Nextcloud Deck integration complete with Da Vinci project management. Hardware upgraded to 128GB DDR4 with 3-tier storage architecture. 20 LXC containers + 1 KVM VM deployed. Da Vinci Stage 2 (RAG) complete with Qdrant + nomic embeddings. Phase 7E (Extended Memory) complete with conversation archival. Pelican panel migration complete with Minecraft/Terraria split. Da Vinci Documentation Pipeline rebuilt May 19, 2026 with 3 separate Haiku API calls and immediate cost logging. Concurrency protection and inbox watcher schedule finalized May 18-19, 2026. Infrastructure troubleshooting complete May 20, 2026: CT 207 Promtail crash loop resolved (53,649 restarts), CT 304 tModLoader CPU leak fixed with cpulimit + daily cron. **Phase 16.4 (Documentation Pipeline Expansion — 8 files) complete May 20, 2026: expanded from 3-file to 8-file sequential Haiku chain. decisions.md promoted to Phase 2 priority.**

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
- **Complex:** Claude Haiku (API for Da Vinci only)
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
- **Langfuse** → LLM performance tracking (pending integration)
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
|---------|-----            |-----------------|--------------|----------------------------------------|
| 10      | VLAN10_MGMT     | 192.168.10.0/24 | 192.168.10.1 | Infrastructure (Proxmox, pfSense, NAS) |
| 20      | VLAN20_MAIN     | 192.168.20.0/24 | 192.168.20.1 | Client devices                         |
| 30      | VLAN30_SERVICES | 192.168.30.0/24 | 192.168.30.1 | All service containers + Pi-hole       |
| 40      | VLAN40_DMZ      | 192.168.40.0/24 | 192.168.40.1 | Future public-facing services          |
| 50      | VLAN50_MALWARE  | 192.168.50.0/24 | 192.168.50.1 | Isolated security lab (air-gapped)     |

---

## 📦 Infrastructure Inventory (Proxmox)

> **Note:** CTID 201–305 are LXC containers. VMID 400 is a KVM virtual machine with PCIe GPU passthrough — it is NOT an LXC container.

| ID  | Type | Name                    | IP             | Subdomain                     | Autostart | Status    |
|-----|------|-------------------------|----------------|--------------------------------|-----------|-----------|
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
| 305 | LXC  | gaming-panel-pelican    | 192.168.30.217 | panel.najhin-gaming.com       | ✅         | ✅ Running |
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
- **Status:** Deployed but not yet wired to agents — zero traces currently

### network-ddns (CT 207)

- **Container:** LXC Ubuntu 22.04
- **IP:** 192.168.30.207
- **Services:** Promtail (log forwarding), ddclient (DNS updates)
- **Promtail Status:** Fixed May 20, 2026 — crash loop resolved
- **Loki URL:** http://192.168.30.204:3100/loki/api/v1/push (CT 204, VLAN 30)
- **Note:** node_exporter in this container reads host /proc/stat, so CPU alerts are host-level metrics, not CT 207-specific

### minecraft-server (CT 303)

- **Container:** Paper 1.21.4 on port 25570 (192.168.30.215)
- **Panel:** Pelican Wings
- **CPU limit:** Pending in Pelican panel (set to reasonable value)
- **Note:** Watch for CPU ramp-up issues; set cpulimit in Proxmox if needed

### terraria-server (CT 304)

- **Container:** tModLoader on port 7777 (192.168.30.216)
- **Panel:** Pelican Wings
- **CPU Management:** cpulimit 1.5 (hard ceiling at Proxmox hypervisor level) set via `pct set 304 --cpulimit 1.5`
- **Daily Cron:** `0 4 * * * pct exec 304 -- bash -c "kill $(pgrep -f tModLoader)" 2>/dev/null` — restart process daily at 4am to clear CPU leak
- **Issue Fixed:** tModLoader idles high (70%+) and ramps to 97%+ CPU over hours with heavy mods. cpulimit + daily cron restart resolves the issue.
- **Panel CPU Limit:** Set to 150% (= 1.5 cores, matching Proxmox cpulimit) in Pelican panel

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

- **Pelican Panel (CT 305):** Next-gen game server panel (192.168.30.217), SQLite + Redis + PHP 8.3, access: panel.najhin-gaming.com
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
|------------|------            |------           |------         ----|
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
- Promtail YAML corruption detection → shell command filtering
- tModLoader CPU leak diagnosis → cpulimit + cron restart pattern

Note: Key Lessons section in AI-CONTEXT.md is the manual version of this — Phase 25.3 automates that capture.

---

## 🎴 Fate Grand Order Agent Ecosystem

Theme: Homelab agents named after Fate/Grand Order servants. Final roster locked April 25, 2026.

### Active Agents

| Servant      | Class  | Role                                                                          | Platform                      | Status                     |
|--------------|--------|-------------------------------------------------------------------------------|-------------------------------|----------------------------|
| Gilgamesh 👑 | Archer | Life Interface & Personal AI Assistant                                        | Telegram (@JhinGilgamesh_bot) | ✅ Active                   |
| Da Vinci 🎨  | Caster | Sync + Goals + Review (Stage 2 RAG active; documentation pipeline active)   | n8n/Nextcloud                 | ✅ Active                   |
| Midas 💰     | Caster | CFO — Cost Tracking & Optimization                                            | n8n                           | ✅ Active                   |
| MERLIN 🔮    | Caster | Proactive Nudges & Scheduler                                                  | n8n                           | ✅ Active                   |

### Final 6-Agent Roster (Redesigned)

| Servant             | Class    | Role                                          | Platform      | Build Order | Status     |
|---------------------|----------|-----------------------------------------------|---------------|-------------|------------|
| Gilgamesh 👑        | Archer   | Life Interface & Personal AI Assistant        | Telegram      | —           | ✅ Active   |
| Da Vinci 🎨         | Caster   | Sync + Goals + Weekly Review + RAG           | n8n/Nextcloud | —           | ✅ Active   |
| Midas 💰            | Caster   | CFO — Cost Tracking & Optimization            | n8n           | 1st         | ✅ Active   |
| MERLIN 🔮           | Caster   | Proactive Nudges & Health Scheduler           | n8n           | 2nd         | ✅ Active   |
| EMIYA 🏹            | Archer   | CTO — Infrastructure + Agent Spawning         | n8n           | 3rd         | 📋 Planned  |
| Guardian 🛡         | —        | Security Monitoring & Threat Detection        | n8n           | 4th         | 📋 Planned  |

**Notes:**

- Roster reduced from 9 to 6 agents for focused deployment
- Build order is firm: Midas complete, MERLIN complete, then EMIYA (24.1-24.8), Guardian

### Da Vinci — Sync + Goals + Weekly Review + RAG

**Role scope:**

- Documentation: maintains AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md via /update and /sync-docs
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
- max_tokens: AI-CONTEXT 20000, changelog 6000, troubleshoot 4000, ROADMAP 8000, agents 8000, current-state 4000, service-catalog 4000, decisions 3000 (prevents hallucinated content)
- Grounding fix pending: add step to fetch current files from Nextcloud before Claude API call
- Deck integration complete with 6 additional nodes for kanban management
- Da Vinci workflow includes Limit node before Notify Complete to prevent notification spam
- Knowledge Indexer runs at 3am daily (full re-index v1)
- **Concurrency lock:** Check Running node queries n8n API before proceeding
- **Schedule:** Inbox Watcher runs every 15 minutes (was 1 minute)
- **Model:** claude-haiku-4-5-20251001 (8 separate API calls, one per file: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
- **Cost logging:** Fires immediately after each of the 8 API calls, before parse/push nodes — logs even on downstream failure
- **Haiku pricing:** $0.80/1M input tokens, $4.00/1M output tokens

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

1. **App Management** — Firefly III, ntfy, Langfuse lifecycle (24.1) ✅ COMPLETE
2. **Alert Translation** — Alertmanager alerts → plain English via ntfy (24.2) 📋 Planned
3. **Container Updates** — Docker image updates, apt upgrades (approval-gated) (24.3) 📋 Planned
4. **Knowledge Ingestion** — URLs → Firecrawl → triple-write pipeline (24.4) 📋 Planned
5. **Proactive Monitoring** — anomaly detection, trend alerts before things break (24.5) 📋 Planned
6. **Performance + Goal Optimization** — resource reallocation, goal tracking (24.6) 📋 Planned
7. **Universal Notifications** — ntfy hub for all agent communications (24.7) ✅ COMPLETE
8. **Langfuse AI Observability** — LLM performance tracking and tracing (24.8) ✅ COMPLETE

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
                         Ollama qwen3:14b      Haiku 4.5           Haiku 4.5
                         (simple, local)    (Ollama fallback)    (Da Vinci only)
                               └────────────────────┼──────────────────┘
                                                    ▼
                                          Memory (n8n Data Tables)
                                                    ▼
                                          RAG Retrieval (Qdrant → nomic-embed-text)
                                                    ▼
                                          Conversation Archival (30+ rows → Qdrant)
                                                    ▼
                                          /update → Da Vinci → Nextcloud → GitHub
                                          /sync-docs → Full Doc Pipeline
```

### Smart Routing + RAG + Extended Memory (Phase 41 + Da Vinci Stage 2 + Phase 7E — COMPLETE)

- **Ollama qwen3:14b** — Primary route for simple queries (local, free, fast)
- **Haiku 4.5** — Fallback if Ollama is down or unavailable (Gilgamesh + Da Vinci both use Haiku)
- **Da Vinci Update Pipeline** — claude-haiku-4-5-20251001 for documentation merging (8 separate calls, one per file)
- **RAG Retrieval** — Qdrant vector search with nomic-embed-text embeddings (768 dims)
- **Extended Memory** — Conversation archival when history exceeds 30 rows
- Routing logic runs in a Route Check If node before calling any model
- RAG skipped for greetings (<10 chars or regex match)
- /update messages filtered from conversation history

### Features

- **Conversation memory:** Last 15 messages stored in n8n Data Tables (reduced from 20 for RAG integration)
- **Extended memory:** Conversations archived to Qdrant at 30+ rows
- **Smart routing:** Ollama (local, primary) → Haiku (fallback)
- **RAG knowledge recall:** Queries Qdrant for relevant Obsidian content + conversations
- **Web search:** Real-time information via Claude's web_search tool
- **Cost tracking:** Token usage logged to gilgamesh_costs table with command_type; cost_usd calculated from token rates
- **Inline keyboard menu:** Full menu system with all submenus working
- **Context sync:** /update command pushes session summaries to 8 documentation files via GitHub
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
| /update    | Session summary   | Merges session into docs (8 files)            |
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
| Haiku Model   | claude-haiku-4-5-20251001 (Ollama fallback + Da Vinci)   |
| n8n Container | CT 211, 192.168.30.211                                    |
| Qdrant        | VM 400, ports 6333 (REST) + 6334 (gRPC)                  |
| Proxmox API   | root@pam!gilgamesh token                                  |
| Proxmox node  | muzakkir (not kuromoon)                                   |

### Cost Tracking Note

- cost_usd calculated from token rates: Haiku $0.80/1M input, $4.00/1M output
- Ollama tokens captured: data.input_tokens + data.output_tokens from Call Ollama node response
- Ollama queries cost $0 (local inference)
- command_type derived from Telegram message text directly
- **Da Vinci:** claude-haiku-4-5-20251001 (8 separate calls), cost logged immediately after each call

### n8n Workflows (Count: 14)

| Workflow                           | Purpose                                  | Nodes | Trigger  |
|------------------------------------|------------------------------------------|-------|----------|
| Gilgamesh — Life Interface         | Main bot, menu, commands, hybrid routing, RAG, extended memory | 18+ | Telegram |
| Documentation Pipeline — Update    | Session summary → 8 files                | 7     | Webhook  |
| Documentation Pipeline — Sync Docs | Full doc regeneration → 7 files          | 7     | Webhook  |
| Da Vinci — Update Pipeline         | 8 separate Haiku API calls (AI-CONTEXT 20k, changelog 6k, troubleshoot 4k, ROADMAP 8k, agents 8k, current-state 4k, service-catalog 4k, decisions 3k) → GitHub push + Nextcloud | 35+ | Execute Workflow |
| Da Vinci — Inbox Watcher           | Staging inbox → calls Update Pipeline (every 15 min) | 6     | Schedule |
| Da Vinci — Knowledge Indexer       | Obsidian → Qdrant indexing (3am daily)   | 8     | Schedule |
| Midas — CFO Report                 | /midas command cost analysis             | 6     | Webhook  |
| Midas — Daily Brief                | 9am scheduled cost summary               | 4     | Schedule |
| MERLIN — Reminders                 | 8am daily infrastructure checks          | 8     | Schedule |
| Gilgamesh — Daily Note Creator     | Midnight Obsidian daily notes            | 4     | Schedule |
| Gilgamesh — Morning Briefing       | 7am daily summary to Telegram            | 5     | Schedule |
| Update Nextcloud File              | Legacy (unpublished)                     | 5     | Webhook  |
| Push to GitHub                     | Legacy (unpublished)                     | 4     | Webhook  |
| Da Vinci — Documentation Pipeline  | INACTIVE (deactivated 2026-05-18, orphaned) | 17+ | Webhook |

> Note: Hybrid routing (Phase 41), RAG retrieval (Da Vinci Stage 2), and Extended Memory (Phase 7E) are all integrated into the Gilgamesh — Life Interface workflow. Health tracking (Phase 22.8B) is also integrated.

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
- **Proxmox API:** root@pam!gilgamesh token (recreated May 16, 2026)
- **Storage IDs:** kinmoon-nfs (not kinmoon-smb in Proxmox)

### Known Constraints

- Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body
- Progress bars use ASCII (= and -) — Unicode block chars don't render on Telegram mobile
- RAG payload field is "content" (not "text" or "pageContent")
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API — use single-quoted strings with concatenation instead

---

## 🔧 Documentation Pipeline (Phase 16.1/16.2/16.3/16.4)

### Architecture

```
Session Summary → /update → Da Vinci Inbox Watcher → Da Vinci Update Pipeline → 8 Separate Haiku API Calls → 8 Files (AI-CONTEXT + changelog + troubleshoot + ROADMAP + agents + current-state + service-catalog + decisions)
                                                                                                ↓
                                                                                    Nextcloud + GitHub + Deck
```

#### Da Vinci Documentation Pipeline (Phase 16.4 — COMPLETE May 20, 2026)

**Expanded workflow from 3 files to 8 files with 8 sequential Haiku API calls:**

```
Raw summaries → AI-CONTEXT-staging.md (rolling append)
                         ↓
        Da Vinci Inbox Watcher (every 15 minutes) → Execute Workflow trigger
                         ↓
        Da Vinci Update Pipeline → 8 separate Haiku API calls
                         ├─→ Call 1: AI-CONTEXT.md (max_tokens 20000)
                         ├─→ Call 2: changelog.md (max_tokens 6000)
                         ├─→ Call 3: troubleshoot.md (max_tokens 4000)
                         ├─→ Call 4: ROADMAP.md (max_tokens 8000)
                         ├─→ Call 5: agents.md (max_tokens 8000)
                         ├─→ Call 6: current-state.md (max_tokens 4000)
                         ├─→ Call 7: service-catalog.md (max_tokens 4000)
                         └─→ Call 8: decisions.md (max_tokens 3000)
                         ↓
        Cost logging (fires immediately after EACH call, not at end)
                         ↓
        Parse Response → GitHub push → Nextcloud sync → Staging archival
```

### Commands

| Command    | Purpose                                   | Files Updated |
|------------|-------------------------------------------|---------------|
| /update    | Every session — merge summary into docs   | 8 files       |
| /sync-docs | Deployment sessions — regenerate all docs | 7 files       |

### File Coverage

| File | Update Strategy | max_tokens | Da Vinci Action |
|------|----------------|------------|-----------------|
| AI-CONTEXT.md | Full rewrite | 20000 | LLM merges session into master doc |
| changelog.md | Append | 6000 | New entry added at top |
| troubleshoot.md | Append | 4000 | New errors/resolutions added |
| ROADMAP.md | Full rewrite | 8000 | Phase statuses updated |
| agents.md | Full rewrite | 8000 | Agent roster and status updated |
| current-state.md | Append/update | 4000 | Container/hardware state updated |
| service-catalog.md | Append/update | 4000 | Service list updated |
| decisions.md | Append | 3000 | New decisions added at top |

### Pipeline Components

| Component    | Details                                             |
|--------------|-----------------------------------------------------|
| Webhooks     | doc-update, doc-sync, da-vinci                      |
| Telegram     | Routes /update and /sync-docs                       |
| Haiku API    | claude-haiku-4-5-20251001 for documentation merging (8 calls) |
| Nextcloud    | File storage via admin user                         |
| GitHub       | Version control via API push                        |
| Da Vinci     | Async processing (she/her pronouns)                 |
| Staging file | AI-CONTEXT-staging.md in Nextcloud (rolling append) |
| Deck         | Kanban cards created from action items               |

### Da Vinci Technical Notes (Phase 16.4)

- **Pipeline architecture:** 8 separate Haiku API calls (one per file) instead of single merged call — sequential chain to prevent hallucination across files
- **Cost logging:** Fires immediately after each of the 8 Haiku API calls, before parse/push nodes — captures cost even if downstream nodes fail
- **Haiku pricing:** $0.80/1M input tokens, $4.00/1M output tokens
- **max_tokens per file:**
  - AI-CONTEXT: 20000 (comprehensive infrastructure state)
  - changelog: 6000 (recent changes only)
  - troubleshoot: 4000 (key lessons)
  - ROADMAP: 8000 (phases, roadmap, decisions)
  - agents: 8000 (agent descriptions and workflows)
  - current-state: 4000 (snapshot of system state)
  - service-catalog: 4000 (services deployed)
  - decisions: 3000 (decisions made this session)
- **Concurrency:** Check Running node at start — queries n8n API for existing Update Pipeline executions before proceeding
- **Inbox Watcher schedule:** Every 15 minutes (was 1 minute, increased to reduce concurrent executions)
- **Staging inbox:** AI-Stuff/Homelab/staging-inbox/ on Nextcloud. Processed files archived to AI-Stuff/Homelab/staging-archive/YYYY-MM/
- **Staging archival:** Automatic upon successful pipeline completion
- **Deck integration:** Extended workflow with 6 additional nodes for Deck kanban management (Parse Deck Actions, If, Fetch Homelab Stacks, Build Deck Requests, Loop Deck Actions, Execute Deck Action)
- **Notification:** Limit node (max 1) before Notify Complete to prevent notification spam
- **Property names:** Verify sessionSummary property name matches across nodes (was reading wrong property causing null merge)
- **Validation:** Parse Response node rejects placeholder outputs before GitHub push
- **decisions.md handling:** New file on first run (null SHA in GitHub API), automatic creation on subsequent runs
- **Cost per run estimate:** ~$0.25-0.35 for 8 files (was ~$0.11 for 3 files)
- **Claude project instructions:** Session summary template now has explicit per-file sections (CHANGES TO AI-CONTEXT.MD, CHANGES TO ROADMAP.MD, CHANGES TO AGENTS.MD, etc.) so Da Vinci gets unambiguous per-file instructions
- **API key handling:** Hardcode Anthropic API key and GitHub token directly in each node (consistent with existing 3 nodes). Trigger payload fields only define fileContent and chatId; adding more fields adds complexity for no benefit.
- **Code node syntax:** Single-quoted strings with concatenation for system prompts. Backtick template literals cause 400 errors on Anthropic API when used in n8n Code nodes.

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
| DNS             | Pi-hole ad/tracker blocking (~489K domains blocked)                                                                                                                                                |
| VPN             | Tailscale (subnet router on pfSense, primary access)                                                                                                                                               |
| External Auth   | Cloudflare Access (Email OTP, muzakkir.kholil06@gmail.com only) for Grafana, n8n, Vault, Vaultwarden, Ollama, Nextcloud, Firefly III, Langfuse, Pelican Panel (9 apps total)                 |
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
| 16.3    | Da Vinci Documentation Pipeline                                  | ✅ Complete | May 19, 2026 |
| 16.4    | Da Vinci Documentation Pipeline Expansion (8 files)              | ✅ Complete | May 20, 2026 |
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
| 41      | Hybrid Routing (Ollama + Haiku + Sonnet)                         | ✅ Complete | May 8, 2026  |

---

## 📝 Session Log (Most Recent 5)

### Session 7: May 20, 2026 — Phase 16.4: Documentation Pipeline Expansion (8 files)

**Duration:** 2h
**Topics:** Reviewed file coverage from previous sessions, reconciled sync-docs command retirement vs workflow continuation, updated Claude project instructions with expanded session summary template (explicit per-file sections), debugged 5 bugs in Da Vinci Update Pipeline, discussed $0 AI Architecture Stack 2026, LM Studio vs Ollama decision, Aider vs Claude Code for future code agent
**Decisions:** decisions.md promoted to Phase 2 priority (auto-append each session), all 8 files in sequential Haiku chain, hardcode API keys in new nodes, single-quoted strings with concatenation for system prompts (avoid backtick template literals causing 400 errors), Ollama confirmed as correct choice for homelab, Aider + local Ollama identified as better fit than Claude Code for EMIYA code agent (future phase)
**Outcomes:** Da Vinci Update Pipeline expanded to 8 files. Cost per run ~$0.25-0.35. 8 cost rows logged per session. decisions.md created on GitHub. Claude project instructions updated with per-file sections. 5 bugs resolved: null GitHub token, null API keys in 5 new nodes, filesToPush only had 3 files, sessionSummary property name wrong, backtick template literals causing 400 errors.
**Errors Fixed:**
  - Fetch GitHub Files had null token — hardcoded directly in node
  - 5 new Claude API nodes had null API key — hardcoded at top of each node
  - filesToPush array only had 3 files — updated to all 8 with null SHA handling
  - sessionSummary undefined — changed to fileContent from trigger
  - Backtick template literals caused 400 errors — replaced with single-quoted strings + concatenation
  - Log Cost — service-catalog has wrong command_type — changed to `/update (Da Vinci - service-catalog)`
**Next:** Test full 8-file pipeline run, verify all 8 cost rows and GitHub push, Phase 24.2 (Alert Translation)

### Session 6: May 20, 2026 — Infrastructure Troubleshooting: CT 207 Promtail + CT 304 tModLoader

**Duration:** 2h
**Topics:** CRITICAL CPU alert on 192.168.30.207, Promtail crash loop (53,649 restarts), wrong Loki URL (192.168.20.13 vs 192.168.30.204), CT 304 tModLoader CPU leak (97% sustained), Pterodactyl vs Pelican CPU limits, alert routing clarification
**Decisions:** Overwrite CT 207 Promtail config with clean YAML, correct Loki IP, set CT 304 cpulimit 1.5, add daily 4am cron restart, set Pelican panel CPU limits
**Outcomes:** Both issues resolved. 4 hours clean with no alerts post-fix. Morning briefing container count fix confirmed complete.
**Next:** Phase 24.2 (Alert Translation), set CPU limits in Pelican panel for CT 303 + CT 304

### Session 5: May 19, 2026 — Da Vinci Documentation Pipeline Rebuild

**Duration:** 3h
**Topics:** Documentation pipeline overhaul, 3 separate Haiku API calls, immediate cost logging, concurrency protection, inbox watcher schedule tuning
**Decisions:** Rebuild as 3 separate calls (AI-CONTEXT, changelog, troubleshoot) instead of merged, fire cost logging immediately after each call, Inbox Watcher runs every 15 min, Check Running node for concurrency lock
**Outcomes:** Pipeline rebuilt and tested. Cost logging verified. Staging inbox/archive directory structure operational.
**Next:** Monitor pipeline stability, begin Phase 24.2

### Session 4: May 15, 2026 — Phase 7E Complete: Extended Memory + Conversation Archival

**Duration:** 2.5h
**Topics:** RAG integration with extended memory, Qdrant collections, nomic-embed-text setup, conversation archival at 30+ rows, system prompt injection
**Decisions:** Archive conversations to Qdrant when gilgamesh_memory exceeds 30 rows, inject top 5 RAG results into system prompt, skip RAG for greetings
**Outcomes:** Extended memory working. All 20 LXC + 1 VM running. RAG retrieval tested successfully.
**Next:** Da Vinci documentation pipeline rebuild (Phase 16.3)

### Session 3: May 14, 2026 — Phase 24.8: Langfuse Deployment + Phase 24.7 Complete

**Duration:** 2h
**Topics:** Langfuse v3.174.1 stack deployment (6 containers), LLM observability setup, ntfy hub integration
**Decisions:** Deploy Langfuse on CT 223, configure health endpoint, integrate with ntfy, plan agent wiring post-Phase 25
**Outcomes:** Langfuse running, zero traces currently. Phase 24.7 (ntfy) confirmed complete.
**Next:** Wire agents to Langfuse (Phase 25+), begin Phase 24.2

---

## 🔧 Key Lessons Learned

### Monitoring & Alerts

| Issue | Resolution |
|-------|------------|
| CRITICAL CPU alert on CT 207 node_exporter | Root cause was Promtail crash loop (53,649 restarts). node_exporter in LXC reads host /proc/stat — alert reflects host CPU, not container CPU. Investigation tip: check systemctl status on container before blaming hardware |
| Promtail YAML broken after install | Shell installation commands were accidentally pasted into promtail.yml after line 30, breaking YAML syntax. Parser failed every 5 seconds for 3+ days. Fix: Overwrite entire file cleanly using `python3 -c "open(...).write(...)"` from inside container instead of heredoc |
| Wrong Loki URL in Promtail config | CT 207 had 192.168.20.13 (VLAN 20, client devices) in promtail.yml. Correct URL is http://192.168.30.204:3100/loki/api/v1/push (CT 204 on VLAN 30, services). Verify network placement of log destinations |
| tModLoader CPU leak on CT 304 | tModLoader has known CPU ramp-up issue when running headless with heavy mods. Idles at 70%+, hits 97%+ over hours. Pterodactyl limited via Docker config; Pelican migration did not carry limits. Fix: `pct set 304 --cpulimit 1.5` at Proxmox level + daily 4am cron: `0 4 * * * pct exec 304 -- bash -c "kill $(pgrep -f tModLoader)" 2>/dev/null` |
| Pterodactyl vs Pelican CPU difference | Pterodactyl panel enforced Docker-level CPU limits via compose config. Pelican migration did not auto-migrate CPU limits — must set manually in both Proxmox (pct set) and Pelican panel UI |

### Documentation & Workflow

| Issue | Resolution |
|-------|------------|
| Da Vinci documentation merged to wrong file | Property name mismatch (sessionSummary vs summary) caused null reads. Verify property chains after Merge nodes — use direct node references like `$('Extract Response').first().json.updatedDoc` |
| Haiku API cost logging missing on failure | Cost logs were firing at end of workflow. If downstream nodes failed, costs weren't captured. Fix: Fire cost logging immediately after each API call (8 calls now for 8 files), before parse/merge nodes |
| Documentation pipeline concurrent executions | Inbox Watcher running every 1 minute caused multiple executions queued. Fix: Increase schedule to every 15 minutes + add Check Running node to query n8n API before proceeding |
| Per-file Da Vinci instructions getting ignored | Single merged session summary section caused Da Vinci to hallucinate content for other files. Fix: Claude project instructions now require explicit per-file sections (CHANGES TO AI-CONTEXT.MD, CHANGES TO ROADMAP.MD, etc.) — Da Vinci gets unambiguous per-file instructions |
| New Da Vinci files not pushed to GitHub | Push to GitHub node still had hardcoded 3-file array. Fix: Updated filesToPush to all 8 files with null SHA handling for new files like decisions.md |
| Backtick template literals in n8n Code nodes | Backticks caused 400 errors on Anthropic API when used in n8n Code nodes. Fix: Use single-quoted strings with concatenation for all system prompts in Claude API nodes |
| GitHub API null token reference | New code referenced `githubToken` from trigger payload which is never passed. Fix: Hardcode GitHub token directly in Fetch GitHub Files node |

### Networking & Infrastructure

| Issue | Resolution |
|-------|------------|
| Node_exporter alert misdiagnosis | Host-level metrics (CPU, memory) trigger alerts on Prometheus even if LXC container itself is idle. The node_exporter reads /proc/stat from host, not cgroup limits. Future work: Relabel alerts to clarify scope (Phase 24.2) |
| heredoc paste mangling on Proxmox host | `cat > file << 'EOF'` failed when pasting multi-line heredocs. Promtail install guide used heredoc; copy-paste broke quoting. Solution: Use `pct enter 207` first, then `python3 -c "..."` to avoid heredoc issues |

### Container & Process Management

| Issue | Resolution |
|-------|------------|
| Game server CPU limits not enforced | Pelican panel migration lost CPU limit configuration from Pterodactyl. CPU limits live in 2 places: Proxmox (hypervisor-level via pct set --cpulimit) and game panel (UI setting). Both must be set |
| Process restart patterns | Long-running processes with memory leaks (tModLoader) benefit from scheduled kills. Use daily cron at 4am to kill and let panel auto-restart. Cleaner than constant monitoring |

### Code & API Integration

| Issue | Resolution |
|-------|------------|
| Backtick template literals break Anthropic API | Backticks in n8n Code nodes cause 400 errors when submitted to Claude API. Anthropic parsing issue with escaped backticks in JSON. Use single-quoted strings with string concatenation instead |
| Trigger payload fields not matching schema | n8n trigger only passes explicitly defined fields (fileContent, chatId). Referencing undefined fields (github