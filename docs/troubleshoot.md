# Troubleshooting Log

> **Format:** Newest first, append-only

---

## 2026-04-24 — kinmoon Storage ID Incorrect in Proxmox

**Symptoms:** Gilgamesh Storage menu showing "kinmoon-smb: N/A" instead of actual storage usage statistics.

**Root Cause:** AI-CONTEXT.md documented the NAS storage as "kinmoon-smb" but the actual Proxmox storage ID is "kinmoon-nfs". The Proxmox API call was requesting the wrong storage identifier.

**Resolution:** Updated n8n workflow to use correct storage ID "kinmoon-nfs" in the Proxmox API request for storage information.

**Verification:** Tested Gilgamesh Storage menu - now correctly displays kinmoon-nfs usage statistics.

**Lesson:** Always verify actual storage IDs in Proxmox web interface before hardcoding them in API calls. Storage names in documentation may not match actual Proxmox configuration.

---

## 2026-04-24 — n8n Command Field Parsing Expressions Incorrectly

**Symptoms:** SSH command `docker ps --format "table {{.Names}}\t{{.Status}}"` failing with n8n syntax error, interpreting `{{.Names}}` as n8n expression instead of Docker Go template.

**Root Cause:** n8n Expression mode interprets any `{{}}` syntax as n8n expressions. Docker's Go template format conflicts with n8n's expression syntax.

**Resolution:** Changed Command field from "Expression" mode to "Fixed" mode in the SSH node configuration.

**Lesson:** When using external command templates (Docker, Kubernetes, etc.) that use `{{}}` syntax, always use "Fixed" mode in n8n to prevent expression parsing conflicts.

---

## 2026-04-24 — SSH Password Authentication Failing to CT 302

**Symptoms:** n8n SSH connection to CT 302 (gaming-wings-1) failing with "Permission denied (publickey)" error despite providing password.

**Root Cause:** Container SSH configuration had `PermitRootLogin prohibit-password` which only allows key-based authentication, not password authentication.

**Resolution:** Modified `/etc/ssh/sshd_config` on CT 302:
```bash
PermitRootLogin yes
systemctl restart sshd
```

**Verification:** SSH connection from n8n to CT 302 now works with password authentication.

**Lesson:** `PermitRootLogin prohibit-password` blocks password auth even when other password auth is enabled. Use `PermitRootLogin yes` for password-based root access.

---

## 2026-04-24 — pfSense Blocking SSH Between VLANs

**Symptoms:** SSH connection hanging when trying to connect from CT 211 (VLAN 30) to Kuromoon (VLAN 10) on port 22.

**Root Cause:** pfSense firewall rules were blocking inter-VLAN communication on port 22. No explicit rule allowed SSH traffic from VLAN 30 to VLAN 10.

**Resolution:** Added pfSense firewall rule:
- Interface: VLAN30_SERVICES  
- Action: Pass
- Protocol: TCP
- Source: Single host 192.168.30.211 (n8n container)
- Destination: Single host 192.168.10.5 (Kuromoon)
- Port: 22

**Verification:** SSH connection from CT 211 to Kuromoon now works successfully.

**Lesson:** Inter-VLAN communication requires explicit firewall rules. Default inter-VLAN rules may not cover specific service ports like SSH.

---

## 2026-04-24 — n8n Send Telegram Message Invalid JSON Body

**Symptoms:** n8n HTTP Request node for sending Telegram messages failing with "Invalid JSON" error when using Raw/JSON body mode.

**Root Cause:** Complex message formatting with dynamic content was causing JSON parsing issues in the HTTP Request node's JSON body field.

**Resolution:** Changed HTTP Request body mode from "Raw/JSON" to "Using Fields Below" and configured individual fields:
- chat_id: 518832696
- text: Dynamic content via expression
- parse_mode: HTML (if needed)

**Lesson:** For HTTP APIs with dynamic content, "Using Fields Below" mode is more reliable than raw JSON when dealing with complex message formatting and special characters.

---

## 2026-04-24 — n8n Callback Query Data Undefined

**Symptoms:** Telegram callback query processing showing `inputData.callback_query` as undefined when trying to access button press data.

**Root Cause:** Callback query data was not being properly extracted from the Telegram webhook payload in subsequent nodes after the Callback Router.

**Resolution:** Modified data access pattern to use:
```javascript
$('Callback Router').first().json.callback_query.data
```
Instead of:
```javascript
$json.callback_query.data
```

**Lesson:** In n8n workflows with multiple trigger types and routing nodes, always reference data from specific nodes using `$('NodeName').first().json` rather than assuming `$json` contains the expected data structure.

---

## 2026-04-23 — Claude Design Visual Content Strategy Learning

**Symptoms:** Need to understand how to effectively use Claude Design for homelab project without overwhelming the workflow or increasing costs significantly.

**Root Cause:** Claude Design is a new visual design tool (launched April 17, 2026) that requires strategic usage to be cost-effective. Unlimited automation would be expensive and unnecessary for most homelab documentation needs.

**Resolution:** Established clear usage strategy for Claude Design:
- **Limit to 1-2 projects per week maximum**
- **High-value content only:** LinkedIn posts, GitHub README visuals, milestone presentations
- **Do NOT automate:** Keep manual to control costs and ensure quality
- **Export artifacts:** Save PNG/SVG files for reuse across platforms
- **Phase 7F scope adjusted:** Automate text/code/Mermaid diagrams only, keep marketing graphics manual

**Verification:** Created homelab infrastructure diagram as test case - successful visual representation of network topology and container architecture.

**Lesson:** New AI tools like Claude Design require strategic integration rather than immediate automation. High-value, manual usage is more cost-effective than automated integration for visual content creation.

---

## 2026-04-23 — Claude Conversation Search Missing Specific Character Names

**Symptoms:** When searching for "Fate themed agent" or "agent ecosystem" in conversation history, Claude initially returned no results despite detailed discussion of Fate/Grand Order servant-themed agents on April 21-22, 2026.

**Root Cause:** Generic search terms failed to match specific character names and servant references in conversation. The discussion included specific servants like "Merlin," "Mash Kyrielight," "Da Vinci," and "EMIYA" but searching for broad themes missed these specific mentions.

**Resolution:** Used specific character/servant names when searching past conversations:
- "Merlin Chaldea" instead of "reminder agent"
- "Mash Kyrielight" instead of "gaming bot"
- "Fate Grand Order" instead of "themed agents"
- Specific servant class names (Caster, Shielder, Archer)

**Verification:** Re-searched with specific names and successfully located the complete agent ecosystem discussion from April 21-22.

**Lesson:** When searching conversation history for specific themed content, use proper nouns and specific terminology rather than generic descriptions. Character names, brand names, and technical terms are more searchable than broad concepts.

---

## 2026-04-19 — Documentation Pipeline Template Literal Syntax Error

**Symptoms:** n8n Documentation Pipeline - Sync Docs workflow failing with JavaScript syntax error when processing AI-CONTEXT.md content containing backticks.

**Root Cause:** AI-CONTEXT.md contains code blocks with backticks which break JavaScript template literal syntax when using `${variableName}` interpolation. Template literals are delimited by backticks, so internal backticks cause parsing errors.

**Resolution:** Replaced template literal string interpolation with string concatenation in the Code node:
```javascript
// Before (failing):
const content = `${aiContextContent}`;

// After (working):
const content = '' + aiContextContent;
```

**Lesson:** When processing file content that may contain backticks, avoid JavaScript template literals. Use string concatenation or other methods that don't conflict with backtick syntax.

---

## 2026-04-19 — n8n Switch Node Routing Miss for /sync-docs

**Symptoms:** /sync-docs command not triggering the correct workflow route in Telegram Agent Switch node, falling through to default case.

**Root Cause:** Switch node expression format inconsistency. Other commands used `$json.message?.text?.startsWith('/command')` but /sync-docs rule was missing optional chaining operator.

**Resolution:** Updated Switch node expression to match other patterns:
```javascript
// Before (not working):
$json.message.text.startsWith('/sync-docs')

// After (working):
$json.message?.text?.startsWith('/sync-docs')
```

**Lesson:** Maintain consistent expression patterns in Switch nodes. Optional chaining (`?.`) prevents errors when properties might be undefined and ensures reliable routing.

---

## 2026-04-19 — Telegram Node Bad Request in Documentation Pipeline

**Symptoms:** n8n Telegram node returning "Bad Request" error when trying to send confirmation messages for /update and /sync-docs commands.

**Root Cause:** n8n's built-in Telegram node had inconsistent behavior with message formatting and API calls in this specific workflow context.

**Resolution:** Replaced n8n Telegram node with direct HTTP Request node using Telegram Bot API:
```http
POST https://api.telegram.org/bot[BOT_TOKEN]/sendMessage
Content-Type: application/json

{
  "chat_id": "518832696",
  "text": "✅ Documentation pipeline triggered successfully"
}
```

**Lesson:** For critical workflows, direct HTTP API calls can be more reliable than abstracted nodes. The Telegram Bot API is straightforward and provides better error visibility.

---

## 2026-04-19 — Telegram Chat ID Incorrect (510832696 vs 518832696)

**Symptoms:** Telegram confirmation messages showing "chat not found" error when trying to send acknowledgments for pipeline commands.

**Root Cause:** Chat ID was incorrectly stored as 510832696 in the AI-CONTEXT.md document and workflow configuration. The correct Chat ID is 518832696.

**Resolution:** Updated Chat ID in both AI-CONTEXT.md documentation and all n8n workflow configurations to use 518832696.

**Verification:** Send test message via Telegram Bot API to confirm correct chat ID:
```bash
curl -X POST "https://api.telegram.org/bot[TOKEN]/sendMessage" \
     -H "Content-Type: application/json" \
     -d '{"chat_id":"518832696","text":"Test"}'
```

**Lesson:** Always verify Chat IDs when setting up Telegram integrations. Chat IDs can be obtained by messaging the bot and checking webhook logs or using `getUpdates` API endpoint.

---

## 2026-04-19 — Nextcloud Push 401 Unauthorized in Documentation Pipeline

**Symptoms:** Documentation Pipeline failing at Nextcloud push step with 401 Unauthorized error when trying to upload updated files.

**Root Cause:** App password stored in Vaultwarden for Nextcloud integration was incorrect or outdated. The password field contained the wrong credential.

**Resolution:** Generated new Nextcloud app password specifically for n8n documentation pipeline:
1. Nextcloud → Settings → Personal → Security → App passwords
2. Created new password named "n8n-doc-pipeline"
3. Updated n8n workflow HTTP Request node with new password
4. Stored correct password in Vaultwarden for reference

**Verification:** Test authentication manually:
```bash
curl -u username:app_password -X PUT \
     https://cloud.najhin-gaming.com/remote.php/dav/files/username/test.txt \
     --data "test content"
```

**Lesson:** App passwords are specific to applications and can expire or be revoked. Always test authentication separately when troubleshooting 401 errors, and maintain accurate password records.

---

## 2026-03-10 — Uptime Kuma (CT 206) High CPU Alerts Every 2 Hours

**Symptoms:** Discord alerts "WARNING: High CPU usage on 192.168.30.206:9100" showing 94-97% CPU. Alerts repeated every ~2 hours.

**Root Cause:** Container was under-resourced (1 CPU core, 512MB RAM). Periodic maintenance tasks (database cleanup, certificate checks) caused CPU spikes that overwhelmed the single core.

**Diagnostic:**
```bash
pct exec 206 -- top -bn1 | head -15
```
Showed:
- `%Cpu(s): 0.0 us, 100.0 sy` — 100% system CPU, 0% user (kernel overhead)
- `load average: 3.72` — severely overloaded for 1-core system
- Node.js process using 11.3GB virtual memory (normal for Node, but needs resources)

**Resolution:**
```bash
pct set 206 -cores 2 -memory 768
pct restart 206
```

**Verification:**
```bash
pct exec 206 -- uptime
# Load average should drop below 1.0
```

**Container Resources After Fix:**
| Setting | Before | After |
|---------|--------|-------|
| CPU Cores | 1 | 2 |
| Memory | 512 MB | 768 MB |

**Lesson:** 
- Single-core containers can struggle with periodic maintenance tasks
- High system CPU (sy) with low user CPU (us) indicates kernel/context-switch overhead
- Load average > number of cores = overloaded system
- Node.js apps benefit from multiple cores for garbage collection

---

## 2026-03-09 — Internet Loss After Firewall Rule Changes

**Symptoms:** Gaming PC lost internet access immediately after applying VLAN20_MAIN firewall rules. WiFi still worked.

**Root Cause:** Firewall rules used "VLAN20_MAIN address" as Source instead of "VLAN20_MAIN subnets".

- "VLAN20_MAIN address" = Only the pfSense gateway IP (192.168.20.1)
- "VLAN20_MAIN subnets" = All devices on the network (192.168.20.0/24)

Rules only matched traffic FROM the gateway itself, not from actual devices.

**Resolution:**
1. Edit each rule on the interface
2. Change Source from "VLANxx address" to "VLANxx subnets"
3. Save and Apply Changes

**Lesson:** In pfSense firewall rules:
- Use "subnets" or "net" for rules that should match device traffic
- Use "address" only when you specifically mean the gateway IP
- Always verify rule source matches intended traffic

---

## 2026-03-09 — Tailscale ICMP Ping Failing But Traffic Works

**Symptoms:** `ping 100.110.165.45` (pfSense Tailscale IP) times out, but `tailscale ping` works and actual services are accessible.

**Root Cause:** pfSense does not respond to ICMP ping requests over the Tailscale interface by default. This is normal behavior, not a configuration error.

**Resolution:** No fix needed. ICMP ping is not required for functionality. Use `tailscale ping` for diagnostics or test actual service access.

**Lesson:** Don't assume ICMP ping failure means connectivity is broken. Test actual service traffic to verify.

---

## 2026-03-09 — Blocked Traffic Still Works Via Tailscale

**Symptoms:** After implementing VLAN20 → VLAN10 block rules, `ping 192.168.10.5` still succeeded from Gaming PC.

**Root Cause:** Tailscale was connected. Traffic to 192.168.10.5 was routing through the Tailscale tunnel (which has allow-all rules) instead of the local VLAN20 interface.

**Resolution:** This is expected behavior. Tailscale traffic uses the Tailscale interface rules, bypassing local VLAN rules. The firewall IS working correctly.

**Lesson:** When testing firewall blocks, disconnect Tailscale first to test local-only traffic. Tailscale is designed to bypass local restrictions.

---

## 2026-03-09 — Monitoring_Servers Alias Contains Old IPs

**Symptoms:** Alias showed IPs from VLAN 20 (192.168.20.x) instead of current VLAN 30 IPs.

**Root Cause:** Alias was created before VLAN migration and never updated.

**Resolution:**
1. Firewall → Aliases → IP tab
2. Edit Monitoring_Servers
3. Update to current IPs: 192.168.30.202, 203, 204, 205, 206
4. Save and Apply Changes

**Lesson:** After any IP migration, audit all firewall aliases to ensure they reference correct addresses.

---

## 2026-03-09 — MONITORING_PORTS Alias Contains Wrong Ports

**Symptoms:** Alias included port 53 (DNS) which doesn't belong in monitoring ports.

**Root Cause:** Alias was incorrectly configured during initial setup.

**Resolution:**
1. Firewall → Aliases → Ports tab
2. Edit MONITORING_PORTS
3. Change to only: 8006, 9100
4. Save and Apply Changes

**Lesson:** Review alias contents before using them in rules. Incorrect aliases cause unexpected allow/block behavior.

---

## 2026-03-08 — Pi-hole Node Exporter Missing

**Symptoms:** Discord alert "CRITICAL: Host pihole is DOWN" and "Prometheus target missing: pihole"

**Root Cause:** After Pi-hole reinstall during VLAN migration, prometheus-node-exporter was never installed.

**Resolution:**
```bash
ssh pi@192.168.30.10
sudo apt update
sudo apt install prometheus-node-exporter -y
```

**Lesson:** After reinstalling any host, remember to reinstall node_exporter for Prometheus monitoring.

---

## 2026-03-08 — Smartmontools Failed on Pi-hole

**Symptoms:** Discord warning "Systemd service failed on pihole: smartmontools.service"

**Root Cause:** smartmontools tries to monitor S.M.A.R.T. disk health, but Raspberry Pi uses SD card which doesn't support S.M.A.R.T.

**Resolution:**
```bash
sudo systemctl mask smartmontools
```

**Lesson:** SD card systems don't support S.M.A.R.T. monitoring. Mask the service instead of trying to fix it.

---

## 2026-03-08 — Uptime Kuma Grafana SSL Error

**Symptoms:** Uptime Kuma shows Grafana as down with SSL/certificate error.

**Root Cause:** Monitor was configured with `https://192.168.30.203:3000` but Grafana runs on HTTP internally (SSL terminates at NPM).

**Resolution:** Changed monitor URL to `http://192.168.30.203:3000`

**Lesson:** Internal monitoring should use HTTP if SSL terminates at reverse proxy.

---

## 2026-03-08 — Uptime Kuma Proxmox SSL Error

**Symptoms:** Proxmox monitor showing SSL certificate errors (self-signed cert).

**Root Cause:** Proxmox uses self-signed SSL certificate by default.

**Resolution:** Enabled "Ignore TLS/SSL error" option in Uptime Kuma monitor settings.

**Lesson:** Self-signed certs require ignoring SSL errors in monitoring tools.

---

## 2026-03-08 — Nextcloud Mobile App Blocked by Cloudflare Access

**Symptoms:** Mobile app shows "Server not found" or authentication errors when Cloudflare Access is enabled.

**Root Cause:** Cloudflare Access intercepts requests and requires browser-based authentication. Mobile apps can't complete this flow.

**Resolution:** Removed Cloudflare Access policy for Nextcloud. Using Nextcloud's built-in brute-force protection instead.

**Lesson:** Cloudflare Access is incompatible with native mobile apps that use API authentication. Use app-native security features instead.

---

## 2026-03-08 — Nextcloud Monitor ECONNREFUSED Port 443

**Symptoms:** Uptime Kuma shows ECONNREFUSED on port 443 for Nextcloud.

**Root Cause:** Nextcloud runs on HTTP (port 80) internally. HTTPS is handled by Cloudflare Tunnel, not Apache.

**Resolution:** Monitor http://192.168.30.220/status.php instead of HTTPS.

**Lesson:** When using Cloudflare Tunnel, internal services run on HTTP. The tunnel handles HTTPS termination.

---

## 2026-03-07 — Tailscale Interface Blocking All Traffic

**Symptoms:** pfSense shows connected in Tailscale, but accessing https://100.110.165.45 times out.

**Root Cause:** No firewall rules defined on Tailscale interface. pfSense blocks all traffic by default.

**Resolution:** Added firewall rules on Firewall → Rules → Tailscale:
- Pass any from any to This Firewall (for WebUI access)
- Pass any from any to any (for subnet routing)

**Lesson:** Every pfSense interface needs explicit firewall rules. No rules = all traffic blocked.

---

## 2026-03-07 — Pterodactyl Panel Login Timeout (20000ms)

**Symptoms:** Panel login page shows "timeout of 20000ms exceeded." Password reset also fails.

**Root Cause:** Container resolv.conf still pointed to old DNS (192.168.20.10). Panel tries to verify reCAPTCHA with Google during login, which requires DNS.

**Resolution:**
```bash
pct set 300 -nameserver 192.168.30.1
pct stop 300 && pct start 300
```
Also updated APP_URL in .env from old IP to 192.168.30.210.

**Lesson:** `pct set -nameserver` updates Proxmox config but NOT running container's resolv.conf. Must restart container.

---

## 2026-03-07 — Cross-VLAN DNS Queries Timing Out

**Symptoms:** `nslookup google.com 192.168.30.10` times out from VLANs 10 and 20. Ping to Pi-hole works fine.

**Root Cause:** Unknown. Firewall rules are allow-all. Possibly pfSense DNS Resolver intercepting or NAT state issues.

**Workaround:** pfSense DNS Resolver with Forwarding Mode. DHCP assigns VLAN gateway as DNS. pfSense forwards to Pi-hole.

**Lesson:** When using pfSense with Pi-hole, DNS proxy architecture is more reliable than direct cross-VLAN queries.

---

## 2026-03-07 — Pi-hole Static IP Not Applied After Reboot

**Symptoms:** Edited `/etc/dhcpcd.conf` with static IP. After reboot, still on DHCP.

**Root Cause:** New Raspberry Pi OS (2026) uses **NetworkManager**, not dhcpcd. The dhcpcd.conf file is ignored.

**Resolution:**
```bash
sudo nmcli con mod "netplan-eth0" ipv4.addresses 192.168.30.10/24
sudo nmcli con mod "netplan-eth0" ipv4.gateway 192.168.30.1
sudo nmcli con mod "netplan-eth0" ipv4.dns "1.1.1.1 8.8.8.8"
sudo nmcli con mod "netplan-eth0" ipv4.method manual
sudo nmcli con down "netplan-eth0" && sudo nmcli con up "netplan-eth0"
```

**Lesson:** Check `systemctl status NetworkManager` vs `systemctl status dhcpcd` to determine which network manager is active.

---

## 2026-03-07 — Proxmox Lost Connectivity After Switch VLAN Changes

**Symptoms:** Proxmox could not ping gateway 192.168.10.1 after switch VLAN changes.

**Root Cause:** Port 2 (Proxmox) was set as access port (untagged VLAN 10). But Proxmox uses vmbr0.10 which sends VLAN 10 **tagged** frames.

**Resolution:** Changed port 2 to trunk (tagged on all VLANs), matching port 1 (pfSense).

**Lesson:** Any device using VLAN-aware bridges or VLAN subinterfaces must be on a **trunk port** (tagged).

---

## 2026-03-07 — Windows nslookup Using IPv6 Instead of DHCP DNS

**Symptoms:** `nslookup google.com` times out. Shows "Server: Unknown, Address: fe80::1" (IPv6).

**Root Cause:** Windows nslookup prefers IPv6 DNS. pfSense advertises fe80::1 via Router Advertisement.

**Resolution:**
```powershell
Disable-NetAdapterBinding -Name "Ethernet 2" -ComponentID ms_tcpip6
```

**Lesson:** If nslookup shows fe80::1 but browser works, it's IPv6 DNS preference.

---

## 2026-03-05 — Proxmox Cannot Ping Gateway After VLAN Migration

**Symptoms:** Proxmox had new IP on VLAN 10, could not reach gateway. "Destination Host Unreachable."

**Root Cause:** Proxmox bridge (vmbr0) sent untagged traffic, but switch port expected VLAN 10 tagged traffic.

**Resolution:** Created VLAN subinterface `vmbr0.10` for management traffic.

**Lesson:** When using VLAN-aware bridges on trunk ports, management IP must be on a VLAN subinterface.

---

## 2026-03-05 — PC Cannot Access Proxmox on New VLAN

**Symptoms:** PC on switch port 4 with static IP 192.168.10.100 couldn't reach Proxmox.

**Root Cause:** Switch port was on Default VLAN 1, not VLAN 10.

**Resolution:** Set port 4 as untagged member of VLAN 10 and set PVID to 10.

**Lesson:** For access ports, must set BOTH VLAN membership (untagged) AND PVID.

---

## 2026-03-03 — NAS NFS Permission Denied for LXC Backups

**Symptoms:** Proxmox backup to NFS failed with permission denied.

**Root Cause:** NFS cannot handle LXC user namespace UID mapping.

**Resolution:** Used SMB instead of NFS for Proxmox backups.

**Lesson:** SMB is more compatible with LXC backups than NFS.

---

## 2026-02 — Large File Upload Failing to Game Servers

**Symptoms:** Could not upload large game mods through Pterodactyl.

**Root Cause:** Default Nginx and PHP limits blocking uploads.

**Resolution:**
```nginx
client_max_body_size 100M;
```

---

## 2026-02 — SSL Certificate Not Propagating

**Symptoms:** Let's Encrypt HTTP-01 validation failing.

**Root Cause:** Cloudflare proxy intercepting the challenge.

**Resolution:** Switched to DNS-01 challenge via Cloudflare API.

---

## 2026-01 — Double NAT Breaking Port Forwarding

**Symptoms:** Port forwarding not working, external access failing.

**Root Cause:** ISP router + pfSense creating double NAT.

**Resolution:** Configured ISP router DMZ to pfSense WAN IP + NAT reflection rules.

---

## 2026-01 — VLAN Bridge Misconfiguration

**Symptoms:** VMs not getting correct VLAN, traffic not segregated.

**Root Cause:** Proxmox bridges not VLAN-aware; trunk ports not properly tagged.

**Resolution:** Set `bridge-vlan-aware yes` on vmbr0 + proper trunk config on switch.

---

*This file is append-only. Add new entries at the top.*