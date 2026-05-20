# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

### 2026-05-21 - Langfuse wiring strategy for Da Vinci
**Decision:** Use a single Langfuse node branched off Push to GitHub (after all 8 files complete) rather than 8 separate nodes after each Claude API call.
**Why:** Cleaner pipeline design with fewer nodes; all 8 generations sent in one batch provides better observability without pipeline clutter.
**Alternatives considered:** 8 individual Langfuse nodes (rejected — too many nodes, marginal observability benefit).

### 2026-05-21 - Langfuse uses internal URL
**Decision:** Use http://192.168.30.223:3000 instead of https://langfuse.najhin-gaming.com for n8n → Langfuse calls.
**Why:** CT 211 and CT 223 are on the same VLAN 30; no need to route through Cloudflare for local communication.
**Alternatives considered:** Use public HTTPS URL (rejected — unnecessary external routing for local traffic).

### 2026-05-19 - Da Vinci Update Pipeline rebuilt with 3 separate Haiku calls
**Decision:** Rebuild Da Vinci Update Pipeline to use 3 separate Haiku API calls (one per file: AI-CONTEXT.md, changelog.md, troubleshoot.md) with immediate cost logging after each call.
**Why:** Previous pipeline design was looping on error due to stuck validation files in staging-inbox. Separating calls per file isolates failures, enables immediate cost logging before parse/push, and provides clearer observability of each update step.
**Alternatives considered:** Keep monolithic single-call pipeline (rejected — error loop indicated design issue), batch all files into one call (rejected — loses granularity for debugging and cost tracking).

### 2026-05-19 - Cost logging moved to fire immediately after API call
**Decision:** Cost logging now executes immediately after each Claude API call completes, before parsing and pushing results.
**Why:** Ensures cost is captured even if downstream parse/push fails, providing accurate billing records and preventing cost loss on partial pipeline failures.
**Alternatives considered:** Log cost after successful push (rejected — loses cost data on downstream failures), batch cost logging at end of pipeline (rejected — same risk as previous approach).

### 2026-05-19 - Haiku pricing corrected in cost tracking
**Decision:** Update Haiku pricing model to $0.80/1M input tokens and $4.00/1M output tokens.
**Why:** Previous pricing was incorrect; correction ensures accurate cost projections and budget tracking for the pipeline.
**Alternatives considered:** Continue with incorrect pricing (rejected — defeats cost monitoring goal).

### 2026-05-20 - Aider over Claude Code for future code agent
**Decision:** Aider + local Ollama is the better fit for EMIYA's future code agent capability rather than Claude Code.
**Why:** Claude Code is Anthropic-proprietary and costs API tokens; Aider works with any LLM including qwen3:14b on VM 400, aligning with the goal of reducing Claude API dependency.
**Alternatives considered:** Claude Code (rejected — proprietary, token-costly, increases Claude dependency).

### 2026-05-20 - LM Studio rejected for homelab
**Decision:** Keep Ollama as the local LLM backend; do not switch to LM Studio.
**Why:** LM Studio requires a display, is less stable as a persistent background server, and offers no integration advantage over Ollama for n8n workflows.
**Alternatives considered:** LM Studio (rejected — GUI-dependent, not suitable for headless VM 400).

### 2026-05-20 - Backtick template literals avoided in n8n Code nodes
**Decision:** Use single-quoted strings with concatenation for all system prompts in Claude API nodes; avoid backtick template literals.
**Why:** Backtick template literals caused 400 errors on Anthropic API when used in n8n Code nodes.
**Alternatives considered:** Continue using template literals (rejected — confirmed cause of 400 errors).

### 2026-05-20 - decisions.md promoted to Phase 2 pipeline
**Decision:** Include decisions.md in the core Update Pipeline (promoted from Phase 3 backlog) to ensure decisions are captured and persisted every session.
**Why:** Decisions kept being lost between sessions when not automatically logged. A dedicated pipeline file ensures all decisions are preserved and accessible.
**Alternatives considered:** Manual decisions log (rejected — too easy to forget between sessions), weekly summary only (rejected — decisions happen every session and need immediate capture).

### 2026-05-20 - Claude project instructions updated to require explicit per-file sections
**Decision:** Each session summary must now have a dedicated section for each of the 8 Da Vinci files (AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, decisions.md) so Da Vinci receives unambiguous per-file update instructions.
**Why:** Previous single merged AI-CONTEXT section caused other files to drift as Da Vinci hallucinated which changes applied to which files.
**Alternatives considered:** Single merged section (rejected — caused hallucination in Da Vinci), inline comments (rejected — less clear than explicit sections).

### 2026-05-20 - Hardcoded API keys in new Claude API nodes
**Decision:** Hardcode API key directly in each Claude API node rather than passing via trigger payload, consistent with existing 3 nodes in the pipeline.
**Why:** Maintains consistency with the original pipeline implementation and avoids unnecessary complexity.
**Alternatives considered:** Pass via trigger payload (rejected — would require trigger schema changes and adds complexity for no benefit).

### 2026-05-20 - Expanded Da Vinci Update Pipeline from 3 files to 8 files
**Decision:** Expand the documentation pipeline to handle 8 files sequentially: AI-CONTEXT.md, changelog.md, troubleshoot.md, ROADMAP.md, agents.md, current-state.md, service-catalog.md, and decisions.md.
**Why:** Documentation coverage was incomplete; decisions and architectural information were scattered. Consolidating into a unified pipeline ensures all critical documentation stays in sync with every session.
**Alternatives considered:** Keep 3-file pipeline and maintain separate manual sync process (rejected — creates inconsistency and drift), update all files in parallel (rejected — would overwhelm token budget).