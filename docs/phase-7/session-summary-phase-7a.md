# Session Summary: Phase 7A — Backup Strategy

> **Session Dates:** March 13-16, 2026  
> **Phase:** 7A (Backup Strategy)  
> **Status:** ✅ COMPLETE

---

## What We Accomplished

### Container Backup Infrastructure (March 13)
- Audited existing storage: kinmoon-smb (CIFS), data-storage, vm-storage, local-lvm, vmpool-fast
- Discovered pre-existing backup job pointing to unsafe local storage
- Migrated NAS protocol from SMB/CIFS to NFS after kernel crash diagnosis
- Implemented hybrid backup strategy for NAS write limitations
- Created two scheduled backup jobs with proper retention

### Configuration Backups (March 16)
- ✅ pfSense: Manual XML downloaded + Auto Config History verified (30+ versions)
- ✅ Pi-hole: Teleporter backup downloaded
- ✅ Pi-hole blocklists enhanced: 81K → 488K domains (6x increase)

### Backup Jobs Configured

| Job | Schedule | Storage | Containers | Retention |
|-----|----------|---------|------------|-----------|
| Small Containers | 02:00 | kinmoon-nfs (NAS) | 201-207, 300 | 7 daily, 4 weekly, 2 monthly |
| Large Containers | 02:30 | data-storage (Local) | 220, 302 | 7 daily, 4 weekly, 2 monthly |

---

## Key Issues & Resolutions

| Issue | Root Cause | Resolution |
|-------|------------|------------|
| SMB backup hung for 7+ hours | Linux kernel CIFS driver crash with large writes | Switch to NFS |
| NFS "Permission denied" | UID mismatch for unprivileged containers | NAS squash: "Map all users to admin" |
| Large container NFS failures | UGREEN NAS can't sustain >10GB writes | Hybrid: large containers → local storage |
| ZFS snapshot stuck | Backup process killed mid-operation | Reboot, then `zfs destroy` orphan snapshot |
| D-state processes unkillable | Kernel-level I/O wait | Requires reboot |
| Phone can't use Pi-hole | ISP router WiFi is separate network | Need WiFi AP on VLAN 20 |

---

## Storage Configuration (Final)

| Storage | Type | Location | Content | Status |
|---------|------|----------|---------|--------|
| kinmoon-nfs | NFS v3 | 192.168.10.15:/volume1/proxmox-backups | backup,vztmpl,iso | ✅ Active |
| data-storage | Directory | /mnt/data-storage (7.2TB HDD) | All types | ✅ Active |
| local-lvm | LVM-Thin | pve/data | images,rootdir | ✅ Active |
| vmpool-fast | ZFS | vmpool | images,rootdir | ✅ Active |
| kinmoon-smb | CIFS | — | — | ❌ Removed |

---

## Pi-hole Blocklists Added

| List | Domains |
|------|---------|
| StevenBlack/hosts (existing) | 81,131 |
| Firebog Easyprivacy | 42,495 |
| Firebog AdguardDNS | 161,024 |
| Firebog Prigent-Malware | 253,528 |
| **Total Unique** | **488,828** |

---

## Future Tasks Added

| Task | Priority | Notes |
|------|----------|-------|
| WiFi Access Point (VLAN 20) | Medium | TP-Link EAP225 recommended (~$60); enables phone Pi-hole |
| Off-site backup (Backblaze B2) | Low | Phase 7A extension |

---

## Phase 7A Summary: COMPLETE ✅

| Component | Method | Location | Automated |
|-----------|--------|----------|-----------|
| LXC Containers (small) | vzdump @ 02:00 | NAS (NFS) | ✅ Yes |
| LXC Containers (large) | vzdump @ 02:30 | Local HDD | ✅ Yes |
| pfSense Config | XML export + Auto history | Gaming PC + Nextcloud | ⚠️ Manual |
| Pi-hole Config | Teleporter export | Gaming PC + Nextcloud | ⚠️ Manual |

---

## Files Generated

1. `phase-7a-backup-strategy.md` — GitHub documentation
2. `linkedin-post-phase-7a.md` — LinkedIn update
3. `session-summary-phase-7a.md` — This file
4. `AI-CONTEXT.md` — Updated context for next session

---

*Session completed: March 16, 2026*
