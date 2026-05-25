# 🤖 AI Context Document — Homelab Infrastructure Project

> **Last Updated:** May 25, 2026
> **Purpose:** Upload this file to any AI (Claude, ChatGPT, Copilot, etc.) to provide full project context
> **Owner:** Muzakkir Kholil
> **GitHub:** github.com/muzakkir97/homelab-infrastructure

---

## Quick Summary

I'm building an **enterprise-grade homelab** for career transition from Customer Service Engineer (F-Secure, cybersecurity) to **Cloud Engineering / DevOps**. The project serves as both a learning environment and professional portfolio documented on GitHub and LinkedIn.

**Current Status:** Architecture redesign complete. 7-layer model finalized. Midas CFO Agent, MERLIN Reminders, Daily Note Creator, Morning Briefing, Health Tracking all active. Obsidian Phases 22.1, 22.2, and 22.8B complete. Phase 24.7 (ntfy), 24.1 (Firefly III), and 24.8 (Langfuse) complete. Nextcloud Deck integration complete with Da Vinci project management. Hardware upgraded to 128GB DDR4 with 3-tier storage architecture. 20 LXC containers + 1 KVM VM deployed. Da Vinci Stage 2 (RAG) complete with Qdrant + nomic embeddings. Phase 7E (Extended Memory) complete with conversation archival. Pelican panel migration complete with Minecraft/Terraria split. Da Vinci Documentation Pipeline rebuilt May 19, 2026 with 3 separate Haiku API calls and immediate cost logging. Concurrency protection and inbox watcher schedule finalized May 18-19, 2026. Infrastructure troubleshooting complete May 20, 2026: CT 207 Promtail crash loop resolved (53,649 restarts), CT 304 tModLoader CPU leak fixed with cpulimit + daily cron. Phase 16.4 (Documentation Pipeline Expansion — 8 files) complete May 21, 2026: expanded from 3-file to 8-file sequential Haiku chain. decisions.md promoted to Phase 2 priority. Pipeline tested and verified May 21, 2026: 3 separate API calls, immediate cost logging, per-file system prompts, all 8 files successfully pushed to GitHub. Phase 24.8 (Langfuse) wired to Da Vinci Update Pipeline May 21, 2026: single trace (da-vinci-update) with 8 child generations logged per run. Traces confirmed in ClickHouse and accessible via API; UI trace list has known v3 self-hosted bug where traces don't appear in list view (1-hour aggregation delay). VM 400 disk expanded from 56GB to 86GB (LVM thin resize, 34GB free). Model testing complete: qwen3:14b confirmed as primary (honest about limitations), gemma3 + phi4-mini removed (confident hallucination). AI-CONTEXT max_tokens bumped to 25000 (was hitting ceiling). Langfuse CT 223 LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES set to true. Phase 24.9 (Personal Knowledge System) complete May 22, 2026: Langfuse UI trace list working (1-hour analytics delay resolved), Gilgamesh wired to Langfuse, Da Vinci Personal Knowledge gateway deployed, muzakkir-profile.md created and indexed in Qdrant (1,736 chunks), Gilgamesh successfully recalling personal facts via RAG, Knowledge Indexer updated to include 04-personal/ and expanded folder set (90 files indexed). Documentation audit (May 22, 2026) — corrections only: ROADMAP.md had 5 archived phases still showing as active; current-state.md had hallucinated hardware (EPYC 5645/256GB/RTX 4070 vs actual Ryzen 5 5600X/128GB/RX 6700 XT); agents.md MERLIN/Midas status incorrect; Knowledge Indexer folder list inconsistent across files. All corrections applied. **Phase 24.10 (Triggered Qdrant Re-indexing) complete May 25, 2026: Post-write webhook added to Da Vinci Personal Knowledge gateway. Partial reindex (/davinci-reindex-personal, ~1s) triggers after muzakkir-profile.md writes. Full daily reindex (3am, ~21s) still operational. Phase 52 (DOCP Memory Optimization) complete May 2026. Web Search (Gilgamesh) deployed May 25, 2026: Firecrawl API integrated. Keyword-based intent detection, search results injected into context, search queries force-routed to Haiku (local models cannot follow injected context). Firecrawl credits per search: 2. All changes verified May 25, 2026.**

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
- **Routing:** Smart complexity detection + web search routing

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
- **Conversation history** → 15 messages (gilgamesh_conversations)
- **Session modes** → health_logging, etc.
- **Goal tracking** → progress + blockers
- **Agent coordination** → shared state

### Layer 6: Observability (Monitoring)
- **Langfuse** → LLM performance tracking (wired to Da Vinci and Gilgamesh)
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
- **Status:** Wired to Da Vinci Update Pipeline and Gilgamesh — traces active and visible in UI (1-hour analytics delay resolved)
- **Tracing:** Two trace types:
  - **da-vinci-update:** One trace with 8 child generations logged per Da Vinci pipeline run (files: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
  - **gilgamesh-chat:** One trace per Gilgamesh message with input/output/metadata (routedTo, ragUsed, commandType, chatId)
- **UI Status:** Working — trace list now displays traces with 1-hour analytics aggregation delay (traces appear after 1 hour, not immediately). Direct API (/api/public/traces) returns results immediately. Direct URL access works immediately.
- **Environment variable:** LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=true

### network-ddns (CT 207)

- **Container:** LXC Ubuntu 22.04
- **IP:** 192.168.30.207
- **Services:** Promtail (log forwarding), ddclient (DNS updates)
- **Promtail Status:** Fixed May 20, 2026 — crash loop resolved (53,649 restarts), YAML corrupted with shell commands pasted into config file
- **Loki URL:** http://192.168.30.204:3100/loki/api/v1/push (CT 204, VLAN 30)
- **Note:** node_exporter in this container reads host /proc/stat, so CPU alerts are host-level metrics, not CT 207-specific

### minecraft-server (CT 303)

- **Container:** Paper 1.21.4 on port 25570 (192.168.30.215)
- **Panel:** Pelican Wings
- **CPU limit:** Set via Pelican panel
- **Note:** Watch for CPU ramp-up issues; set cpulimit in Proxmox if needed

### terraria-server (CT 304)

- **Container:** tModLoader on port 7777 (192.168.30.216)
- **Panel:** Pelican Wings
- **CPU Management:** cpulimit 1.5 (hard ceiling at Proxmox hypervisor level) set via `pct set 304 --cpulimit 1.5`
- **Daily Cron:** `0 4 * * * pct exec 304 -- bash -c "kill $(pgrep -f tModLoader)" 2>/dev/null` — restart process daily at 4am to clear CPU leak
- **Issue Fixed (May 20, 2026):** tModLoader idles high (70%+) and ramps to 97%+ CPU over hours with heavy mods. cpulimit + daily cron restart resolves the issue.
- **Panel CPU Limit:** Set to 150% (= 1.5 cores, matching Proxmox cpulimit) in Pelican panel

### ollama-gpu (VM 400)

- **Type:** KVM VM with PCIe passthrough — NOT an LXC
- **GPU:** RX 6700 XT 12GB (0d:00.0) + audio (0d:00.1) passed through
- **PCIe config:** hostpci0=0000:0d:00.0,pcie=1,rombar=0 hostpci1=0000:0d:00.1,pcie=1
- **OS:** Ubuntu 22.04
- **SSH:** `ssh muzakkir@192.168.30.221` — use `muzakkir` user, not root
- **Disk:** Expanded from 56GB to 86GB (May 21, 2026) via Proxmox resize + LVM extension (device: /dev/vda, not /dev/sda). 34GB free post-expansion.
- **Ollama models:** qwen3:14b (primary, 9.3GB), qwen3.5:latest (secondary), nomic-embed-text (768 dims). Removed (May 21-22): gemma3:4b, gemma3:12b, phi4-mini, llama3.2:latest (all hallucinated factual data in testing).
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
├── 04-personal/       # Personal management, subscriptions, finance (indexed in Qdrant)
│   ├── subscriptions/ # Individual notes per subscription
│   ├── finance/       # Budget tracking, grocery lists
│   ├── grocery/       # Shopping lists and meal planning
│   ├── health/        # Blood pressure tracking, food logs
│   └── muzakkir-profile.md  # Personal profile managed by Da Vinci Personal Knowledge gateway
├── 05-templates/      # Note templates
├── 06-archive/        # Completed or outdated notes
├── 07-daily/          # Daily notes from Phase 22.2
├── 08-agents/         # Agent configurations and knowledge (was 08-projects)
├── 09-people/         # People notes (was 09-meetings)
├── 10-projects/       # Active projects and goals (was 10-reference)
└── 11-reference/      # Quick reference materials
```

### Key Features (Phase 22.1 + 22.2 + 22.8B + Phase 24.9)

- **Subscription tracker:** 6 subscriptions tracked in 04-personal/subscriptions/
- **Dashboard:** Dataview queries at 04-personal/dashboard showing costs and totals
- **Daily notes:** Auto-created at midnight via n8n (Phase 22.2)
- **Morning briefing:** 7am Telegram summary via MERLIN
- **Health tracking:** Food log, BP log, medication log via Gilgamesh buttons (Phase 22.8B)
- **Personal profile:** muzakkir-profile.md managed by Da Vinci Personal Knowledge gateway, indexed in Qdrant obsidian_knowledge collection, recalled by Gilgamesh via RAG (Phase 24.9)
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
- Real-time web search capability

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
| Gilgamesh 👑 | Archer | Life Interface & Personal AI Assistant (Langfuse wired May 22, 2026; Web search live May 25, 2026) | Telegram (@JhinGilgamesh_bot) | ✅ Active                   |
| Da Vinci 🎨  | Caster | Sync + Goals + Review (Stage 2 RAG active; documentation pipeline active; Personal Knowledge gateway active) | n8n/Nextcloud | ✅ Active |
| Midas 💰     | Caster | CFO — Cost Tracking & Optimization                                            | n8n                           | ✅ Active                   |
| MERLIN 🔮    | Caster | Proactive Nudges & Scheduler                                                  | n8n                           | ✅ Active                   |

### Final 6-Agent Roster (Redesigned)

| Servant             | Class    | Role                                          | Platform      | Build Order | Status     |
|---------------------|----------|-----------------------------------------------|---------------|-------------|------------|
| Gilgamesh 👑        | Archer   | Life Interface & Personal AI Assistant (Langfuse wired, RAG for personal profile, web search) | Telegram | —         | ✅ Active   |
| Da Vinci 🎨         | Caster   | Sync + Goals + Weekly Review + RAG (Personal Knowledge gateway active) | n8n/Nextcloud | —      | ✅ Active   |
| Midas 💰            | Caster   | CFO — Cost Tracking & Optimization            | n8n           | 1st         | ✅ Active   |
| MERLIN 🔮           | Caster   | Proactive Nudges & Health Scheduler           | n8n           | 2nd         | ✅ Active   |
| EMIYA 🏹            | Archer   | CTO — Infrastructure + Agent Spawning         | n8n           | 3rd         | 📋 Planned  |
| Guardian 🛡         | —        | Security Monitoring & Threat Detection        | n8n           | 4th         | 📋 Planned  |

**Notes:**

- Roster reduced from 9 to 6 agents for focused deployment
- Build order is firm: Midas complete, MERLIN complete, then EMIYA (24.1-24.8), Guardian
- All agents write Obsidian data through Da Vinci Personal Knowledge gateway (not directly)

### Da Vinci — Sync + Goals + Weekly Review + RAG + Personal Knowledge Gateway

**Role scope:**

- Documentation: maintains AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md via /update and /sync-docs
- Obsidian writes: session summaries written to vault via Nextcloud WebDAV; personal knowledge writes via Da Vinci Personal Knowledge gateway
- Goal tracking: monitors progress, identifies blockers, suggests next steps
- Weekly review: Sunday analysis of week's progress, agent performance, system health
- RAG retrieval (Stage 2): Qdrant + nomic-embed-text for knowledge recall across all agents
- Personal Knowledge gateway: receives facts from Gilgamesh and future agents via webhook, assesses quality, writes to muzakkir-profile.md, logs cost
- Observability logs: all agents log activity to Da Vinci
- Kanban management: Nextcloud Deck card creation and status updates
- Langfuse tracing: logs trace (da-vinci-update) with 8 child generations per pipeline run

**Stage 2 stack:** Qdrant (VM 400, port 6333/6334) + nomic-embed-text + n8n Knowledge Indexer workflow

**Collections:**
- obsidian_knowledge: 1,736 points (90 indexed files including 04-personal/, 08-agents/, 09-people/, 10-projects/)
- gilgamesh_conversations: archived long-term memories

**Pronouns:** she/her

**Technical notes:**

- Direct node references required: use `$('Extract Response').first().json.updatedDoc` instead of `$json` after Merge node
- max_tokens: AI-CONTEXT 25000, changelog 6000, troubleshoot 4000, ROADMAP 8000, agents 8000, current-state 4000, service-catalog 4000, decisions 3000 (prevents hallucinated content)
- Grounding fix: Per-file system prompts in Claude project instructions with explicit per-file sections (CHANGES TO AI-CONTEXT.MD, CHANGES TO CHANGELOG.MD, etc.)
- Deck integration complete with 6 additional nodes for kanban management
- Da Vinci workflow includes Limit node before Notify Complete to prevent notification spam
- Knowledge Indexer runs at 3am daily (full re-index v1), indexes 90 files now (was ~70 before Phase 24.9). Partial reindex triggered via webhook after Da Vinci writes (Phase 24.10).
- **Concurrency lock:** Check Running node queries n8n API before proceeding
- **Schedule:** Inbox Watcher runs every 15 minutes (was 1 minute)
- **Model:** claude-haiku-4-5-20251001 (8 separate API calls, one per file: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
- **Cost logging:** Fires immediately after each of the 8 API calls, before parse/push nodes — logs even on downstream failure
- **Langfuse integration:** Single Langfuse node branched off Push to GitHub (after all 8 files complete). Logs one trace (da-vinci-update) with 8 child generations per run. Uses internal URL: http://192.168.30.223:3000
- **Haiku pricing:** $0.80/1M input tokens, $4.00/1M output tokens
- **Cost per run estimate:** ~$0.25-0.35 for 8 files (up from ~$0.11 for 3 files)
- **Personal Knowledge gateway:** Separate n8n workflow (Da Vinci — Personal Knowledge). Webhook POST /davinci-personal-knowledge. Uses Claude Haiku max_tokens 4000. Writes to muzakkir-profile.md via WebDAV. Filters out one-time events, stores durable personal facts. Quality gate prevents noise.
- **Triggered Qdrant Re-indexing (Phase 24.10):** Webhook POST /davinci-reindex-personal triggers partial reindex (04-personal/ only, ~1s). Fires async after Da Vinci writes. Full reindex still runs daily at 3am (~21s, indexes all 10 folders).

### Nextcloud Deck Integration

Da Vinci integrates with Nextcloud Deck for kanban project management:

**Boards:** 3 boards (Homelab ID: 4, Career ID: 5, Personal ID: 6)
**Stacks:** Backlog → Up Next → Blocked → In Progress → Done
**Labels:** 22 on Homelab (category, priority, agent, effort), 9 on Career/Personal
**Behavior:** Auto-create/complete cards, advisory comments for movement
**API:** http://192.168.30.220/index.php/apps/deck/api/v1.0
**Auth:** Basic auth with app password (credential: NextCloud-Deck)

### Midas — CFO (Cost Tracking & Optimization)

**Status:** Active (Partial — v1 deployed April 27, 2026)
**Ascension Stage:** 2/4

**Two workflows:**

1. **CFO Report** — `/midas` command shows token usage, costs by model, savings from Ollama
2. **Daily Brief** — 9am scheduled summary to Telegram

**Features:**

- Tracks Gilgamesh costs (Haiku, Ollama, Firecrawl searches)
- USD to MYR conversion (hardcoded rate: 4.7)
- Ollama savings calculated at Haiku equivalent rate
- Firecrawl search cost (~$0.001-0.002/search = 2 credits)
- $10 monthly API spend limit monitoring
- command_type tracking for detailed cost breakdown

**Active Responsibilities:**
- /midas command: full API cost report via Telegram
- 9am daily brief: cost summary sent to Telegram
- Reads gilgamesh_costs Data Table for token/cost data
- Planned v2: Firefly III API integration (budget sync, MYR currency)

**Why Midas:**

- Midas touch turns everything to gold → cost optimization
- King of wealth → perfect CFO role

### MERLIN — Proactive Nudges & Health Scheduler

**Status:** Active (Partial — v1 deployed April 27, 2026)
**Ascension Stage:** 2/4

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

**Active Responsibilities:**
- SSL certificate expiry check (hardcoded date — Cloudflare API token truncated in Vault, pending re-store)
- Backup restore test reminder (tracks lastTestDate)
- Proxmox memory check (threshold 85%)
- Vault seal status check
- 8am daily schedule via n8n Cron trigger

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

- Gilgamesh = Telegram-only (life interface), wired to Langfuse (traces input/output/metadata)
- All agents communicate via ntfy (universal notification hub)
- All agents log activity to Da Vinci
- All agents write Obsidian data through Da Vinci Personal Knowledge gateway (Da Vinci is sole writer)
- All agents use Fate/GO servant theming for consistency
- Universal data flow: Input → processing → triple-write (Data Tables + Obsidian + Qdrant)

---

## 🤖 Gilgamesh AI Agent (Phase 7C/7D/7D-Menu/15/41/Da Vinci Stage 2/Phase 7E/Phase 24.9/Phase 24.10/Web Search)

### REPLACE SECTION: Gilgamesh Chat Pipeline — Architecture

**Architecture:**
```
Telegram (@JhinGilgamesh_bot) → n8n Workflow → Get Conversation History
                                                    ↓
                                          Format Messages (RAG + history)
                                                    ↓
                                    If (Needs Web Search?)
                                      ↙              ↖
                               (true)               (false)
                                 ↓                    ↓
                        Web Search (Firecrawl)   Route Model
                                 ↓                    ↓
                         Inject Search Results    Call Ollama or
                                 ↓                 Call Claude API
                               Route Model            ↓
                                 ↓               Merge Response
                        Call Claude API              ↓
                                 ↓             Extract Response
                        Merge Response              ↓
                                 ↓          Save User Message
                        Extract Response      Save Assistant Message
                                 ↓          → gilgamesh_conversations
                        Save User Message         ↓
                        Save Assistant Message  Send Telegram
                        → gilgamesh_conversations ↓
                                 ↓          Count Conversations
                        Send Telegram             ↓
                                 ↓          Archive to Qdrant (async)
                        Count Conversations      ↓
                                 ↓          Langfuse Trace (async)
                        Archive to Qdrant (async) ↓
                                 ↓         Send to Personal Knowledge (async)
                        Langfuse Trace (async)
                                 ↓
                   Send to Personal Knowledge (async)
```

**Key Components:**

| Component | Details |
|-----------|---------|
| **Telegram Trigger** | POST endpoint listening for /messages and /callback_query |
| **Format Messages** | Builds conversation history (last 15 rows from gilgamesh_conversations), performs Qdrant RAG query, sets claude-sonnet-4 as default model |
| **If (Needs Web Search?)** | Keyword detection: search, latest, current, today, weather, news, price, who is, when is, what is, how much, find, look up, cari, semak, berapa |
| **Web Search** | Firecrawl /search endpoint (5 results, 60s timeout) if intent detected |
| **Inject Search Results** | Code node parses Firecrawl data.web array, injects into context with searchContext field |
| **Route Model** | Checks Ollama health (3s timeout), routes: searchContext present → Haiku; Ollama online → qwen3:14b; else → Haiku. Overrides Format Messages default. |
| **Call Ollama** | POST http://192.168.30.221:11434/api/generate with streaming |
| **Call Claude API** | Anthropic API with claude-haiku-4-5-20251001 (fallback) |
| **Extract Response** | Normalizes Ollama (string) vs Claude (object) responses, strips markdown, outputs normalized fields |
| **Save Messages** | Appends to gilgamesh_conversations Data Table (user + assistant rows) |
| **Send Telegram** | Text message, no Parse Mode (markdown stripped) |
| **Archive** | Async: when rows >30, archives to Qdrant gilgamesh_conversations collection |
| **Langfuse** | Async trace (gilgamesh-chat) with input/output/metadata logged to CT 223 |
| **Personal Knowledge** | Async webhook fire-and-forget (5s timeout) to Da Vinci gateway |

**Conversation Buffer Memory:** Already operational. Format Messages Code node fetches last 15 rows from gilgamesh_conversations table and injects as history in messages array.

**Search Routing Rationale:** qwen3:14b ignores injected search context (trained behavior: "I don't have real-time data"). Claude Haiku correctly follows injected Firecrawl results. Search queries always force Haiku routing.

**Cost:**
- Ollama queries: $0 (local inference)
- Haiku fallback: ~$0.001 per query (token efficiency)
- Web search: ~$0.001-0.002 per search (2 Firecrawl credits)

**Pronouns:** she/her. Gilgamesh knows its name + master's name (Muzakkir).

### Features

- **Conversation memory:** Last 15 messages stored in n8n Data Tables (buffer memory)
- **Extended memory:** Conversations archived to Qdrant at 30+ rows (long-term recall)
- **Smart routing:** Ollama (local, primary) → Haiku (fallback or search)
- **RAG knowledge recall:** Queries Qdrant for relevant Obsidian content + personal profile + conversations
- **Web search:** Real-time information via Firecrawl /search API (keyword-based intent detection)
- **Cost tracking:** Token usage logged to gilgamesh_costs table with command_type; cost_usd calculated from token rates (includes Firecrawl search cost)
- **Inline keyboard menu:** Full menu system with all submenus working
- **Context sync:** /update command pushes session summaries to 8 documentation files via GitHub
- **Documentation pipeline:** /sync-docs triggers full documentation regeneration (8 files)
- **Slash commands:** 11 commands for direct actions
- **Health tracking:** Food log, BP log, medication log via interactive button prompts (Phase 22.8B)
- **Personal knowledge:** Recalls profile facts via RAG (name, preferences, history)
- **Langfuse observability:** Traces visible in langfuse.najhin-gaming.com with input/output/metadata

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
| /sync-docs | Full regeneration | Regenerates all documentation (8 files)       |
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
| Web Search    | Firecrawl /search API (2 credits per search)              |
| n8n Container | CT 211, 192.168.30.211                                    |
| Qdrant        | VM 400, ports 6333 (REST) + 6334 (gRPC)                  |
| Langfuse      | CT 223, http://192.168.30.223:3000 (internal VLAN 30 URL) |
| Proxmox API   | root@pam!gilgamesh token                                  |
| Proxmox node  | muzakkir (not kuromoon)                                   |

### Cost Tracking Note

- cost_usd calculated from token rates: Haiku $0.80/1M input, $4.00/1M output
- Ollama tokens captured: data.input_tokens + data.output_tokens from Call Ollama node response
- Ollama queries cost $0 (local inference)
- Firecrawl search cost: ~$0.001-0.002 per search (2 credits at community tier)
- command_type derived from Telegram message text directly
- **Da Vinci:** claude-haiku-4-5-20251001 (8 separate calls), cost logged immediately after each call
- **Da Vinci Personal Knowledge gateway:** claude-haiku-4-5-20251001 max_tokens 4000, cost logged after assessment

### n8n Workflows (Count: 15)

| Workflow                           | Purpose                                  | Nodes | Trigger  |
|------------------------------------|------------------------------------------|-------|----------|
| Gilgamesh — Life Interface         | Main bot, menu, commands, hybrid routing, RAG, extended memory, personal knowledge, web search, Langfuse | 25+  | Telegram |
| Documentation Pipeline — Update    | Session summary → 8 files                | 7     | Webhook  |
| Documentation Pipeline — Sync Docs | Full doc regeneration → 8 files          | 7     | Webhook  |
| Da Vinci — Update Pipeline         | 8 separate Haiku API calls (AI-CONTEXT 25k, changelog 6k, troubleshoot 4k, ROADMAP 8k, agents 8k, current-state 4k, service-catalog 4k, decisions 3k) → GitHub push + Nextcloud + Langfuse | 35+ | Execute Workflow |
| Da Vinci — Inbox Watcher           | Staging inbox → calls Update Pipeline (every 15 min) | 6     | Schedule |
| Da Vinci — Knowledge Indexer       | Obsidian → Qdrant indexing (3am daily, 90 files); partial reindex triggered (04-personal/ only, ~1s) | 8     | Schedule + Webhook |
| Da Vinci — Personal Knowledge      | Receives facts from agents → Claude assessment → muzakkir-profile.md → Qdrant | 7     | Webhook  |
| Midas — CFO Report                 | /midas command cost analysis             | 6     | Webhook  |
| Midas — Daily Brief                | 9am scheduled cost summary               | 4     | Schedule |
| MERLIN — Reminders                 | 8am daily infrastructure checks          | 8     | Schedule |
| Gilgamesh — Daily Note Creator     | Midnight Obsidian daily notes            | 4     | Schedule |
| Gilgamesh — Morning Briefing       | 7am daily summary to Telegram            | 5     | Schedule |
| Update Nextcloud File              | Legacy (unpublished)                     | 5     | Webhook  |
| Push to GitHub                     | Legacy (unpublished)                     | 4     | Webhook  |
| Da Vinci — Documentation Pipeline  | INACTIVE (deactivated 2026-05-18, orphaned) | 17+ | Webhook |

> Note: Hybrid routing (Phase 41), RAG retrieval (Da Vinci Stage 2), Extended Memory (Phase 7E), Personal Knowledge (Phase 24.9), Triggered Qdrant Re-indexing (Phase 24.10), and Web Search are all integrated into the Gilgamesh — Life Interface workflow. Health tracking (Phase 22.8B) is also integrated.

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

### RAG System (Da Vinci Stage 2 + Phase 24.9)

| Component | Details |
|-----------|---------|
| **Vector DB** | Qdrant on VM 400 (http://192.168.30.221:6333) |
| **Embeddings** | nomic-embed-text (768 dimensions) |
| **Collections** | obsidian_knowledge (1,736 points), gilgamesh_conversations (archived) |
| **Indexed Folders** | 10 total: 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 07-daily, 08-agents, 09-people, 10-projects, AI-Stuff/Homelab/homelab-infrastructure (90 files total) |
| **Excluded** | 05-templates/, 06-archive/ |
| **Indexing** | Daily 3am via "Da Vinci — Knowledge Indexer" workflow (full re-index v1). 90 files indexed. Empty file filter added. |
| **Partial Reindex** | Triggered via webhook /davinci-reindex-personal after Da Vinci writes to muzakkir-profile.md (Phase 24.10). Reindexes 04-personal/ folder only (~1s). |
| **Retrieval** | Top 5 results, similarity threshold 0.7, injected into system prompt. Used for both agent context and personal profile recall. |

### SSH & API Access

- **SSH to Kuromoon:** CT 211 → 192.168.10.5:22 (pfSense rule added)
- **SSH to VM 400:** `ssh muzakkir@192.168.30.221` — use muzakkir user, not root
- **SSH to CT 302:** Password auth enabled (PermitRootLogin yes)
- **Proxmox API:** root@pam!gilgamesh token (recreated May 16, 2026)
- **Proxmox node:** muzakkir (not kuromoon)

### Known Constraints

- Telegram messages must be **plain text only** — code blocks, backticks, and complex formatting break JSON parsing in the Claude API request body
- Progress bars use ASCII (= and -) — Unicode block chars don't render on Telegram mobile
- RAG payload field is "content" (not "text" or "pageContent")
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API — use single-quoted strings with concatenation instead
- VM 400 disk block device is /dev/vda (KVM virtio), not /dev/sda (IDE)
- Gemma 3 models hallucinate factual data — not trustworthy for personal assistant role
- llama3.2 removed from VM 400 (hallucinated data in testing)
- Langfuse UI trace list has 1-hour analytics delay by design — traces appear in list after 1 hour, but API and direct URL access work immediately
- Web search queries always route to Haiku: qwen3:14b cannot follow injected search context regardless of instruction strength (model limitation, not prompt engineering)
- Extract Response must detect Ollama vs Claude responses: `typeof data.response === 'string'` for Ollama branch, `claudeData = data.response || data` for Claude branch (Call Claude API wraps in object)
- Send a text message: Parse Mode must NOT be set (leave blank). Extract Response strips all markdown before output.

---

## 🔧 Documentation Pipeline (Phase 16.1/16.2/16.3/16.4)

### Architecture

```
Session Summary → /update → Da Vinci Inbox Watcher → Da Vinci Update Pipeline → 8 Separate Haiku API Calls → 8 Files (AI-CONTEXT + changelog + troubleshoot + ROADMAP + agents + current-state + service-catalog + decisions)
                                                                                                ↓
                                                                                    Nextcloud + GitHub + Langfuse
```

#### Da Vinci Documentation Pipeline (Phase 16.4 — COMPLETE May 21, 2026)

**Expanded workflow from 3 files to 8 files with 8 sequential Haiku API calls:**

```
Raw summaries → AI-CONTEXT-staging.md (rolling append)
                         ↓
        Da Vinci Inbox Watcher (every 15 minutes) → Execute Workflow trigger
                         ↓
        Da Vinci Update Pipeline → 8 separate Haiku API calls
                         ├─→ Call 1: AI-CONTEXT.md (max_tokens 25000)
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
        Parse Response → GitHub push → Nextcloud sync → Langfuse trace → Staging archival
```

### Commands

| Command    | Purpose                                   | Files Updated |
|------------|-------------------------------------------|---------------|
| /update    | Every session — merge summary into docs   | 8 files       |
| /sync-docs | Deployment sessions — regenerate all docs | 8 files       |

### File Coverage

| File | Update Strategy | max_tokens | Da Vinci Action |
|------|----------------|------------|-----------------|
| AI-CONTEXT.md | Full rewrite | 25000 | LLM merges session into master doc |
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
| Langfuse     | Trace logging (http://192.168.30.223:3000)          |
| Da Vinci     | Async processing (she/her pronouns)                 |
| Staging file | AI-CONTEXT-staging.md in Nextcloud (rolling append) |
| Deck         | Kanban cards created from action items               |

### Da Vinci Technical Notes (Phase 16.4)

- **Pipeline architecture:** 8 separate Haiku API calls (one per file) instead of single merged call — sequential chain to prevent hallucination across files
- **Cost logging:** Fires immediately after each of the 8 Haiku API calls, before parse/push nodes — captures cost even if downstream nodes fail
- **Haiku pricing:** $0.80/1M input tokens, $4.00/1M output tokens
- **max_tokens per file:**
  - AI-CONTEXT: 25000 (comprehensive infrastructure state) — bumped from 20000 (was hitting ceiling)
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
- **Property names:** Verify sessionSummary property name matches across nodes — use fileContent from trigger payload
- **Validation:** Parse Response node rejects placeholder outputs before GitHub push
- **decisions.md handling:** New file on first run (null SHA in GitHub API), automatic creation on subsequent runs
- **Langfuse integration (May 21, 2026):** Single Langfuse node branched off Push to GitHub (after all 8 files complete). Logs one trace (da-vinci-update) with 8 child generations per run. Uses internal URL: http://192.168.30.223:3000 for VLAN 30 internal routing. Timestamp uses new Date().toISOString() (UTC, no timezone offset).
- **Cost per run estimate:** ~$0.25-0.35 for 8 files (was ~$0.11 for 3 files)
- **Claude project instructions:** Session summary template now has explicit per-file sections (CHANGES TO AI-CONTEXT.MD, CHANGES TO CHANGELOG.MD, CHANGES TO ROADMAP.MD, CHANGES TO AGENTS.MD, CHANGES TO CURRENT-STATE.MD, CHANGES TO SERVICE-CATALOG.MD, CHANGES TO DECISIONS.MD, ERRORS FIXED) so Da Vinci gets unambiguous per-file instructions
- **API key handling:** Hardcode Anthropic API key and GitHub token directly in each node (consistent with existing 3 nodes). Trigger payload fields only define fileContent and chatId; adding more fields adds complexity for no benefit.
- **Code node syntax:** Single-quoted strings with concatenation for system prompts. Backtick template literals cause 400 errors on Anthropic API when used in n8n Code nodes.
- **Tested & Verified (May 21, 2026):** Full 8-file pipeline tested. All 8 files pushed to GitHub successfully. Cost logging verified (8 rows per session). Staging inbox archival working. Per-file system prompts resolving hallucination across files. Langfuse trace and 8 generations verified in langfuse.najhin-gaming.com UI. One trace per session confirmed.

---

## 🤖 Bots & Integrations

| Bot                | Platform         | Source              | Status   | Purpose                                                        |
|--------------------|------------------|---------------------|----------|----------------------------------------------------------------|
| @JhinGilgamesh_bot | Telegram         | n8n CT 211          | ✅ Active | Personal AI agent — chat, homelab control, /update, /sync-docs, personal knowledge, web search |
| Midas              | Telegram         | n8n CT 211          | ✅ Active | CFO cost tracking — /midas reports, 9am daily briefs           |
| MERLIN             | Telegram         | n8n CT 211          | ✅ Active | Reminders — 8am daily infrastructure checks                    |
| Daily Note Creator | Nextcloud WebDAV | n8n CT 211          | ✅ Active | Midnight daily note creation in Obsidian vault                 |
| Morning Briefing   | Telegram         | n8n CT 211          | ✅ Active | 7am daily summary to Telegram                                  |
| Health Tracking    | Obsidian WebDAV  | n8n CT 211          | ✅ Active | Food/BP/medication logging via Gilgamesh buttons               |
| Personal Knowledge | Obsidian WebDAV  | n8n CT 211          | ✅ Active | muzakkir-profile.md managed by Da Vinci gateway (Phase 24.9)   |
| Homelab Alerts     | Telegram         | Alertmanager CT 205 | ✅ Active | Critical alerts (host down, high CPU/memory/disk)              |
| Homelab Alerts     | Discord webhook  | Alertmanager CT 205 | ✅ Active | Warning-level alerts to #alerts channel                        |
| Homelab-Ntfy       | ntfy CT 222      | Uptime Kuma CT 206  | ✅ Active | Service down alerts via ntfy                                   |
| Nextcloud Deck     | Deck CT 220      | n8n CT 211          | ✅ Active | Kanban project management via Da Vinci                         |
| Langfuse           | CT 223           | n8n CT 211 + Gilgamesh | ✅ Active | LLM tracing (da-vinci-update + gilgamesh-chat traces)         |
| Firecrawl          | Cloud API        | n8n CT 211          | ✅ Active | Web search for Gilgamesh (keyword-based intent routing)        |

**Planned:** Wire Langfuse into MERLIN and Midas. Migrate Alertmanager alerts to route through n8n first (central hub). Game server notifications to Discord via n8n. Triggered Qdrant re-indexing after Da Vinci Personal Knowledge writes (complete May 25, 2026).

### Nextcloud Deck Integration

| Component | Details |
|-----------|---------|
| **App** | Deck on CT 220 (cloud.najhin-gaming.com) |
| **Boards** | 3 boards: Homelab (ID: 4), Career (ID: 5), Personal (ID: 6) |
| **Stacks** | 5 stacks per board: Backlog, Up Next, Blocked, In Progress, Done |
| **Labels** | 22 labels on Homelab (category, priority, agent, effort), 9 on Career/Personal |
| **API Base** | http://192.168.30.220/index.php/apps/deck/api/v1.0 |
| **Auth** | Basic auth with app password (credential: NextCloud-Deck) |

### Firecrawl Web Search Integration

| Component | Details |
|-----------|---------|
| **Service** | Firecrawl (@firecrawl.dev) |
| **Type** | Cloud API for web search and content scraping |
| **Endpoint** | POST https://api.firecrawl.dev/v1/search |
| **Auth** | API key stored in n8n credential "Firecrawl account" |
| **n8n Node** | @mendable/n8n-nodes-firecrawl v2.1.1 |
| **Tier** | Community (free, no credit card required) |
| **Cost** | ~2 credits per search (~$0.001-0.002 per search at community pricing) |
| **Gilgamesh Integration** | Keyword-based intent detection in If node, search results injected into context, search queries force-routed to Haiku |
| **Results Format** | data.web array with item.title, item.url, item.description per result |
| **Status** | Active (deployed May 25, 2026) |

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
| 16.4    | Da Vinci Documentation Pipeline Expansion (8 files)              | ✅ Complete | May 21, 2026 |
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
| 24.8    | Langfuse AI Observability                                        | ✅ Complete | May 21, 2026 |
| 24.9    | Personal Knowledge System (muzakkir-profile.md + gateway)        | ✅ Complete | May 22, 2026 |
| 24.10   | Triggered Qdrant Re-indexing (on Da Vinci Personal Knowledge write) | ✅ Complete | May 25, 2026 |
| 25.1    | Voice Interface — Claude API transcription + voice responses     | 📋 Planned | —            |
| 25.2    | Email Integration — IMAP monitoring + smart filtering            | 📋 Planned | —            |
| 25.3    | Self-Evolving Skills — Hermes-style learning loop (knowledge docs + RAG recall) | 📋 Planned | —            |
| 25.4    | Content Creation — automated reports, social posts              | 📋 Planned | —            |
| 38      | Ollama + ROCm on Kuromoon RX 6700 XT                             | ✅ Complete | Apr 24, 2026 |
| 39      | Open WebUI                                                       | ✅ Complete | Apr 24, 2026 |
| 41      | Hybrid Routing (Ollama + Haiku + Sonnet)                         | ✅ Complete | May 8, 2026  |
| 52      | DOCP Memory Optimization (128GB DDR4-3200)                       | ✅ Complete | May 2026     |
| Web Search | Gilgamesh Web Search (Firecrawl integration)                  | ✅ Complete | May 25, 2026 |

---

## 📝 Session Log (Most Recent 5)

### Session 15: May 25, 2026 — Phase 24.10 + Phase 52 + Web Search Deployment

**Duration:** 3h
**Topics:** Phase 24.10 (Triggered Qdrant Re-indexing) deployed via webhook /davinci-reindex-personal. Phase 52 (DOCP Memory Optimization) confirmed complete. Web Search feature built and deployed in Gilgamesh. Firecrawl API integrated with keyword-based intent detection. Search results forced to Haiku routing. 16 Gilgamesh feature wishlist compiled. Documentation audit findings: Knowledge Indexer indexes 10 folders (not 4), Claude Sonnet 4 overridden in Route Model.
**Decisions:** Triggered reindex via webhook after Da Vinci writes (async, ~1s). Partial reindex (04-personal/ only) reduces full 21s rebuild. Full daily reindex still at 3am. Web search routed to Haiku (qwen3:14b cannot follow injected context). Firecrawl community node used (zero friction). Search keyword list: search, latest, current, today, weather, news, price, who is, when is, what is, how much, find, look up, cari, semak, berapa. Da Vinci Stage 2 marked complete — RAG fully operational. Phase 7E duplicate in ROADMAP removed (conversation buffer already working). Model routing correction: Format Messages sets Sonnet 4 default, Route Model overrides.
**Outcomes:** Phase 24.10 complete. Partial reindex webhook fires at ~990ms post-write. Full daily reindex ~21s. Web Search live in Gilgamesh. Firecrawl searches working (2 credits per search). 16-feature wishlist documented. Phase 52 confirmed. Knowledge Indexer folder count verified at 10 (including AI-Stuff/Homelab/homelab-infrastructure).
**Errors Fixed:**
  - Qdrant re-indexing not triggered after profile writes — webhook /davinci-reindex-personal not wired in Personal Knowledge gateway → added async HTTP Request node (timeout 5s)
  - Reindex path not specified — webhook without reindexType field triggers full rebuild (21s) — added check in Knowledge Indexer If node: reindexType === 'partial' → index only 04-personal/ (~1s)
  - Web Search results not reaching LLM — qwen3:14b ignores injected context entirely (fundamental limitation, not prompt fixable) → always route search queries to Haiku (added searchContext routing in Route Model)
  - If node returning boolean true instead of string — need .toString() — added `.toString()` to If expression return value
  - Firecrawl output structure unknown initially — docs vague — discovered data.web array with per-item structure via testing
  - Extract Response failing on web search results — code expected claudeData for Claude responses but searchContext forced null response in some branches → added fallback typeof checks
**Next:** Apply all corrections in next Da Vinci pipeline run. Test web search across varied query types (current news, weather, prices, "who is" queries). Monitor Phase 24.10 stability over 48h. Decide next priority: Midas v2 (Firefly III + receipts) vs Smart Morning Briefing vs Guardian Agent.

### Session 14: May 25, 2026 — Documentation Audit: Knowledge Indexer Folder Reconciliation

**Duration:** 1h
**Topics:** Full reconciliation of Knowledge Indexer folder list across AI-CONTEXT, agents, and n8n workflow nodes. Found that actual n8n workflow indexes 10 folders, but AI-CONTEXT and agents.md documented only 4. Discovered Sonnet 4 set in Format Messages (not used — Route Model overrides to qwen3:14b). Clarified Da Vinci Stage 2 vs Phase 7E (RAG pipeline complete, conversation buffer already working).
**Decisions:** Correct AI-CONTEXT and agents to reflect 10 actual indexed folders. Remove Phase 7E from ROADMAP Planned table (duplicate — already in Completed). Mark Da Vinci Stage 2 as effectively complete (Qdrant, embeddings, Knowledge Indexer all live). Conversation buffer memory (last 15 rows) already operational in Format Messages.
**Outcomes:** Knowledge Indexer folder list corrected. 10 folders confirmed: 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 07-daily, 08-agents, 09-people, 10-projects, AI-Stuff/Homelab/homelab-infrastructure. Model routing clarified: Format Messages default (Sonnet 4) overridden by Route Model.
**Errors:** None (documentation/reconciliation session only).
**Next:** Incorporate corrections into next /update. Phase 24.10 + Web Search testing (Session 15).

### Session 13: May 22, 2026 — Documentation Audit & Corrections (No Deployment)

**Duration:** 1h
**Topics:** Full audit of all 8 Da Vinci documentation files against full conversation history. Found 11 discrepancies across 4 files: ROADMAP.md had 5 archived phases (22.8C/22.8D/22.8E/22.15/22.16) still showing as active/planned; current-state.md had completely hallucinated hardware specs (EPYC 5645/256GB/RTX 4070 vs actual Ryzen 5 5600X/128GB/RX 6700 XT); agents.md MERLIN/Midas marked Planned/0/4 but deployed April 27, 2026 as Active/2/4; Knowledge Indexer folder lists inconsistent across 3 files; AI-CONTEXT.md still listed llama3.2 as installed (removed May 21-22); Phase 7E missing from ROADMAP.md completed table.
**Decisions:** All corrections applied directly (no deployment — corrections only). Hardware section should always be REPLACE SECTION to prevent drift. Ollama models list: remove llama3.2:latest. Knowledge Indexer folders require manual verification against n8n workflow node.
**Outcomes:** 11 discrepancies identified and corrections provided for Da Vinci next session. ROADMAP.md archived 5 phases. current-state.md hardware corrected. agents.md MERLIN/Midas status updated. AI-CONTEXT.md models list corrected. Phase 7E added to completed table. Langfuse + Personal Knowledge stability confirmed 24h post-deployment.
**Errors Fixed:**
  - ROADMAP.md In Progress: 22.8C still showing — removed, empty In Progress section
  - ROADMAP.md Planned table: 22.8D/22.8E/22.15/22.16 all archived May 10 (Homepage retired) — removed with reason
  - current-state.md Compute: EPYC 5645/256GB listed (hallucinated from January hypothetical) — corrected to actual Ryzen 5 5600X/128GB
  - current-state.md Storage: CT 215 referenced (doesn't exist), Nextcloud is CT 220 — corrected
  - current-state.md GPU: RTX 4070 listed (hallucinated) — corrected to actual RX 6700 XT
  - agents.md MERLIN: Status Planned/0/4 — corrected to Active/2/4 (deployed April 27)
  - agents.md Midas: Status Planned/0/4 — corrected to Active/2/4 (deployed April 27)
  - agents.md Knowledge Indexer: Multiple folder lists across files all different — flagged for manual verification
  - AI-CONTEXT.md Ollama models: llama3.2:latest still listed — removed (hallucinated in testing)
  - ROADMAP.md completed table: Phase 7E missing — added May 15, 2026 completion
  - ROADMAP.md Guardian: dependency listed as Phase 22.8C (archived) — corrected to Phase 24.9
**Next:** Apply corrections in next Da Vinci pipeline run. Manually verify Knowledge Indexer folder list against n8n workflow. Delete muzakkir-profile.md.md conflict artifact. Phase 24.10 (Triggered Qdrant Re-indexing).

### Session 12: May 22, 2026 — Phase 24.9: Personal Knowledge System + Langfuse Gilgamesh Wiring

**Duration:** 2h
**Topics:** Verified Langfuse UI trace list working (1-hour analytics delay resolved overnight), wired Langfuse into Gilgamesh (traces visible with input/output), created Da Vinci Personal Knowledge gateway, built muzakkir-profile.md in Obsidian 04-personal/, updated Gilgamesh to send facts to Da Vinci via webhook, integrated Langfuse tracing into Gilgamesh Extract Response node, tested model behavior on VM 400, updated Knowledge Indexer (added 04-personal/, fixed folder names), discussed $0 AI Architecture Stack diagram
**Decisions:** All agents write Obsidian through Da Vinci Personal Knowledge gateway (not directly), Gilgamesh reads Obsidian directly via RAG, quality filter: store durable facts (skip one-time events), SKIP detection uses startsWith not ===, date placeholder replaced in code not by Claude, use internal Langfuse URL (http://192.168.30.223:3000), qwen3:14b stays primary (honesty critical), VM 400 disk device is /dev/vda not /dev/sda
**Outcomes:** Phase 24.9 complete. Langfuse UI working (traces visible after 1 hour, direct API immediate). Gilgamesh wired to Langfuse (gilgamesh-chat trace). Da Vinci Personal Knowledge gateway deployed (muzakkir-profile.md). Gilgamesh successfully recalling personal profile via RAG (knows "Muzakkir" name, dark mode preference). Knowledge Indexer expanded to 90 files, 1,736 Qdrant chunks. System prompt updated — Gilgamesh now knows its name + master's name.
**Errors Fixed:**
  - Langfuse UI trace list "No results" → resolved overnight (1-hour analytics aggregation delay by design)
  - Gilgamesh system prompt didn't mention its name → qwen3:14b confused "Gil" as name query → fixed with explicit "Your name is Gilgamesh" in Route Model
  - Knowledge Indexer "Document loader not initialized" → empty files in new folders → added If node filter
  - Knowledge Indexer folder names stale (08-projects/09-meetings/10-reference) → vault restructured to 08-agents/09-people/10-projects → updated folder list
  - Fetch Current Profile 404 → muzakkir-profile.md not yet synced → replaced HTTP Request with Code node fallback to template
  - SKIP detection bug → Da Vinci returns "SKIP\n\nReasoning" but code checked === 'SKIP' → fixed with startsWith('SKIP')
  - "Assignment to constant variable" → const currentProfile → changed to let
  - VM 400 disk expansion failed with /dev/sda → device is /dev/vda on KVM → used correct device
  - muzakkir-profile.md.md duplicate in Nextcloud → Nextcloud + Obsidian sync conflict → needs manual deletion
**Next:** Delete conflict artifact (muzakkir-profile.md.md), build triggered Qdrant re-indexing (immediate after Da Vinci writes), wire Langfuse into MERLIN/Midas, test personal knowledge stability 24h

### Session 11: May 21, 2026 — Phase 24.8 Completion + Model Testing + VM 400 Expansion + Langfuse Wiring

**Duration:** 2h
**Topics:** Updated Claude project instructions with expanded 8-file session summary template, expanded Da Vinci Update Pipeline from 3 to 8 files (ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md added), fixed 5 bugs in new pipeline nodes, bumped AI-CONTEXT max_tokens to 25000 (was hitting ceiling), tested Gemma 3:4b/3:12b/phi4-mini/llama3.2 models on VM 400, expanded VM 400 disk from 56GB to 86GB, wired Langfuse observability into Da Vinci Update Pipeline, debugged Langfuse UI trace list bug
**Decisions:** Per-file system prompts prevent cross-file hallucination. decisions.md promoted to Phase 2. decisions.md auto-created on first run with null SHA handling. Cost per run ~$0.25-0.35. qwen3:14b stays primary (honest about limitations vs confident hallucination). Langfuse — Da Vinci uses single batch node (internal URL http://192.168.30.223:3000). AI-CONTEXT max_tokens bumped to 25000 (middle ground). Aider + local Ollama identified as better fit than Claude Code for future EMIYA code agent.
**Outcomes:** Phase 16.4 (8-file pipeline) complete and verified. Phase 24.8 (Langfuse) wired and verified. All 8 files pushed to GitHub. 8 cost rows logged per session. Langfuse traces confirmed in ClickHouse and accessible via API, but UI trace list has known v3 self-hosted bug (traces don't appear in list view). VM 400 disk expanded to 86GB (34GB free). Gemma models removed (hallucination). qwen3:14b + llama3.2 confirmed as trustworthy.
**Errors Fixed:**
  - Null GitHub token in Fetch GitHub Files — hardcoded directly
  - Null API keys in 5 new Claude nodes — hardcoded at top of each
  - filesToPush array only had 3 files — updated to all 8 with null SHA handling
  - sessionSummary undefined — changed to fileContent from trigger
  - Backtick template literals in system prompts causing 400 errors — replaced with single-quoted strings + concatenation
  - Log Cost — service-catalog had wrong command_type (copy-paste error) — corrected
  - Langfuse ingestion returning 400 — timestamp field required in every batch event (fixed)
  - Langfuse node ECONNREFUSED 192.168.30.223:8123 — ClickHouse port 8123 not accessible from CT 211, abandoned ClickHouse time query
  - Gemma3:4b disk pull failed — VM 400 disk 100