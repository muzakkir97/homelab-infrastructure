# n8n Workflow Documentation

## Overview

n8n automation platform runs on **CT 211** (192.168.30.211) and serves as the central hub for:
- Gilgamesh AI agent operations
- Documentation pipeline automation
- System monitoring and alerting
- Legacy workflows (being phased out)

**Total Active Workflows: 7** (2 unpublished legacy workflows)

## Active Workflows

### 1. Telegram Agent
- **Purpose**: Main Gilgamesh AI bot with conversation handling, memory, routing, and menu system
- **Trigger**: Telegram webhook (both messages and callback queries)
- **Node Count**: 15 nodes
- **Status**: ✅ Active
- **Last Updated**: April 24, 2026 (Phase 15)

#### Architecture Diagram
```
Telegram Webhook
    ↓
Callback Router (routes messages vs callbacks)
    ↓                           ↓
Message Handler            Callback Handler
    ↓                           ↓
Command Switch             Menu Actions
    ↓                           ↓
Claude API Route           SSH/API Calls
    ↓                           ↓
Memory Storage             Response Format
    ↓                           ↓
Response Send              Telegram Reply
```

#### Key Components
- **Memory Management**: Last 20 messages stored in gilgamesh_memory table
- **Smart Routing**: Haiku vs Sonnet based on complexity
- **Command Processing**: 6 slash commands (/help, /clear, /memory, /cost, /alerts, /backup)
- **Menu System**: Inline keyboards for homelab monitoring
- **Cost Tracking**: Token usage logged to gilgamesh_costs table

#### Dependencies
- **Credentials**: Telegram Bot Token, Claude API Key, SSH keys
- **External APIs**: Proxmox API, GitHub API, Nextcloud API
- **SSH Access**: Kuromoon (port 22), CT 302 (password auth)

### 2. Documentation Pipeline - Update
- **Purpose**: Merge session summaries into 3 documentation files
- **Trigger**: Webhook (doc-update) via `/update` command
- **Node Count**: 7 nodes
- **Status**: ✅ Active
- **Last Updated**: April 19, 2026 (Phase 16.1)

#### Architecture Diagram
```
Webhook Trigger (/update)
    ↓
Claude API (merge session into docs)
    ↓
Split Response (3 files: AI-CONTEXT, changelog, troubleshoot)
    ↓
Nextcloud Push (parallel file updates)
    ↓
GitHub Push (version control)
    ↓
Confirmation Response
```

#### File Targets
1. **AI-CONTEXT.md**: Master context document
2. **changelog.md**: Project history and milestones
3. **troubleshoot.md**: Issues, solutions, and lessons learned

### 3. Documentation Pipeline - Sync Docs
- **Purpose**: Complete regeneration of 7 documentation files from master context
- **Trigger**: Webhook (doc-sync) via `/sync-docs` command
- **Node Count**: 7 nodes
- **Status**: ✅ Active
- **Last Updated**: April 19, 2026 (Phase 16.2)

#### Architecture Diagram
```
Webhook Trigger (/sync-docs)
    ↓
Claude API (regenerate all docs from AI-CONTEXT)
    ↓
Parse Response (7 files with delimiters)
    ↓
Parallel File Push (Nextcloud + GitHub)
    ↓
Completion Notification
```

#### File Targets
1. **AI-CONTEXT.md** (updated)
2. **changelog.md** (updated)  
3. **troubleshoot.md** (updated)
4. **README.md** (regenerated)
5. **roadmap.md** (regenerated)
6. **current-state.md** (regenerated)
7. **service-catalog.md** (regenerated)

### 4. Service Down Alert
- **Purpose**: Alertmanager webhook integration for critical system alerts
- **Trigger**: HTTP webhook from Alertmanager (CT 205)
- **Node Count**: 4 nodes
- **Status**: ✅ Active
- **Target**: Telegram notifications

#### Alert Flow
```
Alertmanager → n8n Webhook → Format Alert → Telegram Send
```

#### Alert Types
- **Critical**: Container down, high CPU/memory, disk space
- **Warning**: Performance degradation, backup failures
- **Recovery**: Service restoration notifications

## Legacy Workflows (Unpublished)

### 5. Update Nextcloud File
- **Purpose**: Single file updates to Nextcloud (superseded by documentation pipeline)
- **Trigger**: Webhook
- **Node Count**: 5 nodes
- **Status**: 🔄 Unpublished (Legacy)
- **Replacement**: Documentation Pipeline - Update workflow

### 6. Push to GitHub
- **Purpose**: Direct GitHub commits (superseded by documentation pipeline)
- **Trigger**: Webhook  
- **Node Count**: 4 nodes
- **Status**: 🔄 Unpublished (Legacy)
- **Replacement**: Documentation Pipeline workflows

### 7. Test SQLite
- **Purpose**: Database testing and development
- **Status**: 🗑️ Pending Deletion
- **Note**: No longer needed, scheduled for cleanup

## Credential Dependencies

### Required Credentials Per Workflow

| Workflow                    | Telegram | Claude | GitHub | Nextcloud | SSH | Proxmox |
|-----------------------------|---------:|-------:|-------:|----------:|----:|--------:|
| Telegram Agent              |    ✅    |   ✅   |   ✅   |     ✅    | ✅  |   ✅    |
| Documentation Pipeline - Update |   ✅    |   ✅   |   ✅   |     ✅    | ❌  |   ❌    |
| Documentation Pipeline - Sync |   ✅    |   ✅   |   ✅   |     ✅    | ❌  |   ❌    |
| Service Down Alert          |    ✅    |   ❌   |   ❌   |     ❌    | ❌  |   ❌    |

### Credential Details
- **Telegram Bot**: @JhinGilgamesh_bot token
- **Claude API**: Anthropic API key (tier 1)
- **GitHub API**: Personal access token (muzakkir97 account)
- **Nextcloud**: App password (n8n-doc-pipeline)
- **SSH Kuromoon**: Key-based authentication (root@192.168.10.5)
- **SSH CT302**: Password authentication (root@192.168.30.212)
- **Proxmox API**: root@pam!gilgamesh token

## Technical Implementation Notes

### n8n Data Tables

| Table Name        | Purpose              | Columns                                    |
|-------------------|----------------------|--------------------------------------------|
| gilgamesh_memory  | Conversation history | message_id, chat_id, role, content, timestamp |
| gilgamesh_costs   | Token usage tracking | request_id, model, input_tokens, output_tokens, cost_usd |

### API Integration Patterns

#### Proxmox API Calls
```javascript
// Container status
GET /api2/json/nodes/{node}/lxc

// Storage information  
GET /api2/json/nodes/{node}/storage/{storage}/status
```

#### GitHub API Pattern
```javascript
// File update
PUT /repos/muzakkir97/homelab-infrastructure/contents/{path}
{
  "message": "Update via n8n pipeline",
  "content": base64_encode(file_content),
  "sha": current_file_sha
}
```

#### Claude API Structure
```javascript
{
  "model": "claude-haiku-4-5-20251001",
  "max_tokens": 4096,
  "messages": conversation_history,
  "tools": [{"type": "web_search"}]
}
```

### SSH Command Patterns

#### Hardware Temperature Monitoring
```bash
# Kuromoon temperatures
sensors | grep -E "(Core|edge)" | awk '{print $1" "$2" "$3}'
```

#### Docker Container Status
```bash
# Gaming servers on CT 302
docker ps --format "table {{.Names}}\t{{.Status}}"
```

## Error Handling & Recovery

### Common Issues and Solutions

| Issue | Workflow | Resolution |
|-------|----------|------------|
| Claude API 404 | Telegram Agent | Use exact model ID: claude-haiku-4-5-20251001 |
| n8n Data Table empty response | Telegram Agent | Enable "Always Output Data" on DB nodes |
| SSH connection timeout | Telegram Agent | Verify pfSense firewall rules (port 22) |
| GitHub 401 authentication | Documentation Pipeline | Ensure username field populated |
| Nextcloud 401 unauthorized | Documentation Pipeline | Regenerate app password |
| Telegram JSON parsing error | All workflows | Avoid backticks and complex formatting |

### Monitoring and Alerting

#### Workflow Health Checks
- **Telegram Agent**: Response time monitoring via `/cost` command
- **Documentation Pipeline**: GitHub commit verification
- **Service Alerts**: Alertmanager integration status

#### Performance Metrics
- **Average Response Time**: 3-8 seconds (model dependent)
- **Success Rate**: 99.2% (excluding network timeouts)
- **Memory Usage**: ~200MB for n8n container
- **Storage Growth**: ~1MB/month for conversation logs

## Development Best Practices

### Node Configuration Standards
1. **Always Output Data**: Enabled on all database and API nodes
2. **Error Handling**: Try-catch blocks on external API calls
3. **Timeout Settings**: 30 seconds for Claude API, 10 seconds for others
4. **Credential Scoping**: Minimum required permissions

### Testing Procedures
1. **Isolated Testing**: Use test webhooks before production deployment
2. **Data Validation**: Verify JSON structure before API calls  
3. **Rollback Strategy**: Keep previous workflow versions published
4. **Monitoring**: Enable execution logging for debugging

### Security Considerations
- **Credential Rotation**: Monthly for high-privilege tokens
- **Webhook Validation**: IP whitelist for external triggers
- **Data Sanitization**: Remove sensitive info from logs
- **Access Control**: n8n admin panel behind Cloudflare Access

## Future Enhancements

### Planned Workflow Additions
- **MERLIN Reminder Agent**: SSL renewal, backup verification alerts
- **Monthly Infrastructure Audit**: Automated system health reports
- **Gaming Discord Bot**: Server management for gaming friends
- **AI News Scraper**: RSS/Reddit monitoring with Ollama evaluation

### Optimization Targets
- **Workflow Consolidation**: Merge similar functionalities
- **Response Caching**: Cache repeated API responses
- **Batch Processing**: Group similar operations
- **Local LLM Integration**: Reduce external API dependency (Phase 38)