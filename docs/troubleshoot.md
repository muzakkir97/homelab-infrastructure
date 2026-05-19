# Troubleshooting Log

> **Format:** Newest first, append-only

---

## 2026-05-19 — Da Vinci Update Pipeline Error Loop and Cost Logging Failure

**Symptoms:** Da Vinci Update Pipeline executing repeatedly every 15 minutes overnight (May 18-19), consuming Claude API tokens on each cycle despite failing validation. Error loop continued until manual intervention. Cost logging node never executed despite pipeline running multiple times.

**Root Cause:** Two-layer issue:
1. Staging-inbox contained a stuck session summary file that failed validation during Claude API processing. File remained in inbox, triggering Update Pipeline on each 15-minute watcher cycle.
2. Pipeline architecture had single consolidated Claude API call attempting to generate 3 complete documents (AI-CONTEXT.md + changelog.md + troubleshoot.md) in one request with max_tokens=32000. Output exceeded token limit, causing Parse Response to fail mid-document.
3. Cost logging node placed at end of pipeline after Parse Response — never executed because pipeline failed before reaching that node. Result: tokens consumed but no cost tracking recorded.

**Resolution:**
1. Deleted stuck file from staging-inbox to stop the watcher trigger loop
2. Rebuilt Da Vinci Update Pipeline with 3 separate Haiku API calls instead of single consolidated call:
   - Call 1: Generate AI-CONTEXT.md (max_tokens: 16000)
   - Call 2: Generate changelog.md (max_tokens: 4000)
   - Call 3: Generate troubleshoot.md (max_tokens: 4000)
3. Moved cost logging node to fire immediately after each HTTP Request node, before Parse Response and Git Push nodes — ensures cost data recorded even if downstream nodes fail
4. Corrected Haiku pricing in Log Cost nodes from $0.25/1M input + $1.25/1M output (Haiku 3 rates, incorrect) to $0.80/1M input + $4.00/1M output (Haiku 4.5 actual pricing)
5. Each of the 3 API calls now has its own dedicated Log Cost code node in sequence: HTTP Request → Log Cost → Parse Response

**Verification:** Pipeline executed successfully with 3 new rows created in gilgamesh_costs table (one per API call). No repeated error cycles observed. Cost logging fired on all 3 calls and properly calculated using corrected pricing.

**Lesson:** Stuck files in inbox queues cause pipeline loops on watcher intervals. Separate API calls per file provides better error isolation and prevents single-failure cascades. Cost logging nodes must fire immediately after API calls, before downstream parsing — otherwise token consumption goes untracked if parser fails. Always verify inbox state and pipeline success nodes when investigating repeated failures. Historical cost entries pre-May-19 using Haiku are understated by ~3x due to incorrect pricing rates.

---

## 2026-05-19 — Da Vinci Update Pipeline Looping on Error

**Symptoms:** Da Vinci Update Pipeline executing repeatedly every 15 minutes, failing validation and creating duplicate error cycles.

**Root Cause:** staging-inbox contained a stuck file that failed validation during Claude API processing. File remained in inbox, triggering Update Pipeline on each 15-minute watcher cycle, causing repeated failures on the same corrupted input.

**Resolution:** 
1. Deleted stuck file from staging-inbox
2. Rebuilt Da Vinci Update Pipeline with 3 separate Haiku API calls instead of single consolidated call (one call per file: AI-CONTEXT.md, changelog.md, troubleshoot.md)
3. Implemented immediate cost logging after each API call, before Parse Response and Git Push nodes
4. Cost logging now fires per-file rather than end-of-pipeline

**Verification:** Pipeline executed successfully with 3 new rows created in gilgamesh_costs table (one per API call). No repeated error cycles observed.

**Lesson:** Stuck files in inbox queues will cause pipeline loops on watcher intervals. Separate API calls per file provides better error isolation and immediate cost tracking. Always verify inbox state before investigating repeated pipeline failures.

---

## 2026-05-18 — Da Vinci Concurrent Executions

**Symptoms:** Three Update Pipeline instances ran simultaneously because qwen3.5 local inference exceeded 1-minute watcher interval. File not yet archived when next watcher fired, triggering duplicate runs.

**Root Cause:** Local LLM inference time (qwen3.5) exceeded inbox watcher schedule interval (1 minute). Multiple pipeline instances fired on same file, causing duplicate processing.

**Resolution:** 
1. Increased Inbox Watcher schedule from 1 minute to 15 minutes
2. Added Check Running node that queries n8n API before List Inbox node
3. If Update Pipeline already running, inbox watcher skips that cycle via If node
4. Workflow order: Schedule Trigger → Check Running → If → List Inbox → rest of pipeline

**Verification:** End-to-end test passed with qwen3.5:latest. No duplicate executions observed.

**Lesson:** When using local LLMs with longer inference times, always add API-based concurrency lock before processing. GET /api/v1/executions?status=running&workflowId={id} with X-N8N-API-KEY header. Returns running executions — if count > 0, skip current cycle.

---

## 2026-05-18 — Da Vinci Cost Tracking Always Zero

**Symptoms:** Da Vinci Update Pipeline showing 0 tokens and 0 cost in gilgamesh_costs Data Table despite processing session summaries. Claude API credit burning $5.53 in ~1 day but no cost tracking.

**Root Cause:** Two-layer issue discovered:
1. Da Vinci — Documentation Pipeline was dropping usage data in Extract Response node (orphaned workflow)
2. Da Vinci — Update Pipeline (actual active workflow via Inbox Watcher) had no cost logging nodes at all

**Resolution:** 
1. Deactivated orphaned Da Vinci — Documentation Pipeline workflow
2. Added Log Cost node and Insert row to gilgamesh_costs in Da Vinci — Update Pipeline after Send Confirmation
3. Switched from claude-sonnet-4-20250514 to claude-haiku-4-5-20251001 for 10x cost reduction
4. Fixed sessionSummary property name in Claude API Merge node

**Verification:** End-to-end test created gilgamesh_costs row 128 with proper token counts and cost calculation.

**Lesson:** Always trace the complete execution path: Inbox Watcher → Execute Workflow → Update Pipeline. Documentation Pipeline was orphaned. Cost logging must be in the actively triggered workflow, not abandoned workflows.

---

## 2026-05-18 — sessionSummary Property Undefined in Da Vinci

**Symptoms:** Claude API responding with "session summary is undefined" preamble before delimiters, causing Parse Response to fail.

**Root Cause:** Claude API Merge node was reading $input.first().json.fileContent but Fetch GitHub Files outputs $input.first().json.sessionSummary. Property name mismatch caused undefined value to be passed to Claude.

**Resolution:** Changed Claude API Merge node to read correct property:
```javascript
// Before (failing):
const sessionSummary = $input.first().json.fileContent;

// After (working):
const sessionSummary = $input.first().json.sessionSummary;
```

**Verification:** Da Vinci pipeline now processes session summaries correctly without undefined errors.

**Lesson:** Property names must match exactly between nodes. Fetch GitHub Files outputs sessionSummary, not fileContent. Always verify node output structure when debugging undefined values.

---

## 2026-05-18 — Da Vinci Output Truncated at 16000 Tokens

**Symptoms:** Da Vinci Update Pipeline output getting cut off mid-document, producing incomplete AI-CONTEXT.md file.

**Root Cause:** max_tokens set to 16000 was insufficient for three complete document outputs (AI-CONTEXT.md + changelog.md + troubleshoot.md). Haiku hit token limit before completing all sections.

**Resolution:** Reverted max_tokens from 16000 to 32000 in Claude API node. Three complete documents require the higher limit despite cost concerns.

**Verification:** Full AI-CONTEXT.md now generated without truncation.

**Lesson:** Document generation requires sufficient token allowance. 16000 tokens insufficient for three complete files. Cost optimization cannot compromise output completeness.

---

## 2026-04-24 — Extract Response Node Reading Wrong Data

**Symptoms:** Extract Response Code node in Gilgamesh hybrid routing workflow throwing errors when trying to process Claude API responses, showing undefined references.

**Root Cause:** After implementing merge node to combine Ollama and Claude routing paths, the Extract Response node was using generic `$json` reference instead of explicitly targeting the merged data from the correct source node.

**Resolution:** Updated Extract Response node to explicitly reference the Merge node output:
```javascript
// Before (failing):
const response = $json;

// After (working):
const response = $('Merge').first().json;
```

**Verification:** Tested hybrid routing with both simple and complex queries - Extract Response now correctly processes merged data from both routing paths.

**Lesson:** When using merge nodes or complex routing in n8n workflows, always explicitly reference the source node using `$('NodeName').first().json` instead of generic `$json` to ensure proper data flow.

---

## 2026-04-24 — httpRequestWithAuthentication Not Supported in Code Nodes

**Symptoms:** n8n Code node throwing error "httpRequestWithAuthentication is not a function" when attempting HTTP requests with authentication headers.

**Root Cause:** n8n Code nodes do not have direct access to httpRequestWithAuthentication helper. This function is only available in HTTP Request nodes, not in custom JavaScript code execution contexts.

**Resolution:** Replaced Code node with native HTTP Request node for authenticated API calls. HTTP Request node handles authentication headers and credential management natively without requiring custom code.

**Verification:** API calls now execute successfully through HTTP Request node with proper authentication.

**Lesson:** Use native HTTP Request nodes for API calls instead of attempting to replicate functionality in Code nodes. Code nodes are for data transformation and logic, not for making authenticated requests.