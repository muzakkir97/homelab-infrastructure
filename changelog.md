# Changelog

### 2026-05-21 — Phase 24.8 — Langfuse Wiring (Da Vinci) + Model Testing + VM 400 Expansion
- Bumped AI-CONTEXT max_tokens to 25000 (was 20000, hitting ceiling)
- Tested Gemma 3:4b, Gemma 3:12b, phi4-mini, llama3.2, qwen3.5 models on VM 400
- Expanded VM 400 disk from 56GB to 86GB (Proxmox resize + LVM extension); 34GB free post-expansion
- Removed gemma3:4b, gemma3:12b, phi4-mini from VM 400 Ollama (all hallucinated factual data); qwen3:14b remains primary
- Wired Langfuse observability into Da Vinci Update Pipeline with single trace node (Langfuse — Da Vinci) branched off Push to GitHub
- Da Vinci Update Pipeline now logs one trace (da-vinci-update) with 8 generations (one per file) to Langfuse per run
- Langfuse traces confirmed in ClickHouse and accessible via direct URL and public API; UI trace list shows no results (known v3 self-hosted bug)
- Langfuse internal URL configured: http://192.168.30.223:3000 for n8n → Langfuse calls (CT 211 and CT 223 on same VLAN 30)
- LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES changed to true in CT 223 docker-compose (attempted fix for UI trace list bug, did not resolve)
- Decision: Single Langfuse node after all 8 files complete rather than 8 individual nodes — cleaner pipeline, fewer nodes, batch send all generations
- Decision: Use internal URL instead of https://langfuse.najhin-gaming.com — no need to route through Cloudflare for same-VLAN communication
- Decision: Langfuse ingestion timestamps use n8n server UTC time (new Date().toISOString()); do not add timezone offsets
- Decision: Model testing conclusion — qwen3:14b stays as primary; Gemma and phi4-mini hallucinated confidently; honesty over speed for personal assistant
- Decision: AI-CONTEXT max_tokens bumped to 25000 (middle ground between 20000 hitting ceiling and 32000 budget limit); expected cost ~$0.22/run total for 8 files
- Key Lessons: Backtick template literals in n8n Code nodes cause 400 errors on Anthropic API; use single-quoted strings with concatenation instead
- Key Lessons: Langfuse v3 self-hosted UI trace list has known bug where traces don't appear in list view despite being in ClickHouse; accessible via direct URL and public API
- Action items: Verify trace appears in Langfuse UI; consider upgrading Langfuse to latest version; wire Langfuse into MERLIN next

### 2026-05-20 — Phase 16.4 — Documentation Pipeline Expansion
- Expanded Da Vinci Update Pipeline from 3 files to 8 files: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Added ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md to core Update Pipeline
- Promoted decisions.md from Phase 3 to Phase 2 priority — decisions kept getting lost between sessions
- Updated max_tokens allocations: AI-CONTEXT 20000 (was 16000), changelog 6000 (was 4000), ROADMAP 8000, agents 8000, current-state 4000, service-catalog 4000, decisions 3000
- Updated Claude project instructions with explicit per-file sections in session summary template to prevent hallucination
- Fixed bug: Fetch GitHub Files had null githubToken — hardcoded token directly in node
- Fixed bug: 5 new Claude API nodes had null API key — added hardcoded apiKey const in each node
- Fixed bug: Push to GitHub filesToPush only had 3 files — updated array to all 8 files with null SHA handling for decisions.md (new file)
- Fixed bug: sessionSummary was undefined — changed to read fileContent from trigger payload
- Fixed bug: Claude API nodes returned 400 errors — replaced backtick template literals with single-quoted strings and concatenation
- Fixed bug: Log Cost service-catalog node had wrong command_type — corrected from current-state to service-catalog
- Cost per run estimate updated to ~$0.14 (was ~$0.11) for 8-file sequential Haiku chain
- Runtime per run increased to ~5 minutes (was ~4 minutes) due to 8 sequential API calls
- Documentation Pipeline architecture diagram updated to show 8 sequential Haiku API calls plus logging and Git operations
- Decision: Include decisions.md in core pipeline rather than manual log or weekly summary — prevents decisions from being lost
- Decision: Hardcode API keys directly in n8n nodes — trigger schema only defines fileContent and chatId; adding more fields adds unnecessary complexity
- Decision: Use Ollama as local LLM backend instead of LM Studio — LM Studio requires display, less stable as background service
- Decision: Aider + local Ollama identified as better fit than Claude Code for future EMIYA code agent — reduces Claude API dependency
- Identified stale docs/ folder artifacts on GitHub (docs/changelog.md, docs/troubleshoot.md) from old pipeline — manual cleanup required

### 2026-05-19 — Pipeline Test: Da Vinci Update Pipeline Rebuilt with Per-File API Calls
- Da Vinci Update Pipeline rebuilt with 3 separate Haiku API calls (AI-CONTEXT, changelog, troubleshoot) instead of sequential 8-file chain
- Cost logging moved to fire immediately after each Claude API call, before parse/push operations
- Haiku pricing corrected: $0.80/1M input tokens, $4.00/1M output tokens
- Resolved pipeline loop: staging-inbox had stuck file failing validation repeatedly every 15 minutes — deleted stuck file and restructured pipeline
- Per-file API calls reduce error propagation; single file failure no longer blocks entire pipeline
- Action items: Monitor gilgamesh_costs for 3 new rows per pipeline run; observe API usage for 24 hours to confirm cost under target