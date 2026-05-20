# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

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