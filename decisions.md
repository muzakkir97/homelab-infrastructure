# Decisions Log

Architectural decisions, strategy calls, and naming choices made during homelab development.

---

### 2026-07-05 - Bridge mode activation on HG8145B7N to eliminate double NAT
**Decision:** Called TIME support and activated true bridge mode on the Huawei HG8145B7N ISP router, permanently eliminating the double-NAT topology that was blocking UDP port forwarding for gaming servers.
**Why:** Double NAT is a well-documented root cause of exactly this class of UDP forwarding problem. TIME explicitly supports bridge mode on request for this router model. Bridge mode permanently eliminates an entire category of future networking friction (not just Enshrouded, but any future service needing inbound access) rather than working around it repeatedly via reboots and port mapping tweaks. The migration was underway anyway, so deferring it would have wasted the disruption already caused.
**Alternatives considered:** Continue troubleshooting double-NAT indirectly (rejected — infinite game of port mapping tweaks without fixing root cause), wait for a more convenient maintenance window (rejected — disruption already underway), use Steam relay as permanent solution (rejected — doesn't generalize to non-Steam services like Minecraft/Terraria).

### 2026-07-05 - Deploy AX1800 access point immediately post-bridge-mode
**Decision:** Deployed the previously-purchased TP-Link EAP610/AX1800 access point on the same night as the bridge mode migration, rather than deferring it to a future session, once it became clear the ISP router's own Wi-Fi was permanently dead post-bridging and roommates/friends needed working Wi-Fi immediately.
**Why:** Bridge mode disables all local services on the ISP router (DHCP, routing, Wi-Fi), so the household lost internet access on their existing Wi-Fi immediately. Deploying the AX1800 same night restored connectivity for other occupants and was logistically simpler than coordinating a separate maintenance window later.
**Alternatives considered:** Ask roommates to wait for a future session (rejected — leaves them without internet), use cellular hotspot as temporary bridge (rejected — insufficient bandwidth for household use), revert bridge mode temporarily (rejected — defeats the entire point of the migration).

### 2026-07-05 - TL-SG108E ports 7-8 VLAN assignment corrected from legacy default to VLAN20_MAIN
**Decision:** Corrected the PVID and untagged VLAN membership of switch ports 7 and 8 from the legacy default VLAN 1 (never migrated when the VLAN architecture was originally built) to VLAN20_MAIN (PVID 20, untagged).
**Why:** Port 7 was blocking the AX1800 access point from receiving any real IP addresses from pfSense's DHCP server on VLAN20, causing clients to receive APIPA addresses (169.254.x.x) instead. Port 8 had the identical misconfiguration and was proactively fixed to prevent the same issue when friends attempted to use it for wired connections. This was a known gap already flagged in ROADMAP.md Phase 26, but the misconfiguration turned out to be the actual blocking issue for AP deployment tonight.
**Alternatives considered:** Leave ports on VLAN 1 (rejected — breaks any VLAN20 device plugged into those ports), migrate the switch's management IP instead (rejected — larger scope, deferred to Phase 26 as a separate item), use a different switch port for the AP (rejected — ports 7-8 were already going to be used, problem fixed rather than worked around).

### 2026-07-05 - Enshrouded external connectivity fix deferred post-bridge-mode pending DNS and retest
**Decision:** Deferred retesting Enshrouded's external UDP connectivity (the entire point of the bridge mode migration) and fixing DNS records until the next session, despite both being immediate action items. Reasoning: the bridge mode migration and AP deployment had consumed all available troubleshooting time; DNS updates and a proper retest (Termux UDP test + pfSense packet capture) require a fresh maintenance window to avoid compounding issues from fatigue.
**Why:** Attempting DNS updates and UDP retests at the end of a 4+ hour session of network changes increases the risk of copy-paste errors (IP addresses) and misinterpretation of packet capture results. Better to document the exact action items and execute them cleanly in the next session when the changes have stabilized and the network is fully at rest.
**Alternatives considered:** Complete DNS updates before session end (rejected — fatigue risk, could miss re-checking other DNS records), skip DNS updates altogether (rejected — external Enshrouded connectivity will not work until DNS is fixed), retest immediately (rejected — same fatigue risk, no time to interpret results carefully).

### 2026-07-05 - Use switch's Easy Smart Configuration Utility for port VLAN management
**Decision:** Used the TP-Link TL-SG108E's Easy Smart Configuration Utility (web-based, accessed via temporary static IP on a personal PC) to modify port VLAN assignments, rather than attempting to permanently renumber the switch's management IP (which remains on legacy 192.168.1.0/24) in the same session.
**Why:** The switch's management IP (192.168.1.20) is on an unrelated legacy subnet; accessing it for a permanent renumber would have required a separate network configuration step that was out-of-scope for an emergency AP deployment. The web utility works fine with a temporary static IP on the connecting device, and the switch IP renumber remains a Phase 26 cleanup item rather than a blocker for tonight's work.
**Alternatives considered:** Permanently renumber switch IP to VLAN10_MGMT (rejected — out-of-scope for emergency AP fix, deferred to Phase 26), use SSH/CLI access (rejected — complexity unnecessary when web UI works), skip port VLAN fixes entirely (rejected — that was the actual blocking issue for AP functionality).

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
**Why:** Documentation drift compounds over sessions. Hardware and folder lists are critical infrastructure facts that must stay synchronized. Previous