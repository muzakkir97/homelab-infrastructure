# 🤖 Gilgamesh AI Agent — Feature Roadmap & Architecture

> **Last Updated:** May 9, 2026  
> **Primary Goal:** Replace claude.ai with Gilgamesh by August 2026 (16 weeks)  
> **Current Status:** Phase 41 complete, hybrid routing active, 16-week evolution plan in progress

---

## 🎯 Mission Statement

Transform Gilgamesh from a basic Telegram bot into a comprehensive AI assistant that:
- **Replaces Claude Pro subscription** ($30-40/month → $5-10/month)
- **Provides 20+ message conversation retention** via extended memory
- **Offers multi-modal capabilities** (text, vision, file generation)
- **Maintains homelab operational intelligence** through deep system integration

---

## 🏗️ Current Architecture

```
Telegram Bot (@JhinGilgamesh_bot)
         │
         ▼
n8n Telegram Agent Workflow (CT 211)
         │
         ▼
Smart Routing Logic (Phase 41)
    ├─ Ollama qwen3:14b (Primary - Local, Free)
    ├─ Claude Haiku 4.5 (Fallback - Fast, Cheap)
    └─ Claude Sonnet 4 (Complex - Powerful, Expensive)
         │
         ▼
Memory System (n8n Data Tables)
    ├─ gilgamesh_memory (Last 20 messages)
    ├─ gilgamesh_costs (Token usage tracking)
    ├─ gilgamesh_session_state (Mode switching)
    └─ health_* tables (Food, BP, medication logs)
         │
         ▼
Documentation Pipeline (/update, /sync-docs)
         │
         ▼
Da Vinci → Nextcloud → GitHub
```

---

## 🎖️ Current Feature Status

### ✅ Core Features (ACTIVE)

#### Intelligent Routing (Phase 41)
- **Primary:** Ollama qwen3:14b on VM 400 (RX 6700 XT, 12GB VRAM)
- **Fallback:** Claude Haiku 4.5 (if Ollama unavailable)
- **Complex:** Claude Sonnet 4 (50+ word queries, complexity keywords)
- **Cost Savings:** ~80% queries via free local inference

#### Memory System
- **Conversation History:** Last 20 messages via n8n Data Tables
- **Context Retention:** Persistent across Telegram sessions
- **Memory Commands:** `/memory` (view), `/clear` (reset)

#### Homelab Integration
- **System Status:** Real-time container health, metrics, temperatures
- **Alert Management:** Prometheus/Alertmanager integration
- **Backup Monitoring:** Automated backup status reporting
- **Cost Tracking:** Token usage across all AI models

#### Documentation Pipeline
- **Session Summaries:** `/update` merges conversations into docs
- **Full Regeneration:** `/sync-docs` rebuilds all documentation
- **Version Control:** Automatic GitHub commits via API
- **Knowledge Base:** Obsidian integration via Nextcloud WebDAV

#### Health Tracking (Phase 22.8B)
- **Food Logging:** Interactive button prompts, MYR expense tracking
- **Blood Pressure:** Systolic/diastolic readings with timestamps
- **Medication:** Dosage tracking and adherence monitoring
- **Daily Summaries:** Automatic Obsidian daily note generation

#### Interactive Interface
- **Inline Keyboards:** Full menu system with nested submenus
- **Slash Commands:** 11 direct commands for common actions
- **Web Search:** Real-time information via Claude's web_search tool
- **Mode Switching:** Context-aware interactions (health, admin, chat)

### ⚡ Partially Active Features

#### Da Vinci Integration (Stage 1 Complete)
- **Status:** Documentation pipeline active, RAG system planned
- **Current:** AI-CONTEXT.md maintenance via Claude API
- **Planned:** Qdrant vector database for knowledge retrieval

---

## 🗺️ 16-Week Evolution Roadmap

### Phase 1: Foundation (Weeks 1-4) ✅ COMPLETE
- **Week 1:** ✅ Ollama + ROCm on Kuromoon RX 6700 XT (Phase 38)
- **Week 2:** ✅ Obsidian Knowledge Base setup (Phase 22)
- **Week 3:** ✅ Open WebUI deployment (Phase 39)
- **Week 4:** ✅ Hybrid routing implementation (Phase 41)

### Phase 2: Intelligence (Weeks 5-8) 🟡 IN PROGRESS
- **Week 5:** 📋 Phase 7E — Extended Memory (RAG system, 20+ messages)
- **Week 6:** 📋 Da Vinci Stage 2 — Qdrant vector database integration
- **Week 7:** 📋 Memory optimization — conversation threading
- **Week 8:** 📋 Context window management — intelligent pruning

### Phase 3: Capabilities (Weeks 9-12) 📋 PLANNED
- **Week 9:** 📋 Phase 7F — File Generation (code, configs, documentation)
- **Week 10:** 📋 Phase 7G — Vision API (Llama 3.2 Vision 11B on VM 400)
- **Week 11:** 📋 Phase 7H — Document Upload (PDF/text processing)
- **Week 12:** 📋 Multi-modal integration testing

### Phase 4: Refinement (Weeks 13-16) 📋 PLANNED
- **Week 13:** 📋 Phase 7I — Quality Assurance (output comparison with Claude Pro)
- **Week 14:** 📋 Performance optimization and cost monitoring
- **Week 15:** 📋 Phase 7J — Full migration testing
- **Week 16:** 📋 Claude Pro cancellation and final validation

---

## 💰 Cost Analysis & Savings

### Current Spending (Pre-Gilgamesh)
| Component      | Monthly Cost | Annual Cost |
|----------------|--------------|-------------|
| Claude Pro     | $20          | $240        |
| Claude API     | $10-20       | $120-240    |
| **Total**      | **$30-40**   | **$360-480** |

### Target Spending (Post-Gilgamesh)
| Component           | Monthly Cost | Annual Cost | Notes                    |
|---------------------|--------------|-------------|--------------------------|
| Claude API (Hybrid)| $5-10        | $60-120     | 20% of current usage     |
| Ollama (Local)      | $0           | $0          | Free inference on RX 6700 XT |
| **Total**           | **$5-10**    | **$60-120** | **75-83% cost reduction** |

### Projected Annual Savings: **$300-360**

---

## 🎮 Slash Commands Reference

| Command      | Purpose                    | Example Output                           |
|--------------|----------------------------|------------------------------------------|
| `/help`      | Command reference         | Full menu and command documentation       |
| `/clear`     | Reset conversation memory | "Memory cleared. Starting fresh!"        |
| `/memory`    | View conversation history | Last 20 messages with timestamps         |
| `/cost`      | Token usage and expenses  | Model breakdown, MYR costs, savings       |
| `/alerts`    | Active system alerts      | Prometheus alert summary                 |
| `/backup`    | Backup status report      | Last backup times for all containers     |
| `/update`    | Session documentation     | Merges conversation into 3 docs          |
| `/sync-docs` | Full doc regeneration     | Rebuilds all 7 documentation files      |
| `/midas`     | CFO cost analysis        | Detailed spending report with trends     |
| `/daily`     | Create daily note        | Generates Obsidian daily note (today)    |

---

## 🎛️ Inline Keyboard Menu System

### Main Menu Structure
```
┌─ 🏠 Homelab ─┐  ┌─ 🎮 Gaming ─┐  ┌─ 🤖 Gilgamesh ─┐
│   Status ✅   │  │   Status ✅  │  │   Memory ✅     │
│   Metrics ✅  │  │   Players ✅ │  │   Cost ✅       │
│   Temps ⚠️   │  │   Restart ✅ │  │   Sync Docs ✅  │
│   Storage ✅  │  │   Logs ✅    │  │   Clear ✅      │
│   Alerts ✅   │  └─────────────┘  └─────────────────┘
│   Backup ✅   │
└───────────────┘

┌─ ⚕️ Health ─┐  ┌─ 🛠️ Tools ─┐   ┌─ ❓ Help ─┐
│  Food Log ✅ │  │  Weather ✅ │   │  Commands ✅ │
│  BP Log ✅   │  │  QR Code ✅ │   │  Features ✅ │
│  Med Log ✅  │  │  UUID ✅    │   │  Keyboard ✅ │
│  Summary ✅  │  │  Hash ✅    │   └─────────────┘
└─────────────┘  └─────────────┘
```

### Menu Status Legend
- ✅ **Working:** Full functionality implemented
- ⚠️ **Partial:** Known issues or limited functionality  
- 📋 **Planned:** Designed but not yet implemented

### Known Issues
- **Homelab → Temps:** SSH command returns no output (pre-existing bug)

---

## 🧠 Memory & Data Architecture

### n8n Data Tables Schema

#### gilgamesh_memory (Conversation History)
```sql
CREATE TABLE gilgamesh_memory (
    id INTEGER PRIMARY KEY,
    timestamp TEXT,
    user TEXT,
    message TEXT,
    response TEXT
);
```

#### gilgamesh_costs (Token Usage Tracking)
```sql
CREATE TABLE gilgamesh_costs (
    id INTEGER PRIMARY KEY,
    timestamp TEXT,
    model TEXT,
    input_tokens INTEGER,
    output_tokens INTEGER,
    cost_usd REAL,
    command_type TEXT
);
```

#### Health Tracking Tables
```sql
-- Food and expense logging
CREATE TABLE health_food_log (
    id INTEGER PRIMARY KEY,
    logged_at TEXT,
    food_item TEXT,
    notes TEXT,
    chat_id INTEGER
);

-- Blood pressure monitoring
CREATE TABLE health_bp_log (
    id INTEGER PRIMARY KEY,
    logged_at TEXT,
    systolic INTEGER,
    diastolic INTEGER,
    notes TEXT,
    chat_id INTEGER
);

-- Medication adherence
CREATE TABLE health_med_log (
    id INTEGER PRIMARY KEY,
    logged_at TEXT,
    medication TEXT,
    dosage TEXT,
    notes TEXT,
    chat_id INTEGER
);

-- Session state management
CREATE TABLE gilgamesh_session_state (
    chat_id INTEGER PRIMARY KEY,
    mode TEXT,
    updated_at TEXT
);
```

---

## 🎯 Planned Features (Phases 7E-7J)

### Phase 7E: Extended Memory System
**Timeline:** Week 5 (Phase 2)
**Components:**
- Qdrant vector database integration
- Conversation threading and context linkage
- 20+ message retention with semantic search
- RAG-powered knowledge retrieval from Obsidian vault

### Phase 7F: File Generation
**Timeline:** Week 9 (Phase 3)
**Capabilities:**
- Code generation (Python, JavaScript, YAML, JSON)
- Configuration files (Docker Compose, Prometheus, etc.)
- Documentation generation (Markdown, technical specs)
- Infrastructure-as-Code (Terraform, Ansible)

### Phase 7G: Vision API Integration
**Timeline:** Week 10 (Phase 3)
**Implementation:**
- Llama 3.2 Vision 11B on VM 400 (RX 6700 XT)
- Alternative: Qwen2.5-VL-7B
- Screenshot analysis and system diagram interpretation
- Hardware photo documentation and issue diagnosis

### Phase 7H: Document Processing
**Timeline:** Week 11 (Phase 3)
**Features:**
- PDF text extraction and analysis
- Technical documentation parsing
- Log file analysis and pattern recognition
- Configuration file validation

### Phase 7I: Quality Assurance
**Timeline:** Week 13 (Phase 4)
**Methodology:**
- Side-by-side output comparison with Claude Pro
- Response quality metrics and scoring
- Performance benchmarking across model types
- User satisfaction testing

### Phase 7J: Full Migration
**Timeline:** Week 16 (Phase 4)
**Deliverables:**
- Complete Claude Pro replacement validation
- Cost monitoring and optimization
- Backup integration with Claude API (failsafe)
- Documentation and user guide completion

---

## 🔧 Technical Implementation

### Smart Routing Logic
```javascript
// Complexity assessment triggers
const complexityTriggers = [
    'analyze', 'compare', 'explain in detail', 'troubleshoot',
    'debug', 'design', 'architecture', 'strategy'
];

const messageLength = inputMessage.length;
const hasComplexityKeywords = complexityTriggers.some(trigger => 
    inputMessage.toLowerCase().includes(trigger)
);

if (messageLength > 50 || hasComplexityKeywords) {
    route = 'sonnet';  // High-quality, expensive
} else if (ollamaStatus === 'available') {
    route = 'ollama';  // Local, free
} else {
    route = 'haiku';   // Fast, cheap fallback
}
```

### Memory Management
- **Storage:** n8n Data Tables (SQLite backend)
- **Capacity:** 20 most recent message pairs
- **Pruning:** FIFO with timestamp-based cleanup
- **Context:** Full conversation history included in each API call

### Cost Tracking
- **Token Capture:** Input/output tokens from all model APIs
- **Rate Calculation:** Model-specific pricing (Sonnet: $3/$15, Haiku: $0.80/$1 per 1M tokens)
- **Currency Conversion:** USD → MYR at 4.7 exchange rate
- **Budget Alerts:** $10 monthly API spend limit monitoring

---

## 🤝 Agent Ecosystem Integration

### Multi-Agent Architecture
Gilgamesh operates as the primary interface within the Fate/Grand Order themed agent ecosystem:

#### Active Integrations
- **Da Vinci (Intelligence Officer):** Documentation pipeline and knowledge management
- **Midas (CFO):** Cost tracking and financial optimization reporting
- **MERLIN (Scheduler):** Reminder system and proactive maintenance alerts

#### Planned Integrations
- **Guardian (Security):** Threat monitoring and alert translation
- **EMIYA (Infrastructure):** System management and automation approval gateway
- **Nexus (Automation):** Cross-platform workflow orchestration

---

## 🎪 Success Metrics

### Primary Objectives (August 2026)
- **Cost Reduction:** 75%+ savings vs Claude Pro subscription
- **Conversation Depth:** 20+ message context retention
- **Response Quality:** Comparable to Claude Pro for 80%+ queries
- **System Integration:** Full homelab operational capabilities

### Key Performance Indicators
| Metric                    | Current Status | Target (August 2026) |
|---------------------------|----------------|----------------------|
| Monthly API Cost          | $10-20         | $5-10               |
| Local Inference Rate      | 80%            | 85%                 |
| Context Retention         | 20 messages    | 50+ messages        |
| Response Time (Local)     | 2-5 seconds    | 1-3 seconds         |
| Response Time (API)       | 3-8 seconds    | 2-5 seconds         |
| Multi-modal Support       | Text only      | Text + Vision       |
| File Generation           | None           | Full capability     |

### Backup Plan
If quality targets aren't met by August 2026:
- **Hybrid Approach:** Keep Claude Pro, reduce API usage 50%
- **Estimated Savings:** Still $15-20/month ($180-240/year)
- **Continued Development:** Extend timeline to December 2026

---

## 🛠️ Development Environment

### Infrastructure Requirements
- **Primary Host:** VM 400 (ollama-gpu) on Kuromoon Proxmox
- **GPU:** RX 6700 XT 12GB (PCIe passthrough, ROCm support)
- **Models:** qwen3:14b (9.3GB), llama3.2:3b (secondary)
- **Vision Model:** Llama 3.2 Vision 11B (planned)
- **Vector DB:** Qdrant (planned for Phase 7E)
- **Orchestration:** n8n workflows on CT 211

### API Integrations
- **Telegram Bot API:** @JhinGilgamesh_bot webhook
- **Claude API:** Haiku 4.5 + Sonnet 4 via Anthropic
- **Proxmox API:** System status and container management
- **GitHub API:** Documentation pipeline automation
- **Nextcloud WebDAV:** Knowledge base and file storage

---

*This roadmap represents a comprehensive 16-week transformation plan to build an enterprise-grade AI assistant that combines local inference, cloud APIs, and deep homelab integration while achieving significant cost savings and enhanced capabilities.*