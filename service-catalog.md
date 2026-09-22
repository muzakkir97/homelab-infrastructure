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
16. **node_exporter /mnt exclusion (2026-07-14):** Debian package default `--collector.filesystem.mount