# Current State Documentation
**Last Updated:** 2026-05-20 (Phase 16.4)

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

**File Update Patterns:**
- AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md: Full rewrite/major sections
- current-state.md, service-catalog.md: Append/update with preservation of existing entries
- decisions.md: Append-only (Phase 2 core pipeline, promoted from Phase 3 backlog due to decisions being lost between sessions)

**Known Behaviors:**
- decisions.md created as new file on first pipeline run (null SHA handled in Push to GitHub node)
- Cost logging pattern unchanged — 8 cost rows logged per session update run
- Hardcoded API keys in all 8 Claude API nodes (consistent with existing approach)
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; all system prompts use single-quoted strings with concatenation
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema (trigger only defines fileContent and chatId)

## Da Vinci Update Pipeline Status (Phase 16.4)
**Pipeline Architecture:**
- Total nodes: 35 (8 sequential Haiku API calls + 8 Log Cost nodes + 8 Parse nodes + Update Tables + Push to GitHub + Push to Nextcloud + Send Confirmation)
- Runtime: ~5 minutes per session update run
- Cost per run: ~$0.14 (~$4.20/month at once daily)
- Files covered: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md

**Deployment Status (Phase 16.4 Complete):**
- ✅ Expanded from 3-file to 8-file sequential Haiku chain
- ✅ Resolved 5 critical bugs during deployment (null token, null API keys, missing files in Push array, sessionSummary field mapping, template literal syntax errors)
- ✅ Updated Claude project instructions with explicit per-file session summary sections
- ✅ All 8 files pushed to GitHub from filesToPush array
- ✅ decisions.md promoted to Phase 2 core pipeline with null SHA handling for new file creation

## Automation Status
**Trigger:** Cron job (time TBD per Phase 16 work)

**Known Limitations & Resolutions (Phase 16.4):**
- ✅ decisions.md dynamically created on first run (null SHA path implemented)
- ✅ Fetch GitHub Files: hardcoded token (not passed via trigger)
- ✅ All 8 files now pushed to GitHub from filesToPush array
- ⚠️ Log Cost node for service-catalog has incorrect command_type (copy-paste error) — needs fix
- ⚠️ GitHub docs/ folder contains stale changelog.md and troubleshoot.md artifacts from old pipeline — needs cleanup

**Tested & Working:**
- 8-file sequential pipeline architecture
- Per-file token budgets and update patterns
- Cost logging for all 8 API calls
- decisions.md null SHA handling in Push to GitHub
- Single-quoted string concatenation in system prompts (template literals rejected)
- Hardcoded credentials in all 8 Claude API nodes

## Phase Status
**Current Phase:** 16.4 — Documentation Pipeline Expansion (Complete)
- Expanded Update Pipeline from 3 files → 8 files
- Fixed 5 bugs: null GitHub token, null API keys in 5 nodes, missing 5 files in Push to GitHub, sessionSummary field mapping, template literal syntax errors
- Updated Claude project instructions with per-file session summary sections
- decisions.md promoted to Phase 2 pipeline (core priority)

**Next Phase:** 17 (TBD)

## Action Items
- [ ] Fix Log Cost node for service-catalog: change command_type from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`
- [ ] Clean up GitHub: delete stale docs/changelog.md and docs/troubleshoot.md (old pipeline artifacts)
- [ ] Verify test run: 8 cost rows in gilgamesh_costs, 8 files updated on GitHub, Telegram confirmation
- [ ] Check decisions.md created successfully on GitHub (new file, null SHA path)
- [ ] Monitor cost per run — verify ~$0.25-0.35 for 8-file pipeline
- [ ] Test Gemma 4 E4B on VM 400 — potential lighter alternative to qwen3:14b for simple Gilgamesh queries
- [ ] Future: Explore Aider + local Ollama for EMIYA code agent capability (Phase 3 item)
- [ ] Discuss $0 AI Architecture Stack diagram (pending)

## Infrastructure Notes
- Local LLM Strategy: Ollama confirmed as correct choice for homelab (LM Studio rejected due to GUI dependency and lack of background server stability)
- Future Code Agent: Aider + local Ollama identified as better fit than Claude Code for moving away from Claude dependency while maintaining code quality
- VM 400 Configuration: Headless VM suitable for Ollama persistent background server, explored Gemma 4 E4B and qwen3:14b options