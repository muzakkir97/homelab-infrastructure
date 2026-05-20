# Service Catalog

## Overview
Central registry of all homelab services, APIs, and infrastructure components. Last updated: 2026-05-20.

## Da Vinci Documentation Pipeline
**Status:** Active  
**Phase:** 16.4 — Documentation Pipeline Expansion (Complete)

### File Coverage
The Da Vinci Update Pipeline now handles 8 files per session update run:
1. AI-CONTEXT.md (20000 tokens)
2. changelog.md (6000 tokens)
3. troubleshoot.md (4000 tokens)
4. ROADMAP.md (8000 tokens)
5. agents.md (8000 tokens)
6. current-state.md (4000 tokens)
7. service-catalog.md (4000 tokens)
8. decisions.md (3000 tokens)

**Previously:** 3 files (AI-CONTEXT.md, changelog.md, troubleshoot.md)

### Architecture
- **Pipeline Type:** Sequential Haiku API chain
- **API Calls per Run:** 8 (one per file)
- **Cost Logging:** Fires immediately after each API call; generates 8 cost rows per session update
- **Cost per Run:** ~$0.25–0.35 (previously ~$0.11 for 3-file pipeline)

### New Files Added (Phase 16.4)
- **ROADMAP.md** — Full rewrite via Haiku API (8000 tokens)
- **agents.md** — Full rewrite via Haiku API (8000 tokens)
- **current-state.md** — Append/update via Haiku API (4000 tokens)
- **service-catalog.md** — Append/update via Haiku API (4000 tokens)
- **decisions.md** — New file; appended via Haiku API (3000 tokens); promoted from Phase 3 to Phase 2 priority

### GitHub Push Integration
- All 8 files pushed to GitHub after pipeline completion
- decisions.md handled with null SHA on first creation
- Push to GitHub node updated to include all 8 files in filesToPush array

### Recent Bug Fixes (Phase 16.4)
1. **Fetch GitHub Files:** Null githubToken → hardcoded token directly in node
2. **5 new Claude API nodes:** Null apiKey from trigger → hardcoded apiKey const at top of each node
3. **Push to GitHub:** Only 3 files in array → updated to all 8 files with null SHA handling

### Dependencies
- Haiku 3.5 API (8 sequential calls)
- GitHub API (fetch files, push updates)
- Workflow trigger payload (session summary)
- Telegram notification service (confirmation)

## Monitoring & Verification
- **Cost Tracking:** 8 rows logged per run in gilgamesh_costs
- **File Updates:** All 8 files updated on GitHub after each session
- **Telegram Alerts:** Confirmation message sent on completion
- **Expected Cost Range:** $0.25–0.35 per full pipeline run

---

*Catalog maintained by Da Vinci Documentation System. Next review: Phase 17 planning.*