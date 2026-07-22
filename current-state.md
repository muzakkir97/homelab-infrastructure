# Current State Documentation
**Last Updated:** 2026-07-23 (Session 5 — Alertmanager iowait alert investigation, backup infrastructure audit, critical Kinmoon hardware discovery)

## Overview
Homelab infrastructure documentation and automation project. Core system uses Claude AI agents to maintain living documentation across 8 coordinated files through automated pipelines triggered by cron jobs. Agent ecosystem renamed from "Kuromoon" to "Chaldea" as of 2026-07-08.

## Core Documentation Files (Da Vinci Update Pipeline)
Pipeline expanded Phase 16.4 from 3 files to 8 files, each with dedicated token budget and handling logic. Phase 24.11 planning: 9th step adds Nextcloud Deck sync (deferred pending manual backfill of existing cards).

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

**Total Pipeline Tokens:** ~62,000 per session update run (before Deck sync 9th step integration)

## Da Vinci Agent Configuration
**Role:** Documentation librarian, maintains consistency across 8-file knowledge base. Also operates Personal Knowledge gateway for ingesting agent-generated facts into Obsidian. Planned expansion: sole writer to Nextcloud Deck (Homelab board) via sync-id matching mechanism.

**Update Pipeline Architecture:**
- 8 sequential Haiku API calls (one per file)
- Each call receives full session summary with dedicated per-file section
- Cost logging fires immediately after each API call (8 rows per session)
- Estimated cost per run: ~$0.14-0.16 (AI-CONTEXT bumped to 25,000 tokens)
- Haiku pricing: $0.80/1M input tokens, $4.00/1M output tokens
- Langfuse observability: Active as of Phase 24.8 — single trace (da-vinci-update) with 8 child generations per run

**Personal Knowledge Gateway (Phase 24.9, Enhanced Phase 24.10):**
- Separate n8n workflow: Da Vinci — Personal Knowledge
- Webhook: POST /davinci-personal-knowledge (internal CT 211 URL)
- Receives facts from all agents (currently wired from Jeanne Alter)
- Claude Haiku (max_tokens 4000) assesses and merges facts into muzakkir-profile.md
- Quality filter: stores durable personal facts, SKIPs one-time events and conversational noise
- Writes to Obsidian via WebDAV, costs logged to gilgamesh_costs table
- All agents route Obsidian writes through this gateway (not direct writes)
- Phase 24.10: Webhook trigger added for triggered re-indexing. POST /davinci-reindex-personal fires after successful muzakkir-profile.md write

**Nextcloud Deck Sync (Phase 24.11, Planning Stage):**
- Planned 9th step in Da Vinci Update Pipeline (after Push to GitHub, before Langfuse)
- Trigger: every pipeline run — items written across any of the 8 files get reflected as Deck cards
- Matching mechanism: hidden `sync-id` tag in card description footer (e.g. `---\n🔖 sync-id: phase-24.10`), human-visible but structured for parsing
- Update behavior: if matching sync-id exists, update that card silently (title/description/stack); if no match, create new card with new sync-id
- Stack routing: status-keyword based (e.g. "Complete" → Done, "In Progress" → In Progress), not fixed default stack
- Scope: Homelab board (ID 4) only, for now — Career/Personal boards not yet in scope
- **Precondition, not yet implemented:** all ~30 existing cards on Homelab board must be manually backfilled with sync-id tags before this automation goes live (chosen deliberately over automated adopt pass, due to known conflicts on board)
- API base: `http://192.168.30.220/index.php/apps/deck/api/v1.0`, using existing `NextCloud-Deck` credential

**File Update Patterns:**
- AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md: Full rewrite/major sections
- current-state.md, service-catalog.md: Append/update with preservation of existing entries
- decisions.md: Append-only (Phase 2 core pipeline, promoted from Phase 3 backlog due to decisions being lost between sessions)

**Known Behaviors:**
- decisions.md created as new file on first pipeline run (null SHA handled in Push to GitHub node)
- Cost logging pattern: 8 cost rows logged per session update run, fires immediately after each API call
- Hardcoded API keys in all 8 Claude API nodes (consistent with existing approach; ecosystem-wide credential store migration planned)
- Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; all system prompts use single-quoted strings with concatenation
- Fetch GitHub Files must hardcode token directly; do not reference trigger payload fields that are not explicitly defined in trigger schema (trigger only defines fileContent and chatId)
- Langfuse wiring: Single node branched off Push to GitHub sends all 8 generations in one batch with internal URL (http://192.168.30.223:3000)
- SKIP detection in Da Vinci gateway uses startsWith('SKIP') not strict equality (Da Vinci may return "SKIP\n\nReasoning...")
- Date placeholder ({{date}}) replaced in Code node before Claude API call with actual MYT date (YYYY-MM-DD format)

## Da Vinci Update Pipeline Status (Phase 24.8-24.11)
**Pipeline Architecture:**
- Total nodes: 36+ (8 sequential Haiku API calls + 8 Log Cost nodes + 8 Parse nodes + Update Tables + Push to GitHub + Langfuse — Da Vinci + Push to Nextcloud + Send Confirmation)
- Runtime: ~5 minutes per session update run
- Cost per run: ~$0.14-0.16 (~$4.20-4.80/month at once daily)
- Files covered: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Per-file API calls: 8 separate Haiku calls with immediate cost logging
- Langfuse observability: Wired Phase 24.8 — trace name da-vinci-update with 8 child generations per run
- Deck sync (9th step): Planned Phase 24.11, deferred pending manual backfill

**Deployment Status (Phase 24.8-24.11):**
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
- ✅ Langfuse wired to Jeanne Alter agent (Phase 24.9)
- ✅ Documentation audit completed (Phase 24.9) — 11 discrepancies corrected across 4 files
- ✅ Phase 24.10 deployed (May 25, 2026): Triggered Qdrant re-indexing via webhook POST /davinci-reindex-personal
- ✅ Jeanne Alter web search feature deployed (May 25, 2026): Firecrawl API wired, keyword detection, search-to-Haiku routing, search results injected
- ✅ Ecosystem renamed: Kuromoon (hardware only) ↔ Chaldea (agents ecosystem) clarified (Phase 24.11)
- ✅ Gilgamesh renamed to Jeanne Alter ("The Corrupted Ruler") — pending full propagation across bot identity, system prompt, Telegram username, all 8 docs
- ✅ Documentation audit completed (July 9, 2026): Cross-session gap analysis identified 7 undocumented items and confirmed 3 structural gaps in agents.md; ready for Phase 27 planning
- ✅ agents.md structural completeness restored (July 9, 2026): full sections now exist for Da Vinci, MERLIN, Midas, EMIYA, Cu Chulainn, and Scathach. Agent Design Principles section added at top of agents.md establishing Funnel Agent pattern classification.
- ✅ Funnel Agent design principle established (July 9, 2026): defines when an agent should sit on top of existing infrastructure and translate it (MERLIN from Uptime Kuma, Midas from Langfuse, planned Cu Chulainn from Alertmanager) vs. when it should be generative/interface-only (Jeanne Alter, Da Vinci, Scathach) or hybrid (EMIYA). Why: prevents duplicate effort, reduces drift risk, keeps build focus on aggregation + recommendation rather than reinventing existing checks.
- ✅ MERLIN and Midas design corrections identified (July 9, 2026): both currently reinvent infrastructure monitoring that already exists — MERLIN's SSL check should source from Uptime Kuma (CT 206) instead of broken Cloudflare API token; Midas's cost tracking should source from Langfuse Metrics API instead of duplicate Data Table. Implementation pending.
- ✅ Jeanne Alter Email Management Pipeline designed (July 9, 2026): design complete, not yet implemented. Architecture: 4× per-account trigger workflows (3× Gmail OAuth2, 1× IMAP for iCloud) → shared Email Classifier sub-workflow → Telegram notify + staging store → permanent facts to Da Vinci Personal Knowledge gateway. Scope: 4 personal accounts (muzakkir.kholil06@gmail.com, muzakkirkholil97@icloud.com, hyperjhin00@gmail.com, business.najhin@gmail.com); work and NSFW accounts excluded. Credentials will use n8n credential store (first concrete step of ecosystem-wide credential migration). Schedule: 3x/day. Status: Design Complete, 0/6 rollout steps implemented.
- ✅ node_exporter configuration audit completed (July 14, 2026): Confirmed `prometheus-node-exporter.service` (Debian package) was excluding all `/mnt` mountpoints via default `--collector.filesystem.mount-points-exclude` regex since at least 2026-05-16. Fixed by editing `/etc/default/prometheus-node-exporter` to remove `mnt` from exclude list. Restarted service 2026-07-14 17:45 +08 (PID 1307682 post-fix). Confirmed `hdd-backup-1`, `hdd-backup-2`, and `pve/kinmoon-smb` now report metrics correctly. Alert `MountpointMissing_hddbackup1` auto-resolved after fix.
- ✅ Palworld server connectivity debugging and fixes deployed (July 17, 2026): PublicIP field corrected from internal LAN IP (192.168.30.219) to current WAN IP (202.184.109.124); Pelican egg "Public IP" variable permissions enabled (User Editable + User Viewable) to prevent future silent reverts; PalWorldSettings.ini corruption from web editor line breaks resolved via pct exec sed commands from Proxmox host; SaveGames backup created via pct exec/pct pull; consolidated maintenance schedule planning initiated to address nightly vzdump backup contention with active gameplay.
- ✅ Alertmanager "CPU usage" alert root-caused (July 23, 2026): Prometheus's CPU-usage query sums all non-idle time including iowait (`%wa`); CT 205 "100% CPU" alert on 2026-07-20 03:46-03:47 AM was pure I/O wait (0% user + system, 100% wa) from backup-daily vzdump job writing to kinmoon-smb CIFS share at 94% capacity. No compute load issue; lingering I/O pressure past job completion (~03:35-03:46). Documented in AI-CONTEXT: CPU alerts require `top` verification before assuming compute-bound. Alert rule not yet updated; diagnosis-only this session.
- ✅ backup-daily vzdump job inventory completed (July 23, 2026): schedule 02:00 daily, compress zstd, mode snapshot, prune-backups keep-daily=7/keep-weekly=4, storage kinmoon-smb. Runtime ~93-96 minutes (02:00 start, ~03:33-03:36 finish). VMID list: 201,202,203,204,205,206,207,208,211,213,214,220,221,222,223,302,303,304,305,400. **CT 306 (Enshrouded) and CT 307 (Palworld) NOT included** — zero backup coverage for both game servers. Documented that kinmoon-smb is Proxmox's native vzdump CIFS target, NOT a separate rsync step (previous documentation was inaccurate).
- ✅ kinmoon-smb disk usage investigation completed (July 23, 2026): root cause is UGOS NAS-level recycle bin (`#recycle` folder, 1.3TB) double-counting space already logically freed by Proxmox's `prune-backups` retention policy. Actual backup archive (`dump` folder) is only 1.3TB; nextcloud-backup 31GB. Retention policy (keep-daily=7, keep-weekly=4) is working correctly; space bloat is from NAS-side recycle bin mechanics. Decision made to defer recycle bin cleanup in favor of Hard Drive 1 failure investigation.
- ✅ Kinmoon Hard Drive 1 SMART failure diagnosed (July 23, 2026): Storage Pool 1 (RAID 1) degraded, Storage Pool 1 status shows explicit failure notice; Hard Drive 1 SMART status Critical; Reallocated Sector Count (Index 5) present value 133, below reference threshold 140; sector count trend March 573 → April 609 → May 1,963 (accelerating media failure signature); spin retry 0 (not a motor issue); temperature 48°C (normal); power-on 47,901 hours. Diagnosis confirmed as genuine progressive media degradation (different failure mode than hdd-backup-1 2026-07-14 SATA cable issue). RAID 1 mirror currently running on single healthy drive (Hard Drive 2) with zero redundancy. Decision made: defer storage pool "