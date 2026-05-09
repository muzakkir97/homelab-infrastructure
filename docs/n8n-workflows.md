# 🔄 n8n Workflow Automation — Complete Documentation

> **Last Updated:** May 9, 2026  
> **Total Active Workflows:** 12  
> **Platform:** n8n CE on CT 211 (192.168.30.211)  
> **Purpose:** Complete automation layer for homelab operations and AI agent orchestration

---

## 📊 Workflow Overview

| Workflow Name                      | Nodes | Trigger Type | Status      | Purpose                                    |
|------------------------------------|-------|--------------|-------------|--------------------------------------------|
| Telegram Agent                     | 15+   | Telegram     | ✅ Active    | Main Gilgamesh AI bot with hybrid routing  |
| Documentation Pipeline — Update    | 7     | Webhook      | ✅ Active    | Session summaries → 3 files               |
| Documentation Pipeline — Sync Docs | 7     | Webhook      | ✅ Active    | Full documentation regeneration → 7 files |
| Da Vinci Documentation Pipeline    | 11    | Webhook      | ✅ Active    | Raw summaries → formatted documentation   |
| Midas — CFO Report                 | 6     | Webhook      | ✅ Active    | /midas command cost analysis              |
| Midas — Daily Brief                | 4     | Schedule     | ✅ Active    | 9am daily cost summary to Telegram       |
| MERLIN — Reminders                 | 8     | Schedule     | ✅ Active    | 8am daily infrastructure checks           |
| Daily Note Creator                 | 4     | Schedule     | ✅ Active    | Midnight Obsidian daily note creation     |
| Morning Briefing                   | 5     | Schedule     | ✅ Active    | 7am daily summary to Telegram            |
| Service Down Alert                 | 3     | Webhook      | ✅ Active    | Alertmanager → Telegram/Discord           |
| Update Nextcloud File              | 5     | Webhook      | 📋 Legacy    | File update via WebDAV (unpublished)     |
| Push to GitHub                     | 4     | Webhook      | 📋 Legacy    | Git commits via API (unpublished)        |

---

## 🤖 Core AI Agent Workflows

### Telegram Agent (Primary Workflow)
**Purpose:** Main Gilgamesh AI assistant with comprehensive features  
**Trigger:** Telegram webhook (`@JhinGilgamesh_bot`)  
**Node Count:** 15+ (dynamic, includes subflows)

#### Architecture Flow
```
Telegram Trigger
    │
    ▼
Message Type Router
├─ Text Message → Smart Routing Logic
├─ Callback Query → Menu Handler  
└─ Command → Command Processor
    │
    ▼
Smart AI Routing (Phase 41)
├─ Ollama qwen3:14b (Primary - Local)
├─ Claude Haiku 4.5 (Fallback)
└─ Claude Sonnet 4 (Complex queries)
    │
    ▼
Response Processing
├─ Memory Storage (Data Tables)
├─ Cost Tracking (Token logging)
└─ Context Management
    │
    ▼
Telegram Response
├─ Text Reply
├─ Inline Keyboard
└─ Error Handling
```

#### Key Features
- **Hybrid AI Routing:** Automatic model selection based on complexity
- **Memory Management:** 20-message conversation history
- **Health Tracking:** Interactive food/BP/medication logging (Phase 22.8B)
- **System Integration:** Real-time homelab status and metrics
- **Menu System:** Complete inline keyboard interface
- **Slash Commands:** 11 direct commands for power users

#### Technical Notes
- **JSON Parsing Constraint:** Messages must be plain text only (no code blocks/backticks)
- **Callback Handling:** Single webhook handles both messages and callback queries
- **Memory Storage:** n8n Data Tables with automatic pruning
- **Error Recovery:** Graceful fallback between AI models

---

## 📄 Documentation Workflows

### Documentation Pipeline — Update
**Purpose:** Session summary integration into existing documentation  
**Trigger:** Webhook (`doc-update`)  
**Node Count:** 7

#### Flow Architecture
```
Webhook Trigger (/update command)
    │
    ▼
Session Summary Input
    │
    ▼
Claude API Call
├─ Model: claude-sonnet-4
├─ Temperature: 0.1
└─ max_tokens: 32000
    │
    ▼
Document Updates (Parallel)
├─ AI-CONTEXT.md
├─ changelog.md  
└─ troubleshoot.md
    │
    ▼
File Storage (Parallel)
├─ Nextcloud WebDAV
└─ GitHub API Push
```

#### Processing Logic
- **Merge Strategy:** Intelligent integration of session notes into existing docs
- **File Targets:** 3 core documentation files
- **Version Control:** Automatic GitHub commits with descriptive messages
- **Backup:** Nextcloud WebDAV storage for redundancy

### Documentation Pipeline — Sync Docs
**Purpose:** Complete documentation regeneration from AI-CONTEXT.md  
**Trigger:** Webhook (`doc-sync`)  
**Node Count:** 7

#### Extended Coverage
- **Update Files:** AI-CONTEXT.md, changelog.md, troubleshoot.md
- **Sync-Only Files:** README.md, roadmap.md, current-state.md, service-catalog.md
- **Total Output:** 7 documentation files
- **Use Case:** Deployment sessions requiring comprehensive doc refresh

### Da Vinci Documentation Pipeline  
**Purpose:** Asynchronous processing of raw documentation summaries  
**Trigger:** Webhook (`da-vinci`)  
**Node Count:** 11

#### Workflow Design
```
Da Vinci Webhook
    │
    ▼
Staging File Processing
├─ Fetch AI-CONTEXT-staging.md
├─ Parse raw summaries
└─ Extract session data
    │
    ▼
Claude API Intelligence
├─ Model: claude-sonnet-4
├─ max_tokens: 32000
└─ Grounding: Current doc context
    │
    ▼
Formatted Output Generation
├─ AI-CONTEXT.md (primary)
├─ changelog.md
└─ troubleshoot.md
    │
    ▼
Multi-Platform Distribution
├─ Nextcloud WebDAV
└─ GitHub API
```

#### Technical Implementation Notes
- **Direct Node References:** Use `$('Extract Response').first().json.updatedDoc` instead of `$json`
- **Token Limit:** 32,000 max tokens to prevent content truncation
- **Grounding Issue:** Pending fix to fetch current AI-CONTEXT.md before API call
- **Pronouns:** Da Vinci uses she/her pronouns throughout

---

## 💰 Financial Management Workflows

### Midas — CFO Report
**Purpose:** On-demand cost analysis and financial reporting  
**Trigger:** Webhook (`midas-report`)  
**Node Count:** 6

#### Data Sources
- **Token Usage:** gilgamesh_costs table aggregation
- **Model Breakdown:** Sonnet, Haiku, Ollama usage statistics
- **Currency Conversion:** USD → MYR at 4.7 exchange rate
- **Savings Calculation:** Ollama usage valued at Haiku equivalent rates

#### Report Components
```
Cost Analysis Dashboard
├─ Current Month Spending (MYR)
├─ Model Usage Breakdown (%)
├─ Token Consumption (Input/Output)
├─ Ollama Savings Projection
├─ Budget Status ($10 USD limit)
└─ Trend Analysis (7-day average)
```

### Midas — Daily Brief
**Purpose:** Automated morning financial summary  
**Trigger:** Schedule (9:00 AM daily)  
**Node Count:** 4

#### Brief Format
- **Yesterday's Spending:** API costs in MYR
- **Model Distribution:** Usage percentages
- **Monthly Progress:** Budget burn rate
- **Savings Achieved:** Local inference benefits

---

## ⏰ Scheduled Automation Workflows

### MERLIN — Reminders
**Purpose:** Proactive infrastructure monitoring and alerts  
**Trigger:** Schedule (8:00 AM daily)  
**Node Count:** 8

#### Check Categories
```
Infrastructure Health Monitoring
├─ SSL Certificate Expiry (July 14, 2026 hardcoded)
├─ Backup Restore Testing (baseline: 2026-01-01)
├─ Prometheus Memory Usage (85% threshold)
├─ Vault Seal Status (auto-unseal check)
├─ Service Availability (critical containers)
└─ Storage Capacity (disk usage alerts)
```

#### Alert Delivery
- **Platform:** Telegram to homelab admin
- **Severity Levels:** Info, Warning, Critical
- **Escalation:** Discord webhook for critical issues

### Daily Note Creator
**Purpose:** Automatic Obsidian daily note generation  
**Trigger:** Schedule (00:00 midnight)  
**Node Count:** 4

#### Note Template
```markdown
# {{date}}

## Daily Agenda
- [ ] Morning review
- [ ] Health tracking
- [ ] Project progress

## Homelab Status
- Containers: All running
- Alerts: None active
- Backups: Completed

## Personal Log
<!-- Health tracking and notes -->

## Learning & Progress
<!-- Career development notes -->

---
*Auto-generated by n8n Daily Note Creator*
```

### Morning Briefing
**Purpose:** Daily summary delivery to Telegram  
**Trigger:** Schedule (7:00 AM)  
**Node Count:** 5

#### Briefing Content
- **Weather:** Local conditions and forecast
- **Infrastructure:** Overnight alerts and status
- **Schedule:** Today's planned activities
- **Reminders:** Important tasks and deadlines
- **Health:** Previous day's tracking summary

---

## 🚨 Monitoring & Alert Workflows

### Service Down Alert
**Purpose:** Real-time infrastructure failure notifications  
**Trigger:** Webhook from Alertmanager (CT 205)  
**Node Count:** 3

#### Alert Processing Flow
```
Alertmanager Webhook
    │
    ▼
Alert Classification
├─ Critical → Telegram + Discord
├─ Warning → Discord only
└─ Info → Log only
    │
    ▼
Message Formatting
├─ Service identification
├─ Severity assessment  
├─ Timestamp conversion
└─ Action recommendations
    │
    ▼
Multi-Channel Delivery
├─ Telegram: @JhinGilgamesh_bot
└─ Discord: #alerts channel
```

#### Alert Types Monitored
- **Container Down:** LXC/VM failure states
- **High Resource Usage:** CPU, Memory, Disk > 85%
- **Network Issues:** Interface down, high latency
- **Storage Problems:** Disk errors, backup failures
- **Security Events:** Failed authentication, intrusion attempts

---

## 🗄️ Legacy Workflows (Unpublished)

### Update Nextcloud File
**Status:** Unpublished (superseded by Documentation Pipeline)  
**Node Count:** 5  
**Purpose:** Direct file updates via WebDAV API

#### Deprecation Reason
- **Replaced By:** Da Vinci Documentation Pipeline
- **Issues:** Limited error handling, no version control integration
- **Migration Path:** Functionality absorbed into modern documentation workflows

### Push to GitHub
**Status:** Unpublished (superseded by Documentation Pipeline)  
**Node Count:** 4  
**Purpose:** Git commits via GitHub API

#### Deprecation Reason
- **Replaced By:** Integrated GitHub pushes in doc pipelines
- **Issues:** Isolated from documentation generation
- **Migration Path:** Git operations now coupled with document updates

---

## 🔧 Technical Architecture

### n8n Infrastructure Details
| Component        | Configuration                                    |
|------------------|--------------------------------------------------|
| **Container**    | CT 211, 192.168.30.211, Ubuntu 22.04 LXC       |
| **n8n Version**  | Community Edition (latest)                       |
| **Node.js**      | v18 LTS                                          |
| **Database**     | SQLite (Data Tables)                             |
| **Storage**      | vmpool-fast (NVMe SSD)                           |
| **Memory**       | 4GB allocated, 2GB typical usage                |

### Community Nodes Installed
| Node Package                      | Version | Purpose                           |
|-----------------------------------|---------|-----------------------------------|
| @mendable/n8n-nodes-firecrawl     | 2.1.1   | Web scraping and content extraction|
| n8n-nodes-puppeteer              | 1.5.0   | Browser automation and screenshots |
| n8n-nodes-tesseractjs            | 1.5.1   | OCR and image text extraction     |

### Data Tables Schema
```sql
-- Conversation memory (20 messages)
CREATE TABLE gilgamesh_memory (
    id INTEGER PRIMARY KEY,
    timestamp TEXT,
    user TEXT,
    message TEXT,
    response TEXT
);

-- Cost tracking and analysis
CREATE TABLE gilgamesh_costs (
    id INTEGER PRIMARY KEY,
    timestamp TEXT,
    model TEXT,
    input_tokens INTEGER,
    output_tokens INTEGER,
    cost_usd REAL,
    command_type TEXT
);

-- Health tracking tables
CREATE TABLE health_food_log (
    id INTEGER PRIMARY KEY,
    logged_at TEXT,
    food_item TEXT,
    notes TEXT,
    chat_id INTEGER
);

CREATE TABLE health_bp_log (
    id INTEGER PRIMARY KEY,
    logged_at TEXT,
    systolic INTEGER,
    diastolic INTEGER,  
    notes TEXT,
    chat_id INTEGER
);

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

## 🔐 Credential Dependencies

### External API Integration
| Service      | Credential Type    | Stored Location | Usage                          |
|--------------|-------------------|-----------------|--------------------------------|
| Telegram     | Bot Token         | n8n Credentials | @JhinGilgamesh_bot API         |
| Claude API   | API Key           | Vault kv/gilgamesh | Haiku 4.5 + Sonnet 4 models   |
| GitHub       | Personal Token    | Vault kv/github | Repository commits and updates |
| Nextcloud    | WebDAV Auth       | Vault kv/nextcloud | File storage and Obsidian sync |
| Proxmox      | API Token         | Vault kv/proxmox | Container management and stats |
| Cloudflare   | API Key           | Vault kv/cloudflare | DNS and tunnel management     |
| Alertmanager | Webhook URLs      | n8n Credentials | Alert routing configuration    |

### Secret Management Integration
- **Primary Store:** HashiCorp Vault (CT 213)
- **n8n Access:** Manual credential entry (Phase 27 will automate)
- **Rotation Policy:** Quarterly for API keys, annually for tokens
- **Backup Strategy:** Vault snapshots to kinmoon-nfs

---

## 🐛 Known Issues & Lessons Learned

### Data Tables Limitations
**Issue:** Schema caching bug requires table recreation for changes  
**Resolution:** Delete and recreate entire table when modifying structure  
**Impact:** Temporary data loss during schema migrations

### Telegram Integration Challenges
**Issue:** Single webhook constraint for multiple update types  
**Resolution:** Handle both message and callback_query in one trigger  
**Lesson:** Design for Telegram's architectural limitations from the start

### API Model Evolution
**Issue:** Claude model names change without notice (404 errors)  
**Resolution:** Use exact model IDs: `claude-haiku-4-5-20251001`  
**Monitoring:** Version tracking in cost analysis reports

### Memory Management Complexity
**Issue:** Expression scoping problems with `$json` references  
**Resolution:** Use Code nodes with `this.helpers.httpRequest`  
**Best Practice:** Explicit node referencing over implicit JSON access

### File Processing Gotchas
**Issue:** `input.first()/input.last()` unreliable after Merge nodes  
**Resolution:** Direct node references: `$('Extract Response').first().json`  
**Pattern:** Always reference specific nodes in complex workflows

### Health Tracking Time Zones
**Issue:** UTC timestamps in Malaysian operation environment  
**Resolution:** Add 8-hour offset (8 * 60 * 60 * 1000ms) in Code nodes  
**Standard:** All user-facing times display in MYT (UTC+8)

---

## 📈 Performance Metrics

### Workflow Execution Statistics (7-day average)
| Workflow                    | Executions/Day | Avg Duration | Success Rate |
|-----------------------------|----------------|--------------|--------------|
| Telegram Agent              | 45-60          | 3.2s         | 98.5%        |
| Documentation Pipeline      | 2-4            | 12.1s        | 99.2%        |
| Midas Daily Brief           | 1              | 2.8s         | 100%         |
| MERLIN Reminders           | 1              | 4.5s         | 97.8%        |
| Daily Note Creator          | 1              | 1.2s         | 100%         |
| Morning Briefing            | 1              | 3.1s         | 99.1%        |

### Resource Utilization
- **Peak CPU:** 85% during documentation generation
- **Average Memory:** 2.1GB / 4GB allocated
- **Storage Growth:** ~50MB per month (logs and data)
- **Network:** 1-2GB monthly API calls

---

## 🚀 Future Roadmap

### Planned Workflow Enhancements

#### Phase 7E: Extended Memory Integration
- **Qdrant Vector Database:** Semantic search across conversation history
- **RAG Implementation:** Knowledge retrieval from Obsidian vault
- **Context Threading:** Conversation continuity across sessions

#### EMIYA Infrastructure Agent (Phases 24.1-24.8)
- **Proxmox Lifecycle:** VM/LXC creation, management, deletion
- **Alert Translation:** Plain English Alertmanager notifications
- **Container Updates:** Docker image updates with approval gates
- **Proactive Monitoring:** Anomaly detection and trend analysis

#### Guardian Security Agent
- **Threat Detection:** Real-time security event monitoring
- **Incident Response:** Automated containment and notification
- **Compliance Reporting:** Security posture dashboards

### Integration Improvements
- **Vault Secret Automation:** Phase 27 credential management
- **Multi-Agent Communication:** Shared memory and task handoffs
- **Cross-Platform Orchestration:** Discord, Telegram, web interfaces

---

*This workflow documentation represents a comprehensive automation layer built on n8n that orchestrates AI agents, monitors infrastructure, manages documentation, and provides intelligent homelab operations through conversational interfaces.*