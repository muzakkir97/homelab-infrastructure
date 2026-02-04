# Phase 5 Planning: Monitoring & Observability Stack

## Document Information

| Field | Value |
|-------|-------|
| **Phase** | 5 - Monitoring & Observability |
| **Status** | 📋 Planning Complete |
| **Created** | February 2026 |
| **Prerequisites** | Phase 4 Complete (Pi-hole, NPM, Tailscale) |
| **Estimated Duration** | 8-12 hours |

---

## Executive Summary

Phase 5 implements a comprehensive monitoring and observability stack for the homelab infrastructure. This includes metrics collection (Prometheus), visualization (Grafana), log aggregation (Loki), alerting (Alertmanager), and service availability monitoring (Uptime Kuma). Additionally, Dynamic DNS will be configured to handle ISP-assigned dynamic IP addresses.

### Why Monitoring Matters

Without monitoring, you're flying blind. As infrastructure grows, questions like "Why is this service slow?" or "Which container is using all the RAM?" become impossible to answer without proper observability. This phase provides:

- **Real-time visibility** into all infrastructure components
- **Historical data** for trend analysis and capacity planning
- **Proactive alerting** before issues impact users
- **Centralized logging** for troubleshooting and security analysis
- **Service availability tracking** with uptime percentages

---

## Architecture Overview

### Network Placement

All monitoring components deploy to **VLAN 20 (Homelab Services)** alongside Pi-hole. This decision was made because:

1. Monitoring is an internal service, not exposed to the internet (except Grafana dashboard)
2. Avoids VLAN sprawl - consolidates related services
3. Firewall rules already permit VLAN 20 to reach other VLANs for metric collection

### Component Architecture

```
┌─────────────────────────────────────────────────────────────────────────┐
│                        VLAN 20 (Homelab Services)                       │
│                           192.168.20.0/24                               │
│                                                                         │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐            │
│  │    Pi-hole     │  │   Prometheus   │  │    Grafana     │            │
│  │ 192.168.20.10  │  │ 192.168.20.11  │  │ 192.168.20.12  │◄─── HTTPS  │
│  │   (existing)   │  │    CT 202      │  │    CT 203      │    via NPM │
│  └────────────────┘  └───────┬────────┘  └───────┬────────┘            │
│                              │                   │                      │
│                              │ Queries           │ Queries              │
│                              ▼                   ▼                      │
│  ┌────────────────┐  ┌────────────────┐  ┌────────────────┐            │
│  │      Loki      │  │  Alertmanager  │  │  Uptime Kuma   │            │
│  │ 192.168.20.13  │  │ 192.168.20.14  │  │ 192.168.20.15  │            │
│  │    CT 204      │  │    CT 205      │  │    CT 206      │            │
│  └───────┬────────┘  └───────┬────────┘  └────────────────┘            │
│          │                   │                                          │
│          │ Receives          │ Sends Alerts                            │
│          │ Logs              ▼                                          │
│          │           ┌──────────────┐                                   │
│          │           │  Telegram &  │                                   │
│          │           │   Discord    │                                   │
│          │           └──────────────┘                                   │
└──────────┼──────────────────────────────────────────────────────────────┘
           │
   ┌───────┴───────────────────────────────────────────────┐
   │              Promtail Agents (Log Shippers)           │
   │  Installed on: Proxmox, NPM, all monitored hosts      │
   └───────────────────────────────────────────────────────┘
```

### Data Flow

```
                    METRICS FLOW
┌─────────────┐    scrape     ┌─────────────┐    query    ┌─────────────┐
│ Exporters   │──────────────►│ Prometheus  │◄────────────│  Grafana    │
│ (node_exp,  │   :9090       │             │             │             │
│  pihole,etc)│               │  stores     │             │ visualizes  │
└─────────────┘               │  metrics    │             │  dashboards │
                              └──────┬──────┘             └─────────────┘
                                     │
                                     │ evaluates rules
                                     ▼
                              ┌─────────────┐    routes    ┌─────────────┐
                              │Alertmanager │─────────────►│ Telegram/   │
                              │             │   alerts     │ Discord     │
                              └─────────────┘              └─────────────┘

                    LOGS FLOW
┌─────────────┐    push       ┌─────────────┐    query    ┌─────────────┐
│  Promtail   │──────────────►│    Loki     │◄────────────│  Grafana    │
│  (agents)   │   :3100       │             │             │             │
│             │               │  indexes &  │             │  explores   │
└─────────────┘               │  stores     │             │  logs       │
                              └─────────────┘             └─────────────┘
```

---

## Resource Allocation

### Container Specifications

| Container | Hostname | CT ID | IP Address | CPU | RAM | Disk | Purpose |
|-----------|----------|-------|------------|-----|-----|------|---------|
| Prometheus | monitoring-prometheus | 202 | 192.168.20.11 | 2 | 2GB | 50GB | Metrics database |
| Grafana | monitoring-grafana | 203 | 192.168.20.12 | 1 | 1GB | 10GB | Visualization |
| Loki | monitoring-loki | 204 | 192.168.20.13 | 2 | 2GB | 50GB | Log aggregation |
| Alertmanager | monitoring-alertmanager | 205 | 192.168.20.14 | 1 | 512MB | 5GB | Alert routing |
| Uptime Kuma | monitoring-uptime | 206 | 192.168.20.15 | 1 | 512MB | 5GB | Availability |

**Total Resources:** 7 CPU cores, 6GB RAM, 120GB disk

### Host System Impact

| Resource | Total Available | Phase 5 Usage | Remaining |
|----------|-----------------|---------------|-----------|
| CPU Threads | 12 | 7 | 5 |
| RAM | 32GB | 6GB | 26GB |
| Storage | Varies | 120GB | Depends on pool |

---

## Implementation Phases

### Phase 5A: Prometheus & Node Exporters

**Objective:** Deploy Prometheus and install node_exporter on all hosts

**Components:**
- Prometheus server (LXC container)
- node_exporter on Proxmox host
- node_exporter on all existing containers

**Scrape Targets:**

| Target | Endpoint | Metrics |
|--------|----------|---------|
| Proxmox Host | 192.168.1.5:9100 | CPU, RAM, disk, network, ZFS |
| Pi-hole | 192.168.20.10:9100 | Host metrics |
| NPM Container | 192.168.30.10:9100 | Container metrics |
| Prometheus (self) | localhost:9090 | Prometheus internals |

**Success Criteria:**
- [ ] Prometheus UI accessible at 192.168.20.11:9090
- [ ] All scrape targets showing "UP" status
- [ ] Basic queries returning data (e.g., `node_cpu_seconds_total`)

---

### Phase 5B: Grafana & Dashboards

**Objective:** Deploy Grafana, connect to Prometheus, import dashboards, configure external access

**Components:**
- Grafana server (LXC container)
- Prometheus data source configuration
- Pre-built dashboard imports
- NPM reverse proxy configuration
- Let's Encrypt SSL certificate
- TOTP 2FA configuration

**Dashboards to Import:**

| Dashboard Name | Grafana ID | Purpose |
|----------------|------------|---------|
| Node Exporter Full | 1860 | Comprehensive host metrics |
| Proxmox VE | 10347 | Proxmox-specific metrics |
| Docker and System Monitoring | 893 | Container overview |

**External Access Configuration:**

| Setting | Value |
|---------|-------|
| Domain | grafana.najhin-gaming.com |
| Proxy Host | NPM → 192.168.20.12:3000 |
| SSL | Let's Encrypt |
| Authentication | Grafana built-in + TOTP 2FA |

**Success Criteria:**
- [ ] Grafana UI accessible at 192.168.20.12:3000
- [ ] Prometheus data source connected and working
- [ ] Node Exporter dashboard showing real data
- [ ] External access via https://grafana.najhin-gaming.com working
- [ ] 2FA enabled and tested

---

### Phase 5C: Loki & Log Aggregation

**Objective:** Deploy Loki for centralized logging, install Promtail agents

**Components:**
- Loki server (LXC container)
- Promtail agent on Proxmox host
- Promtail agent on NPM container
- Grafana data source for Loki

**Log Sources:**

| Source | Log Path | Content |
|--------|----------|---------|
| Proxmox | /var/log/syslog | System logs |
| Proxmox | /var/log/pve/* | PVE task logs |
| pfSense | Syslog UDP | Firewall logs |
| NPM | /data/logs/* | Access & error logs |
| Pi-hole | /var/log/pihole.log | DNS query logs |

**Retention Policy:**
- Log retention: 14 days
- Chunk retention: 14 days
- Index retention: 14 days

**Success Criteria:**
- [ ] Loki UI accessible (via Grafana Explore)
- [ ] Promtail agents pushing logs
- [ ] Log queries returning results in Grafana
- [ ] pfSense syslog integration working

---

### Phase 5D: Alertmanager & Notifications

**Objective:** Deploy Alertmanager, configure Telegram and Discord notifications

**Components:**
- Alertmanager server (LXC container or alongside Prometheus)
- Telegram bot configuration
- Discord webhook configuration
- Alert rules in Prometheus

**Alert Rules:**

| Alert Name | Condition | Severity | Description |
|------------|-----------|----------|-------------|
| HighCPU | CPU > 80% for 5m | warning | Host CPU usage high |
| HighMemory | RAM > 90% for 5m | warning | Host memory usage high |
| DiskSpaceLow | Disk > 85% | warning | Disk space running low |
| DiskSpaceCritical | Disk > 95% | critical | Disk space critical |
| TargetDown | Target unreachable 2m | critical | Monitoring target offline |
| ZFSPoolDegraded | Pool != healthy | critical | ZFS pool degraded |
| ServiceDown | HTTP check fails | critical | Service unavailable |

**Notification Routing:**

| Severity | Destinations | Timing |
|----------|--------------|--------|
| critical | Telegram + Discord | Immediate |
| warning | Discord only | 5 minute delay |
| info | Discord only | 15 minute delay |

**Telegram Bot Setup Steps:**
1. Message @BotFather on Telegram
2. Send `/newbot` command
3. Choose bot name and username
4. Save the API token
5. Message your bot to start conversation
6. Get chat ID via API call

**Discord Webhook Setup Steps:**
1. Open Discord server settings
2. Navigate to Integrations → Webhooks
3. Create new webhook
4. Choose channel for alerts
5. Copy webhook URL

**Success Criteria:**
- [ ] Alertmanager UI accessible at 192.168.20.14:9093
- [ ] Test alert successfully sent to Telegram
- [ ] Test alert successfully sent to Discord
- [ ] Alert rules loaded in Prometheus
- [ ] Silencing and inhibition rules configured

---

### Phase 5E: Uptime Kuma

**Objective:** Deploy Uptime Kuma for service availability monitoring

**Components:**
- Uptime Kuma server (LXC container)
- Service monitors configuration
- Status page (optional)

**Monitors to Configure:**

| Service | Type | Target | Interval |
|---------|------|--------|----------|
| Proxmox Web UI | HTTPS | 192.168.1.5:8006 | 60s |
| pfSense Web UI | HTTPS | 192.168.1.1:443 | 60s |
| Pi-hole Admin | HTTP | 192.168.20.10/admin | 60s |
| NPM Admin | HTTP | 192.168.30.10:81 | 60s |
| Grafana | HTTP | 192.168.20.12:3000 | 60s |
| Prometheus | HTTP | 192.168.20.11:9090 | 60s |
| External - Proxmox | HTTPS | proxmox.najhin-gaming.com | 60s |
| External - Pi-hole | HTTPS | pihole.najhin-gaming.com | 60s |
| DNS Resolution | DNS | Pi-hole → google.com | 60s |
| Internet | Ping | 1.1.1.1 | 30s |

**Notification Integration:**
- Connect to same Telegram bot as Alertmanager
- Connect to same Discord webhook

**Success Criteria:**
- [ ] Uptime Kuma UI accessible at 192.168.20.15:3001
- [ ] All monitors configured and showing status
- [ ] Notifications working for down/up events
- [ ] Historical uptime data being recorded

---

### Phase 5F: Dynamic DNS (DDNS)

**Objective:** Configure automatic DNS updates when public IP changes

**Background:**
ISP provides dynamic IP that changes approximately every 2 weeks. Without DDNS, external access breaks when IP changes.

**Implementation:** pfSense built-in DDNS client with Cloudflare

**Configuration:**

| Setting | Value |
|---------|-------|
| Service Type | Cloudflare |
| Hostname | @ (root) or specific subdomain |
| Domain | najhin-gaming.com |
| API Token | Cloudflare API token (Zone:DNS:Edit) |
| Update Interval | 300 seconds (5 minutes) |
| Interface | WAN |

**Cloudflare API Token Permissions:**
- Zone: DNS: Edit
- Zone: Zone: Read
- Zone Resources: Include specific zone (najhin-gaming.com)

**Success Criteria:**
- [ ] DDNS service showing "Updated" in pfSense
- [ ] Manual IP change test successful
- [ ] Cloudflare DNS record matches current public IP
- [ ] External access continues working after IP change

---

### Phase 5G: Additional Exporters

**Objective:** Install specialized exporters for pfSense, Pi-hole, and switches

**Exporters:**

| Exporter | Target | Metrics |
|----------|--------|---------|
| pfsense-exporter | pfSense | Firewall states, interface traffic, CPU |
| pihole-exporter | Pi-hole | Queries, blocks, cache, top clients |
| snmp_exporter | TP-Link Switch | Port traffic, errors (if SNMP available) |

**pfSense Exporter Options:**
1. **Built-in Telegraf package** - Sends to Prometheus
2. **External exporter** - Runs separately, scrapes pfSense API

**Pi-hole Exporter:**
- Uses Pi-hole API (no authentication needed for read)
- Provides: queries today, blocked today, percentage blocked, domains on blocklist

**Success Criteria:**
- [ ] pfSense metrics visible in Prometheus
- [ ] Pi-hole metrics visible in Prometheus
- [ ] Dedicated Grafana dashboards for each
- [ ] Network traffic per VLAN visible

---

## Security Considerations

### External Access Security (Grafana)

| Layer | Implementation |
|-------|----------------|
| Transport | TLS 1.3 via Let's Encrypt |
| Authentication | Grafana built-in auth |
| 2FA | TOTP (Google Authenticator compatible) |
| Rate Limiting | NPM proxy settings |
| Session | 24-hour timeout |

### Internal Network Security

| Consideration | Implementation |
|---------------|----------------|
| Container Isolation | Each service in separate LXC |
| Network Segmentation | Monitoring in VLAN 20, firewall rules control access |
| Credential Storage | Alertmanager secrets in config files with restricted permissions |
| API Security | Prometheus/Loki APIs only accessible from VLAN 20 |

### Firewall Rules Required

| Source | Destination | Port | Purpose |
|--------|-------------|------|---------|
| VLAN 20 | VLAN 1 (Proxmox) | 9100 | node_exporter scrape |
| VLAN 20 | VLAN 1 (pfSense) | 9100 | pfSense exporter |
| VLAN 20 | VLAN 30 (NPM) | 9100 | NPM node_exporter |
| VLAN 30 | VLAN 20 (Grafana) | 3000 | NPM reverse proxy |
| Any | VLAN 20 (Prometheus) | 9090 | Prometheus UI (internal) |
| Internet | VLAN 30 (NPM) | 443 | Grafana external access |

---

## Data Retention & Storage

### Prometheus Retention

```yaml
# prometheus.yml storage settings
storage:
  tsdb:
    retention.time: 30d
    retention.size: 45GB
```

### Loki Retention

```yaml
# loki-config.yml
limits_config:
  retention_period: 336h  # 14 days

compactor:
  retention_enabled: true
  retention_delete_delay: 2h
```

### Storage Monitoring

Alert when storage approaches limits:
- Prometheus: Alert at 40GB used (80% of 50GB allocation)
- Loki: Alert at 40GB used (80% of 50GB allocation)

---

## Verification Checklist

### Phase 5 Complete When:

**Infrastructure:**
- [ ] All 5 containers running and healthy
- [ ] All containers accessible via assigned IPs
- [ ] Container resource usage within allocation

**Metrics (Prometheus):**
- [ ] Prometheus UI accessible
- [ ] All scrape targets UP
- [ ] Metrics queryable via PromQL
- [ ] 30-day retention configured

**Visualization (Grafana):**
- [ ] Grafana UI accessible internally
- [ ] Grafana accessible via https://grafana.najhin-gaming.com
- [ ] 2FA enabled and working
- [ ] Prometheus data source connected
- [ ] Loki data source connected
- [ ] Node Exporter dashboard working
- [ ] Custom dashboards created

**Logging (Loki):**
- [ ] Loki receiving logs
- [ ] Promtail agents running on all hosts
- [ ] Logs queryable in Grafana Explore
- [ ] 14-day retention configured

**Alerting:**
- [ ] Alertmanager receiving alerts from Prometheus
- [ ] Telegram notifications working
- [ ] Discord notifications working
- [ ] Alert rules firing correctly
- [ ] Silence/inhibit rules configured

**Availability (Uptime Kuma):**
- [ ] All services monitored
- [ ] Historical data recording
- [ ] Down/up notifications working

**DDNS:**
- [ ] Cloudflare DDNS configured in pfSense
- [ ] Automatic updates working
- [ ] External access survives IP change

---

## Rollback Plan

If issues occur during deployment:

1. **Container Issues:** Delete container, recreate from template
2. **Network Issues:** Verify VLAN configuration, check firewall rules
3. **Storage Issues:** Reduce retention periods, add storage
4. **External Access Issues:** Verify NPM configuration, check Cloudflare DNS

---

## Future Enhancements (Post Phase 5)

| Enhancement | Description | Phase |
|-------------|-------------|-------|
| Grafana OAuth | SSO with GitHub/Google | Future |
| Alert Escalation | PagerDuty integration | Future |
| Long-term Storage | Thanos/Cortex for years of metrics | Future |
| Log Analysis | ML-based anomaly detection | Future |
| Status Page | Public uptime status page | Future |

---

## References

- [Prometheus Documentation](https://prometheus.io/docs/)
- [Grafana Documentation](https://grafana.com/docs/)
- [Loki Documentation](https://grafana.com/docs/loki/)
- [Alertmanager Documentation](https://prometheus.io/docs/alerting/alertmanager/)
- [Uptime Kuma GitHub](https://github.com/louislam/uptime-kuma)
- [pfSense DDNS Documentation](https://docs.netgate.com/pfsense/en/latest/services/dyndns/index.html)

---

## Document History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | February 2026 | Initial planning document |
