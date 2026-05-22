# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

### 2026-05-22 - Documentation audit reveals hallucination pattern in hardware specs and folder lists
**Decision:** Conduct full audit of all 8 Da Vinci documentation files against conversation history. Identified 11 discrepancies across 4 files (ROADMAP.md, current-state.md, agents.md, AI-CONTEXT.md). Root causes: Da Vinci hallucinated hardware (EPYC 5645/256GB/RTX 4070 from January hypothetical discussion instead of actual Ryzen 5 5600X/128GB/RX 6700 XT), folder names (Knowledge Indexer list never matched actual vault), and failed to propagate May 10 phase retirement decision (22.8C/22.8D/22.8E/22.15/22.16) to ROADMAP.
**Why:** Documentation drift compounds over sessions. Hardware and folder lists are critical infrastructure facts that must stay synchronized. Previous sessions' decisions were not reaching all relevant files because session summaries lacked explicit per-file sections.
**Alternatives considered:** Continue accepting hallucinated specs (rejected — causes infrastructure misalignment and wasted troubleshooting), manual audits only (rejected — too slow, happens every session).

### 2026-05-22 - Hardware section in current-state.md must always be REPLACE SECTION in session summaries
**Decision:** Any hardware change, clarification, or correction must include explicit REPLACE SECTION markup in the current-state.md section of the session summary to prevent drift.
**Why:** Hardware specs are infrastructure facts, not narrative updates. Appending new information causes old hallucinated specs to persist. REPLACE SECTION forces overwrite of entire subsection.
**Alternatives considered:** Append-only updates (rejected — doesn't remove hallucinated data), manual verification (rejected — already proven unreliable).

### 2026-05-22 - Phase retirement decisions must include explicit ROADMAP.md REPLACE SECTION
**Decision:** Any phase that is archived, cancelled, or moved must include explicit REPLACE SECTION markup in the ROADMAP.md section of the session summary for all affected tables (In Progress, Planned, Completed).
**Why:** May 10, 2026 decision to retire Homepage and archive phases 22.8C/22.8D/22.8E/22.15/22.16 never reached ROADMAP.md because that session summary used short-form format without explicit ROADMAP sections. Lesson learned: decisions about phase lifecycle must be explicitly propagated.
**Alternatives considered:** Trust narrative update (rejected — failed May 10), separate phase management file (rejected — adds complexity beyond current pipeline).

### 2026-05-22 - Knowledge Indexer folder list requires manual verification against n8n workflow
**Decision:** Cannot determine correct Qdrant Knowledge Indexer folder list from documentation alone. Three different lists exist across agents.md, current-state.md, and service-catalog.md — none fully matching actual vault structure. Use known folders (04-personal/, 08-agents/, 09-people/, 10-projects/) as baseline and manually verify against n8n Knowledge Indexer node configuration next session.
**Why:** Hallucinated folder lists (05-books/, 06-projects/, 07-reference/, 11-learning/, 12-research/, 13-archive/) don't exist in actual vault. Documentation cannot be trusted as source of truth for this configuration. Must verify against running system.
**Alternatives considered:** Trust current documentation (rejected — proven unreliable), generate new folder list from Obsidian export (rejected — requires manual Obsidian access from this session), skip verification (rejected — hallucinated lists cause RAG failures).

### 2026-05-22 - All agents route Obsidian writes through Da Vinci — Personal Knowledge gateway
**Decision:** Gilgamesh, EMIYA, Midas and all future agents send data to Da Vinci rather than writing directly to Obsidian. Da Vinci owns all Obsidian writes.
**Why:** Consistent formatting across all agents, prevents file conflicts, Da Vinci acts as quality gate for personal knowledge. Centralizes write authority.
**Alternatives considered:** Direct agent writes to Obsidian (rejected — causes conflicts, inconsistent formatting), per-agent folders (rejected — adds complexity, no unified quality control).

### 2026-05-22 - Gil reads Obsidian directly, not through Da Vinci
**Decision:** Gil queries Qdrant/RAG directly for read operations. Only writes go through Da Vinci — Personal Knowledge gateway.
**Why:** Read path doesn't need gating or quality filtering. Direct RAG access is faster and simpler for retrieving personal context during conversations.
**Alternatives considered:** Route all Gil reads through Da Vinci (rejected — unnecessary complexity, adds latency).

### 2026-05-22 - 04-personal/ included in Qdrant indexing
**Decision:** Reverse previous privacy exclusion — 04-personal/ folder now indexed by Knowledge Indexer in Qdrant obsidian_knowledge collection.
**Why:** All infrastructure is internal VLAN 30 with no external exposure. Gil needs to read personal profile via RAG to maintain conversation context.
**Alternatives considered:** Keep 04-personal/ excluded (rejected — Gil can't access own profile), separate Qdrant collection (rejected — adds complexity, redundant indexing).

### 2026-05-22 - Da Vinci quality filter: stores durable personal facts, SKIPs ephemeral data
**Decision:** Da Vinci assesses each fact with Claude Haiku — stores persistent characteristics ("prefers dark mode"), SKIPs one-time events ("slept at 12am tonight") and conversational noise.
**Why:** Personal knowledge base should contain stable, reusable context. Transient facts clutter the profile and reduce RAG relevance.
**Alternatives considered:** Store everything (rejected — noise reduces usefulness), manual curation (rejected — defeats automation goal).

### 2026-05-22 - SKIP detection uses startsWith not strict equality
**Decision:** Da Vinci Code nodes check content.trim().startsWith('SKIP') instead of === 'SKIP'. Da Vinci may return "SKIP\n\nReasoning..." with trailing content.
**Why:** Prevents SKIP-flagged content from overwriting the profile when reasoning text follows the SKIP marker. Happened in this session when equality check failed.
**Alternatives considered:** Strict equality check (rejected — caused data loss), trim then equality (rejected — doesn't handle multiline content correctly).

### 2026-05-22 - Date placeholder replaced in Code node before Claude call
**Decision:** {{date}} template in profile replaced with today's MYT date (YYYY-MM-DD) in the Claude API — Assess & Merge Code node before sending to Claude. Never rely on Claude to replace it.
**Why:** More reliable and deterministic than asking Claude to do string replacement. Code node has direct access to server time; Claude may hallucinate or skip replacement.
**Alternatives considered:** Let Claude replace {{date}} (rejected — unreliable), hardcode date in profile (rejected — stale after one day).

### 2026-05-22 - Gil uses internal URL for Da Vinci personal knowledge gateway
**Decision:** Gil's Send to Da Vinci node uses http://192.168.30.211:5678/webhook/davinci-personal-knowledge (internal VLAN 30) instead of external Cloudflare URL.
**Why:** CT 211 and CT 400 are on same VLAN 30. Internal routing is faster, avoids unnecessary external hops, eliminates Cloudflare dependency for internal traffic.
**Alternatives considered:** Use external https://davinci.najhin-gaming.com (rejected — unnecessary external routing, adds latency).

### 2026-05-22 - qwen3:14b stays as primary Ollama model; gemma3 and phi4-mini removed
**Decision:** Keep qwen3:14b as primary. Remove gemma3:4b, gemma3:12b, phi4-mini (tested this session). Retain qwen3.5:latest as secondary.
**Why:** All three removed models hallucinated factual data confidently in testing. qwen3:14b honest about limitations. Personal assistant handling real data requires honesty over speed.
**Alternatives considered:** Use faster models (rejected — hallucination risk unacceptable), keep all models (rejected — disk space, slow inference).

### 2026-05-22 - VM 400 disk block device is /dev/vda not /dev/sda
**Decision:** KVM virtio disks use /dev/vda naming convention. All VM 400 disk operations must reference /dev/vda partition, not /dev/sda.
**Why:** VM 400 is KVM-based (not QEMU/IDE). Correct device naming is required for growpart, pvresize, lvextend, and resize2fs to succeed.
**Alternatives considered:** Use /dev/sda (rejected — device doesn't exist, causes operation failure).

### 2026-05-21 - decisions.md promoted to Phase 2 pipeline
**Decision:** Include decisions.md in the core Update Pipeline (promoted from Phase 3 backlog) to ensure decisions are captured and persisted every session.
**Why:** Decisions kept being lost between sessions when not automatically logged. A dedicated pipeline file ensures all decisions are preserved and accessible.
**Alternatives considered:** Manual decisions log (rejected — too easy to forget between sessions), weekly summary only (rejected — decisions happen every session and need immediate capture).

### 2026-05-21 - Claude project instructions updated to require explicit per-file sections
**Decision:** Each session summary must now have a dedicated section for each of the 8 Da Vinci files (AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md) so Da Vinci receives unambiguous per-file update instructions.
**Why:** Previous single merged AI-CONTEXT section caused other files to drift as Da Vinci hallucinated which changes applied to which files.
**Alternatives considered:** Single merged section (rejected — caused hallucination in Da Vinci), inline comments (rejected — less clear than explicit sections).

### 2026-05-21 - Hardcoded API keys in new Claude API nodes
**Decision:** Hardcode API key directly in each Claude API node rather than passing via trigger payload, consistent with existing 3 nodes in the pipeline.
**Why:** Maintains consistency with the original pipeline implementation and avoids unnecessary complexity. Trigger schema only defines fileContent and chatId; referencing undefined trigger fields caused null errors.
**Alternatives considered:** Pass via trigger payload (rejected — would require trigger schema changes and adds complexity for no benefit).

### 2026-05-21 - Backtick template literals avoided in n8n Code nodes
**Decision:** Use single-quoted strings with concatenation for all system prompts in Claude API nodes; avoid backtick template literals.
**Why:** Backtick template literals caused 400 errors on Anthropic API when used in n8n Code nodes. Malformed JSON in request body.
**Alternatives considered:** Continue using template literals (rejected — confirmed cause of 400 errors).

### 2026-05-21 - AI-CONTEXT max_tokens bumped to 25000
**Decision:** Increase AI-CONTEXT.md max_tokens from 20000 to 25000 (middle ground between ceiling and RM 30 budget limit).
**Why:** Da Vinci was hitting 20000 token ceiling every run, truncating documentation. 25000 provides breathing room without exceeding cost targets (~$0.22/run total for 8 files).
**Alternatives considered:** 32000 (rejected — exceeds RM 30 budget ceiling), keep 20000 (rejected — consistently hitting limit).

### 2026-05-21 - Model testing conclusion — qwen3:14b stays as primary
**Decision:** Keep qwen3:14b as primary local model on VM 400; remove Gemma 3:4b, Gemma 3:12b, and phi4-mini.
**Why:** Gemma 3 and phi4-mini all hallucinated factual weather data confidently in testing. qwen3:14b and llama3.2 were honest about their limitations. For a personal assistant handling real data, honesty is critical over speed.
**Alternatives considered:** Use faster models (rejected — hallucination risk unacceptable for factual data), keep all models (rejected — disk space needed, slow inference).

### 2026-05-21 - Langfuse — Da Vinci uses single batch node
**Decision:** Wire Langfuse via one node (branched off Push to