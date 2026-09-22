# Service Catalog

## Overview
Central registry of all homelab services, APIs, and infrastructure components. Last updated: 2026-09-22.

## Cloudflare-Protected Services (najhin-gaming.com)

### Access Applications: 17 Total
The following applications are protected by Cloudflare Zero Trust Access on najhin-gaming.com (updated Phase 27.1):

| Application | Domain | Auth Policy | Purpose |
|-------------|--------|-------------|---------|
| panel | panel.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Proxmox management dashboard |
| langfuse | langfuse.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Observability and tracing pipeline |
| finance | finance.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Financial tracking (details TBD) |
| NextCloud | cloud.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | File storage and sync |
| Nextcloud Web | cloud.najhin-gaming.com/web | Bypass (path-scoped) | WebUI access without OTP (Phase 27.1) |
| Nextcloud WebDAV | cloud.najhin-gaming.com/remote.php | Bypass (path-scoped) | Obsidian sync and client access (Phase 27.1) |
| Nextcloud OCS | cloud.najhin-gaming.com/ocs | Bypass (path-scoped) | API access for clients (Phase 27.1) |
| Nextcloud Status | cloud.najhin-gaming.com/status.php | Bypass (path-scoped) | Server status endpoint (Phase 27.1) |
| Nextcloud Public Share | cloud.najhin-gaming.com/public.php | Bypass (path-scoped) | Public share links (Phase 27.1) |
| n8n | n8n.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Workflow automation |
| n8n Webhooks | n8n.najhin-gaming.com/webhook | Bypass (Email OTP validated server-side; requires manual verification per Phase 27.1 action items) | External trigger endpoints |
| vault | vault.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | HashiCorp Vault secret management |
| Vaultwarden | passwords.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Password manager (Bitwarden-compatible) |
| Vaultwarden API | passwords.najhin-gaming.com/api | Bypass (scoped, for native clients) | Native client API access |
| Vaultwarden Identity | passwords.najhin-gaming.com/identity | Bypass (scoped, for native clients) | OAuth/SSO for native clients |
| ollama webui | ollama.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | LLM model management UI |
| Grafana | grafana.najhin-gaming.com | Email OTP (hyperjhin00@gmail.com) | Metrics visualization and alerting |
| Pulse | home.najhin-gaming.com | Email OTP (muzakkir.kholil06@gmail.com) | Home automation dashboard (new Phase 27.1) |

**Note:** Vaultwarden parent app Bypass/Everyone policy was removed in Phase 27.1; only Email OTP now enforces on login. `/api` and `/identity` sub-apps retain their correctly-scoped bypass policies for native client access.

**Note:** Nextcloud parent app Bypass/Everyone policy was removed in Phase 27.1; only 4 path-scoped bypass apps remain for WebDAV, OCS, status, and public shares. Everything else (settings, admin, file browser UI) now requires Email OTP.

---

## DNS & TLS Configuration (najhin-gaming.com)

**Zone:** najhin-gaming.com  
**Status:** Active  
**Activated:** 2026-07-01  
**Current WAN IP:** 202.184.116.231 (updated Phase 27.1)  
**Nameservers:** Cloudflare  
**SSL/TLS Mode:** Full  
**Minimum TLS Version:** 1.2 (updated Phase 27.1; was 1.0)  
**Always Use HTTPS:** On (updated Phase 27.1; was off)  

### A Records (All Proxied via Cloudflare)
| Subdomain | IP Address | Last Updated | Status |
|-----------|-----------|--------------|--------|
| @ (root) | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| * (wildcard) | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| grafana | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| home | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| n8n | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| panel | 202.184.116.231 | 2026-09-22 | Current WAN IP |
| mc | 202.184.35.79 | Pre-2026-09-22 | **STALE — unproxied game server, out of scope Phase 27.1** |
| terraria | 202.184.35.79 | Pre-2026-09-22 | **STALE — unproxied game server, out of scope Phase 27.1** |

**DNS Hygiene Issue (Fixed Phase 27.1):** 6 A-records (root, wildcard, grafana, home, n8n, panel) were stuck on dead IP 202.184.35.79 (pre-July 5 bridge-mode migration); corrected to current WAN 202.184.116.231. Root cause: CT 207 (network-ddns) had invalid DDNS API token since 2026-07-04 21:10 UTC.

---

## DDNS Service (CT 207 / network-ddns)

**Status:** Active  
**Service:** favonia/cloudflare-ddns Docker container  
**Location:** /opt/cloudflare-ddns/docker-compose.yml  
**Domains Tracked:** `najhin-gaming.com,*.najhin-gaming.com`  
**API Token Scope:** Zone:DNS:Edit (najhin-gaming.com only)  
**API Token Status:** Valid (renewed Phase 27.1 after expiry on 2026-07-04)  
**Last Restart:** 2026-09-22 (Phase 27.1)  
**Last Successful Update:** 2026-09-22  

**Note:** Previously documented as ddclient but is actually favonia/cloudflare-ddns container. Token expiry at 2026-07-04 21:10 UTC was the root cause of DNS drift (6 stale A-records).

---

## Alertmanager (CT 205)

**Status:** Active  
**Version:** 0.27.0  
**Configuration File:** /etc/alertmanager/alertmanager.yml  
**Last Modified:** 2026-02-04  
**Last Reload:** 2026-09-22 (Phase 37 — Telegram bot token migration)  

### Alert Routing & Receivers

#### Critical Alerts Receiver
- **Name:** critical-alerts  
- **Channels:** Telegram (dedicated bot), Discord  
- **Telegram Integration:**
  - **Bot:** Dedicated Alertmanager bot (created Phase 37, separate from Jeanne Alter's bot)
  - **Token Storage:** `/etc/alertmanager/secrets/telegram_bot_token` (permissions 600, owner alertmanager:alertmanager)
  - **Config Reference:** `bot_token_file` (supported as of Alertmanager 0.27.0)
  - **Chat ID:** 518832696  
  - **Status:** Verified working (Phase 37 — end-to-end delivery tested with synthetic alerts)
  - **Previous Issue:** Token invalid since at least 2026-09-21 (critical-alerts Telegram delivery silently failed during Sept 21 host-freeze incident; only Discord received that alert). Root cause: token expiry unknown, no monitoring on credential itself. Fixed by generating dedicated bot and migrating to `bot_token_file` storage.
- **Discord Integration:** Active, no changes Phase 37

#### Warning Alerts Receiver
- **Name:** warning-alerts  
- **Channels:** Discord only  
- **Telegram:** Not currently routed (by design; see Action Items below)  

#### Default Receiver
- **Name:** default  
- **Channels:** Discord only  
- **Telegram:** Not currently routed (by design; see Action Items below)  

**Note (Phase 37):** Existing documentation previously described an "Alertmanager → n8n webhook → Telegram/Discord" integration via Emiya — Service Down Alert workflow. This was inaccurate. Alertmanager's live config (unchanged since 2026-02-04) has never contained a `webhook_configs` block referencing n8n; it has always routed directly to Telegram (critical-alerts only) and Discord (all 3 receivers) via native Alertmanager integrations. The Emiya workflow was archived (Phase 37) after confirming zero live dependency.

---

## n8n Workflows (Updated Phase 37)

**Total Active Workflows:** 9 (reduced from 12; 3 archived Phase 37)  
**Archived Workflows:** Da Vinci — Sync Docs Pipeline, Midas — CFO Report, Emiya — Service Down Alert  

### Archived Workflows (Phase 37)

#### Da Vinci — Sync Docs Pipeline
- **Status:** Archived  
- **Reason:** Zero execution history ever detected; unused public webhook is pure attack surface  
- **Archive Date:** 2026-09-22  

#### Midas — CFO Report
- **Status:** Archived  
- **Reason:** Zero execution history ever detected; unused public webhook is pure attack surface  
- **Archive Date:** 2026-09-22  

#### Emiya — Service Down Alert
- **Status:** Archived  
- **Reason:** Confirmed zero live dependency from Alertmanager. Existing documentation described an Alertmanager webhook integration that never existed in the live config (last modified 2026-02-04).  
- **Archive Date:** 2026-09-22  
- **Note:** If Emiya agent has other responsibilities beyond this workflow, those were not identified in Phase 37 investigation.

---

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
- **Internal URL:** http://192.168.30