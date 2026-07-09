# Service Catalog

## Overview
Central registry of all homelab services, APIs, and infrastructure components. Last updated: 2026-07-09.

## Da Vinci Documentation Pipeline
**Status:** Active  
**Phase:** 24.10 — Triggered Qdrant Re-indexing (Complete)

### File Coverage
The Da Vinci Update Pipeline now handles 8 files per session update run, with a planned 9th step (Nextcloud Deck sync):

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
| **nextcloud-deck-sync** | **Planned (Phase TBD)** | **~2000** | **Create/update Deck cards from file changes** |

**Previously:** 3 files (AI-CONTEXT.md, changelog.md, troubleshoot.md)

### Architecture
- **Pipeline Type:** Sequential Haiku API chain (8 current calls, 9th planned for Deck sync)
- **API Calls per Run:** 8 (one per file; 9th planned)
- **Observability:** Langfuse wiring active — single trace (da-vinci-update) with 8 child generations logged per run
- **Langfuse Node Location:** Branched off Push to GitHub (after all 8 files complete)
- **Langfuse Internal URL:** http://192.168.30.223:3000 (CT 223 on VLAN 30, same as n8n CT 211)
- **Langfuse Public URL:** https://langfuse.najhin-gaming.com
- **Cost Logging:** Fires immediately after each API call (before parse/push); generates 8 cost rows per session update
- **Cost per Run:** ~$0.14–0.16 (previously ~$0.11 for 3-file pipeline; increased due to AI-CONTEXT max_tokens bump to 25000)
- **Runtime:** ~5 minutes per run (previously ~4 minutes; 9th step will add minimal overhead with Deck API calls)
- **Haiku Pricing:** $0.80/1M input tokens, $4.00/1M output tokens

### New Files Added (Phase 16.4)
- **ROADMAP.md** — Full rewrite via Haiku API (8000 tokens)
- **agents.md** — Full rewrite via Haiku API (8000 tokens)
- **current-state.md** — Append/update via Haiku API (4000 tokens)
- **service-catalog.md** — Append/update via Haiku API (4000 tokens)
- **decisions.md** — New file; appended via Haiku API (3000 tokens); promoted from Phase 3 to Phase 2 priority

### Nextcloud Deck Sync (Planned Phase TBD)
- **Status:** Designed, not yet implemented
- **Trigger:** Every Da Vinci pipeline run (items across any 8 files get reflected as Deck cards)
- **Target Board:** Homelab board (ID 4) only — Career and Personal boards not yet in scope
- **Matching Mechanism:** Hidden `sync-id` tag embedded in card description footer, separated by `---` divider; human-visible but structured for parsing
- **Update Behavior:** If matching sync-id exists, update card silently (title/description/stack); if no match, create new card with new sync-id
- **Stack Routing:** Status-keyword based (e.g., "Complete" → Done, "In Progress" → In Progress), not fixed default stack
- **Precondition:** All ~30 existing Homelab board cards must be manually backfilled with sync-id tags by user before automation goes live (chosen over automated adopt pass due to known conflicts — duplicate Phase 7E cards, stale RAM-upgrade cards, Da Vinci Stage 2 status conflict)
- **Example sync-id format:** `🔖 sync-id: phase-24.10`
- **Nextcloud Deck API Base:** http://192.168.30.220/index.php/apps/deck/api/v1.0
- **Credential:** NextCloud-Deck (existing)
- **Dependencies:** Nextcloud API, manual backfill completion

### GitHub Push Integration
- All 8 files pushed to GitHub after pipeline completion
- decisions.md handled with null SHA on first creation
- Push to GitHub node updated to include all 8 files in filesToPush array

### Langfuse Observability Integration (Phase 24.8–24.9)
- **Status:** Active — Wired and tested (2026-05-21); UI trace list now working (2026-05-22)
- **Trace Name:** da-vinci-update
- **Trace Structure:** 1 parent trace with 8 child generations (one per file: AI-CONTEXT, changelog, troubleshoot, ROADMAP, agents, current-state, service-catalog, decisions)
- **Node Architecture:** Single Langfuse node branched off Push to GitHub, executing after all 8 files complete and are pushed
- **Node Name:** Langfuse — Da Vinci (in agents.md workflow diagram)
- **Internal URL:** http://192.168.30.223:3000 (n8n CT 211 → Langfuse CT 223 on VLAN 30)
- **Public URL:** https://langfuse.najhin-gaming.com (for verification and UI access)
- **Batch Delivery:** All 8 generations sent in single request to Langfuse
- **Design Rationale:** Cleaner pipeline, fewer nodes, all generations grouped in single trace for better observability
- **Alternatives Rejected:** 8 individual Langfuse nodes after each Claude API call (too many nodes, marginal benefit)
- **UI Trace List Bug:** Resolved (2026-05-22) — 1-hour analytics delay by design for aggregation stability. Traces now visible in Langfuse UI trace list with all 8 child generations. Direct URL and public API access worked immediately after fix.

### Recent Bug Fixes (Phase 24.8–24.9)
1. **Fetch GitHub Files:** Null githubToken → hardcoded token directly in node
2. **5 new Claude API nodes:** Null apiKey from trigger → hardcoded apiKey const at top of each node
3. **Push to GitHub:** Only 3 files in array → updated to all 8 files with null SHA handling
4. **sessionSummary reference:** Changed to read fileContent from trigger payload
5. **Claude API nodes:** Backtick template literals → replaced with single-quoted strings using concatenation
6. **Log Cost — service-catalog node:** Fixed command_type from `/update (Da Vinci - current-state)` to `/update (Da Vinci - service-catalog)`
7. **AI-CONTEXT max_tokens:** Bumped from 20000 to 25000 (was hitting ceiling on every run)
8. **Langfuse ingestion timestamp:** Now uses n8n server UTC time (new Date().toISOString()); do not add timezone offsets
9. **Knowledge Indexer empty file filter:** Added If node to skip files where $json.data is empty (prevents crash on new empty folders)
10. **Knowledge Indexer folder list:** Updated to 00-inbox, 01-homelab, 02-career, 03-knowledge, 04-personal, 07-daily, 08-agents, 09-people, 10-projects, AI-Stuff/Homelab/homelab-infrastructure (confirmed 2026-05-25; requires periodic verification against n8n workflow node configuration)
11. **Da Vinci Personal Knowledge SKIP detection:** Changed from === 'SKIP' to startsWith('SKIP') — Da Vinci returns "SKIP\n\nReasoning" which was being overwritten
12. **Da Vinci Personal Knowledge date placeholder:** Changed from Claude handling {{date}} to Code node replacing it before API call (more reliable)
13. **VM 400 disk expansion:** Used /dev/vda not /dev/sda (KVM virtio device naming)
14. **Gilgamesh/Jeanne Alter system prompt:** Explicitly states identity and authority (pending full rename to Jeanne Alter)
15. **hdd-backup-2 Prometheus alert (2026-07-08):** Copy-paste bug in `alert_rules.yml` (CT 202, line 163) — `MountpointMissing_hddbackup2` rule checked `/mnt/hdd-backup-1` instead of `/mnt/hdd-backup-2`. Fixed via sed, then fully removed per user decision. hdd-backup-2 currently has zero Prometheus alert coverage (intentional tradeoff).

### Pipeline Rebuild (2026-05-19)
- **Issue:** Pipeline looping on error due to stuck file in staging-inbox that failed validation repeatedly (every 15 minutes)
- **Resolution:** Deleted stuck file; rebuilt pipeline with per-file API calls and immediate cost logging
- **Testing:** Verified cost logging fires immediately after each Claude API call, before parse/push operations
- **Verification:** Monitor gilgamesh_costs for 8 new rows per pipeline run; observe API usage for 24 hours to confirm cost is under target

### Langfuse Wiring Test (2026-05-21)
- **Status:** Completed successfully
- **Test Date:** 2026-05-21
- **Result:** Single trace (da-vinci-update) with 8 child generations confirmed visible in Langfuse UI via direct URL and public API; confirmed in ClickHouse analytics data
- **Verification Steps:**
  - [x] Verify Langfuse trace appears at https://langfuse.najhin-gaming.com after test run
  - [x] Confirm 8 generations visible under da-vinci-update trace (via API and direct URL)
  - [x] Investigate and fix UI trace list display bug (resolved 2026-05-22 — 1-hour analytics delay)
  - [ ] Wire Langfuse into MERLIN and Midas next

### Key Technical Notes
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; use single-quoted strings with concatenation instead
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema
- decisions.md on first creation uses null SHA path in GitHub push to create file fresh; subsequent runs use fetched SHA
- Cost logging must fire immediately after each API call, not after parse or push operations
- Langfuse integration uses internal VLAN URL (http://192.168.30.223:3000) for n8n calls to avoid unnecessary external routing; trace batching consolidates all 8 file generations into single observability record
- Langfuse public URL (https://langfuse.najhin-gaming.com) used for UI verification and external access
- Langfuse v3 self-hosted UI trace list bug (2026-05-21) resolved overnight due to 1-hour analytics aggregation delay; traces now visible in list with all generations
- Langfuse ingestion timestamps must use n8n server UTC time (new Date().toISOString()); do not add timezone offsets
- Log Cost command_type strings must match file names exactly; copy-paste errors are silent bugs
- SKIP detection in Da Vinci nodes must use startsWith() not strict equality (===) — Da Vinci may return multiline responses like "SKIP\n\nReasoning..."
- Date placeholders ({{date}}) must be replaced in Code nodes before Claude API call, not by Claude itself — more reliable and deterministic
- VM 400 block device is /dev/vda not /dev/sda (KVM virtio); always use vda for disk operations on VM 400

### Dependencies
- Haiku 3.5 API (8 sequential calls, one per file; 9th planned for Deck sync)
- GitHub API (fetch files, push updates)
- Langfuse API (http://192.168.30.223:3000 internal; https://langfuse.najhin-gaming.com public) for trace logging
- Workflow trigger payload (session summary via fileContent field, chatId)
- Telegram notification service (confirmation)
- Nextcloud Deck API (planned 9th step)
- n8n workflow runtime (~5 minutes per session)

## Monitoring & Verification
- **Cost Tracking:** 8 rows logged per run in gilgamesh_costs table (~$0.14–0.16 per run)
- **Observability Tracking:** 1 trace (da-vinci-update) with 8 child generations per run in Langfuse; accessible at https://langfuse.najhin-gaming.com via direct URL, list view, and public API
- **File Updates:** All 8 files updated on GitHub after each session update
- **Telegram Alerts:** Confirmation message sent on completion
- **Langfuse UI:** Accessible at https://langfuse.najhin-gaming.com; pipeline traces visible in trace list (UI bug resolved 2026-05-22). All 8 child generations visible in trace detail view. Internal n8n calls use http://192.168.30.223:3000.
- **Expected Cost Range:** ~$0.14–0.16 per full pipeline run (~$4.20–4.80/month at once daily)
- **Files Pushed:** AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- **Deck Sync Status:** Designed, awaiting manual backfill of sync-id tags on existing ~30 Homelab cards before activation

---

## Da Vinci — Personal Knowledge Gateway
**Status:** Active (deployed 2026-05-22)  
**Type:** Internal n8n Webhook Service  
**URL:** http://192.168.30.211:5678/webhook/davinci-personal-knowledge  
**Method:** POST  
**Auth Method:** None (internal VLAN 30 only)  
**Purpose:** Receive personal facts from agents (Jeanne Alter, EMIYA, Midas), assess durability via Claude Haiku, merge with existing profile, write durable facts to muzakkir-profile.md in Obsidian, skip one-time events and conversational noise  
**Workflow Name:** Da Vinci — Personal Knowledge  
**Container:** CT 211 (automation-n8n)  
**Profile Location:** Obsidian/second-brain/04-personal/muzakkir-profile.md  
**Profile Access:** WebDAV GET with 404 fallback to bootstrap template  
**Profile Write:** WebDAV PUT (Nextcloud)  
**Triggered Re-index Webhook:** http://192.168.30.211:5678/webhook/davinci-reindex-personal (POST) — triggers partial Knowledge Indexer run for 04-personal/ folder only (~1s completion time)  
**Assessment Model:** Claude Haiku 3.5  
**Assessment Tokens:** max_tokens 4000  
**Quality Filter:** Stores durable personal facts (e.g., "prefers dark mode"), SKIPs one-time events (e.g., "I slept at 12am tonight") and conversational noise  
**Cost Logging:** Fires after Claude assessment; entries logged to gilgamesh_costs table  
**Dependencies:** Claude Haiku API, WebDAV (Nextcloud), agents sending via POST request  
**Consumers:** Jeanne Alter (writes via async fire-and-forget after all messages >20 chars not starting with /), future agents (EMIYA, Midas all route Obsidian writes through Da Vinci)  
**Profile Indexed:** Yes — 04-personal/ included in Qdrant obsidian_knowledge collection for RAG by Jeanne Alter

---

## Da Vinci — Knowledge Indexer (Qdrant Updates)
**Status:** Active (updated 2026-05-