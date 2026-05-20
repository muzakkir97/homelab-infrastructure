# Current State Documentation
**Last Updated:** 2026-05-21 (Phase 24.8)

## Overview
Homelab infrastructure documentation and automation project. Core system uses Claude AI agents to maintain living documentation across 8 coordinated files through automated pipelines triggered by cron jobs.

## Core Documentation Files (Da Vinci Update Pipeline)
Pipeline expanded Phase 16.4 from 3 files to 8 files, each with dedicated token budget and handling logic.

| File | Purpose | Token Budget | Status |
|------|---------|--------------|--------|
| AI-CONTEXT.md | System context, infrastructure overview, agent definitions | 20,000 | Active |
| changelog.md | Session history, changes, features added | 6,000 | Active |
| troubleshoot.md | Known issues, solutions, debug procedures | 4,000 | Active |
| ROADMAP.md | Phase tracking, priorities, planned work | 8,000 | Active |
| agents.md | Agent definitions, capabilities, pipeline architecture | 8,000 | Active |
| current-state.md | This file; infrastructure state snapshot | 4,000 | Active |
| service-catalog.md | Service/container inventory, networking | 4,000 | Active |
| decisions.md | Decision log with alternatives considered | 3,000 | Active (Phase 2, promoted from Phase 3) |

**Total Pipeline Tokens:** ~57,000 per session update run

## Da Vinci Agent Configuration
**Role:** Documentation librarian, maintains consistency across 8-file knowledge base

**Update Pipeline Architecture:**
- 8 sequential Haiku API calls (one per file)
- Each call receives full session summary with dedicated per-file section
- Cost logging fires immediately after each API call (8 rows per session)
- Estimated cost per run: ~$0.25-0.35 (increased from ~$0.11 with 3-file pipeline)
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
- Cost per run: ~$0.14 (~$4.20/month at once daily)
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
- ✅ Langfuse trace created (da-vinci-update) with 8 generations visible (Phase 24.8 test run)

## Automation Status
**Trigger:** Cron job (time TBD per Phase 16 work)

**Known Limitations & Resolutions (Phase 24.8):**
- ✅ decisions.md dynamically created on first run (null SHA path implemented)
- ✅ Fetch GitHub Files: hardcoded token (not passed via trigger)
- ✅ All 8 files now pushed to GitHub from filesToPush array
- ✅ Per-file API calls: 8 separate Haiku calls with immediate cost logging
- ✅ Langfuse wiring: Single node branched off Push to GitHub using internal URL (http://192.168.30.223:3000)
- ✅ Staging-inbox stuck file issue resolved: deleted stuck file, rebuilt pipeline
- ⚠️ Log Cost node for service-catalog has incorrect command_type (copy-paste error) — needs fix
- ⚠️ GitHub docs/ folder contains stale changelog.md and troubleshoot.md artifacts from old pipeline — needs cleanup

**Tested & Working:**
- 8-file sequential pipeline architecture
- Per-file token budgets and update patterns
- Cost logging for all 8 API calls, fires immediately after each call
- decisions.md null SHA handling in Push to GitHub
- Single-quoted string concatenation in system prompts (template literals rejected)
- Hardcoded credentials in all 8 Claude API nodes
- 8-file separate API call pattern with immediate cost logging
- Langfuse observability with single trace and 8 child generations per run

## Phase Status
**Current Phase:** 24.8 — Langfuse Wiring (Da Vinci) (Complete)
- Wired Langfuse observability into Da Vinci Update Pipeline
- Single Langfuse node branched off Push to GitHub (after all 8 files complete)
- Langfuse trace: da-vinci-update with 8 child generations (one per file)
- Uses internal URL: http://192.168.30.223:3000 (CT 211 and CT 223 on same VLAN 30)
- Test run 2026-05-21: trace and 8 generations confirmed appearing in Langfuse UI

**Next Phase:** Wire Langfuse into MERLIN

## Action Items
- [ ] Verify Langfuse trace appears at langfuse.najhin-gaming.com after this test run
- [ ] Confirm 8 generations visible under da-vinci-update trace
- [ ] Wire Langfuse into MERLIN next
- [ ] Fix Log Cost node for service-catalog: change command_type from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`
- [ ] Clean up GitHub: delete stale docs/changelog.md and docs/troubleshoot.md (old pipeline artifacts)
- [ ] Test Gemma 4 E4B on VM 400 — potential lighter alternative to qwen3:14b for simple Gilgamesh queries
- [ ] Future: Explore Aider + local Ollama for EMIYA code agent capability (Phase 3 item)
- [ ] Discuss $0 AI Architecture Stack diagram (pending)

## Infrastructure Notes
- Local LLM Strategy: Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)
- Future Code Agent: Aider + local Ollama identified as better fit than Claude Code for moving away from Claude dependency while maintaining code quality
- VM 400 Configuration: Headless VM suitable for Ollama persistent background server, explored Gemma 4 E4B and qwen3:14b options
- Langfuse Integration: Observability pipeline now in place; monitoring Da Vinci Update Pipeline traces and generation metrics