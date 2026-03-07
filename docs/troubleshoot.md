# Troubleshooting Log

> **Format:** Newest first, append-only

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
