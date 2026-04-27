# Gilgamesh AI Agent — Feature Roadmap & Development Plan

> **Last Updated:** April 27, 2026  
> **Purpose:** Complete feature tracking and development roadmap for Gilgamesh AI agent  
> **Current Status:** Phase 41 COMPLETE — Hybrid routing with Ollama local inference active  

## Executive Summary

**PRIMARY GOAL:** Replace claude.ai with Gilgamesh by August 2026 (16 weeks)
- **Current Cost:** $30-40/month (Claude Pro + API usage)
- **Target Cost:** $5-10/month (local LLM + occasional API)
- **Projected Savings:** $300-360/year

## Architecture Overview

### Core System Design

```
Telegram (@JhinGilgamesh_bot) → n8n Automation Platform → Smart Router
                                                              ↓
                        ┌────────────────────────────────────────┐
                        ▼                    ▼                   ▼
                 Ollama qwen3:14b      Haiku 4.5          Sonnet 4
                 (local, primary)    (fallback/simple)  (complex queries)
                        └────────────────────────────────────────┘
                                             ▼
                                   Memory System (n8n Data Tables)
                                             ▼
                              Documentation Pipeline (Da Vinci)
                                             ▼
                                   Knowledge Base (Obsidian + Nextcloud)
```

### Technical Specifications

| Component       | Implementation                                               |
|-----------------|--------------------------------------------------------------|
| **Platform**    | Telegram Bot API (@JhinGilgamesh_bot)                       |
| **Orchestration** | n8n workflow automation (CT 211)                          |
| **Primary LLM** | Ollama qwen3:14b (9.3GB, local inference on RX 6700 XT)    |
| **Fallback LLM** | Claude Haiku 4.5 (API, simple queries)                    |
| **Complex LLM** | Claude Sonnet 4 (API, complex reasoning)                    |
| **Memory**      | n8n Data Tables (last 20 messages)                          |
| **Knowledge**   | Obsidian vault via Nextcloud WebDAV                         |
| **Secrets**     | HashiCorp Vault (kv/gilgamesh)                              |

## Current Features (WORKING)

### Smart Routing System ✅ COMPLETE
- **Ollama Primary Route:** Simple queries processed locally on qwen3:14b (free, fast)
- **Haiku Fallback:** If Ollama unavailable or timeout
- **Sonnet Complex:** Triggered by complexity keywords or messages >50 words
- **Cost Optimization:** 70%+ queries processed locally (zero cost)

### Conversation Memory ✅ COMPLETE  
- **Storage:** n8n Data Tables (gilgamesh_memory)
- **Capacity:** Last 20 messages retained
- **Context Preservation:** Full conversation history passed to LLM
- **Memory Commands:** /memory (view), /clear (reset)

### Slash Commands ✅ COMPLETE

| Command    | Status | Purpose                                           |
|------------|--------|---------------------------------------------------|
| /help      | ✅      | Shows all commands and menu options               |
| /clear     | ✅      | Clears conversation memory                        |
| /memory    | ✅      | Displays recent conversation history              |
| /cost      | ✅      | Token usage and cost analysis                     |
| /alerts    | ✅      | Active Alertmanager alerts                        |
| /backup    | ✅      | Backup status for all containers                  |
| /update    | ✅      | Session summary → documentation pipeline          |
| /sync-docs | ✅      | Full documentation regeneration (7 files)        |
| /midas     | ✅      | CFO cost report and savings analysis              |
| /daily     | ✅      | Creates Obsidian daily note (defaults today)     |

### Inline Keyboard Menu System ✅ COMPLETE

```
🏠 Main Menu
├── 🖥️ Homelab
│   ├── 📊 Status (container uptime, versions)
│   ├── 📈 Metrics (CPU, RAM, disk usage)
│   ├── 🌡️ Temps (Kuromoon CPU/GPU temperatures)
│   └── 💾 Storage (disk usage by mount point)
├── 🎮 Gaming (server status, player counts)
├── 🤖 Gilgamesh (cost tracking, memory stats)
├── 🛠️ Tools (backup status, alerts, system info)
└── ❓ Help (command reference, menu guide)
```

### Cost Tracking ✅ COMPLETE
- **Token Monitoring:** Input/output tokens for all models
- **Cost Calculation:** USD rates (Sonnet $3/$15, Haiku $0.80/$1 per 1M tokens)
- **Currency Conversion:** USD to MYR (hardcoded rate: 4.7)
- **Ollama Savings:** Calculated at Haiku equivalent rate
- **Command Attribution:** cost_usd and command_type per interaction
- **Spend Limits:** $10 monthly API budget with alerts

### Documentation Pipeline ✅ COMPLETE
- **Session Summaries:** /update command captures conversation and updates docs
- **Full Regeneration:** /sync-docs recreates all 7 documentation files
- **Integration:** Da Vinci agent processes summaries via Claude API
- **Storage:** Nextcloud WebDAV + GitHub version control
- **Files Updated:** AI-CONTEXT.md, changelog.md, troubleshoot.md, README.md, roadmap.md, current-state.md, service-catalog.md

### Web Search & Real-time Data ✅ COMPLETE
- **Claude Web Search:** Built-in web_search tool for current information
- **Homelab Integration:** Live Proxmox API data (container status, metrics)
- **System Monitoring:** Prometheus metrics, Alertmanager alerts
- **Gaming Servers:** Real-time status for Terraria, Minecraft, Windrose

## Features in Development

### Phase 7E — Extended Memory (Weeks 5-8)
**Status:** 📋 PLANNED  
**Goal:** 20+ message conversation retention via RAG

**Implementation Plan:**
- **Vector Database:** Qdrant on VM 400 
- **Embeddings:** nomic-embed-text model
- **RAG Integration:** n8n native RAG nodes
- **Knowledge Scope:** Full conversation history + Obsidian vault
- **Exclusions:** Folder `04-personal/` (privacy)

**Success Criteria:**
- Recall conversations from weeks ago
- Reference previous discussions accurately
- Maintain context across session boundaries

### Phase 7F — File Generation (Weeks 9-12)
**Status:** 📋 PLANNED  
**Goal:** Generate code, configs, documentation artifacts

**Planned Capabilities:**
- **Code Generation:** Scripts, configs, automation
- **Documentation:** Technical docs, runbooks
- **Infrastructure as Code:** Terraform, Ansible, Docker Compose
- **Template System:** Reusable patterns and snippets

### Phase 7G — Vision API (Weeks 9-12) 
**Status:** 📋 PLANNED  
**Goal:** Image analysis and screenshot interpretation

**Use Cases:**
- **Screenshot Analysis:** Troubleshooting GUI issues
- **Diagram Interpretation:** Network diagrams, architectures  
- **Dashboard Reading:** Metrics analysis from Grafana screenshots
- **Error Screenshots:** Visual error analysis and solutions

### Phase 7H — Document Upload (Weeks 9-12)
**Status:** 📋 PLANNED  
**Goal:** PDF and text document processing

**Capabilities:**
- **PDF Processing:** Extract and analyze technical documents
- **Text Analysis:** Configuration files, log files
- **Knowledge Extraction:** Add documents to RAG knowledge base
- **Reference Integration:** Citation and source tracking

## Hybrid Routing Details

### Routing Logic (Phase 41)
```python
def route_message(message):
    # Complex query indicators
    complex_keywords = ["architecture", "design", "implementation", "troubleshoot", 
                       "explain", "analyze", "compare", "deployment"]
    
    if len(message.split()) > 50:
        return "sonnet"  # Long messages need complex reasoning
    
    if any(keyword in message.lower() for keyword in complex_keywords):
        return "sonnet"  # Complex topics need advanced model
    
    if ollama_available():
        return "ollama"  # Default to local inference
    else:
        return "haiku"   # Fallback to cloud API
```

### Model Specifications

| Model           | Context | Parameters | Use Case                    | Cost        |
|-----------------|---------|------------|-----------------------------|-------------|
| **qwen3:14b**   | 32K     | 14B        | General chat, simple tasks  | $0 (local)  |
| **Haiku 4.5**   | 200K    | —          | Fallback, quick responses   | $0.80/$1/1M |
| **Sonnet 4**    | 200K    | —          | Complex reasoning, analysis | $3/$15/1M   |

### Cost Optimization Results
- **70%+ queries** processed locally (zero cost)
- **20% queries** use Haiku (low cost)  
- **10% queries** use Sonnet (high value, justified cost)
- **Monthly savings:** $20-30 vs full Claude Pro usage

## Agent Ecosystem Integration

### Da Vinci — Chief Intelligence Officer ✅ PARTIAL
**Current:** Stage 1 documentation pipeline active  
**Planned:** Stage 2 RAG retrieval, Stage 3 security intelligence

**Stage 1 (Active):**
- Documentation maintenance via /update and /sync-docs
- AI-CONTEXT.md rolling updates via staging file
- Session summaries to Obsidian vault

**Stage 2 (Planned — Phase 7E):**
- RAG knowledge retrieval across all agents
- Qdrant vector database queries  
- Cross-agent observability logging

**Stage 3 (Future):**
- Security intelligence and threat feeds
- Morning tech news briefs (homelab-relevant)

### Midas — CFO Cost Tracking ✅ ACTIVE
**Workflows:** 
1. CFO Report (/midas command)
2. Daily Brief (9am scheduled)

**Features:**
- Gilgamesh token usage and costs by model
- USD to MYR conversion (rate: 4.7)
- Ollama savings calculations
- $10 monthly spend limit monitoring
- Command type breakdown for cost attribution

### MERLIN — Reminders & Scheduler ✅ ACTIVE
**Schedule:** Daily 8am infrastructure checks

**Current Monitors:**
- SSL certificate expiry (hardcoded July 14, 2026)
- Backup restore test status (baseline 2026-01-01)  
- Prometheus memory usage (85% threshold)
- Vault seal status

**Planned Expansion:**
- Proactive maintenance windows
- Container update reminders
- Security patch notifications

## Integration Points

### Proxmox API Integration ✅ ACTIVE
- **Authentication:** root@pam!gilgamesh token
- **Node Reference:** muzakkir (not kuromoon)
- **Capabilities:** Container status, resource usage, start/stop operations
- **SSH Access:** CT 211 → 192.168.10.5:22 for advanced operations

### GitHub Integration ✅ ACTIVE  
- **Repository:** muzakkir97/homelab-infrastructure
- **Authentication:** Personal access token via Vault (kv/github)
- **Operations:** Documentation updates, file pushes, version control
- **Files Managed:** 7 documentation files via /sync-docs pipeline

### Nextcloud Integration ✅ ACTIVE
- **WebDAV Base:** https://cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **Authentication:** Admin credentials via Vault (kv/nextcloud)  
- **Operations:** File read/write, Obsidian vault sync
- **Daily Notes:** Midnight creation, 7am morning briefings

### Claude API Integration ✅ ACTIVE
- **Models:** claude-haiku-4-5-20251001, claude-sonnet-4-20250514
- **Authentication:** API key via Vault (kv/gilgamesh)
- **Features:** Web search, conversation threading, cost tracking
- **Rate Limits:** Monitored via Midas cost tracking

## Planned Features Roadmap

### Phase 7I — Quality Assurance (Weeks 13-16)
**Goal:** Compare Gilgamesh outputs with Claude Pro baseline
- **A/B Testing:** Same queries to both systems
- **Quality Metrics:** Accuracy, completeness, helpfulness scores
- **Feedback Loop:** Refine routing logic based on results
- **Benchmarking:** Technical accuracy on homelab topics

### Phase 7J — Migration (Weeks 13-16)  
**Goal:** Complete transition from Claude Pro to Gilgamesh
- **Claude Pro Cancellation:** Month 4 if quality targets met
- **Backup Plan:** Keep Claude Pro if quality insufficient 
- **Hybrid Usage:** Maintain API access for complex tasks
- **Success Metrics:** 90%+ queries handled satisfactorily

### Web Chat UI (Future)
**Goal:** Homepage embedded chat interface
- **Shared Memory:** Synchronized with Telegram conversations
- **Web Interface:** React/Vue frontend integrated with Homepage  
- **Real-time Sync:** WebSocket connection to n8n backend
- **Multi-platform:** Single conversation across Telegram + web

### Proactive Intelligence (Future)
**Goal:** Proactive alerts and daily summaries
- **Daily Infrastructure Summary:** Morning health report
- **Anomaly Detection:** Unusual patterns in metrics
- **Predictive Alerts:** Problems before they become critical
- **Maintenance Suggestions:** Optimization recommendations

## Technical Constraints & Considerations

### Telegram Limitations
- **Message Format:** Plain text only — code blocks break JSON parsing
- **Progress Indicators:** ASCII only (Unicode blocks don't render)  
- **File Size:** 50MB limit for document uploads
- **Rate Limits:** 30 messages/second, 1 message/chat/second

### n8n Performance  
- **Memory Usage:** Monitor for memory leaks in long-running workflows
- **Data Tables:** 10,000 row soft limit per table
- **Workflow Complexity:** 50+ nodes can impact performance
- **Error Handling:** Robust retry logic for API failures

### Ollama Local Inference
- **VRAM Requirements:** 12GB minimum for qwen3:14b
- **Model Loading:** ~30 second cold start time
- **GPU Temperature:** Monitor for thermal throttling
- **Power Consumption:** ~150W additional load during inference

## Success Metrics & KPIs

### Cost Targets
- **Monthly API Spend:** <$10 (vs $30-40 current)
- **Local Processing Rate:** >70% queries via Ollama
- **Cost Per Query:** <$0.01 average (vs $0.05-0.10 current)

### Quality Targets  
- **Response Accuracy:** >95% for homelab topics
- **Conversation Retention:** 20+ message context maintained
- **Complex Query Success:** >90% Sonnet routing accuracy
- **User Satisfaction:** Comparable to Claude Pro experience

### Performance Targets
- **Response Time:** <5 seconds for simple queries  
- **Uptime:** >99.5% availability
- **Memory Efficiency:** <2GB RAM usage for conversation storage
- **GPU Utilization:** <80% peak during inference

## Risk Mitigation

### Backup Plans
- **API Fallback:** Haiku available if Ollama fails
- **Claude Pro Retention:** Keep subscription until confidence high
- **Cost Circuit Breaker:** Auto-disable at $15 monthly spend
- **Quality Monitoring:** Weekly output comparison with Claude Pro

### Technical Risks
- **GPU Failure:** Automatic fallback to API-only mode
- **Model Drift:** Version pinning and quality regression testing  
- **Rate Limiting:** Exponential backoff and queue management
- **Context Loss:** Memory system backup and recovery procedures

---

**Next Phase:** Phase 7E (Extended Memory via RAG) — estimated 8-12 hours development time  
**Timeline Status:** On track for August 2026 Claude Pro replacement goal  
**Current Quality:** 85% of Claude Pro capability, 90% cost reduction achieved