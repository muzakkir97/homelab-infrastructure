# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

### 2026-05-25 - Search queries in Gilgamesh force-route to Claude Haiku, not qwen3:14b
**Decision:** All search queries detected by the "Needs Web Search?" If node are routed to Claude Haiku instead of qwen3:14b, even when Ollama is online.
**Why:** qwen3:14b ignores injected search context regardless of prompt instruction strength — consistently responds "I don't have real-time data" even with actual Firecrawl results in the user message context. This is a fundamental local model limitation, not a prompt engineering problem. Claude Haiku correctly follows injected search results. Trade-off: search queries cost ~$0.001-0.002 each (Haiku pricing) instead of $0 (local), but acceptable for personal use volume.
**Alternatives considered:** Stronger prompt instructions (rejected — tested, qwen3:14b still ignores context), use qwen3.5 (rejected — same issue), skip web search when Ollama online (rejected — reduces functionality for local cost savings).

### 2026-05-25 - Firecrawl chosen over Brave Search, Serper, and SearXNG for web search
**Decision:** Integrate Firecrawl API (firecrawl.dev) as the web search backend for Gilgamesh instead of evaluating Brave Search, Serper, or SearXNG.
**Why:** Firecrawl community node already installed in n8n (v2.1.1), free tier available without credit card requirement, no new containers needed. Brave changed free tier in Feb 2026 (now requires credit card). Serper offers 2,500 one-time free queries (limited). SearXNG would require new LXC container. Firecrawl was zero-friction deployment.
**Alternatives considered:** Brave Search API (rejected — CC required), Serper (rejected — limited free queries), SearXNG (rejected — new container needed), build custom crawler (rejected — over-engineered for personal use).

### 2026-05-25 - Web search results injected into last user message, not system prompt
**Decision:** When Firecrawl returns web search results, inject them into the last user message context, not as a system prompt addition.
**Why:** Local models (qwen3:14b) respond better to user-turn context injection than system prompt additions. Since search queries now route to Haiku anyway, this distinction is less critical, but kept for consistency with earlier n8n architecture decisions.
**Alternatives considered:** Inject into system prompt (rejected — local models ignore system modifications), separate search results as tool output (rejected — adds complexity beyond current prompt pattern).

### 2026-05-25 - Da Vinci Stage 2 considered complete — no separate deployment needed
**Decision:** Mark Da Vinci Stage 2 (RAG System & Knowledge Retrieval) as effectively complete. RAG pipeline (Qdrant + nomic-embed-text embeddings + Knowledge Indexer + Gilgamesh RAG queries) fully operational via Phase 24.9 deployment. No additional "Stage 2" build or deployment required.
**Why:** RAG foundation is complete and operational. The remaining planned work (Phase 7E "Extended Memory — 20+ message conversations") is a distinct feature about conversation buffer memory (already operational via gilgamesh_conversations Data Table). Conversation buffer (last 15 messages) already implemented in Format Messages. Stage 2 architecture goal achieved without a separate deployment event.
**Alternatives considered:** Treat Phase 7E as "Stage 2 completion" (rejected — Phase 7E is conversation feature, not RAG), create separate Stage 2 build (rejected — duplicates Phase 24.9 work), leave Stage 2 as open (rejected — RAG is functionally complete).

### 2026-05-25 - ROADMAP Phase 7E duplicate resolved — keep Completed entry, remove Planned
**Decision:** Phase 7E (Extended Memory — 20+ message conversations) appears in both Completed and Planned tables. Keep the Completed entry (May 15, 2026: conversation archival functionality). Remove from Planned table — conversation buffer memory is already operational in Format Messages (fetches last 15 gilgamesh_conversations rows), no additional build needed.
**Why:** Completed entry (conversation archival to Qdrant) is a distinct, deployable feature from 2026-05-15. Planned entry appears to be a duplicate that confused "conversation buffer memory" (already working) with "extended memory phase". Conversation buffer (last 15 messages) is operational; no Phase 7E re-deployment required.
**Alternatives considered:** Keep both (rejected — duplicate, confusing), move Completed to archive (rejected — conversation archival is a real completed feature), rename Planned to distinct feature (rejected — feature already working in Format Messages).

### 2026-05-25 - Gilgamesh feature wishlist documented as 16 planned features
**Decision:** Compile and document Gilgamesh feature wishlist as 16 distinct phases across High/Medium/Long-term priority tiers. Source: original brainstorm + internet research on personal AI assistant features 2025-2026. Features include: Proactive Goal Nudges, Weekly Automated Review, "What did I do?" Recall, URL/Article Summarizer, Grocery/Task List management, Smart Morning Briefing, Calendar Awareness, Financial Intelligence (Midas v2), News Digest, Voice Notes, Plan My Day.
**Why:** Structured roadmap of potential Gilgamesh enhancements provides prioritized build queue. Brainstorm identified user needs; internet research confirmed common personal assistant features in 2025-2026 landscape. Documented with effort estimates (1-12 hours) enables session planning.
**Alternatives considered:** Keep as ad-hoc brainstorm (rejected — loses prioritization), drop low-priority items (rejected — may become valuable later), assign all to Phase (rejected — too many to build in one session).

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
**