# Current State Documentation
**Last Updated:** 2026-05-22 (Phase 24.9)

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

## Da Vinci Update Pipeline Status (Phase 24.8)
**Pipeline Architecture:**
- Total nodes: 36 (8 sequential Haiku API calls + 8 Log Cost nodes + 8 Parse nodes + Update Tables + Push to GitHub + Langfuse — Da Vinci + Push to Nextcloud + Send Confirmation)
- Runtime: ~5 minutes per session update run
- Cost per run: ~$0.14-0.16 (~$4.20-4.80/month at once daily)
- Files covered: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Per-file API calls: 8 separate Haiku calls with immediate cost logging
- Langfuse observability: Wired Phase 24.8 — trace name da-vinci-update with 8 child generations per run

**Deployment Status (Phase 24.8-24.9):**
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
- ⚠️ GitHub docs/ folder contains stale changelog.md and troubleshoot.md artifacts from old pipeline — needs cleanup
- ⚠️ muzakkir-profile.md.md duplicate file exists in Nextcloud 04-personal/ folder (conflict artifact from Obsidian sync + WebDAV write race)

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
- Knowledge Indexer indexing 90 files from 10 folders (04-personal through 13-archive)
- muzakkir-profile.md creation and RAG retrieval by Gil
- Gil successfully recalling personal facts via Qdrant RAG (name: Muzakkir, dark mode preference)

## Hardware Infrastructure

### Compute
- **Proxmox Host:** EPYC 5645 w/ 256GB RAM, 2x 2TB NVME
- **CT 211 (n8n-automation):** 4 vCPU, 4GB RAM, Ubuntu 22.04 LTS
- **VM 400 (ollama-gpu):** 4 vCPU, 16GB RAM, 86GB disk (expanded from 56GB 2026-05-21), Ubuntu 22.04 LTS, RTX 4070 GPU

### Storage
- **Nextcloud (CT 215):** Primary project backup repository
- **GitHub:** Documentation and automation code repository
- **ClickHouse (CT 223):** Analytics database for cost tracking and Langfuse observability

## Containerized Services

### CT 211 (n8n-automation)
- **Image:** n8n:latest
- **IP:** 192.168.30.211
- **Purpose:** Workflow automation, Da Vinci Update Pipeline, Da Vinci Personal Knowledge gateway, Gilgamesh, and future agents
- **Workflows:** 
  - Da Vinci Update Pipeline (8-file document sync)
  - Da Vinci — Personal Knowledge (gateway: POST /davinci-personal-knowledge)
  - Gilgamesh (chat + RAG + memory + Langfuse)
  - Knowledge Indexer (Qdrant sync)
  - MERLIN (weather + alerts + Langfuse incoming)
  - Midas (notifications)
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
- **CT 215 (Nextcloud):** File backup and collaboration (second-brain/, 04-personal/, etc.)
- **VM 400 (Ollama + Qdrant):** Local LLM inference server and vector database

## VM 400 (ollama-gpu) Status
- **Disk:** 86GB (expanded from 56GB on 2026-05-21 via Proxmox resize + LVM extension)
- **Available Space:** 34GB free post-expansion
- **Block Device:** /dev/vda (KVM virtio, not /dev/sda)
- **Installed Ollama Models:**
  - qwen3:14b (primary model, confirmed honest about limitations)
  - qwen3.5:latest (secondary comparison model)
  - nomic-embed-text:latest (embeddings for Qdrant)
- **Removed Models:** 
  - gemma3:4b (hallucinated factual data)
  - gemma3:12b (hallucinated factual data)
  - phi4-mini (hallucinated factual data)
  - llama3.2 (similar hallucination issues during testing)
- **Qdrant Vector Database:**
  - Collection: obsidian_knowledge
  - Chunks: 1,736 (was 1,567 before adding 04-personal/ folder)
  - Indexed folders: 04-personal/, 05-books/, 06-projects/, 07-reference/, 08-agents/, 09-people/, 10-projects/, 11-learning/, 12-research/, 13-archive/
- **Purpose:** Local LLM inference for Gilgamesh and future agents (EMIYA, Midas, etc.); vector embeddings for RAG
- **Strategy:** Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)

## Obsidian Integration (Personal Knowledge System)
- **Vault Location:** second-brain/ in Nextcloud (CT 215)
- **04-personal/ Folder:** Now included in Qdrant indexing (privacy exclusion reversed — all internal VLAN 30)
- **muzakkir-profile.md:** Personal profile file managed by Da Vinci Personal Knowledge gateway
  - Path: 04-personal/muzakkir-profile.md
  - Current content: dark mode preference
  - Source: Gilgamesh sends facts via Da Vinci gateway
  - Updated: Da Vinci assesses and merges new facts with existing profile
  - Indexed in Qdrant obsidian_knowledge collection
  - Accessible to Gil via RAG for memory/recall
- **All Agent Writes:** Route through Da Vinci — Personal Knowledge gateway (not direct writes)
  - Rationale: consistent formatting, no file conflicts, Da Vinci as quality gate
  - Future agents (EMIYA, Midas, Guardian) will also write through this gateway
- **Agent Reads:** Agents query Qdrant/RAG directly (no gating required)

## Knowledge Indexer Status (Phase 24.9)
- **Indexing Frequency:** Daily at 3am (manual trigger available)
- **Indexed Folders:** 04-personal/, 05-books/, 06-projects/, 07-reference/, 08-agents/, 09-people/, 10-projects/, 11-learning/, 12-research/, 13-archive/
- **File Count:** 90 files (was ~70 before adding 04-personal/)
- **Qdrant Chunks:** 1,736 (was 1,567)
- **Empty File Filter:** Added Phase 24.9 — prevents "Document loader is not initialized" crash on empty