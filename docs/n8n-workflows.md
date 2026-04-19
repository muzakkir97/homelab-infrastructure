# n8n Workflows Documentation

> **Last Updated:** December 19, 2024  
> **n8n Instance:** CT 211 (automation-n8n)  
> **Domain:** n8n.najhin-gaming.com  
> **Version:** 1.xx (latest stable)

## Overview

The n8n automation platform serves as the central orchestration hub for the homelab infrastructure, managing everything from AI agent responses to documentation pipelines and system monitoring. All workflows run on a dedicated LXC container with external access via Cloudflare Access protection.

### n8n Architecture

```
External Triggers (Webhooks, Schedules)
    ↓
n8n Workflow Engine (CT 211)
    ├── Data Tables (Persistent Storage)
    ├── Credentials Manager (API Keys, Tokens)  
    ├── HTTP Request Nodes (API Calls)
    └── Code Nodes (Custom Logic)
        ↓
External Integrations
├── Telegram API
├── Claude API  
├── Proxmox API
├── GitHub API
├── Nextcloud WebDAV
└── Alertmanager Webhooks
```

## Active Workflows

### 1. Telegram Agent (Gilgamesh AI Bot)

**Status:** ✅ Production  
**Trigger:** Webhook (Telegram bot updates)  
**Node Count:** ~25 nodes  
**Executions:** 500+ daily

**Purpose:**
Primary workflow powering the Gilgamesh AI agent. Handles all Telegram interactions, conversation memory, Claude API routing, and inline keyboard menu system.

**Workflow Architecture:**
```
Telegram Webhook Trigger
    ↓
Update Type Detection (Message vs Callback)
    ├── Message Path
    │   ├── Memory Retrieval (Data Tables)
    │   ├── Smart Model Routing (Haiku vs Sonnet)
    │   ├── Claude API Request
    │   ├── Memory Update
    │   └── Telegram Response
    └── Callback Path
        ├── Menu Handler
        ├── Action Processing (Status, etc.)
        └── Inline Keyboard Update
```

**Key Nodes:**
- **Webhook Trigger:** Receives all Telegram updates
- **Switch Node:** Routes messages vs callback queries  
- **HTTP Request (Memory):** n8n Data Tables CRUD operations
- **Code Node (Routing):** Smart model selection logic
- **HTTP Request (Claude):** Claude API integration
- **HTTP Request (Proxmox):** Homelab status queries
- **Telegram Response:** Message and keyboard updates

**Data Tables Used:**
- `gilgamesh_memory` - Conversation history (20 messages)
- `gilgamesh_costs` - API usage and cost tracking

**Credentials Required:**
- Telegram Bot Token
- Claude API Key
- Proxmox API Token (root@pam!gilgamesh)

**Known Issues:**
- Answer Callback node causes data loss - bypassed
- Plain text only responses (Telegram JSON parsing limitation)
- Single webhook constraint requires all update types in one workflow

### 2. Documentation Pipeline - Update

**Status:** ✅ Production  
**Trigger:** Webhook (/update command from Telegram)  
**Node Count:** ~15 nodes  
**Executions:** 20-30 monthly

**Purpose:**
Processes /update commands from Gilgamesh, generates session summaries, and pushes documentation updates to both Nextcloud and GitHub repositories.

**Workflow Architecture:**
```
/update Telegram Command
    ↓
Session Summary Generation (Claude API)
    ↓
AI-CONTEXT.md Update (Append to Session Log)
    ↓
Nextcloud File Update (WebDAV PUT)
    ↓
GitHub Commit & Push
    ↓
Telegram Confirmation
```

**Key Nodes:**
- **Webhook Trigger:** /update command detection
- **HTTP Request (Claude):** Session summary generation
- **Code Node:** File content manipulation
- **HTTP Request (Nextcloud):** WebDAV file operations
- **HTTP Request (GitHub):** Repository updates
- **Telegram Response:** Completion notification

**File Operations:**
- **Source:** Active conversation memory from Gilgamesh workflow
- **Target:** AI-CONTEXT.md Session Log section
- **Backup:** Nextcloud maintains file history
- **Repository:** github.com/muzakkir97/homelab-infrastructure

**Credentials Required:**
- Claude API Key (for summarization)
- Nextcloud WebDAV credentials
- GitHub Fine-grained PAT (Contents read/write)
- Telegram Bot Token (for confirmation)

**Current Limitations:**
- Appends to Session Log only (no section-aware merging)
- No conflict resolution for concurrent updates
- Single file update per execution

### 3. Documentation Pipeline - Sync Docs

**Status:** 📋 Planned  
**Trigger:** Manual/Scheduled  
**Node Count:** TBD  
**Executions:** TBD

**Purpose:**
Bidirectional synchronization between Nextcloud documentation storage and GitHub repository, with conflict resolution and version management.

**Planned Architecture:**
```
Trigger (Manual/Schedule)
    ↓
Compare File Timestamps (Nextcloud vs GitHub)
    ├── Nextcloud Newer
    │   ├── Pull from Nextcloud
    │   ├── Commit to GitHub
    │   └── Update GitHub
    ├── GitHub Newer  
    │   ├── Pull from GitHub
    │   ├── Update Nextcloud
    │   └── Confirm Sync
    └── Conflict Detected
        ├── Create Conflict Branch
        ├── Notify User
        └── Manual Resolution Required
```

**Planned Features:**
- Multi-file synchronization
- Conflict detection and resolution
- Automated versioning
- Rollback capability
- Sync status reporting

### 4. Update Nextcloud File (Legacy)

**Status:** 🚫 Deprecated  
**Trigger:** N/A (superseded by Documentation Pipeline)  
**Node Count:** 8 nodes  
**Executions:** 0 (disabled)

**Purpose:**
Original implementation for updating files in Nextcloud. Replaced by the integrated Documentation Pipeline workflow for better error handling and multi-step operations.

**Deprecation Reason:**
- Limited error handling
- No GitHub integration
- Single-purpose workflow
- Superseded by comprehensive documentation pipeline

### 5. Push to GitHub (Legacy)

**Status:** 🚫 Deprecated  
**Trigger:** N/A (superseded by Documentation Pipeline)  
**Node Count:** 6 nodes  
**Executions:** 0 (disabled)

**Purpose:**
Standalone GitHub repository update workflow. Merged into Documentation Pipeline for atomic operations and better error handling.

**Deprecation Reason:**
- No file content generation
- Manual trigger only
- Inconsistent with Nextcloud updates
- Better integration in combined workflow

### 6. Service Down Alert

**Status:** ✅ Production  
**Trigger:** Webhook (Alertmanager)  
**Node Count:** ~10 nodes  
**Executions:** 5-10 monthly

**Purpose:**
Receives critical alerts from Alertmanager and routes them to appropriate notification channels (Telegram, Discord) based on severity and service type.

**Workflow Architecture:**
```
Alertmanager Webhook
    ↓
Alert Severity Detection
    ├── Critical Alerts
    │   ├── Telegram (Immediate)
    │   └── Discord (Backup)
    ├── Warning Alerts
    │   └── Discord (Log)
    └── Info Alerts
        └── Log Only
```

**Key Nodes:**
- **Webhook Trigger:** Alertmanager notifications
- **Switch Node:** Severity-based routing
- **HTTP Request (Telegram):** Critical alert notifications
- **HTTP Request (Discord):** Warning alert logging
- **Code Node:** Alert formatting and enrichment

**Alert Types Handled:**
- Host down alerts (Proxmox, containers)
- High resource usage (CPU > 80%, RAM > 90%)
- Disk space warnings (> 85% full)
- Service unavailability (HTTP check failures)
- Backup job failures

**Notification Channels:**
- **Telegram:** Critical alerts to Gilgamesh chat
- **Discord:** Warning alerts to #alerts channel
- **Grafana:** All alerts logged in dashboard

**Credentials Required:**
- Telegram Bot Token
- Discord Webhook URL

## Workflow Dependencies

### Credential Management

All sensitive credentials are stored in n8n's built-in credential manager with appropriate access controls.

| Credential Name | Type | Used By | Purpose |
|-----------------|------|---------|---------|
| `telegram-bot` | HTTP Header Auth | Telegram Agent, Alerts | Bot API access |
| `claude-api` | HTTP Header Auth | Telegram Agent, Doc Pipeline | Claude API access |
| `proxmox-api` | HTTP Basic Auth | Telegram Agent | Infrastructure queries |
| `github-pat` | HTTP Header Auth | Documentation Pipeline | Repository updates |
| `nextcloud-webdav` | HTTP Basic Auth | Documentation Pipeline | File operations |
| `discord-webhook` | Webhook URL | Service Alerts | Discord notifications |

### External Dependencies

**API Endpoints:**
- **Telegram Bot API:** api.telegram.org/bot[TOKEN]
- **Claude API:** api.anthropic.com/v1/messages
- **Proxmox API:** 192.168.10.5:8006/api2/json
- **GitHub API:** api.github.com/repos/muzakkir97/homelab-infrastructure
- **Nextcloud WebDAV:** cloud.najhin-gaming.com/remote.php/dav/files/[USER]
- **Discord Webhooks:** discord.com/api/webhooks/[ID]/[TOKEN]

**Internal Dependencies:**
- **n8n Data Tables:** Persistent storage for conversation memory
- **Container Networking:** Access to VLAN 30 services
- **DNS Resolution:** Pi-hole for external domain resolution
- **SSL Certificates:** NPM for HTTPS endpoint access

## Data Architecture

### n8n Data Tables

**Table: gilgamesh_memory**
```sql
CREATE TABLE gilgamesh_memory (
  id INTEGER PRIMARY KEY,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  role VARCHAR(20) NOT NULL,  -- 'user' or 'assistant'
  content TEXT NOT NULL,
  model_used VARCHAR(50)       -- 'haiku-4-5' or 'sonnet-4'
);
```

**Table: gilgamesh_costs**
```sql  
CREATE TABLE gilgamesh_costs (
  id INTEGER PRIMARY KEY,
  timestamp DATETIME DEFAULT CURRENT_TIMESTAMP,
  model VARCHAR(50) NOT NULL,
  input_tokens INTEGER NOT NULL,
  output_tokens INTEGER NOT NULL,
  cost_usd DECIMAL(10,6) NOT NULL
);
```

**Data Retention:**
- **Memory:** Rolling 20-message window, older records deleted
- **Costs:** Permanent retention for billing analysis
- **Backups:** Included in CT 211 daily backup job

### File Storage

**Nextcloud Integration:**
- **Path:** /AI-CONTEXT.md (root directory)
- **Protocol:** WebDAV over HTTPS
- **Access:** cloud.najhin-gaming.com/remote.php/dav/files/admin/
- **Versioning:** Nextcloud maintains automatic file history

**GitHub Integration:**
- **Repository:** homelab-infrastructure (public)
- **Branch:** main
- **Files:** AI-CONTEXT.md, documentation/*
- **Access:** Fine-grained PAT with Contents read/write

## Workflow Monitoring

### Execution Tracking

**Built-in Metrics:**
- Workflow execution count and frequency
- Success/failure rates per workflow  
- Average execution time
- Error logs and stack traces
- Resource usage per execution

**Custom Monitoring:**
- **Gilgamesh Costs:** Tracked in dedicated data table
- **Alert Response Time:** Alertmanager to notification delivery
- **Documentation Sync Status:** Last successful update timestamps
- **API Rate Limiting:** Claude, GitHub, Telegram usage tracking

### Error Handling

**Common Error Patterns:**

| Error Type | Cause | Resolution | Prevention |
|------------|-------|------------|------------|
| **Claude API 404** | Invalid model ID | Use exact model names | Validate model strings |
| **n8n Data Tables Schema** | Caching bug after table changes | Delete and recreate table | Version schema changes |
| **GitHub 401 Unauthorized** | Missing username in credentials | Populate user field in credential | Document credential requirements |
| **Telegram Single Webhook** | Multiple bots on same endpoint | Combine all triggers in one workflow | Webhook planning |
| **Memory Data Loss** | Answer Callback node bug | Bypass Answer Callback entirely | Use manual acknowledgment |

**Error Notification:**
- Critical workflow failures → Telegram alert
- API rate limiting → Graceful degradation
- Data corruption → Automatic backup restoration
- Credential expiry → Proactive renewal alerts

## Performance Optimization

### Current Performance

| Workflow | Avg Execution Time | Success Rate | Resource Usage |
|----------|-------------------|--------------|----------------|
| **Telegram Agent** | 2-3 seconds | 98% | Low CPU, moderate memory |
| **Documentation Pipeline** | 5-8 seconds | 95% | Moderate CPU/memory |
| **Service Alerts** | < 1 second | 99% | Minimal resources |

### Optimization Strategies

**Response Time Improvements:**
- Cache frequently accessed data (container status)
- Parallel API calls where possible
- Reduce Claude API context size for simple queries
- Pre-warm connections to external APIs

**Reliability Improvements:**
- Implement retry logic for transient failures
- Circuit breaker pattern for unreliable APIs
- Graceful degradation when services unavailable
- Health checks for critical dependencies

**Resource Optimization:**
- Clean up temporary data after execution
- Optimize memory usage in Code nodes
- Batch operations where possible
- Schedule resource-intensive workflows during off-peak

## Development Best Practices

### Workflow Design Patterns

**Error Handling Pattern:**
```
HTTP Request Node
    ├── Success (200-299)
    │   └── Continue workflow
    └── Error (400+, network)
        ├── Log error details
        ├── Send error notification
        └── Graceful failure response
```

**Data Validation Pattern:**
```
Input Data
    ↓
Validation Code Node
    ├── Valid Data
    │   └── Process normally
    └── Invalid Data
        ├── Log validation error
        ├── Notify user of issue
        └── Return helpful error message
```

**API Integration Pattern:**
```
Prepare Request Data
    ↓
HTTP Request with Retry
    ├── Success
    │   ├── Extract relevant data
    │   └── Continue workflow
    └── Failure
        ├── Check retry count
        ├── Exponential backoff
        └── Final error handling
```

### Testing Methodology

**Manual Testing:**
- Test each trigger type independently
- Verify error paths with invalid data
- Confirm credential access and permissions
- Validate external API integration

**Automated Testing (Planned):**
- Mock external APIs for consistent testing
- Regression test suite for critical workflows
- Performance benchmarking
- Data integrity validation

### Version Control

**Current Approach:**
- n8n workflows auto-save on modification
- Manual backup exports before major changes
- Documentation updates in parallel with workflow changes

**Planned Improvements:**
- Export workflows to GitHub for version control
- Staging environment for testing changes
- Automated deployment pipeline
- Rollback procedures for failed deployments

## Security Considerations

### Access Control

**n8n Interface Security:**
- Protected by Cloudflare Access (email OTP)
- Domain: n8n.najhin-gaming.com
- No direct IP access allowed
- Session timeout configured

**API Security:**
- All credentials stored in n8n credential manager
- No hardcoded secrets in workflow nodes
- Regular credential rotation (manual)
- Least privilege principle for API tokens

### Data Protection

**Sensitive Data Handling:**
- Conversation data stays within n8n instance
- No logging of sensitive content in execution logs
- Memory data purged after 20 messages
- API keys never appear in workflow output

**Network Security:**
- n8n container isolated in VLAN 30
- Outbound HTTPS only for API calls
- No direct database access from external networks
- Firewall rules restrict unnecessary access

## Lessons Learned

### Critical Discoveries

**n8n Data Tables Issues:**
- Schema changes cause persistent caching bugs
- Only solution: Delete and recreate affected tables
- Always backup data before schema modifications
- Consider external database for production use

**Telegram Bot Limitations:**
- Single webhook per bot token
- Must handle all update types in one workflow
- Answer Callback Query node causes data loss
- Plain text responses only (no code formatting)

**API Integration Challenges:**
- Claude API model IDs must be exact
- GitHub credentials require username field populated
- Proxmox API node name differs from hostname
- Rate limiting varies significantly between providers

**Workflow Architecture Lessons:**
- Avoid Answer Callback nodes - handle acknowledgments manually
- Use Code nodes for complex expressions due to scoping issues
- Combine related operations in single workflow for atomicity
- Plan webhook endpoints carefully to avoid conflicts

### Best Practices Learned

**Credential Management:**
- Document all required fields for each credential type
- Test credentials immediately after creation
- Implement credential health checks
- Plan for token rotation and expiration

**Error Handling:**
- Always implement error paths for HTTP requests
- Log enough detail for troubleshooting without exposing secrets
- Provide user-friendly error messages
- Consider graceful degradation over hard failures

**Data Management:**
- Implement data retention policies from the start
- Plan for data export and backup procedures
- Consider scalability when choosing storage methods
- Validate data integrity at critical checkpoints

---

*This documentation reflects the current state of all n8n workflows as of December 19, 2024. Workflows marked as "Planned" are in the development roadmap and subject to change based on infrastructure needs and technical feasibility.*