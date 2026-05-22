# Current State Documentation
**Last Updated:** 2026-05-22 (Phase 24.9 — Documentation Audit & Corrections)

## Overview
Homelab infrastructure documentation and automation project. Core system uses Claude AI agents to maintain living documentation across 8 coordinated files through automated pipelines triggered by cron jobs.

## Core Documentation Files (Da Vinci Update Pipeline)
Pipeline expanded Phase 16.4 from 3 files to 8 files, each with dedicated token budget and handling logic.

| File | Purpose | Token Budget | Status |
|------|---------|--------------|--------|
| AI-CONTEXT.md | System context, infrastructure overview, agent definitions | 25,000 | Active |
| changelog.md | Session history, changes, features added | 6,000 | Active |
| troubleshoot.md | Known issues, solutions, debug procedures | 4,000 | Active |
| ROADMAP.md | Phase tracking, priorities, planned work | 8,000 | Active |
| agents.md | Agent definitions, capabilities, pipeline architecture | 8,000 | Active |
| current-state.md | This file; infrastructure state snapshot | 4,000 | Active |
| service-catalog.md | Service/container inventory, networking | 4,000 | Active |
| decisions.md | Decision log with alternatives considered | 3,000 | Active (Phase 2, promoted from Phase 3) |

**Total Pipeline Tokens:** ~62,000 per session update run

## Da Vinci Agent Configuration
**Role:** Documentation librarian, maintains consistency across 8-file knowledge base. Also operates Personal Knowledge gateway for ingesting agent-generated facts into Obsidian.

**Update Pipeline Architecture:**
- 8 sequential Haiku API calls (one per file)
- Each call receives full session summary with dedicated per-file section
- Cost logging fires immediately after each API call (8 rows per session)
- Estimated cost per run: ~$0.14-0.16 (AI-CONTEXT bumped to 25,000 tokens)
- Haiku pricing: $0.80/1M input tokens, $4.00/1M output tokens
- Langfuse observability: Active as of Phase 24.8 — single trace (da-vinci-update) with 8 child generations per run

**Personal Knowledge Gateway (Phase 24.9):**
- Separate n8n workflow: Da Vinci — Personal Knowledge
- Webhook: POST /davinci-personal-knowledge (internal CT 211 URL)
- Receives facts from all agents (currently wired from Gilgamesh)
- Claude Haiku (max_tokens 4000) assesses and merges facts into muzakkir-profile.md
- Quality filter: stores durable personal facts, SKIPs one-time events and conversational noise
- Writes to Obsidian via WebDAV, costs logged to gilgamesh_costs table
- All agents route Obsidian writes through this gateway (not direct writes)

**File Update Patterns:**
- AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md: Full rewrite/major sections
- current-state.md, service-catalog.md: Append/update with preservation of existing entries
- decisions.md: Append-only (Phase 2 core pipeline, promoted from Phase 3 backlog due to decisions being lost between sessions)

**Known Behaviors:**
- decisions.md created as new file on first pipeline run (null SHA handled in Push to GitHub node)
- Cost logging pattern: 8 cost rows logged per session update run, fires immediately after each API call
- Hardcoded API keys in all 8 Claude API nodes (consistent with existing approach)
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; all system prompts use single-quoted strings with concatenation
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema (trigger only defines fileContent and chatId)
- Langfuse wiring: Single node branched off Push to GitHub sends all 8 generations in one batch with internal URL (http://192.168.30.223:3000)
- SKIP detection in Da Vinci gateway uses startsWith('SKIP') not strict equality (Da Vinci may return "SKIP\n\nReasoning...")
- Date placeholder ({{date}}) replaced in Code node before Claude API call with actual MYT date (YYYY-MM-DD format)

## Da Vinci Update Pipeline Status (Phase 24.8-24.9)
**Pipeline Architecture:**
- Total nodes: 36 (8 sequential Haiku API calls + 8 Log Cost nodes + 8 Parse nodes + Update Tables + Push to GitHub + Langfuse — Da Vinci + Push to Nextcloud + Send Confirmation)
- Runtime: ~5 minutes per session update run
- Cost per run: ~$0.14-0.16 (~$4.20-4.80/month at once daily)
- Files covered: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Per-file API calls: 8 separate Haiku calls with immediate cost logging
- Langfuse observability: Wired Phase 24.8 — trace name da-vinci-update with 8 child generations per run

**Deployment Status (Phase 24.9):**
- ✅ Expanded from 3-file to 8-file sequential Haiku chain (Phase 16.4)
- ✅ Resolved 5 critical bugs during deployment (Phase 16.4)
- ✅ Updated Claude project instructions with explicit per-file session summary sections (Phase 16.4)
- ✅ All 8 files pushed to GitHub from filesToPush array (Phase 16.4)
- ✅ decisions.md promoted to Phase 2 core pipeline with null SHA handling for new file creation (Phase 16.4)
- ✅ Pipeline rebuilt 2026-05-19: 8 separate Haiku API calls with immediate cost logging verified working (Phase 16.4)
- ✅ Langfuse wired to Da Vinci Update Pipeline: single node off Push to GitHub (Phase 24.8)
- ✅ Langfuse trace created (da-vinci-update) with 8 generations visible (Phase 24.8 test run 2026-05-21)
- ✅ AI-CONTEXT max_tokens bumped to 25,000 (was 20,000 — was hitting ceiling every run)
- ✅ Log Cost — service-catalog command_type fixed: corrected from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`
- ✅ Da Vinci Personal Knowledge gateway deployed (Phase 24.9)
- ✅ Langfuse wired to Gilgamesh agent (Phase 24.9)
- ✅ Documentation audit completed (Phase 24.9) — 11 discrepancies corrected across 4 files

## Automation Status
**Trigger:** Cron job (time TBD per Phase 16 work)

**Known Limitations & Resolutions (Phase 24.9):**
- ✅ decisions.md dynamically created on first run (null SHA path implemented)
- ✅ Fetch GitHub Files: hardcoded token (not passed via trigger)
- ✅ All 8 files now pushed to GitHub from filesToPush array
- ✅ Per-file API calls: 8 separate Haiku calls with immediate cost logging
- ✅ Langfuse wiring: Single node branched off Push to GitHub using internal URL (http://192.168.30.223:3000)
- ✅ Staging-inbox stuck file issue resolved: deleted stuck file, rebuilt pipeline
- ✅ Langfuse trace (da-vinci-update) confirmed appearing in Langfuse UI with 8 child generations (Phase 24.8 test run)
- ✅ Log Cost — service-catalog command_type fixed (was copy-paste error)
- ✅ Langfuse UI trace list bug resolved (1-hour analytics delay by design; traces now visible overnight)
- ✅ Da Vinci Personal Knowledge gateway filters via startsWith('SKIP') not strict equality
- ✅ Date placeholder replaced in Code node before Claude call (not by Claude)
- ✅ Knowledge Indexer: added empty file filter to prevent crash on empty downloads
- ✅ Hardware specs corrected (Phase 24.9): EPYC 5645/256GB/RTX 4070 were hallucinated; real hardware is Ryzen 5 5600X, 128GB DDR4, RX 6700 XT
- ✅ CT 215 reference corrected to CT 220 (Nextcloud)
- ⚠️ GitHub docs/ folder contains stale changelog.md and troubleshoot.md artifacts from old pipeline — needs cleanup
- ⚠️ muzakkir-profile.md.md duplicate file exists in Nextcloud 04-personal/ folder (conflict artifact from Obsidian sync + WebDAV write race)
- ⚠️ Knowledge Indexer folder list requires manual verification against n8n workflow node — inconsistent lists found across 3 documentation files

**Tested & Working:**
- 8-file sequential pipeline architecture
- Per-file token budgets and update patterns
- Cost logging for all 8 API calls, fires immediately after each call
- decisions.md null SHA handling in Push to GitHub
- Single-quoted string concatenation in system prompts (template literals rejected)
- Hardcoded credentials in all 8 Claude API nodes
- 8-file separate API call pattern with immediate cost logging
- Langfuse observability with single trace and 8 child generations per run
- Langfuse trace visibility in UI (da-vinci-update trace with 8 generations confirmed 2026-05-21)
- AI-CONTEXT max_tokens at 25,000 no longer hitting ceiling
- Da Vinci Personal Knowledge gateway (filter, assess, merge, write, log)
- Gilgamesh sending personal facts to Da Vinci Personal Knowledge gateway
- Langfuse wired to Gilgamesh (trace: gilgamesh-chat, contains input/output/metadata)
- Knowledge Indexer indexing 90 files from confirmed folders
- muzakkir-profile.md creation and RAG retrieval by Gil
- Gil successfully recalling personal facts via Qdrant RAG (name: Muzakkir, dark mode preference)

## Hardware Infrastructure

### Compute
- **Proxmox Host (Kuromoon):** Ryzen 5 5600X, 128GB DDR4-3200 (4x32GB Corsair Vengeance LPX, installed May 16, 2026), RX 6700 XT 12GB (passed through to VM 400), ASUS TUF B550M-E mATX motherboard
- **CT 211 (automation-n8n):** 4 vCPU, 4GB RAM, Debian 12
- **VM 400 (ollama-gpu):** 4 vCPU, 16GB RAM, 86GB disk (expanded from 56GB 2026-05-21), Ubuntu 22.04 LTS, RX 6700 XT 12GB GPU (AMD, ROCm backend)

### Storage
- **GitHub:** Primary source of truth for all 8 documentation files
- **Nextcloud (CT 220):** File backup and collaboration (second-brain/, 04-personal/, staging-inbox/, etc.)
- **VM 400 (Ollama + Qdrant):** Local LLM inference server and vector database

## Containerized Services

### CT 211 (automation-n8n)
- **Image:** n8n:latest
- **IP:** 192.168.30.211
- **Purpose:** Workflow automation, Da Vinci Update Pipeline, Da Vinci Personal Knowledge gateway, Gilgamesh, and future agents
- **Workflows:** 
  - Da Vinci Update Pipeline (8-file document sync)
  - Da Vinci — Personal Knowledge (gateway: POST /davinci-personal-knowledge)
  - Gilgamesh (chat + RAG + memory + Langfuse)
  - Knowledge Indexer (Qdrant sync)
  - MERLIN (weather + alerts + Langfuse incoming) — deployed April 27, 2026
  - Midas (notifications) — deployed April 27, 2026
  - And others
- **Databases:** PostgreSQL (internal)
- **Connections:** Anthropic API, GitHub API, Nextcloud, ClickHouse (CT 223), Ollama (VM 400), Qdrant (VM 400), Langfuse (CT 223)

### CT 223 (observability-langfuse)
- **Image:** langfuse/langfuse:3.174.1
- **IP:** 192.168.30.223
- **Purpose:** LLM observability and cost tracking
- **Status:** Wired to Da Vinci Update Pipeline and Gilgamesh — traces active and visible
- **Dependencies:** PostgreSQL (internal), ClickHouse (internal)
- **Known Issue:** UI trace list shows no results despite traces being present in ClickHouse — RESOLVED Phase 24.9 (1-hour analytics delay by design for aggregation stability; traces now visible in UI overnight)
- **Configuration:** LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=true (set 2026-05-21)
- **Traces:**
  - da-vinci-update: 8 child generations (one per documentation file), fires daily after Push to GitHub
  - gilgamesh-chat: input/output/metadata, fires on every user message, async from Extract Response node

### Other Infrastructure
- **RM 30 (cost database):** ClickHouse instance for cost metrics
- **CT 220 (Nextcloud):** File backup and collaboration (second-brain/, 04-personal/, etc.)
- **VM 400 (Ollama + Qdrant):** Local LLM inference server and vector database

## VM 400 (ollama-gpu) Status
- **Disk:** 86GB (expanded from 56GB on 2026-05-21 via Proxmox resize + LVM extension)
- **Available Space:** 34GB free post-expansion
- **Block Device:** /dev/vda (KVM virtio, not /dev/sda)
- **Installed Ollama Models:**
  - qwen3:14b (primary model, 9.3GB, confirmed honest about limitations)
  - qwen3.5:latest (secondary comparison model)
  - nomic-embed-text:latest (embeddings for Qdrant, 768 dims)
- **Removed Models (all hallucinated factual data in testing):**
  - gemma3:4b (removed May 21-22)
  - gemma3:12b (removed May 21-22)
  - phi4-mini (removed May 21-22)
  - llama3.2:latest (removed May 21-22)
- **Qdrant Vector Database:**
  - Collection: obsidian_knowledge
  - Chunks: 1,736 (was 1,567 before adding 04-personal/ folder)
  - Indexed folders: 04-personal/ (added May 22, 2026, includes muzakkir-profile.md), 08-agents/, 09-people/, 10-projects/
  - Note: Folder list requires manual verification against n8n Knowledge Indexer workflow node
- **Purpose:** Local LLM inference for Gilgamesh and future agents (MERLIN, Midas, etc.); vector embeddings for RAG
- **Strategy:** Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)

## Obsidian Integration (Personal Knowledge System)
- **Vault Location:** second-brain/ in Nextcloud (CT 220)
- **04-personal/ Folder:** Now included in Qdrant indexing (privacy exclusion reversed — all internal VLAN 30)
- **muzakkir-profile.md:** Personal profile file managed by Da Vinci Personal Knowledge gateway
  - Path: 04-personal/muzakkir-profile.md
  - Current content: dark mode preference
  - Source: Gilgamesh sends facts via Da Vinci gateway
  - Updated: Da