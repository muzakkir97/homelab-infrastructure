# Troubleshooting Log

> **Format:** Newest first, append-only

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

**Symptoms:** n8n Code node throwing error "httpRequestWithAuthentication