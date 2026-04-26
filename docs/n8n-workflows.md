# n8n Workflows Documentation

## Overview

n8n serves as the central automation hub for the homelab infrastructure, hosted on CT 211 (192.168.30.211). All workflows support the AI agent ecosystem and documentation pipeline, with Gilgamesh as the primary user interface.

**Current Status:** 6 active workflows (2 legacy unpublished)
**Platform:** n8n Community Edition in LXC container
**External Access:** n8n.najhin-gaming.com via Cloudflare Tunnel + Access

## Active Workflow Inventory

| Workflow Name                       | Trigger Type | Node Count | Status        | Purpose                                    |
|-------------------------------------|--------------|------------|---------------|--------------------------------------------|
| Telegram Agent                      | Telegram     | 15+        | ✅ Active      | Main Gilgamesh bot with hybrid AI routing  |
| Documentation Pipeline — Update     | Webhook      | 7          | ✅ Active      | Session summaries → 3 files               |
| Documentation Pipeline — Sync Docs  | Webhook      | 7          | ✅ Active      | Full documentation regeneration → 7 files |
| Da Vinci Documentation Pipeline     | Webhook      | 11         | ✅ Active      | Raw summaries → formatted documentation   |
| Service Down Alert                  | HTTP Request | 5          | ✅ Active      | Alertmanager webhook → Telegram alerts   |
| Update Nextcloud File               | Webhook      | 5          | 🚫 Unpublished | Legacy documentation updater             |
| Push to GitHub                      | Webhook      | 4          | 🚫 Unpublished | Legacy GitHub integration                |

**Note:** Hybrid AI routing (Phase 41) is integrated into the Telegram Agent workflow, not implemented as a separate workflow.

## Core Workflows Architecture

### 1. Telegram Agent Workflow

**Purpose:** Primary interface for Gilgamesh AI agent with smart routing between local and cloud AI models.

**Architecture Diagram:**
```
Telegram Webhook → Route Check → Model Selection
                       ↓
    ┌──────────────────┼──────────────────┐
    ▼                  ▼                  ▼
Ollama qwen3:14b   Haiku 4.5          Sonnet 4
   (Primary)       (Fallback)        (Complex)
    └──────────────────┼──────────────────┘
                       ▼
              Response Processing
                       ↓
         ┌─────────────┼─────────────┐
         ▼             ▼             ▼
   Save Memory   Save Costs   Format Response
         └─────────────┼─────────────┘
                       ▼
              Telegram Response
```

**Key Components:**
- **Telegram Trigger:** Handles both messages and callback queries
- **Route Check If:** Determines AI model based on complexity
- **Smart Routing Logic:**
  - Default: Ollama qwen3:14b (free, local)
  - Fallback: Claude Haiku 4.5 (if Ollama unavailable)
  - Complex: Claude Sonnet 4 (50+ words or complexity keywords)
- **Memory Management:** Last 20 messages stored in gilgamesh_memory table
- **Cost Tracking:** Token usage logged to gilgamesh_costs table
- **Inline Keyboard Menu:** Complete menu system with homelab monitoring

**Node Breakdown:**
```
Nodes by Function:
├── Telegram Trigger (1)
├── Route Check If (1)
├── AI Model Calls (3)
├── Memory Operations (3)
├── Cost Tracking (2)
├── Response Processing (2)
├── Menu System (3)
└── Error Handling (2)
Total: 15+ nodes
```

**Credentials Required:**
- Telegram Bot Token (Gilgamesh)
- Claude API Key (Anthropic)
- n8n Data Tables Access
- Proxmox API (root@pam!gilgamesh)
- SSH Keys (Kuromoon, VM 400, CT 302)

### 2. Documentation Pipeline — Update Workflow

**Purpose:** Process session summaries via /update command, merging content into 3 core documentation files.

**Architecture Diagram:**
```
Webhook Trigger → Extract Summary → Call Da Vinci Pipeline
      ↓                               ↓
Session Text              Raw Summary → Formatted Docs
      ↓                               ↓
Parse Content            Update Files (AI-CONTEXT.md, 
      ↓                  changelog.md, troubleshoot.md)
Format for Pipeline               ↓
      ↓                    Push to Nextcloud + GitHub
Async Handoff → Da Vinci
```

**Node Structure:**
```
Documentation Update Flow:
├── Webhook Trigger (1)
├── Extract Session Data (1)
├── Append to Staging File (1)
├── Call Da Vinci Pipeline (1)
├── Response Processing (1)
├── Nextcloud Update (1)
└── GitHub Push (1)
Total: 7 nodes
```

**Integration Points:**
- **Input:** Telegram /update command via webhook
- **Processing:** Da Vinci Documentation Pipeline
- **Output:** Nextcloud WebDAV + GitHub API
- **Staging:** AI-CONTEXT-staging.md (rolling append)

### 3. Documentation Pipeline — Sync Docs Workflow  

**Purpose:** Complete documentation regeneration triggered by /sync-docs command, updating all 7 documentation files.

**Architecture Diagram:**
```
Webhook Trigger → Full Doc Generation → Claude API Processing
      ↓                    ↓                      ↓
/sync-docs Command    Extract Current Docs    Generate Updated Files
      ↓                    ↓                      ↓
Parse Request        Master Context File      7 Files Updated:
      ↓                    ↓                  ├── AI-CONTEXT.md
Batch Processing     Comprehensive Update    ├── changelog.md
      ↓                    ↓                  ├── troubleshoot.md
Multi-file Update    Version Control         ├── README.md
                                            ├── roadmap.md
                                            ├── current-state.md
                                            └── service-catalog.md
```

**Files Updated (7 Total):**
- **Core Files:** AI-CONTEXT.md, changelog.md, troubleshoot.md
- **Project Files:** README.md, roadmap.md, current-state.md, service-catalog.md

**Node Structure:**
```
Full Sync Process:
├── Webhook Trigger (1)
├── Context Gathering (1)
├── Claude API Generation (1)
├── File Processing (1)
├── Multi-file Update (1)
├── Nextcloud Sync (1)
└── GitHub Push (1)
Total: 7 nodes
```

### 4. Da Vinci Documentation Pipeline

**Purpose:** Advanced documentation processing with intelligent content merging and formatting.

**Architecture Diagram:**
```
Webhook Input → Fetch Current Docs → Claude API Processing
     ↓                 ↓                      ↓
Raw Session      AI-CONTEXT.md          Intelligent Merge
  Summary          (current)                  ↓
     ↓                 ↓               Format & Structure
Context Build    Extract Content             ↓
     ↓                 ↓               Generate Updates
Staging File     Merge Contexts             ↓
Processing           ↓                 Validate Output
     ↓          Comprehensive             ↓
Send to Claude      Context          Final Documentation
     API                              ↓
                                 Update Nextcloud + GitHub
```

**Key Features:**
- **Grounding Fix:** Fetches current AI-CONTEXT.md before processing
- **max_tokens:** Set to 32000 to prevent content truncation
- **Direct Node References:** Uses `$('Extract Response').first().json.updatedDoc`
- **Staging Integration:** Processes AI-CONTEXT-staging.md rolling append file

**Technical Implementation:**
```
Processing Pipeline:
├── Webhook Trigger (1)
├── Fetch Current Context (1)  
├── Extract Staging Content (1)
├── Build Processing Context (1)
├── Claude API Call (1)
├── Extract Response (1)
├── Validate Content (1)
├── Update Nextcloud (1)
├── Commit to GitHub (1)
├── Cleanup Operations (1)
└── Response Handling (1)
Total: 11 nodes
```

**Critical Notes:**
- **Pronouns:** She/her for Da Vinci (Caster servant theme)
- **Grounding:** Always fetch current docs before Claude API call
- **Error Handling:** Direct node references to prevent data loss

## Legacy Workflows (Unpublished)

### Update Nextcloud File (Legacy)

**Status:** Superseded by integrated documentation pipeline
**Node Count:** 5 nodes
**Purpose:** Simple file updates to Nextcloud via WebDAV
**Reason for Deprecation:** Lacked integration with version control and multi-file support

### Push to GitHub (Legacy)

**Status:** Superseded by integrated documentation pipeline  
**Node Count:** 4 nodes
**Purpose:** Basic GitHub commits via API
**Reason for Deprecation:** No integration with Nextcloud sync and content processing

## Monitoring & Alerting Workflows

### Service Down Alert Workflow

**Purpose:** Alertmanager webhook processing for critical system alerts.

**Architecture Diagram:**
```
Alertmanager → n8n Webhook → Parse Alert Data → Format Message
     ↓              ↓              ↓               ↓
Critical Alert   JSON Payload   Extract Fields   Plain English
   Fired            ↓              ↓               ↓
     ↓         Alert Details   Severity Level   User-Friendly
Alert Data         ↓              ↓               ↓
Processing    Service Name    Priority Check   Telegram Message
     ↓              ↓              ↓               ↓
Route to n8n   Component ID   Route Decision   Send Alert
                   ↓              ↓
              Alert Context   Notification
                   ↓         Delivery
              Timestamp +
              Metadata
```

**Node Structure:**
```
Alert Processing Flow:
├── HTTP Request Trigger (1)
├── Parse Alert JSON (1)
├── Extract Critical Fields (1)
├── Format User Message (1)
└── Send Telegram Alert (1)
Total: 5 nodes
```

**Alert Types Handled:**
- Container down/unreachable
- High resource usage (CPU >90%, Memory >85%, Disk >90%)
- Service failures (systemd unit failures)
- Network connectivity issues
- Backup job failures

## Workflow Development Best Practices

### Error Handling Patterns

```
Standard Error Flow:
Primary Node → Error Catch → Format Error Message → Alert/Log
     ↓              ↓              ↓                    ↓
Success Path   Exception     User-Friendly         Notification
Continue       Caught        Error Text            or Recovery
     ↓              ↓              ↓                    ↓
Next Node     Log Error     Send to Telegram      Continue/Stop
```

### Data Table Schema Design

**gilgamesh_memory Table:**
```sql
Columns:
├── id (auto-increment)
├── user_id (518832696)  
├── message_text (TEXT)
├── response_text (TEXT)
├── timestamp (DATETIME)
├── message_type (VARCHAR)
└── tokens_used (INT)
```

**gilgamesh_costs Table:**
```sql
Columns:
├── id (auto-increment)
├── timestamp (DATETIME)
├── model_used (VARCHAR)
├── input_tokens (INT)
├── output_tokens (INT)
├── cost_usd (DECIMAL)
└── query_type (VARCHAR)
```

### Credential Management

**Security Best Practices:**
- All sensitive credentials stored in HashiCorp Vault (CT 213)
- n8n accesses credentials via environment variables
- API tokens rotated regularly
- SSH keys use dedicated service accounts
- Webhook URLs use HTTPS with authentication

**Credential Dependencies by Workflow:**

| Workflow                    | Required Credentials                                    |
|-----------------------------|--------------------------------------------------------|
| Telegram Agent              | Telegram Bot, Claude API, Proxmox API, SSH Keys       |
| Documentation Pipelines     | Claude API, Nextcloud WebDAV, GitHub API              |
| Da Vinci Pipeline           | Claude API, Nextcloud WebDAV, GitHub API              |
| Service Down Alert          | Telegram Bot                                           |

## Known Issues & Lessons Learned

### Common Issues & Resolutions

| Issue                                    | Resolution                                                    |
|------------------------------------------|---------------------------------------------------------------|
| n8n Data Tables schema caching bug      | Delete and recreate table entirely                           |
| Claude API 404 model not found          | Use exact model ID: `claude-haiku-4-5-20251001`              |
| Web search partial response capture     | Add Extract Response Code node, filter by `type === "text"`  |
| Expression scoping issues               | Use Code node with `this.helpers.httpRequest`                |
| Telegram single webhook constraint      | Handle all update types (message + callback) in one trigger  |
| Answer Callback node loses data         | Bypass and handle callback acknowledgment separately         |
| Da Vinci grounding bug                  | Use direct node references; increase max_tokens to 32000     |
| GitHub credential 401 errors            | Populate user field (`muzakkir97`) in addition to token      |

### Performance Optimization Notes

**Workflow Efficiency:**
- Use Code nodes for complex logic instead of multiple If nodes
- Implement early returns to avoid unnecessary processing  
- Cache frequently accessed data in variables
- Use async processing for non-blocking operations

**Resource Management:**
- Monitor memory usage during large file processing
- Implement rate limiting for external API calls
- Use batch processing for multiple operations
- Set appropriate timeouts for long-running tasks

### Development Guidelines

**Node Naming Conventions:**
- Descriptive names: "Extract Alert Data" not "If Node 1"
- Consistent prefixes: "Call Claude API", "Save to Memory"  
- Error nodes: "Handle Error - [Operation]"

**Documentation Requirements:**
- Each workflow must have description and tags
- Complex nodes require inline comments
- External integrations documented with API versions
- Credential requirements listed in workflow notes

### Future Workflow Development

**Planned Workflows:**
1. **Monthly Infrastructure Audit** (New phase TBD)
2. **Midas Cost Tracking** (Phase 24.1+)
3. **MERLIN Scheduling** (Phase 24.2+)
4. **EMIYA Infrastructure Management** (Phases 24.1-24.8)
5. **Guardian Security Monitoring** (TBD)

**Architecture Evolution:**
- Migration from webhooks to direct API integrations
- Implementation of workflow templates for agent development
- Enhanced error handling and recovery mechanisms
- Real-time monitoring and alerting for workflow health
- Integration with external monitoring systems (Grafana/Prometheus)

This documentation provides comprehensive coverage of all n8n workflows supporting the homelab AI agent ecosystem, serving as both operational reference and development guide for future enhancements.