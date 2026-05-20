# Service Catalog

## Overview
Central registry of all homelab services, APIs, and infrastructure components. Last updated: 2026-05-20.

## Da Vinci Documentation Pipeline
**Status:** Active  
**Phase:** 16.4 — Documentation Pipeline Expansion (Complete)

### File Coverage
The Da Vinci Update Pipeline now handles 8 files per session update run:

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

**Previously:** 3 files (AI-CONTEXT.md, changelog.md, troubleshoot.md)

### Architecture
- **Pipeline Type:** Sequential Haiku API chain
- **API Calls per Run:** 8 (one per file)
- **Cost Logging:** Fires immediately after each API call; generates 8 cost rows per session update
- **Cost per Run:** ~$0.14 (previously ~$0.11 for 3-file pipeline)
- **Runtime:** ~5 minutes per run (previously ~4 minutes)

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
4. **sessionSummary reference:** Changed to read fileContent from trigger payload
5. **Claude API nodes:** Backtick template literals → replaced with single-quoted strings using concatenation
6. **Log Cost — service-catalog node:** Fixed command_type from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`

### Key Technical Notes
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; use single-quoted strings with concatenation instead
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema
- decisions.md on first creation uses null SHA path in GitHub push to create file fresh; subsequent runs use fetched SHA

### Dependencies
- Haiku 3.5 API (8 sequential calls)
- GitHub API (fetch files, push updates)
- Workflow trigger payload (session summary via fileContent field, chatId)
- Telegram notification service (confirmation)
- n8n workflow runtime (~5 minutes per session)

## Monitoring & Verification
- **Cost Tracking:** 8 rows logged per run in gilgamesh_costs table
- **File Updates:** All 8 files updated on GitHub after each session update
- **Telegram Alerts:** Confirmation message sent on completion
- **Expected Cost Range:** ~$0.14 per full pipeline run (~$4.20/month at once daily)
- **Files Pushed:** AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md

---

*Catalog maintained by Da Vinci Documentation System. Next review: Phase 17 planning.*