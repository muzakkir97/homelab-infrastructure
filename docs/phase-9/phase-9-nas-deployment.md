# Phase 9: NAS Deployment

> **Status:** ✅ Complete | **Completed:** March 3, 2026

---

## Overview

Phase 9 deployed network-attached storage using a UGREEN DXP2800 NAS for centralized backup storage, integrated with Proxmox via SMB.

### Objectives

| Objective | Status |
|-----------|--------|
| Deploy UGREEN NAS hardware | ✅ Complete |
| Create storage pool | ✅ Complete |
| Configure SMB file sharing | ✅ Complete |
| Integrate with Proxmox | ✅ Complete |
| Test backup functionality | ✅ Complete |

---

## Hardware

| Component | Details |
|-----------|---------|
| **NAS Model** | UGREEN DXP2800 |
| **Hostname** | Kinmoon |
| **IP Address** | 192.168.1.15 (LAN) → 192.168.10.15 (target) |
| **Storage** | 1x WD Purple 4TB (3.6TB usable) |
| **OS** | UGOS (UGREEN OS) |

### Drive Status

| Drive | Status | Notes |
|-------|--------|-------|
| WD Purple 4TB | ✅ Working | Primary storage pool |
| WD Green 1TB SSD | ❌ Not detected | Investigation pending |
| Kingston 120GB NVMe | ❌ Not detected | Investigation pending |

---

## Configuration

### Storage Pool

| Setting | Value |
|---------|-------|
| Pool Name | storage-pool |
| Type | Single drive (no RAID) |
| Capacity | 3.6TB usable |
| File System | ext4 |

### SMB Share

| Setting | Value |
|---------|-------|
| Share Name | proxmox-backups |
| Path | /storage-pool/proxmox-backups |
| Access | Read/Write |
| Authentication | Username/Password |

### Proxmox Integration

| Setting | Value |
|---------|-------|
| Storage ID | kinmoon-smb |
| Type | SMB/CIFS |
| Server | 192.168.1.15 |
| Share | proxmox-backups |
| Content | VZDump backup files |

---

## Architecture

```
┌─────────────────────────────────────────┐
│            Proxmox Server               │
│              (Kuromoon)                 │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │     Backup Job (vzdump)         │   │
│  │                                  │   │
│  │  CT/VM → .vma.zst → kinmoon-smb │   │
│  └─────────────────────────────────┘   │
│                  │                      │
└──────────────────┼──────────────────────┘
                   │ SMB/CIFS
                   ▼
┌─────────────────────────────────────────┐
│            UGREEN NAS                   │
│             (Kinmoon)                   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │  /proxmox-backups               │   │
│  │                                  │   │
│  │  └── vzdump-lxc-201-*.vma.zst  │   │
│  │  └── vzdump-lxc-202-*.vma.zst  │   │
│  │  └── ...                        │   │
│  └─────────────────────────────────┘   │
│                                         │
│  ┌─────────────────────────────────┐   │
│  │  WD Purple 4TB (3.6TB usable)   │   │
│  └─────────────────────────────────┘   │
└─────────────────────────────────────────┘
```

---

## Setup Steps

### 1. NAS Initial Configuration

1. Connected NAS to network (obtained 192.168.1.15 via DHCP)
2. Accessed web UI at `http://192.168.1.15:9999`
3. Set hostname: `Kinmoon`
4. Created admin account
5. Updated firmware

### 2. Storage Pool Creation

1. Navigated to Storage Manager
2. Created new storage pool with WD Purple 4TB
3. Named pool: `storage-pool`
4. Rebooted NAS (required after pool creation)

### 3. SMB Share Configuration

1. Created shared folder: `proxmox-backups`
2. Enabled SMB service
3. Configured access permissions

### 4. Proxmox Storage Addition

```bash
# Added via Proxmox UI: Datacenter → Storage → Add → SMB/CIFS

# Settings used:
# ID: kinmoon-smb
# Server: 192.168.1.15
# Username: [admin-user]
# Password: [password]
# Share: proxmox-backups
# Content: VZDump backup file
```

### 5. Backup Test

```bash
# Tested backup of CT 201 (nginx-proxy-manager)
vzdump 201 --storage kinmoon-smb --compress zstd --mode snapshot

# Result: Success
# File: vzdump-lxc-201-2026_03_03-*.vma.zst
# Size: ~500MB
```

---

## Issues Encountered & Resolutions

| Issue | Resolution |
|-------|------------|
| NAS I/O errors after setup | Reboot NAS after initial pool creation |
| NFS permission denied | Use SMB instead — NFS can't handle LXC user namespace UIDs |
| Drives not detected | WD Green SSD and Kingston NVMe — investigation pending |

### Why SMB Instead of NFS

When attempting NFS for Proxmox backups, encountered:

```
permission denied: unable to create file
```

**Root cause:** LXC containers use user namespaces that map UIDs differently. NFS doesn't handle this well for backup files.

**Solution:** SMB/CIFS handles authentication at the protocol level, bypassing the UID mapping issue.

---

## Backup Strategy (Phase 7A Preview)

With NAS storage now available, Phase 7A will implement:

| Component | Backup Target | Schedule |
|-----------|---------------|----------|
| All LXC containers | kinmoon-smb | Daily 2:00 AM |
| Proxmox configs | kinmoon-smb | Weekly |
| pfSense config | kinmoon-smb | After changes |

---

## Future Enhancements

1. **Investigate missing drives** — WD Green SSD, Kingston NVMe
2. **Move NAS to VLAN 10** — After Phase 6F VLAN migration
3. **Configure scheduled backups** — Phase 7A
4. **Add RAID redundancy** — If additional drives added
5. **Off-site backup** — Consider cloud sync for critical data

---

## Network Considerations

### Current State
- NAS on legacy LAN (192.168.1.15)
- Accessible from Proxmox on same LAN

### After Phase 6F Migration
- NAS target: 192.168.10.15 (VLAN 10 - Management)
- Proxmox SMB storage config will need IP update
- Firewall rules: Allow VLAN 10 → NAS for backups

---

*Documentation created: March 2026*
