# troubleshoot.md

## Common Issues

### Documentation Pipeline Failures

**Symptoms:** Da Vinci Update Pipeline stops mid-execution or produces incomplete file updates

**Root Cause:** Missing or null API credentials in Claude API nodes

**Resolution:** Hardcode API key directly in each node (const apiKey = "...") rather than attempting to retrieve from trigger payload. Ensure all 8 nodes in pipeline have explicit apiKey declarations.

**Lesson:** Avoid dynamic credential retrieval from trigger payloads in sequential workflows. Hardcoded credentials in individual nodes prevent cascading failures.

---

**Symptoms:** Some files updated on GitHub but others missing; only 3 of 8 files pushed

**Root Cause:** filesToPush array in Push to GitHub node not updated when pipeline expanded from 3 to 8 files

**Resolution:** Manually update filesToPush array to include all 8 files: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md. Handle null SHA for new files like decisions.md.

**Lesson:** When expanding sequential pipelines, update all downstream nodes (fetch, push, validation) simultaneously. File lists propagate through the entire workflow.

---

**Symptoms:** Fetch GitHub Files node returns null or empty token error

**Root Cause:** Node references githubToken from trigger payload which is never populated by workflow caller

**Resolution:** Hardcode GitHub token directly in the node (const githubToken = "...") instead of attempting dynamic retrieval from trigger data

**Lesson:** In internal automation workflows, hardcoded credentials for service accounts are more reliable than trigger-based injection.

---

**Symptoms:** Fetch GitHub Files reads undefined sessionSummary field; workflow fails immediately after fetch

**Root Cause:** Node attempted to read `sessionSummary` from trigger payload, but the trigger field is actually named `fileContent`

**Resolution:** Changed node to read from correct field: `$('When Executed by Another Workflow').first().json.fileContent` instead of referencing undefined `sessionSummary`

**Lesson:** Always verify trigger payload field names match what the calling workflow actually passes. Document trigger schema explicitly to prevent field name mismatches in sequential workflows.

---

**Symptoms:** Claude API nodes return 400 Bad Request errors on Anthropic API; requests fail validation

**Root Cause:** Backtick template literals in system prompts cause malformed JSON when sent to Anthropic API from n8n Code nodes

**Resolution:** Replace all backtick template literals with single-quoted strings using string concatenation. Example: change `` `Update ${file}` `` to `'Update ' + file`

**Lesson:** n8n Code nodes handle template literals differently than standard JavaScript; backticks cause escaping issues in API payloads. Use explicit concatenation for all dynamic strings in n8n nodes that call external APIs.

---

**Symptoms:** Log Cost node logs incorrect command_type; service catalog cost rows misattributed to current-state

**Root Cause:** Copy-paste error when creating Log Cost node for service-catalog — command_type field retained old value `/update (Da Vinci - current-state)` instead of being updated

**Resolution:** Correct command_type field to `/update (Da Vinci - service-catalog)` in the service-catalog Log Cost node

**Lesson:** When duplicating nodes in sequential pipelines, audit all configuration fields, not just the primary ones. Copy-paste remnants in metadata fields cause subtle tracking errors.

---

**Symptoms:** Da Vinci Update Pipeline loops continuously with validation errors; staging-inbox contains stuck file that fails repeatedly every 15 minutes

**Root Cause:** Stuck file in staging-inbox failed validation repeatedly, causing pipeline to restart loop without successful completion or file removal

**Resolution:** Deleted stuck file from staging-inbox. Rebuilt Da Vinci Update Pipeline as 3 separate Haiku API calls (one per file: AI-CONTEXT.md, changelog.md, troubleshoot.md) with immediate cost logging after each API call, before parse/push operations. This isolation prevents one file's failure from blocking others and enables faster cost tracking.

**Lesson:** In multi-file sequential workflows, separate API calls per file prevent cascading failures and allow independent retry/troubleshooting. Implement cost logging immediately after API calls rather than batching at end of pipeline to catch issues faster.

---

**Symptoms:** Langfuse ingestion node returns 400 Bad Request; batch events rejected by Langfuse API

**Root Cause:** Timestamp field removed from batch events; Langfuse v3 requires timestamp (createdAt) in every event in the batch payload

**Resolution:** Add `createdAt: new Date().toISOString()` to each event in the batch. Use n8n server UTC time directly via `new Date().toISOString()` without adding timezone offsets.

**Lesson:** Langfuse ingestion API validates all required fields on every event in a batch, not just the batch envelope. When constructing batch payloads in n8n Code nodes, include all required fields (event_name, timestamp, trace_id, observation_id, etc.) for every event. Use server-local UTC time; do not attempt to query ClickHouse or add manual offsets.

---

**Symptoms:** Langfuse UI trace list shows "No results" for recent traces; traces confirmed to exist in ClickHouse and accessible via /api/public/traces

**Root Cause:** Known bug in Langfuse v3 self-hosted UI where trace list view does not display traces despite data existing in ClickHouse. Analytics_traces view in ClickHouse only shows data >1 hour old by design (not a freshness issue). PostgreSQL traces table empty (expected — ClickHouse is source of truth). Background migrations complete.

**Resolution:** Traces are accessible via direct URL links and public API (/api/public/traces). Use API and direct URLs for trace inspection until UI trace list bug is resolved. Consider upgrading Langfuse to latest version or filing bug report with Langfuse maintainers.

**Lesson:** Langfuse v3 self-hosted has known UI bugs that do not indicate data loss. Verify trace ingestion via ClickHouse queries and public API before assuming ingestion failure. UI bugs are cosmetic; data integrity is maintained.

---

**Symptoms:** Knowledge Indexer crashes when downloading files from new Obsidian folders; "Document loader is not initialized" error on manual runs

**Root Cause:** New folders added to indexing (04-personal/, 08-agents/, 09-people/, 10-projects/) contained empty files or files that failed download, causing loader initialization to fail for the entire batch

**Resolution:** Added If node filter before Document Loader node to skip files where `$json.data` is empty or null. Filter checks: `$json.data && $json.data.length > 0` to prevent empty payloads from reaching loader.

**Lesson:** Sequential document processing workflows need defensive checks for empty/null data before passing to loaders. Always validate payload existence before instantiating resource-intensive components. Empty data is recoverable if filtered early; failures downstream cascade to entire batch.

---

**Symptoms:** Da Vinci — Personal Knowledge gateway SKIP detection overwrites profile with empty content; "SKIP\n\nReasoning..." treated as valid data instead of rejection signal

**Root Cause:** SKIP detection used strict equality (`content === 'SKIP'`) but Claude returns multi-line response like `SKIP\n\nReasoning: ...`. Condition failed, code proceeded to save empty profile, overwriting previous data.

**Resolution:** Changed SKIP detection to use `content.trim().startsWith('SKIP')` instead of strict equality. Now correctly identifies rejection signals regardless of trailing whitespace or reasoning text.

**Lesson:** LLM response parsing must account for multi-line outputs and whitespace. Use startsWith/includes for signal detection rather than strict equality on multi-line responses. Always trim before pattern matching on Claude output.

---

**Symptoms:** Da Vinci — Personal Knowledge gateway fails to replace {{date}} placeholder; date shows literally in profile as "{{date}}" instead of actual date

**Root Cause:** Date placeholder replacement delegated to Claude prompt, but Claude sometimes fails to recognize or replace template syntax, especially when prompt is constructed dynamically

**Resolution:** Replace {{date}} with actual MYT date (YYYY-MM-DD format) in the Code node BEFORE sending payload to Claude API. Use `let currentProfile = bootstrapTemplate.replace(/\{\{date\}\}/g, getTodayMYT())` in Code node. This ensures date is always substituted regardless of Claude's parsing.

**Lesson:** Template substitution for infrastructure values should happen in deterministic code paths, not delegated to LLM processing. Use regex replace in Code nodes for system values; reserve Claude processing for semantic decisions only. Separating concerns prevents both parsing failures and data inconsistencies.

---

**Symptoms:** VM 400 disk expansion commands fail; growpart returns "No partitions found" or "Device not found"

**Root Cause:** Assumed VM 400 uses /dev/sda (SATA), but Proxmox KVM deployment uses /dev/vda (virtio). Commands targeted wrong device.

**Resolution:** Use /dev/vda instead of /dev/sda for all VM 400 disk operations. Sequence: `growpart /dev/vda 3` → `pvresize /dev/vda3` → `lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv` → `resize2fs`. Confirm device with `lsblk` before any resize command.

**Lesson:** KVM/virtio VMs use vda naming; bare metal and older hypervisors use sda. Always run `lsblk` to verify actual device names before destructive storage operations. Device naming varies by hypervisor — assumptions cause data loss.