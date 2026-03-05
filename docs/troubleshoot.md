# Troubleshooting Log

> **Format:** Newest first, append-only

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
