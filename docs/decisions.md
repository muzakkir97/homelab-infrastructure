# Architecture decision records

Key architectural decisions and their rationale.
This document answers the *why* behind the technical choices.

---

## ADR-001: n8n over LangChain/CrewAI for agent orchestration
**Date:** January 2026 | **Status:** Active

**Decision:** Use n8n as the primary agent orchestration layer.

**Rationale:** n8n provides visual workflow debugging, built-in credential management, webhook triggers, and a large node library — without boilerplate code. LangChain and CrewAI add abstraction layers that make debugging harder without improving output quality. For a solo operator, n8n's visual execution history is more valuable than programmatic flexibility.

**Tradeoff:** Less flexibility for complex multi-agent coordination. Acceptable for current use case.

---

## ADR-002: Flat agent architecture over hierarchical
**Date:** January 2026 | **Status:** Active

**Decision:** Each agent (Gilgamesh, Da Vinci, Midas, MERLIN) is an independent n8n workflow rather than a sub-agent under a master orchestrator.

**Rationale:** Narrow agents are easier to debug, test, and extend independently. A constellation of reliable specialists outperforms one powerful general agent in production.

**Tradeoff:** No dynamic agent spawning. Each capability requires a deliberate workflow build.

---

## ADR-003: Da Vinci as sole Obsidian writer
**Date:** February 2026 | **Status:** Active

**Decision:** Only Da Vinci writes to the Obsidian vault. All other agents route documentation through Da Vinci.

**Rationale:** Concurrent WebDAV writes from multiple agents cause file conflicts and data loss. A single writer with a queue ensures consistency — the same principle as a database write lock.

**Tradeoff:** Documentation updates are async (5-minute inbox polling delay).

---

## ADR-004: Hybrid LLM routing — local + cloud
**Date:** March 2026 | **Status:** Active

**Decision:** Route simple queries to local Ollama (qwen3.5), complex tasks to Claude API (Haiku).

**Rationale:** Local inference is free but slow for large-output tasks. Cloud API is fast and reliable but costs money. Routing by task type optimises cost without sacrificing quality.

**Cost impact:** Reduced API spend from ~$58/month (Sonnet-only) to ~$5/month (hybrid).

---

## ADR-005: Claude Haiku for documentation pipeline
**Date:** May 2026 | **Status:** Active

**Decision:** Da Vinci uses claude-haiku-4-5-20251001 for documentation merging, not Sonnet.

**Rationale:** Documentation merging is a structured instruction-following task, not a reasoning task. Haiku handles it reliably at 12x lower cost. A validation guard (minimum length check + placeholder detection) prevents corrupt output from reaching GitHub.

---

## ADR-006: Proxmox LXC over Docker for most services
**Date:** June 2025 | **Status:** Active

**Decision:** Run services as Proxmox LXC containers rather than Docker containers where possible.

**Rationale:** LXC containers have lower overhead, easier resource management via Proxmox UI, and better isolation. Docker is used inside LXC only where required (n8n, Langfuse).

---

## ADR-007: ZFS mirror for Proxmox boot/data drives
**Date:** January 2026 | **Status:** Active

**Decision:** Use ZFS mirror (RAID-1) for Proxmox NVMe storage.

**Rationale:** ZFS provides data integrity verification, snapshots, and automatic corruption detection. Mirror configuration tolerates single drive failure without data loss.
