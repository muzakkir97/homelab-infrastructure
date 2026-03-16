# Phase 7A: Backup Strategy Implementation

> **Date:** March 13-16, 2026  
> **Status:** ✅ Complete  
> **Author:** Muzakkir Kholil  
> **GitHub:** [github.com/muzakkir97](https://github.com/muzakkir97)

---

## Overview

This phase implements a comprehensive backup strategy for the homelab infrastructure. After encountering stability issues with SMB/CIFS during large file transfers, a hybrid approach was developed using NFS for smaller containers and local storage for large containers.

---

## Problem Statement

### Initial Challenges
- No automated backup system in place
- 10 LXC containers with critical data (Nextcloud, game servers, monitoring)
- Single point of failure if Proxmox storage fails

### Issues Discovered During Implementation
- **SMB/CIFS instability:** Kernel-level crashes during large file writes (>10GB)
- **NFS permission errors:** User namespace mapping issues with unprivileged containers
- **NAS limitations:** UGREEN DXP2800 struggles with sustained large writes

---

## Solution Architecture

### Hybrid Backup Strategy

```
┌─────────────────────────────────────────────────────────────────┐
│                     PROXMOX SERVER                              │
│                                                                 │
│  ┌─────────────────────────────────────────────────────────┐   │
│  │              LXC CONTAINERS                              │   │
│  │  201  202  203  204  205  206  207  300  │  220   302   │   │
│  │  ←──────── Small (<2GB) ────────────────→│←─ Large ────→│   │
│  └─────────────────────────────────────────────────────────┘   │
│           │                                        │            │
│           │ Job 1 @ 02:00                         │ Job 2 @ 02:30
│           ▼                                        ▼            │
│  ┌─────────────────┐                    ┌─────────────────┐    │
│  │   kinmoon-nfs   │                    │  data-storage   │    │
│  │   (NAS - NFS)   │                    │  (Local HDD)    │    │
│  │     3.6 TB      │                    │     7.2 TB      │    │
│  └─────────────────┘                    └─────────────────┘    │
│           │                                                     │
└───────────│─────────────────────────────────────────────────────┘
            │ Network (VLAN 10)
            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    UGREEN NAS (Kinmoon)                         │
│                      192.168.10.15                              │
│                   /volume1/proxmox-backups                      │
└─────────────────────────────────────────────────────────────────┘
```

### Backup Jobs Configuration

| Job | Schedule | Storage | Containers | Est. Size |
|-----|----------|---------|------------|-----------|
| Small Containers | 02:00 | kinmoon-nfs | 201, 202, 203, 204, 205, 206, 207, 300 | ~6 GB |
| Large Containers | 02:30 | data-storage | 220, 302 | ~27 GB |

### Retention Policy (Both Jobs)

| Type | Count | Purpose |
|------|-------|---------|
| Keep Last | 7 | One week of daily recovery points |
| Keep Weekly | 4 | One month of weekly checkpoints |
| Keep Monthly | 2 | Two months for long-term issues |

**Estimated Total Storage:** ~400-600 GB (13 retention points × ~35 GB per full backup)

---

## Implementation Steps

### 1. NAS Storage Configuration

#### Enable NFS on UGREEN NAS
1. Control Panel → File Service → NFS
2. Enable NFS service
3. Set Max NFS protocol: NFSv3

#### Configure NFS Permissions
1. Shared Folder → proxmox-backups → NFS Permissions
2. Add rule:
   - Client: 192.168.x.5 (Proxmox IP)
   - Permissions: Read & Write
   - Squash: Map all users to admin
   - Async: ✅ Enabled
   - Unprivileged port: ✅ Enabled

### 2. Proxmox Storage Setup

#### Add NFS Storage
```bash
pvesm add nfs kinmoon-nfs \
  --server 192.168.x.15 \
  --export /volume1/proxmox-backups \
  --path /mnt/pve/kinmoon-nfs \
  --content backup,vztmpl,iso \
  --options vers=3
```

#### Verify Storage
```bash
pvesm status | grep kinmoon
# Output: kinmoon-nfs  nfs  active  3828671488  ...
```

### 3. Backup Job Configuration

#### Job 1: Small Containers → NFS
- **Node:** proxmox-node
- **Storage:** kinmoon-nfs
- **Schedule:** 02:00
- **Selection:** 201, 202, 203, 204, 205, 206, 207, 300
- **Compression:** ZSTD
- **Mode:** Snapshot

#### Job 2: Large Containers → Local
- **Node:** proxmox-node
- **Storage:** data-storage
- **Schedule:** 02:30
- **Selection:** 220, 302
- **Compression:** ZSTD
- **Mode:** Snapshot

---

## Troubleshooting Guide

### SMB/CIFS Kernel Crashes

**Symptoms:**
- Backup hangs for hours
- Terminal commands freeze
- `dmesg` shows CIFS errors

**Root Cause:** Linux kernel CIFS driver instability with large sustained writes to consumer NAS.

**Solution:** Use NFS instead of SMB for backup storage.

### NFS Permission Denied

**Symptoms:**
```
tar: Cannot open: Permission denied
```

**Root Cause:** Unprivileged LXC containers use user namespace mapping (UID 100000+). NFS with "No mapping" squash cannot handle these UIDs.

**Solution:** Change NFS squash setting to "Map all users to admin" on the NAS.

### ZFS Snapshot Busy

**Symptoms:**
```
cannot destroy snapshot: dataset is busy
```

**Root Cause:** Previous failed backup left orphaned snapshot with stuck processes.

**Solution:**
```bash
# Find stuck processes
ps aux | grep -E "tar|zstd|vzdump"

# Kill stuck processes
kill -9 <PID>

# If processes are in D state (uninterruptible), reboot required
reboot

# After reboot, destroy orphaned snapshot
zfs destroy vmpool/subvol-XXX-disk-0@vzdump
```

### Container Locked

**Symptoms:**
```
CT is locked (backup)
```

**Solution:**
```bash
pct unlock <CTID>
```

---

## Backup Verification

### Manual Backup Test
```bash
# Test small container to NFS
vzdump 201 --storage kinmoon-nfs --compress zstd --mode snapshot

# Test large container to local
vzdump 302 --storage data-storage --compress zstd --mode snapshot
```

### Verify Backup Files
```bash
# List NFS backups
ls -lh /mnt/pve/kinmoon-nfs/dump/

# List local backups
ls -lh /mnt/data-storage/dump/
```

### Test Restore (Non-Destructive)
```bash
# Restore to a new CTID (doesn't overwrite original)
pct restore 999 /mnt/pve/kinmoon-nfs/dump/vzdump-lxc-201-<timestamp>.tar.zst --storage local-lvm

# Verify restore worked
pct list | grep 999

# Delete test container
pct destroy 999
```

---

## Storage Summary

| Storage | Type | Protocol | Capacity | Used | Purpose |
|---------|------|----------|----------|------|---------|
| kinmoon-nfs | NAS | NFS v3 | 3.6 TB | ~35 GB | Small container backups |
| data-storage | Local HDD | Direct | 7.2 TB | ~200 GB | Large container backups |

---

## Performance Benchmarks

| Container | Size | Storage | Speed | Duration |
|-----------|------|---------|-------|----------|
| CT 201 (NPM) | 1.08 GB | NFS | 62 MB/s | 1:03 |
| CT 220 (Nextcloud) | 16.09 GB | Local | 248 MB/s | 1:15 |
| CT 302 (Wings) | 10.52 GB | Local | 66 MB/s | 5:02 |

---

## Lessons Learned

1. **SMB vs NFS:** NFS is significantly more stable for Linux backup operations than SMB/CIFS, especially with large files.

2. **Consumer NAS limitations:** UGREEN NAS (and similar consumer devices) struggle with sustained large writes. Plan accordingly with hybrid strategies.

3. **Unprivileged containers need squash:** NFS exports for unprivileged LXC backups require "Map all users to admin" or similar squash settings.

4. **Test before scheduling:** Always run manual backup tests before relying on scheduled jobs.

5. **Monitor D-state processes:** Processes in uninterruptible sleep (D state) indicate kernel-level I/O issues and often require a reboot to resolve.

---

## Future Improvements

- [ ] Off-site backup to Backblaze B2 or similar
- [x] pfSense config backup (manual XML + auto config history)
- [x] Pi-hole Teleporter backup
- [ ] Backup monitoring alerts via Prometheus/Alertmanager
- [ ] Periodic restore testing automation

---

## Configuration Backup Summary

### pfSense
- **Method:** Manual XML export + Auto Config History (built-in)
- **Location:** Gaming PC + Nextcloud (homelab folder)
- **Auto backups:** 30+ versions in Diagnostics → Backup & Restore → Config History

### Pi-hole
- **Method:** Teleporter export (Settings → Teleporter → Export)
- **Location:** Gaming PC + Nextcloud (homelab folder)
- **Blocklists:** 488,828 unique domains (StevenBlack, Easyprivacy, AdguardDNS, Prigent-Malware)

---

*Documentation created: March 13, 2026 | Updated: March 16, 2026*
