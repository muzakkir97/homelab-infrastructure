# Current State Documentation
**Last Updated:** 2026-05-21 (Phase 24.8)

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
**Role:** Documentation librarian, maintains consistency across 8-file knowledge base

**Update Pipeline Architecture:**
- 8 sequential Haiku API calls (one per file)
- Each call receives full session summary with dedicated per-file section
- Cost logging fires immediately after each API call (8 rows per session)
- Estimated cost per run: ~$0.14-0.16 (AI-CONTEXT bumped to 25,000 tokens)
- Haiku pricing: $0.80/1M input tokens, $4.00/1M output tokens
- Langfuse observability: Active as of Phase 24.8 — single trace (da-vinci-update) with 8 child generations per run

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

## Da Vinci Update Pipeline Status (Phase 24.8)
**Pipeline Architecture:**
- Total nodes: 36 (8 sequential Haiku API calls + 8 Log Cost nodes + 8 Parse nodes + Update Tables + Push to GitHub + Langfuse — Da Vinci + Push to Nextcloud + Send Confirmation)
- Runtime: ~5 minutes per session update run
- Cost per run: ~$0.14-0.16 (~$4.20-4.80/month at once daily)
- Files covered: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Per-file API calls: 8 separate Haiku calls with immediate cost logging
- Langfuse observability: Wired Phase 24.8 — trace name da-vinci-update with 8 child generations per run

**Deployment Status (Phase 24.8):**
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

## Automation Status
**Trigger:** Cron job (time TBD per Phase 16 work)

**Known Limitations & Resolutions (Phase 24.8):**
- ✅ decisions.md dynamically created on first run (null SHA path implemented)
- ✅ Fetch GitHub Files: hardcoded token (not passed via trigger)
- ✅ All 8 files now pushed to GitHub from filesToPush array
- ✅ Per-file API calls: 8 separate Haiku calls with immediate cost logging
- ✅ Langfuse wiring: Single node branched off Push to GitHub using internal URL (http://192.168.30.223:3000)
- ✅ Staging-inbox stuck file issue resolved: deleted stuck file, rebuilt pipeline
- ✅ Langfuse trace (da-vinci-update) confirmed appearing in Langfuse UI with 8 child generations (Phase 24.8 test run)
- ✅ Log Cost — service-catalog command_type fixed (was copy-paste error)
- ⚠️ GitHub docs/ folder contains stale changelog.md and troubleshoot.md artifacts from old pipeline — needs cleanup
- ⚠️ Langfuse v3 self-hosted UI trace list has known bug: traces exist in ClickHouse and are accessible via direct URL and public API, but do not appear in the Tracing list page UI. Pending fix.

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
- **Purpose:** Workflow automation, Da Vinci Update Pipeline
- **Databases:** PostgreSQL (internal)
- **Connections:** Anthropic API, GitHub API, Nextcloud, ClickHouse (CT 223)

### CT 223 (observability-langfuse)
- **Image:** langfuse/langfuse:3.174.1
- **IP:** 192.168.30.223
- **Purpose:** LLM observability and cost tracking
- **Status:** Wired to Da Vinci Update Pipeline — traces active
- **Dependencies:** PostgreSQL (internal), ClickHouse (internal)
- **Known Issue:** UI trace list shows no results despite traces being present in ClickHouse and accessible via /api/public/traces and direct URL
- **Configuration:** LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES=true (set 2026-05-21, did not resolve UI list bug)

### Other Infrastructure
- **RM 30 (cost database):** ClickHouse instance for cost metrics
- **CT 215 (Nextcloud):** File backup and collaboration
- **Ollama (VM 400):** Local LLM inference server

## VM 400 (ollama-gpu) Status
- **Disk:** 86GB (expanded from 56GB on 2026-05-21 via Proxmox resize + LVM extension)
- **Available Space:** 34GB free post-expansion
- **Installed Ollama Models:**
  - qwen3:14b (primary model, confirmed reliable)
  - qwen3.5:latest (comparison model)
- **Removed Models:** gemma3:4b, gemma3:12b, phi4-mini (all hallucinated factual data in testing; llama3.2 failed similar testing)
- **Purpose:** Local LLM inference for Gilgamesh and potential future EMIYA code agent
- **Strategy:** Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)

## Phase Status
**Current Phase:** 24.8 — Langfuse Wiring (Da Vinci) (Complete)
- ✅ Wired Langfuse observability into Da Vinci Update Pipeline
- ✅ Single Langfuse node branched off Push to GitHub (after all 8 files complete)
- ✅ Langfuse trace: da-vinci-update with 8 child generations (one per file)
- ✅ Uses internal URL: http://192.168.30.223:3000 (CT 211 and CT 223 on same VLAN 30)
- ✅ Test run 2026-05-21: trace and 8 generations confirmed appearing in Langfuse UI
- ✅ AI-CONTEXT max_tokens bumped to 25,000 (was hitting ceiling at 20,000)
- ✅ Log Cost — service-catalog command_type corrected
- ✅ Fixed 8 bugs in new pipeline nodes and configurations

**Next Phase:** Wire Langfuse into MERLIN

## Action Items
- [ ] Investigate Langfuse UI trace list bug — try removing all filters (environment + time range) to see if traces appear
- [ ] Consider upgrading Langfuse from v3.174.1 to latest version (may fix UI trace list bug)
- [ ] Verify Langfuse trace appears at langfuse.najhin-gaming.com after recent test
- [ ] Confirm 8 generations visible under da-vinci-update trace in Langfuse UI
- [ ] Wire Langfuse into MERLIN next
- [ ] Clean up GitHub: delete stale docs/changelog.md and docs/troubleshoot.md (old pipeline artifacts)
- [ ] Test Gemma 4 E4B on VM 400 — potential lighter alternative to qwen3:14b for simple Gilgamesh queries
- [ ] Future: Explore Aider + local Ollama for EMIYA code agent capability (Phase 3 item)
- [ ] Discuss $0 AI Architecture Stack diagram (pending)
- [ ] Replace Claude project instructions with updated version incorporating expanded session summary template

## Infrastructure Notes
- Local LLM Strategy: Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)
- Model Reliability: qwen3:14b selected as primary over gemma3 and phi4-mini due to honesty about limitations. All three rejected models confidently hallucinated factual weather data; qwen3:14b and llama3.2 admitted uncertainty.
- Future Code Agent: Aider + local Ollama identified as better fit than Claude Code for moving away from Claude dependency while maintaining code quality
- VM 400 Configuration: Headless VM suitable for Ollama persistent background server. Disk expanded to 86GB to support model testing. 34GB free space.
- Langfuse Integration: Observability pipeline now in place; monitoring Da Vinci Update Pipeline traces and generation metrics. Single Langfuse node branched off Push to GitHub (after all 8 files complete) creates 1 trace + 8 generations per run. Internal URL (http://192.168.30.223:3000) used for n8n → Langfuse calls (CT 211 and CT 223 on same VLAN 30). Known UI bug: trace list page shows "No results" despite traces being in ClickHouse and accessible via API.
- Key Lessons: Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; use single-quoted strings with concatenation
- Key Lessons: Fetch GitHub Files must hardcode token; do not reference trigger payload fields not explicitly defined in trigger schema
- Key Lessons: Log Cost node command_type strings are error-prone when duplicating nodes; always verify strings match intended file
- Key Lessons: Langfuse ingestion timestamps must use n8n server UTC time (new Date().toISOString()); do not add timezone offsets
- Key Lessons: Langfuse v3 self-hosted UI trace list has known bug where traces don't appear in list view despite being in ClickHouse; accessible via direct URL and public API