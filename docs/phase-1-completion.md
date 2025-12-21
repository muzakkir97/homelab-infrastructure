# Phase 1: Proxmox Foundation - Completion Report

**Project**: Homelab Infrastructure  
**Phase**: 1 - Foundation  
**Date Started**: December 20, 2024  
**Date Completed**: December 21, 2024  
**Duration**: ~5-6 hours  
**Status**: ✅ **COMPLETE**

---

## 📋 Executive Summary

Successfully deployed enterprise-grade virtualization platform using Proxmox VE with redundant ZFS storage. Created first production VM with automated backup strategy. All primary objectives achieved with one storage device deferred due to hardware failure.

---

## 🎯 Objectives & Results

| Objective | Status | Notes |
|-----------|--------|-------|
| Install Proxmox VE | ✅ Complete | Version 8.x installed on 250GB SSD |
| Configure ZFS mirror storage | ✅ Complete | 2x 1TB NVMe in RAID1, 860GB usable |
| Mount additional storage | ✅ Complete | 8TB + 2TB HDDs configured |
| Create first VM | ✅ Complete | Ubuntu Server 22.04.5 LTS |
| Test snapshots | ✅ Complete | ZFS instant snapshots verified |
| Configure backups | ✅ Complete | Daily automated backups at 2 AM |

---

## 🖥️ Infrastructure Details

### Proxmox Host Configuration

**System Information:**
- **Hostname**: muzakkir
- **Proxmox Version**: 8.x (Proxmox VE)
- **Kernel**: 6.17.4-1-pve
- **CPU**: AMD Ryzen 5 5600X (6 cores, 12 threads)
- **RAM**: 32GB DDR4
- **Network**: 1Gbps Ethernet (vmbr0)

### Storage Configuration

#### Overview Table

| Storage ID | Type | Device | Capacity | Usable | Purpose |
|------------|------|--------|----------|--------|---------|
| `local` | Directory | /dev/sdd (250GB SSD) | 232GB | 232GB | Proxmox system, ISOs, templates |
| `vmpool-fast` | ZFS Mirror | /dev/nvme0n1 + /dev/nvme1n1 | 2TB | 860GB | Primary VM storage (fast, redundant) |
| `vm-storage` | Directory | /dev/sda (2TB HDD) | 1.8TB | 1.8TB | Additional VM storage |
| `data-storage` | Directory | /dev/sdb (8TB HDD) | 7.3TB | 7.3TB | User data, backups |

**Total Usable Storage**: ~10.6TB

#### ZFS Pool Details

**Pool Name**: `vmpool`  
**Configuration**: Mirror (RAID1)  
**Devices**: 
- WD Green SN350 1TB NVMe (/dev/nvme0n1)
- WD Green SN350 1TB NVMe (/dev/nvme1n1)

**ZFS Features Enabled:**
- **Compression**: LZ4 (transparent, automatic)
- **ashift**: 12 (optimized for SSDs)
- **atime**: off (better performance)

**Benefits:**
- ✅ Redundancy: Can survive single drive failure
- ✅ Performance: Read speeds doubled
- ✅ Integrity: Built-in checksums
- ✅ Snapshots: Instant, space-efficient
- ✅ Compression: ~10-20% space savings

**Verification:**
```bash
root@muzakkir:~# zpool status
  pool: vmpool
 state: ONLINE
config:
	NAME          STATE
	vmpool        ONLINE
	  mirror-0    ONLINE
	    nvme0n1   ONLINE
	    nvme1n1   ONLINE

errors: No known data errors
```

---

## 💻 Virtual Machines

### VM 100: ubuntu-test

**Purpose**: First production VM for testing and validation

**Specifications:**
- **OS**: Ubuntu Server 22.04.5 LTS
- **CPU**: 2 cores (host passthrough)
- **RAM**: 4GB (4096 MiB)
- **Disk**: 32GB on vmpool-fast (ZFS)
- **Network**: vmbr0 (VirtIO)
- **IP Address**: 192.168.100.159 (DHCP)
- **Status**: Running ✅

**Features Enabled:**
- ✅ UEFI boot
- ✅ Qemu Guest Agent (IP address visible in Proxmox)
- ✅ SSH access enabled
- ✅ System fully updated
- ✅ Snapshots functional
- ✅ Included in automated backups

**Post-Installation:**
```bash
# System updated
sudo apt update && sudo apt upgrade -y

# Qemu agent installed
sudo apt install qemu-guest-agent -y
sudo systemctl enable qemu-guest-agent
sudo systemctl start qemu-guest-agent
```

**Snapshot Testing:**
- Created snapshot: `clean-install`
- Made changes (created test files)
- Rolled back successfully
- **Rollback time**: < 5 seconds ✨

---

## 💾 Backup Configuration

### Automated Backup Schedule

**Schedule Details:**
- **Storage**: data-storage (8TB HDD)
- **Frequency**: Daily at 02:00 (2 AM)
- **Selection**: All VMs (automatic)
- **Mode**: Snapshot (consistent backups)
- **Compression**: ZSTD (fast, efficient)
- **Retention**: Keep last 7 backups

**Backup Testing:**
- Manual backup executed successfully
- Backup file: `vzdump-qemu-500-2025_12_21-22_59_52.vma.zst`
- Size: 2.40 GB (compressed)
- **Backup time**: ~3-5 minutes
- **Restore tested**: ✅ Successful

**Backup Storage Capacity:**
- Available space: 7.3TB
- Estimated backup size per VM: ~2-3GB
- Can store 2,400+ VM backups (with 7-day rotation)

---

## 🔧 Technical Challenges & Solutions

### Challenge 1: Old LVM Installation on NVMe

**Problem:**
- nvme1n1 contained old Proxmox LVM installation
- `pve--OLD--A2EB6816` volume group preventing ZFS creation

**Solution:**
```bash
# Removed LVM structures
vgremove pve-OLD-A2EB6816
pvremove /dev/nvme1n1p3

# Wiped partition table
sgdisk --zap-all /dev/nvme1n1

# Successfully created ZFS pool
zpool create -f -o ashift=12 -O compression=lz4 -O atime=off vmpool mirror /dev/nvme0n1 /dev/nvme1n1
```

**Lesson Learned**: Always check for existing LVM/partition structures before creating ZFS pools

---

### Challenge 2: Repository Subscription Warnings

**Problem:**
- Enterprise repository enabled (requires paid subscription)
- Update warnings appearing

**Solution:**
```bash
# Disabled enterprise repository
echo "# deb https://enterprise.proxmox.com/debian/pve bookworm pve-enterprise" > /etc/apt/sources.list.d/pve-enterprise.list

# Enabled no-subscription repository
echo "deb http://download.proxmox.com/debian/pve bookworm pve-no-subscription" > /etc/apt/sources.list.d/pve-no-subscription.list

# Updated system
systemctl daemon-reload
apt update && apt dist-upgrade -y
```

**Lesson Learned**: Configure repositories before system updates

---

### Challenge 3: Failed 4TB Backup Drive

**Problem:**
- One 4TB internal HDD showing I/O errors
- SATA link failures, device not ready
- Drive not reliably detectable

**Diagnostic Output:**
```
[22034.758413] I/O error, dev sdc, sector 2 op 0x0:(READ)
[22034.758453] ata3.00: detaching (SCSI 2:0:0:0)
[22035.503367] ata3: SATA link down (SStatus 0 SControl 300)
```

**Decision:**
- Skipped the failing drive
- Removed from fstab configuration
- Using 8TB drive (data-storage) for backups instead
- USB dock with 2x 4TB drives available for future expansion

**Lesson Learned**: 
- Always have backup storage options
- Hardware failures are normal - plan for redundancy
- Current setup has sufficient backup capacity

---

## 📊 Performance Metrics

### Storage Performance

| Metric | Value | Notes |
|--------|-------|-------|
| ZFS read speed | ~3,500 MB/s | NVMe performance |
| ZFS write speed | ~2,000 MB/s | Limited by mirror writes |
| Snapshot creation | < 2 seconds | Instant (ZFS COW) |
| VM boot time | ~30 seconds | Ubuntu Server |
| Backup time (32GB VM) | ~3-5 minutes | ZSTD compression |

### System Resource Usage

**Proxmox Host:**
- CPU: 0.5-2% idle, ~10-15% during VM operation
- RAM: 1.3GB used (Proxmox), 3GB available for VMs
- Storage: 10.6TB total, ~10.5TB free

**VM 100 (Ubuntu):**
- CPU: 0.92% average
- RAM: 965MB / 4GB (24% utilization)
- Disk I/O: Minimal (idle state)

---

## ✅ Phase 1 Deliverables

### Infrastructure
- [x] Proxmox VE installed and configured
- [x] ZFS mirror pool operational (860GB)
- [x] Storage hierarchy established (10TB total)
- [x] Network connectivity configured
- [x] Repository sources configured

### Virtual Machines
- [x] Ubuntu Server 22.04 VM created
- [x] Qemu Guest Agent functional
- [x] SSH access enabled
- [x] System fully updated

### Backup & Recovery
- [x] Automated backup schedule configured
- [x] Manual backup tested and verified
- [x] Restore procedure tested
- [x] Snapshot functionality validated

### Documentation
- [x] Main README created
- [x] Phase 1 completion report written
- [x] Storage configuration documented
- [x] Troubleshooting notes captured

---

## 📈 Skills Acquired

### Technical Skills
- ✅ Proxmox VE installation and configuration
- ✅ ZFS administration (pools, mirrors, snapshots)
- ✅ Storage management (fstab, mounting, partitioning)
- ✅ VM creation and configuration
- ✅ Linux system administration
- ✅ QEMU/KVM virtualization
- ✅ Backup strategy implementation
- ✅ Troubleshooting hardware issues

### Soft Skills
- ✅ Technical documentation
- ✅ Problem-solving under constraints
- ✅ Project planning and execution
- ✅ Risk assessment (hardware failure)

---

## 🎯 Phase 1 Success Criteria

| Criteria | Target | Achieved | Status |
|----------|--------|----------|--------|
| Proxmox installed | Yes | Yes | ✅ |
| Redundant storage configured | Yes | Yes (ZFS mirror) | ✅ |
| First VM operational | Yes | Yes (Ubuntu Server) | ✅ |
| Backups automated | Yes | Yes (daily 2 AM) | ✅ |
| Documentation complete | Yes | Yes | ✅ |
| Total time investment | < 8 hours | ~5-6 hours | ✅ |

**Overall Success Rate**: 100% ✅

---

## 🔜 Next Steps

### Immediate (Phase 2)
- [ ] Design VLAN architecture
- [ ] Configure managed switch (TP-Link TL-SG108E)
- [ ] Implement network segmentation
- [ ] Test inter-VLAN routing

### Short-term (Phase 3-4)
- [ ] Deploy Nextcloud (Google Drive replacement)
- [ ] Deploy Immich (iCloud Photos replacement)
- [ ] Set up Nginx Proxy Manager
- [ ] Configure SSL certificates with Let's Encrypt

### Hardware Arrivals
- [ ] V60 J4125 router (end of December 2024)
- [ ] Raspberry Pi 4 8GB (early January 2025)

---

## 💡 Lessons Learned

### What Went Well
1. **ZFS Implementation**: Smooth setup, instant snapshots impressive
2. **Planning**: Good hardware inventory before starting saved time
3. **Documentation**: Documenting as I go made troubleshooting easier
4. **Community Resources**: YouTube tutorials very helpful

### What Could Be Improved
1. **Hardware Testing**: Should have tested all drives before starting
2. **Time Estimation**: Took longer than expected (good learning!)
3. **Backup Planning**: Should have planned for drive failure upfront

### Key Takeaways
- ✅ Enterprise features (ZFS, snapshots) are worth the complexity
- ✅ Redundancy is essential - hardware failures happen
- ✅ Documentation pays off immediately during troubleshooting
- ✅ Start simple, expand gradually (didn't need all 4TB drives immediately)

---

## 📸 Evidence & Screenshots

### Storage Configuration
- Proxmox Storage overview showing all configured storage
- ZFS pool status (`zpool status`)
- Disk usage (`df -h`)

### Virtual Machine
- VM 100 Summary showing IP address and agent status
- Console screenshot of Ubuntu login
- Resource usage graphs

### Backups
- Backup job schedule configuration
- Successful backup file in data-storage
- Backup restore test completion

*Note: Screenshots to be added to `/docs/screenshots/phase-1/` directory*

---

## 🎊 Phase 1 Conclusion

Phase 1 successfully established the foundation for the entire homelab infrastructure. Despite encountering a hardware failure (4TB drive), alternative solutions were implemented without impacting project objectives.

The ZFS mirror configuration provides enterprise-grade redundancy and performance, while the automated backup strategy ensures data protection. The first VM validates the entire stack from hypervisor to guest OS integration.

**Ready to proceed to Phase 2: Network Segmentation** 🚀

---

## 📝 Technical Reference

### Useful Commands

```bash
# ZFS Management
zpool status                    # Check pool health
zfs list                        # List all datasets
zpool scrub vmpool             # Verify data integrity

# VM Management
qm list                         # List all VMs
qm start 100                    # Start VM
qm stop 100                     # Stop VM
qm snapshot 100 snapshot-name   # Create snapshot

# Backup Management
vzdump 100 --storage data-storage  # Manual backup
pvesm status                       # Check storage status

# System Monitoring
htop                            # Interactive process viewer
df -h                           # Disk usage
free -h                         # Memory usage
```

### Important File Locations

```
/etc/pve/                       # Proxmox configuration
/etc/pve/qemu-server/           # VM configurations
/etc/pve/storage.cfg            # Storage configuration
/etc/fstab                      # Filesystem mount table
```

---

**Report Compiled By**: Muzakkir  
**Date**: December 21, 2024  
**Next Update**: After Phase 2 completion
