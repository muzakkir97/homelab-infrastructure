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