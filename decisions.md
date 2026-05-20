# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

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