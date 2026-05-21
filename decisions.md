# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

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
**Decision:** Wire Langfuse via one node (branched off Push to GitHub) that sends 1 trace + 8 generations in a single batch per run.
**Why:** Cleaner pipeline design with fewer nodes; all 8 generations sent in one batch provides better observability without pipeline clutter. Matches decisions from earlier in the day.
**Alternatives considered:** 8 individual Langfuse nodes (rejected — too many nodes, marginal observability benefit).

### 2026-05-21 - Langfuse internal URL used
**Decision:** Use http://192.168.30.223:3000 instead of https://langfuse.najhin-gaming.com for n8n → Langfuse calls.
**Why:** CT 211 and CT 223 are on the same VLAN 30; no need to route through Cloudflare for local communication. Matches decisions from earlier in the day.
**Alternatives considered:** Use public HTTPS URL (rejected — unnecessary external routing for local traffic).

### 2026-05-21 - Langfuse timestamp uses new Date().toISOString()
**Decision:** Use n8n server UTC time (new Date().toISOString()) for Langfuse batch event timestamps; do not add timezone offsets.
**Why:** Langfuse v3 requires timestamp in every batch event. n8n server UTC time is correct and consistent with ClickHouse's clock in normal operation.
**Alternatives considered:** Query ClickHouse for time (rejected — port 8123 not accessible from CT 211), add timezone offset (rejected — causes timestamp mismatch with server clock).

### 2026-05-21 - LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES set to true
**Decision:** Change LANGFUSE_ENABLE_EXPERIMENTAL_FEATURES from false to true in CT 223 docker-compose.yml.
**Why:** Attempted fix for Langfuse UI trace list bug (traces not appearing in list view despite being in ClickHouse). Did not resolve the issue but left as true for potential future benefit.
**Alternatives considered:** Keep false (rejected — attempted to unlock experimental features), upgrade Langfuse version (deferred to future action item).

### 2026-05-21 - VM 400 disk expanded to 86GB
**Decision:** Expand VM 400 (ollama-gpu) disk from 56GB to 86GB via Proxmox resize + LVM extension (growpart /dev/vda 3, pvresize, lvextend, resize2fs).
**Why:** VM 400 disk was 100% full (56GB) during Ollama model testing (Gemma3:12b required 8.1GB). Expansion provides 34GB free space for future models and testing.
**Alternatives considered:** Delete old models only (rejected — not enough capacity for comprehensive testing), use separate storage (rejected — adds complexity, Proxmox resize simpler).

### 2026-05-21 - Expanded Da Vinci Update Pipeline from 3 files to 8 files
**Decision:** Expand the documentation pipeline to handle 8 files sequentially: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, and decisions.md.
**Why:** Documentation coverage was incomplete; decisions and architectural information were scattered. Consolidating into a unified pipeline ensures all critical documentation stays in sync with every session. Decisions.md is now auto-logged.
**Alternatives considered:** Keep 3-file pipeline and maintain separate manual sync process (rejected — creates inconsistency and drift), update all files in parallel (rejected — would overwhelm token budget).

### 2