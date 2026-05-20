# Changelog

### 2026-05-20 — Phase 16.4 — Documentation Pipeline Expansion
- Expanded Da Vinci Update Pipeline from 3 files to 8 files: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md
- Added ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md to core Update Pipeline
- Promoted decisions.md from Phase 3 to Phase 2 priority — decisions kept getting lost between sessions
- Updated max_tokens allocations: AI-CONTEXT 20000 (was 16000), changelog 6000 (was 4000), ROADMAP 8000, agents 8000, current-state 4000, service-catalog 4000, decisions 3000
- Updated Claude project instructions with explicit per-file sections in session summary template to prevent hallucination
- Fixed bug: Fetch GitHub Files had null githubToken — hardcoded token directly in node
- Fixed bug: 5 new Claude API nodes had null API key — added hardcoded apiKey const in each node
- Fixed bug: Push to GitHub filesToPush only had 3 files — updated array to all 8 files with null SHA handling for decisions.md (new file)
- Cost per run estimate updated to ~$0.25-0.35 (was ~$0.11) for 8-file sequential Haiku chain
- Documentation Pipeline architecture diagram updated to show 8 sequential Haiku API calls
- Decision: Include decisions.md in core pipeline rather than manual log or weekly summary — prevents decisions from being lost