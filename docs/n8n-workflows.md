# n8n Workflow Documentation

## Overview

n8n automation platform deployed on CT 211 (192.168.30.211) serving as the central automation hub for homelab infrastructure. All workflows focus on Gilgamesh AI agent functionality, documentation pipeline, and legacy integrations.

## Active Workflows

### 1. Telegram Agent (Primary Workflow)
- **Purpose:** Main Gilgamesh AI bot with chat, menu system, and slash commands
- **Trigger Type:** Telegram Bot (webhook)
- **Node Count:** 15
- **Status:** ✅ Active (Published)

#### Architecture Flow
```
Telegram Trigger → Message Router → Smart Model Selection → Claude API → Response Formatting → Memory Storage → Telegram Response
                      ↓
                Callback Router → Menu Actions → API Calls → Format Results → Update Message
                      ↓
                Command Router → Slash Commands → Execute Action → Format Response
```

#### Key Nodes
1. **Telegram Trigger:** Handles all update types (messages + callbacks)
2. **Message/Callback Router:** Separates message types and callback queries
3. **Smart Routing Logic:** Haiku vs Sonnet model selection
4. **Claude API Integration:** Main AI processing with web search
5. **Memory Management:** Store/retrieve last 20 messages from Data Tables
6. **Cost Tracking:** Token usage logging (with known cost_usd bug)
7. **Menu System:** Inline keyboard with homelab/gaming/tools submenus
8. **Command Handler:** 8 slash commands (/help, /clear, /memory, etc.)
9. **API Integrations:** Proxmox, SSH, system monitoring

#### Credentials Required
- **Telegram Bot Token:** @JhinGilgamesh_bot authentication
- **Claude API Key:** Anthropic API access
- **Proxmox API Token:** root@pam!gilgamesh
- **SSH Keys:** Kuromoon key-based, CT302 password-based
- **n8n Database:** Local SQLite for Data Tables

#### Known Issues
- **Message Format:** Only plain text supported (JSON parsing constraint)
- **Cost Tracking:** cost_usd column always returns 0
- **Memory Persistence:** Lost on n8n restart
- **Single Webhook:** All Telegram updates must use one trigger

### 2. Documentation Pipeline - Update
- **Purpose:** Process /update commands - merge session summaries into 3 core docs
- **Trigger Type:** Webhook (doc-update)
- **Node Count:** 7
- **Status:** ✅ Active (Published)

#### Architecture Flow
```
Webhook Trigger → Extract Summary → Claude Processing → File Generation → Nextcloud Upload → GitHub Push → Success Response
```

#### Files Updated (3 Total)
1. **AI-CONTEXT.md:** Master context document
2. **changelog.md:** Version history and changes
3. **troubleshoot.md:** Lessons learned and error resolutions

#### Key Nodes
1. **Webhook Trigger:** Receives session summary from Telegram workflow
2. **Claude API:** Intelligent document merging
3. **File Generation:** Create updated content for each file
4. **Nextcloud HTTP Request:** Upload to cloud storage
5. **GitHub API:** Push changes to repository
6. **Response Handler:** Confirm success to Telegram

#### Credentials Required
- **Claude API Key:** Document processing
- **Nextcloud App Password:** n8n-doc-pipeline
- **GitHub API Token:** muzakkir97 with repo access
- **Webhook Authentication:** Shared secret with main workflow

### 3. Documentation Pipeline - Sync Docs
- **Purpose:** Full documentation regeneration for /sync-docs command
- **Trigger Type:** Webhook (doc-sync)
- **Node Count:** 7
- **Status:** ✅ Active (Published)

#### Architecture Flow
```
Webhook Trigger → Load Context → Claude Processing → Generate All Docs → Batch Upload → GitHub Push → Completion Report
```

#### Files Updated (7 Total)
1. **AI-CONTEXT.md** (from update pipeline)
2. **changelog.md** (from update pipeline)  
3. **troubleshoot.md** (from update pipeline)
4. **README.md** (project overview)
5. **roadmap.md** (phase planning)
6. **current-state.md** (infrastructure status)
7. **service-catalog.md** (container inventory)

#### Key Nodes
1. **Webhook Trigger:** Receives full sync request
2. **Context Loader:** Read current AI-CONTEXT.md
3. **Claude API:** Generate complete documentation set
4. **Document Generator:** Create all 7 files with sanitized content
5. **Batch Uploader:** Multiple Nextcloud/GitHub operations
6. **Status Reporter:** Progress updates to Telegram

#### Credentials Required
- **Same as Update Pipeline**
- **Extended GitHub Access:** Multiple file operations
- **Rate Limiting:** Handle multiple API calls

### 4. Service Down Alert (Monitoring Integration)
- **Purpose:** Critical service failure notifications from Alertmanager
- **Trigger Type:** HTTP Webhook from Alertmanager CT 205
- **Node Count:** 8
- **Status:** ✅ Active (Published)

#### Architecture Flow
```
Alertmanager Webhook → Alert Parser → Severity Filter → Format Message → Telegram Notification → Discord Webhook → Log Entry
```

#### Alert Categories
- **Critical:** Service completely down (Telegram + Discord)
- **Warning:** Performance degradation (Discord only)
- **Info:** Maintenance notifications (Discord only)

#### Key Nodes
1. **Webhook Receiver:** Alertmanager JSON payload
2. **Alert Parser:** Extract service, severity, description
3. **Severity Router:** Route based on alert level
4. **Message Formatter:** Create human-readable notifications
5. **Telegram Sender:** Critical alerts to Gilgamesh chat
6. **Discord Webhook:** All alerts to #alerts channel
7. **Alert Logger:** Store in n8n for trend analysis

#### Integration Points
- **Alertmanager:** CT 205 webhook configuration
- **Telegram:** Same bot as Gilgamesh (@JhinGilgamesh_bot)
- **Discord:** Gaming server #alerts channel
- **Prometheus:** Alert rule sources

### 5. Update Nextcloud File (Legacy)
- **Purpose:** Original file update mechanism before pipeline redesign
- **Trigger Type:** Webhook
- **Node Count:** 5
- **Status:** 🟡 Inactive (Unpublished, kept for reference)

#### Replacement
- Superseded by "Documentation Pipeline - Update"
- Maintained for rollback if needed
- No active triggers or credentials

### 6. Push to GitHub (Legacy)
- **Purpose:** Original GitHub push mechanism before pipeline integration
- **Trigger Type:** Webhook  
- **Node Count:** 4
- **Status:** 🟡 Inactive (Unpublished, kept for reference)

#### Replacement
- Functionality integrated into documentation pipelines
- Simpler node structure but less robust
- Credential configuration preserved

### 7. Test SQLite (Development)
- **Purpose:** n8n Data Tables testing and debugging
- **Trigger Type:** Manual
- **Node Count:** 3
- **Status:** 🔴 Pending Deletion

## Workflow Dependencies

### Credential Management
| Credential Name | Type | Used By | Purpose |
|-----------------|------|---------|---------|
| Telegram Bot Token | Generic Auth | Telegram Agent, Service Alerts | @JhinGilgamesh_bot access |
| Claude API | Generic Auth | All documentation pipelines | Anthropic API |
| Proxmox API Token | Generic Auth | Telegram Agent | Infrastructure monitoring |
| SSH Password account | SSH | Telegram Agent | Kuromoon management |  
| SSH CT302 Wings | SSH | Telegram Agent | Gaming server access |
| Nextcloud App Password | Generic Auth | Documentation pipelines | File storage |
| GitHub API Token | Generic Auth | Documentation pipelines | Repository access |
| Discord Webhook | Webhook | Service Alerts | Gaming server notifications |

### n8n Data Tables
| Table Name | Schema | Purpose | Retention |
|------------|--------|---------|-----------|
| gilgamesh_memory | id, chat_id, role, content, timestamp, tokens_used | Conversation history | 20 messages |
| gilgamesh_costs | id, timestamp, model, input_tokens, output_tokens, cost_usd | Usage tracking | Unlimited |

#### Known Data Issues
- **cost_usd Column:** Always returns 0 (bug from earlier implementation)
- **Memory Persistence:** Lost on container restart
- **Schema Evolution:** Manual table recreation required for structure changes

## Workflow Troubleshooting

### Common Issues and Resolutions

#### Telegram Workflow Issues
| Issue | Symptom | Resolution |
|-------|---------|------------|
| Claude API 404 | "Model not found" error | Use exact model ID: `claude-haiku-4-5-20251001` |
| JSON parsing error | Webhook fails on complex formatting | Strip markdown/code blocks before Claude API |
| Memory table empty | "At least one condition required" | Enable "Always Output Data" on DB nodes |
| Callback query timeout | Menu buttons unresponsive | Handle callback_query acknowledgment separately |
| SSH connection hang | Workflow stalls on system commands | Verify pfSense rules allow VLAN30→VLAN10 port 22 |

#### Documentation Pipeline Issues  
| Issue | Symptom | Resolution |
|-------|---------|------------|
| Nextcloud upload 401 | Authentication failed | Regenerate app password, ensure "n8n-doc-pipeline" |
| GitHub push rejected | 403/404 errors | Verify token has repo access, user field populated |
| Claude context overflow | Truncated responses | Split large documents, process in chunks |
| File corruption | Malformed JSON/YAML | Add content validation before upload |
| Webhook timeout | No response from trigger | Increase timeout, add retry logic |

#### Infrastructure Dependencies
| Issue | Symptom | Resolution |
|-------|---------|------------|
| CT 211 container down | All workflows offline | Check autostart, restart container |
| Network connectivity | API calls failing | Verify VLAN 30 internet access, DNS resolution |
| Proxmox API timeout | Monitoring data stale | Check Proxmox certificate, API token expiry |
| SSH key authentication | Permission denied errors | Regenerate keys, verify authorized_keys |
| Data Tables corruption | n8n startup errors | Recreate tables from scratch, lose history |

### Monitoring and Maintenance

#### Workflow Health Checks
- **Execution History:** Review failed/successful runs daily
- **Credential Expiry:** Rotate API keys quarterly
- **Performance:** Monitor response times and timeout rates
- **Data Tables:** Check memory growth and cleanup old entries

#### Scheduled Maintenance
- **Weekly:** Review workflow execution logs
- **Monthly:** Update n8n container and node packages  
- **Quarterly:** Credential rotation and security review
- **Annually:** Complete workflow architecture review

### Best Practices Learned

#### Workflow Design
- **Single Telegram Webhook:** All update types must use one trigger
- **Error Handling:** Always enable "Always Output Data" for conditional flows
- **Credential Security:** Use app passwords instead of main passwords
- **Node Naming:** Descriptive names for debugging
- **Documentation:** Comment complex expressions and routing logic

#### API Integration
- **Rate Limiting:** Implement delays between API calls
- **Retry Logic:** Exponential backoff for failed requests
- **Response Validation:** Check API response structure before processing
- **Token Management:** Monitor usage against quotas
- **Timeout Handling:** Set appropriate timeouts for external services

#### Data Management
- **Table Design:** Plan schema evolution carefully
- **Data Retention:** Implement cleanup for growing tables
- **Backup Strategy:** Export critical workflow data
- **Version Control:** Export workflows for external backup
- **Migration Planning:** Test data migration procedures

## Future Enhancements

### Planned Workflow Additions
1. **MERLIN Reminder Agent:** Proactive maintenance scheduling
2. **Mash Discord Bot:** Gaming server management
3. **Infrastructure Audit:** Monthly automated health checks
4. **Cost Optimization:** Resource usage analysis and recommendations
5. **Security Monitoring:** Automated threat detection and response

### Architecture Improvements
1. **Centralized Alerting:** Route all notifications through n8n
2. **Workflow Templates:** Standardized patterns for new integrations
3. **Error Handling:** Comprehensive failure recovery
4. **Performance Optimization:** Reduce API calls and improve response times
5. **Multi-tenancy:** Separate workflows for different service categories

### Integration Roadmap
1. **Obsidian Sync:** Knowledge base integration
2. **Grafana Annotations:** Automated event marking
3. **Vault Integration:** Dynamic secret retrieval
4. **Backup Automation:** Intelligent backup scheduling
5. **Service Discovery:** Automatic endpoint monitoring