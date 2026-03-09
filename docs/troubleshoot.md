# Troubleshooting Log

> **Format:** Newest first, append-only

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

**Verification:**
```powershell
# This fails (ICMP blocked)
ping 100.110.165.45

# This works (Tailscale protocol)
tailscale ping 100.110.165.45

# This works (actual traffic)
curl https://192.168.10.5:8006
```

**Resolution:** No fix needed. ICMP ping is not required for functionality. Use `tailscale ping` for diagnostics or test actual service access.

**Lesson:** Don't assume ICMP ping failure means connectivity is broken. Test actual service traffic to verify.

---

## 2026-03-09 — Blocked Traffic Still Works Via Tailscale

**Symptoms:** After implementing VLAN20 → VLAN10 block rules, `ping 192.168.10.5` still succeeded from Gaming PC.

**Root Cause:** Tailscale was connected. Traffic to 192.168.10.5 was routing through the Tailscale tunnel (which has allow-all rules) instead of the local VLAN20 interface.

**Verification:**
1. Disconnect Tailscale (right-click tray icon → Disconnect)
2. Run `ping 192.168.10.5`
3. Should now timeout (blocked by RFC1918 rule)
4. Reconnect Tailscale
5. Ping works again (via Tailscale tunnel)

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

**Symptoms:** `nslookup google.com 192.168.30.10` times out from VLANs 10 and 20. Ping to Pi-hole works fine. `nc -zvu 192.168.30.10 53` shows port open.

**Root Cause:** Unknown. Firewall rules are allow-all. Possibly pfSense DNS Resolver intercepting or NAT state issues.

**Workaround:** pfSense DNS Resolver with Forwarding Mode. DHCP assigns VLAN gateway as DNS. pfSense forwards to Pi-hole.

**Lesson:** When using pfSense with Pi-hole, DNS proxy architecture is more reliable than direct cross-VLAN queries.

---

## 2026-03-07 — Pi-hole Static IP Not Applied After Reboot

**Symptoms:** Edited `/etc/dhcpcd.conf` with static IP. After reboot, still on DHCP. `systemctl status dhcpcd` shows "Unit could not be found."

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

**Symptoms:** Proxmox could not ping gateway 192.168.10.1 after switch VLAN changes. Tailscale access also stopped.

**Root Cause:** Port 2 (Proxmox) was set as access port (untagged VLAN 10). But Proxmox uses vmbr0.10 which sends VLAN 10 **tagged** frames. Switch was stripping the tag.

**Resolution:** Changed port 2 to trunk (tagged on all VLANs), matching port 1 (pfSense).

**Lesson:** Any device using VLAN-aware bridges or VLAN subinterfaces must be on a **trunk port** (tagged). Access ports are only for devices sending untagged traffic.

---

## 2026-03-07 — Windows nslookup Using IPv6 Instead of DHCP DNS

**Symptoms:** `nslookup google.com` times out. Shows "Server: Unknown, Address: fe80::1" (IPv6). `nslookup google.com 192.168.20.1` works.

**Root Cause:** Windows nslookup prefers IPv6 DNS. pfSense advertises fe80::1 via Router Advertisement.

**Resolution:**
```powershell
Disable-NetAdapterBinding -Name "Ethernet 2" -ComponentID ms_tcpip6
```

**Lesson:** If nslookup shows fe80::1 but browser works, it's IPv6 DNS preference. Disable IPv6 or configure it properly.

---

## 2026-03-05 — Proxmox Cannot Ping Gateway After VLAN Migration

**Symptoms:** Proxmox had new IP on VLAN 10, could not reach gateway. "Destination Host Unreachable."

**Root Cause:** Proxmox bridge (vmbr0) sent untagged traffic, but switch port expected VLAN 10 tagged traffic on the trunk.

**Resolution:** Created VLAN subinterface `vmbr0.10` for management traffic:
```
auto vmbr0.10
iface vmbr0.10 inet static
    address 192.168.10.5/24
    gateway 192.168.10.1
```

**Lesson:** When using VLAN-aware bridges on trunk ports, management IP must be on a VLAN subinterface, not the base bridge.

---

## 2026-03-05 — Proxmox Interface Name Wrong (eno0 vs nic0)

**Symptoms:** Network apply failed with "No such device" error for `eno0`.

**Root Cause:** Config specified `eno0` but actual interface was `nic0`.

**Resolution:** Checked backup config to find correct name, updated to `nic0`.

**Lesson:** Always check `ip link show` before editing network config.

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

## 2026-03-03 — NAS I/O Errors After Initial Setup

**Symptoms:** UGREEN NAS showing I/O errors after pool creation.

**Resolution:** Rebooted NAS after storage pool creation.

---

## 2026-02 — Large File Upload Failing to Game Servers

**Symptoms:** Could not upload large game mods through Pterodactyl.

**Root Cause:** Default Nginx and PHP limits blocking uploads.

**Resolution:**
```nginx
client_max_body_size 100M;
```
```php
upload_max_filesize = 100M
post_max_size = 100M
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
