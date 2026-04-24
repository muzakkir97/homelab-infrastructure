# n8n Workflow Documentation

## Overview

n8n automation platform deployed on CT 211 (192.168.30.211) serving as the central workflow orchestration hub for the homelab. All workflows integrate with Gilgamesh AI agent and support the documentation pipeline.

## Active Workflows

### 1. Telegram Agent (Primary Bot Workflow)
**Purpose:** Complete Gilgamesh AI agent implementation with chat, memory, and homelab control

**Technical Details:**
- **Nodes:** 15 total nodes
- **Trigger:** Telegram webhook (handles both messages and callback queries)
- **Status:** ✅ Active and Published
- **Bot:** @JhinGilgamesh_bot (Chat ID: 518832696)

**Architecture Flow:**
```
Telegram Input → Message Type Detection → Memory Retrieval
                                              ↓
Smart Model Router → Claude API Call → Response Generation
                                              ↓
Memory Storage → Cost Tracking → Telegram Response
```

**Core Node Structure:**
```
1. Telegram Trigger (webhook)
2. Message Type Router (message/callback_query)
3. Get Memory (Data Table query - last 20 messages)
4. Smart Router (Haiku vs Sonnet decision)
5. Claude API (Anthropic node)
6. Save Memory (Data Table insert)
7. Save Cost (Data Table insert - cost_usd bug present)
8. Send Response (Telegram node)
9-15. Inline Menu System (callback handlers)
```

**Data Table Dependencies:**
- `gilgamesh_memory`: Conversation storage
- `gilgamesh_costs`: Token usage tracking

**API Integrations:**
- Claude API (Haiku 4.5 + Sonnet 4)
- Proxmox VE API (metrics, status)
- SSH to Kuromoon (temperature monitoring)
- SSH to CT 302 (Docker status)

**Menu System Implementation:**
```
Main Menu → 4 submenus (Homelab, Gaming, Gilgamesh, Tools)
Homelab → 4 actions (Status, Metrics, Temps, Storage)
Gaming → Real-time Docker status
Gilgamesh → Information panel
Tools → Service quick links
Help → Command reference
```

**Known Issues:**
- `cost_usd` column in `gilgamesh_costs` always returns 0
- Telegram messages must be plain text only (formatting breaks JSON)
- Single webhook constraint requires handling all update types in one trigger

### 2. Documentation Pipeline - Update
**Purpose:** Process session summaries and update core documentation files

**Technical Details:**
- **Nodes:** 7 nodes
- **Trigger:** Webhook (`doc-update`)
- **Status:** ✅ Active and Published
- **Command:** `/update` from Telegram Agent

**Architecture Flow:**
```
Webhook Trigger → Claude Processing → File Generation (3 files)
                                              ↓
                   Nextcloud Upload → GitHub Push → Confirmation
```

**Node Structure:**
```
1. Webhook Trigger (doc-update endpoint)
2. Claude API (documentation merging)
3. Update AI-CONTEXT (append session log)
4. Update Changelog (add phase completion)
5. Update Troubleshoot (merge issues/resolutions)
6. Push to Nextcloud (app password auth)
7. Push to GitHub (API push)
```

**File Outputs:**
- `AI-CONTEXT.md`: Master context file with session log
- `changelog.md`: Phase completion history
- `troubleshoot.md`: Issues and resolutions database

**Authentication:**
- Nextcloud: App password (`n8n-doc-pipeline`)
- GitHub: API token with repo access

### 3. Documentation Pipeline - Sync Docs
**Purpose:** Full documentation regeneration from master AI-CONTEXT.md

**Technical Details:**
- **Nodes:** 7 nodes
- **Trigger:** Webhook (`doc-sync`)
- **Status:** ✅ Active and Published  
- **Command:** `/sync-docs` from Telegram Agent

**Architecture Flow:**
```
Webhook Trigger → Claude Analysis → Multi-file Generation (7 files)
                                              ↓
                   Batch Upload → GitHub Push → Confirmation
```

**Node Structure:**
```
1. Webhook Trigger (doc-sync endpoint)
2. Claude API (full documentation generation)
3. Generate README (project overview)
4. Generate Roadmap (phase planning)
5. Generate Current State (infrastructure status)
6. Generate Service Catalog (container inventory)
7. Batch Upload Process (Nextcloud + GitHub)
```

**File Outputs:**
- All 3 files from `/update` workflow
- `README.md`: Project overview and quick start
- `roadmap.md`: Phase planning and priorities
- `current-state.md`: Current infrastructure status
- `service-catalog.md`: Complete service inventory

**Processing Logic:**
- Parse AI-CONTEXT.md for all infrastructure data
- Generate architecture documentation
- Create service catalog with status
- Build roadmap from phase history
- Maintain consistent formatting and structure

### 4. Service Down Alert (Monitoring Integration)
**Purpose:** Route Alertmanager alerts to Telegram and Discord

**Technical Details:**
- **Nodes:** 6 nodes
- **Trigger:** Webhook (Alertmanager)
- **Status:** ✅ Active
- **Integration:** CT 205 (Alertmanager) → n8n → Notifications

**Architecture Flow:**
```
Alertmanager → Webhook → Alert Processing → Notification Router
                                              ↓
                           Telegram (Critical) + Discord (Warnings)
```

**Alert Routing Logic:**
```
Severity: critical → Telegram (@JhinGilgamesh_bot)
Severity: warning → Discord (#alerts channel)
All levels → Prometheus alert counter
```

**Node Structure:**
```
1. Webhook Trigger (alertmanager endpoint)
2. Alert Parser (JSON processing)
3. Severity Router (critical/warning decision)
4. Format Telegram Alert
5. Format Discord Alert  
6. Send Notifications
```

**Alert Types Handled:**
- Host down (node_down)
- High CPU usage (>90% for 5 minutes)
- High memory usage (>90%)
- High disk usage (>85%)
- Container down (container_down)
- Service health checks

## Legacy Workflows (Unpublished)

### 5. Update Nextcloud File
**Status:** 🔄 Legacy (Unpublished)
- **Nodes:** 5 nodes
- **Purpose:** Direct file updates to Nextcloud
- **Superseded by:** Documentation Pipeline - Update
- **Reason:** Consolidated into main documentation workflow

### 6. Push to GitHub  
**Status:** 🔄 Legacy (Unpublished)
- **Nodes:** 4 nodes
- **Purpose:** Direct GitHub repository updates
- **Superseded by:** Documentation Pipeline workflows
- **Reason:** GitHub push integrated into main pipelines

### 7. Test SQLite (Development)
**Status:** 🗑️ Pending Deletion
- **Purpose:** Database testing and development
- **Current State:** No longer needed
- **Action Required:** Delete from n8n interface

## Workflow Dependencies

### Credentials Configuration

| Credential Name | Type | Usage | Status |
|-----------------|------|-------|--------|
| Anthropic API | API Key | Claude API access | ✅ Active |
| Telegram Bot | Token | @JhinGilgamesh_bot | ✅ Active |
| Nextcloud App Password | Basic Auth | File uploads | ✅ Active |
| GitHub API | Personal Access Token | Repository push | ✅ Active |
| Proxmox API | Token | Infrastructure monitoring | ✅ Active |
| SSH Password (Kuromoon) | SSH Key | Host monitoring | ✅ Active |
| SSH CT302 Wings | Password | Gaming server status | ✅ Active |

### External Service Dependencies

| Service | Container | Purpose | Health Check |
|---------|-----------|---------|--------------|
| Claude API | External | AI processing | API rate limits |
| Telegram API | External | Bot communication | Webhook validation |
| GitHub API | External | Repository management | Token permissions |
| Nextcloud | CT 220 | File storage | App password auth |
| Proxmox API | Kuromoon | Infrastructure data | Token validation |
| Alertmanager | CT 205 | Alert routing | Webhook delivery |

## Data Tables Schema

### gilgamesh_memory
**Purpose:** Conversation history storage
```sql
CREATE TABLE gilgamesh_memory (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id TEXT,
    message TEXT,
    response TEXT, 
    timestamp DATETIME,
    model TEXT,
    tokens_input INTEGER,
    tokens_output INTEGER
);
```
**Retention:** Last 20 messages via LIMIT query
**Access Pattern:** Read on message, write on response

### gilgamesh_costs
**Purpose:** Token usage and cost tracking
```sql
CREATE TABLE gilgamesh_costs (
    id INTEGER PRIMARY KEY AUTOINCREMENT,
    user_id TEXT,
    model TEXT,
    tokens_input INTEGER,
    tokens_output INTEGER,
    cost_usd REAL,
    timestamp DATETIME
);
```
**Known Issue:** `cost_usd` column always returns 0
**Workaround:** Manual calculation using token counts × API rates

## Webhook Endpoints

### Active Endpoints
```
https://n8n.najhin-gaming.com/webhook/doc-update
https://n8n.najhin-gaming.com/webhook/doc-sync
https://n8n.najhin-gaming.com/webhook/alertmanager
https://n8n.najhin-gaming.com/webhook/telegram-bot
```

### Security Configuration
- **Authentication:** Cloudflare Access (email OTP)
- **SSL:** Cloudflare managed certificates
- **Rate Limiting:** Cloudflare standard limits
- **Access Control:** IP restrictions via Cloudflare

## Performance Metrics

### Execution Statistics (Approximate)
```
Telegram Agent: ~2-5 seconds per query
Documentation Update: ~10-15 seconds
Documentation Sync: ~20-30 seconds  
Alert Processing: <1 second
```

### Resource Usage
- **CPU:** Low (text processing only)
- **Memory:** ~512MB for n8n container
- **Storage:** Minimal (workflow definitions + logs)
- **Network:** API-dependent (Claude calls)

### Reliability Metrics
- **Uptime:** 99%+ (container autostart enabled)
- **Error Rate:** <1% (mostly API timeouts)
- **Recovery:** Automatic via container restart
- **Monitoring:** Prometheus metrics + Alertmanager

## Known Issues & Lessons Learned

### Data Tables Issues
**Issue:** Schema caching bug in n8n Data Tables
**Resolution:** Delete and recreate table entirely for schema changes
**Prevention:** Plan schema carefully before implementation

**Issue:** n8n expression scoping in complex workflows
**Resolution:** Use Code nodes with `this.helpers.httpRequest`
**Prevention:** Reference `$json` correctly in expressions

### Claude API Integration
**Issue:** 404 "model not found" errors
**Resolution:** Use exact model IDs: `claude-haiku-4-5-20251001`
**Prevention:** Verify model names in Anthropic console

**Issue:** Web search partial response capture
**Resolution:** Add Extract Response Code node, filter by `type === "text"`
**Prevention:** Always validate API response structure

### Telegram Integration
**Issue:** Single webhook constraint for bot
**Resolution:** Handle all update types (message + callback_query) in one trigger
**Prevention:** Design unified webhook handlers from start

**Issue:** Answer Callback node loses data
**Resolution:** Bypass Answer Callback, handle acknowledgment separately
**Prevention:** Use manual callback_query acknowledgment

**Issue:** Complex message formatting breaks JSON parsing
**Resolution:** Restrict to plain text messages only
**Prevention:** Validate message format before API calls

### GitHub Integration
**Issue:** 401 unauthorized errors despite valid token
**Resolution:** Populate username field (`muzakkir97`) in credentials
**Prevention:** Fill all required credential fields

### SSH Integration
**Issue:** SSH connections hanging from containers
**Resolution:** Add pfSense firewall rule for CT 211 → Kuromoon:22
**Prevention:** Verify network connectivity before SSH configuration

**Issue:** Password authentication failures
**Resolution:** Set `PermitRootLogin yes` on target containers
**Prevention:** Configure SSH server settings before client setup

## Workflow Architecture Patterns

### Message Processing Pattern
```
Input Validation → Context Retrieval → Processing → Response Generation → Storage
```
Used in: Telegram Agent, Documentation workflows

### Fan-out Pattern
```
Single Trigger → Multiple Parallel Processes → Aggregate Results
```
Used in: Documentation Sync (7 files), Alert routing

### API Integration Pattern  
```
Authentication → Request Formatting → API Call → Response Validation → Error Handling
```
Used in: All external API integrations

### State Management Pattern
```
Current State Retrieval → State Modification → State Persistence → State Validation
```
Used in: Memory system, cost tracking

## Future Workflow Development

### Planned Workflows

#### MERLIN Reminder System (High Priority)
**Purpose:** Proactive infrastructure maintenance reminders
**Triggers:** Cron-based scheduling
**Integrations:** SSL certificate monitoring, backup validation, resource alerts
**Implementation:** Phase 24+ (Service Update Manager integration)

#### Service Update Manager (Phase 24.1)
**Purpose:** Automated update management with approval gates
**Scope:** Docker containers, apt packages, Proxmox updates
**Approval:** Telegram confirmation before execution
**Rollback:** Automated rollback on failure detection

#### Mash Gaming Bot (Phases 59-64)
**Purpose:** Discord integration for gaming server management
**Platform:** Discord webhook integration
**Separation:** Keep gaming workflows separate from homelab admin
**Commands:** `!start`, `!stop`, `!status`, player announcements

### Architecture Evolution
- **Central Hub Model:** All notifications route through n8n first
- **Agent Coordination:** Multiple specialized agents (Fate/GO theme)
- **Cross-platform Integration:** Telegram + Discord + Web UI
- **Intelligent Routing:** AI-driven workflow decision making

## Maintenance Procedures

### Regular Maintenance
- **Weekly:** Review workflow execution logs
- **Monthly:** Update credentials and test webhook endpoints
- **Quarterly:** Performance optimization and cleanup
- **Semi-annually:** Full workflow audit and documentation update

### Troubleshooting Checklist
1. **Workflow Not Triggering:** Check webhook URL and credentials
2. **API Errors:** Verify rate limits and token validity
3. **Data Table Issues:** Check schema and query syntax
4. **Authentication Failures:** Validate all credential configurations
5. **Performance Issues:** Review node complexity and API timeouts

### Backup Strategy
- **Workflow Definitions:** Exported as JSON monthly
- **Credentials:** Stored in HashiCorp Vault (CT 213)
- **Data Tables:** Included in CT 211 backup job
- **Documentation:** Auto-generated and version controlled

### Monitoring Integration
- **Container Health:** Prometheus monitoring on CT 211
- **Workflow Metrics:** n8n internal metrics + custom counters
- **Error Alerting:** Failed executions → Alertmanager → Telegram
- **Performance Tracking:** Execution times and resource usage