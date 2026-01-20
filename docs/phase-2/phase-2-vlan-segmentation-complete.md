# Phase 2: VLAN Network Segmentation

## 📋 Overview

**Phase:** 2 of 5  
**Status:** ✅ Complete  
**Duration:** 2 sessions  
**Completion Date:** January 2025

### Objectives
- Configure managed switch with VLAN segmentation
- Set up trunk port between switch and Proxmox
- Configure Proxmox VLAN-aware bridge
- Establish network isolation for future services
- Prepare infrastructure for inter-VLAN routing (Phase 3)

### Hardware Used
- **Switch:** TP-Link TL-SG108E Managed Switch (Firmware 1.0.0 Build 20230218)
- **Server:** Proxmox VE 8.x on Ryzen 5 5600X, 32GB RAM
- **Network:** 1Gbps Ethernet connections

---

## 🎯 What Was Accomplished

### VLAN Infrastructure
Created 5 VLANs with specific purposes:

| VLAN ID | Name | Subnet | Purpose |
|---------|------|--------|---------|
| 1 | Management | 192.168.100.0/24 | Management network (router, switch, Proxmox) |
| 10 | Main_Netwo | TBD (Future: 192.168.10.0/24) | Client devices and workstations |
| 20 | Homelab_se | TBD (Future: 192.168.20.0/24) | Self-hosted services (Nextcloud, Immich, game servers) |
| 30 | DMZ | TBD (Future: 192.168.30.0/24) | Public-facing services (isolated) |
| 40 | Malware_La | TBD (Future: 192.168.40.0/24) | Malware analysis environment (completely isolated) |

### Switch Configuration
- **Port 1 (Router):** VLAN 1 Untagged, PVID 1
- **Port 2 (Proxmox):** VLAN 1 Untagged, VLAN 10/20/30/40 Tagged, PVID 1 (Trunk)
- **Port 3 (Work Laptop):** VLAN 10 Untagged, PVID 10
- **Port 4 (Gaming PC):** VLAN 1 Untagged, PVID 1
- **Ports 5-8:** Available for future devices

### Proxmox Configuration
- **vmbr0:** Configured as VLAN-aware bridge
- **Host IP:** 192.168.100.25/24 on VLAN 1 (untagged)
- **VM Support:** VLANs 10, 20, 30, 40 available as tagged VLANs

---

## 🛠️ Configuration Steps

### Step 1: Switch Initial Setup

**Access Switch Web Interface:**
```bash
# Default IP: 192.168.0.1
# Navigate to: http://192.168.0.1
# Login: admin / admin (default)
```

**Change Management IP:**
1. Navigate to: **System → Management VLAN**
2. Set new IP: `192.168.100.2`
3. Set Subnet: `255.255.255.0`
4. Set Gateway: `192.168.100.1`
5. Apply and reconnect

---

### Step 2: Create VLANs

**Navigate to:** VLAN → 802.1Q VLAN → VLAN Config

**Create each VLAN:**

```
VLAN 1 (Management) - Already exists
VLAN 10 (Main_Netwo)
VLAN 20 (Homelab_se)
VLAN 30 (DMZ)
VLAN 40 (Malware_La)
```

**For each VLAN:**
1. Click "Add/Modify"
2. Enter VLAN ID
3. Enter VLAN Name
4. Configure port membership (see Port Configuration below)
5. Click "Apply"

---

### Step 3: Configure Port Assignments

**Navigate to:** VLAN → 802.1Q VLAN → Port Config

#### Port 1 (Router)
```
VLAN 1: Untagged
PVID: 1
Purpose: Router connection (management VLAN)
```

#### Port 2 (Proxmox - TRUNK PORT) ⭐ CRITICAL
```
VLAN 1: Untagged (for Proxmox host management)
VLAN 10: Tagged (for VMs)
VLAN 20: Tagged (for VMs)
VLAN 30: Tagged (for VMs)
VLAN 40: Tagged (for VMs)
PVID: 1

Why this configuration:
- Untagged VLAN 1 allows Proxmox host to communicate on management network
- Tagged VLANs 10-40 allow VMs to use VLAN segmentation
- PVID 1 means default traffic (untagged) goes to VLAN 1
```

#### Port 3 (Work Laptop)
```
VLAN 10: Untagged
PVID: 10
Purpose: Work laptop on client VLAN
```

#### Port 4 (Gaming PC)
```
VLAN 1: Untagged
PVID: 1
Purpose: Gaming PC on management VLAN (temporary for Phase 2)
Note: Will move to VLAN 10 in Phase 3 when inter-VLAN routing is configured
```

#### Ports 5-8 (Available)
```
VLAN 1: Untagged
PVID: 1
Purpose: Available for future devices
```

---

### Step 4: Configure Port VIDs

**Navigate to:** VLAN → 802.1Q VLAN → Port PVID

Set PVIDs for each port:
```
Port 1: PVID = 1 (Router)
Port 2: PVID = 1 (Proxmox trunk - default VLAN is 1)
Port 3: PVID = 10 (Work Laptop)
Port 4: PVID = 1 (Gaming PC)
Ports 5-8: PVID = 1 (Default)
```

**What is PVID?**
> PVID (Port VLAN ID) determines which VLAN untagged traffic belongs to. When a device sends an untagged packet, the switch tags it with the port's PVID.

---

### Step 5: Backup Switch Configuration

**Before proceeding to Proxmox:**

1. Navigate to: **System Tools → Config Files**
2. Click "Backup"
3. Save file as: `TP-Link-SG108E-VLAN-Config-YYYY-MM-DD.bin`
4. Store in safe location

**Why backup?**
- Allows quick recovery if configuration is lost
- Documents working configuration
- Enables restoration after switch reset

---

### Step 6: Configure Proxmox VLAN-Aware Bridge

**Method 1: Web UI (Recommended)**

1. Access Proxmox: `https://192.168.100.25:8006`
2. Navigate to: Node → System → Network
3. Click on `vmbr0`
4. Click "Edit"
5. Check: **"VLAN aware"** ✅
6. Verify existing settings:
   ```
   Bridge ports: eno1 (or your NIC name)
   IPv4/CIDR: 192.168.100.25/24
   Gateway: 192.168.100.1
   ```
7. Click "OK"
8. Click "Apply Configuration"
9. Confirm (network will briefly disconnect)

**Method 2: CLI**

```bash
# SSH to Proxmox or use Shell

# Backup current configuration
cp /etc/network/interfaces /etc/network/interfaces.backup.$(date +%Y%m%d)

# Edit network configuration
nano /etc/network/interfaces

# Modify vmbr0 section:
auto vmbr0
iface vmbr0 inet static
        address 192.168.100.25/24
        gateway 192.168.100.1
        bridge-ports eno1
        bridge-stp off
        bridge-fd 0
        bridge-vlan-aware yes          # ADD THIS LINE
        bridge-vids 2-4094             # ADD THIS LINE

# Save and apply
ifreload -a
```

---

### Step 7: Verify Configuration

**Verify VLAN-aware bridge:**
```bash
# Check if VLAN filtering is enabled (should output: 1)
cat /sys/class/net/vmbr0/bridge/vlan_filtering

# View bridge VLAN configuration
bridge vlan show

# Expected output:
# port              vlan-id  
# eno1              1 PVID Egress Untagged
#                   10
#                   20
#                   30
#                   40
# vmbr0             1 PVID Egress Untagged
#                   10
#                   20
#                   30
#                   40

# Verify network interface
ip addr show vmbr0
# Should show: inet 192.168.100.25/24
```

**Test connectivity:**
```bash
# From Proxmox, ping key devices
ping -c 3 192.168.100.1    # Router
ping -c 3 192.168.100.2    # Switch
ping -c 3 192.168.100.49   # Gaming PC (or other device on VLAN 1)
```

---

## 🐛 Troubleshooting Guide

### Issue 1: Cannot Ping Proxmox from Gaming PC on VLAN 10

**Symptoms:**
- Switch ping works (192.168.100.2)
- Proxmox ping fails (192.168.100.25)
- Gaming PC is on VLAN 10 (Port PVID 10)

**Root Cause:**
- VLANs are isolated by design
- Proxmox host is on VLAN 1 (untagged)
- Gaming PC is on VLAN 10
- No inter-VLAN routing configured

**Solution:**
```
Option A: Move Gaming PC to VLAN 1 (temporary)
- Change Port 4 PVID to 1
- Configure Port 4 as VLAN 1 Untagged
- Remove Port 4 from VLAN 10

Option B: Configure inter-VLAN routing (Phase 3)
- Install and configure pfSense or router
- Create routing rules between VLANs
```

---

### Issue 2: Port 2 (Proxmox) Not Receiving Tagged VLAN Traffic

**Symptoms:**
- Proxmox host connectivity works
- VMs cannot communicate on tagged VLANs

**Root Cause:**
- Port 2 VLAN membership not configured correctly
- VLANs might not be tagged on Port 2

**Solution:**
```
Verify Port 2 configuration:
1. VLAN 1: Untagged (for host)
2. VLAN 10, 20, 30, 40: Tagged (for VMs)
3. PVID: 1

Check in: VLAN → 802.1Q VLAN → Port Config
Each VLAN should show:
- Port 2 as "Tagged" (except VLAN 1 which should be "Untagged")
```

---

### Issue 3: Switch Management IP Not Accessible

**Symptoms:**
- Cannot access switch web interface
- Switch configured with new IP but not responding

**Root Cause:**
- Switch management VLAN not configured correctly
- Management IP on wrong VLAN
- Client device not on same VLAN as switch management

**Solution:**
```bash
# Verify client and switch are on same VLAN
# Switch management should be on VLAN 1
# Client should also be on VLAN 1

# Check client IP configuration
ipconfig (Windows)
ip addr (Linux)

# Ensure client is on 192.168.100.0/24 network

# If still cannot access:
# 1. Reset switch to factory defaults (hold reset button 10 seconds)
# 2. Reconfigure from default IP 192.168.0.1
# 3. Apply VLAN configuration more carefully
```

---

### Issue 4: "General Failure" Error When Pinging

**Symptoms:**
```powershell
PS> ping 192.168.100.25
General failure.
```

**Root Cause:**
- Network adapter has incorrect or missing default gateway
- ARP (Address Resolution Protocol) failure
- Network interface not fully configured

**Solution:**
```powershell
# Check network configuration
ipconfig /all

# Verify:
# 1. IP address is correct
# 2. Subnet mask is correct (255.255.255.0)
# 3. Default gateway is set

# If gateway is missing or wrong:
# 1. Open Network Adapter settings
# 2. Set static IP with correct gateway
# 3. For VLAN 1: Gateway = 192.168.100.1
```

---

### Issue 5: Proxmox VLAN-Aware Bridge Not Working

**Symptoms:**
- VMs with VLAN tags cannot communicate
- VLAN filtering shows as disabled

**Root Cause:**
- VLAN-aware setting not applied correctly
- Network interface not restarted after configuration

**Solution:**
```bash
# Check VLAN filtering status
cat /sys/class/net/vmbr0/bridge/vlan_filtering

# If output is 0 (disabled), re-enable:
echo 1 > /sys/class/net/vmbr0/bridge/vlan_filtering

# Or restart network service
systemctl restart networking

# Or reapply network configuration
ifreload -a

# Verify in Proxmox GUI:
# Node → System → Network → vmbr0
# Should show "VLAN aware: yes"
```

---

## 📚 Key Concepts Learned

### 1. VLAN Tagging vs Untagged Traffic

**Untagged Traffic:**
- Normal network packets without VLAN identification
- Switch uses port's PVID to determine which VLAN the packet belongs to
- Example: Proxmox host sends untagged packets → Switch sees Port 2 PVID=1 → Assigns to VLAN 1

**Tagged Traffic:**
- Network packets with 802.1Q VLAN tag embedded
- Contains 12-bit VLAN ID (0-4095)
- Switch reads the tag and forwards to appropriate VLAN
- Example: VM on VLAN 10 → Sends packet with VLAN 10 tag → Switch forwards to VLAN 10

### 2. Trunk Ports vs Access Ports

**Access Port:**
- Connects to end devices (PCs, phones, printers)
- Carries traffic for ONE VLAN only (untagged)
- PVID determines which VLAN
- Example: Port 4 (Gaming PC) - Access port on VLAN 1

**Trunk Port:**
- Connects network equipment (switches, routers, hypervisors)
- Carries traffic for MULTIPLE VLANs (tagged and/or untagged)
- Can have one native/untagged VLAN (PVID) plus tagged VLANs
- Example: Port 2 (Proxmox) - Trunk port with VLAN 1 untagged + VLANs 10,20,30,40 tagged

### 3. PVID (Port VLAN ID)

**Purpose:**
- Determines which VLAN untagged traffic is assigned to
- Every port must have a PVID
- Default PVID is usually 1

**How it works:**
1. Device sends untagged packet to switch port
2. Switch checks port's PVID
3. Switch tags packet internally with PVID value
4. Switch forwards packet through VLAN

**Example:**
```
Gaming PC (untagged packet) → Port 4 (PVID=1) → Switch tags as VLAN 1 → Forwards to VLAN 1
```

### 4. VLAN Isolation

**Key Principle:**
> VLANs create separate broadcast domains. Devices on different VLANs cannot communicate directly - they need a router/firewall to enable inter-VLAN routing.

**Why this matters:**
- Security: Malware lab (VLAN 40) cannot access management network (VLAN 1)
- Organization: Separate client devices from services
- Performance: Reduces broadcast traffic

**Current Status:**
- VLANs are isolated (Phase 2)
- Inter-VLAN routing will be configured in Phase 3 with pfSense

### 5. Hybrid Trunk Port Configuration

**Port 2 (Proxmox) is a "Hybrid" trunk:**
- Carries untagged traffic for Proxmox host (VLAN 1)
- Carries tagged traffic for VMs (VLANs 10, 20, 30, 40)

**Why this design:**
- Proxmox host needs to communicate on management VLAN (VLAN 1)
- VMs need to use various VLANs for segmentation
- Single NIC on Proxmox handles both requirements

### 6. VLAN-Aware Bridge on Proxmox

**Purpose:**
- Allows VMs to use VLAN tags
- Proxmox bridge understands and processes 802.1Q VLAN tags
- Enables VMs to specify which VLAN they belong to

**Configuration:**
```
VM Network Settings:
├─ Bridge: vmbr0
├─ VLAN Tag: 10, 20, 30, or 40
└─ VM sends traffic tagged with specified VLAN
```

**How it works:**
1. VM configured with VLAN tag (e.g., 10)
2. VM sends packet
3. vmbr0 bridge adds VLAN 10 tag
4. Packet sent through eno1 to switch
5. Switch sees VLAN 10 tag on Port 2 (trunk)
6. Switch forwards to VLAN 10 network

---

## 💡 Lessons Learned

### Technical Insights

1. **Always verify PVID settings**
   - Incorrect PVID causes traffic to go to wrong VLAN
   - Port 2 PVID must be 1 for Proxmox host communication

2. **Trunk port native VLAN must be untagged**
   - Many tutorials show all VLANs as tagged
   - This breaks host management traffic
   - Native VLAN (PVID VLAN) should be untagged

3. **Test connectivity at each step**
   - Don't configure everything at once
   - Test after each change
   - Easier to identify what broke

4. **Document as you go**
   - Take screenshots of configurations
   - Note IP addresses and VLAN assignments
   - Save switch backups regularly

5. **VLAN isolation is REAL**
   - Be prepared to lose connectivity
   - Have backup access method (Wi-Fi)
   - Understand that VLANs need routing to communicate

### Troubleshooting Methodology

1. **Layer-by-layer approach:**
   ```
   Layer 1 (Physical): Cables connected? Link lights on?
   Layer 2 (Data Link): Switch ports configured? VLANs created?
   Layer 3 (Network): IP addresses correct? Gateway set?
   ```

2. **Test from known-good to unknown:**
   - Start with switch (known to work)
   - Move to next hop (Proxmox)
   - Isolate where failure occurs

3. **Use process of elimination:**
   - Test with Wi-Fi (VLAN 1) vs Ethernet (VLAN 10)
   - If one works and other doesn't, VLAN configuration issue
   - If neither works, broader network issue

4. **Verify assumptions:**
   - Don't assume configuration was saved
   - Check actual running config
   - Use CLI tools to verify (`bridge vlan show`)

### Common Mistakes to Avoid

1. ❌ **Setting all VLANs as tagged on trunk port**
   - Host OS cannot communicate
   - Native VLAN should be untagged

2. ❌ **Forgetting to set PVID**
   - Untagged traffic has no VLAN assignment
   - Can cause intermittent connectivity issues

3. ❌ **Not backing up switch configuration**
   - One mistake = reconfigure everything
   - Always backup before major changes

4. ❌ **Configuring Port 4 on both VLAN 1 and VLAN 10**
   - Creates VLAN conflict
   - Device should be on ONE access VLAN only

5. ❌ **Enabling VLAN-aware bridge without testing switch first**
   - If switch trunk isn't working, Proxmox changes won't help
   - Verify switch configuration before Proxmox

---

## 📊 Network Status After Phase 2

### Operational Status
✅ **Working:**
- All devices on VLAN 1 can communicate
- Switch management accessible at 192.168.100.2
- Proxmox accessible at 192.168.100.25
- Internet access working through router
- Proxmox VLAN-aware bridge configured and verified

⏳ **Pending (Phase 3):**
- Inter-VLAN routing (devices on VLAN 10 cannot reach VLAN 1 yet)
- DHCP per VLAN (currently only router provides DHCP for VLAN 1)
- Firewall rules per VLAN
- DNS per VLAN

### IP Address Allocation

**VLAN 1 (Management) - 192.168.100.0/24:**
```
.1    - Router (Gateway)
.2    - Switch Management
.25   - Proxmox Host
.49   - Gaming PC Ethernet
.51   - Gaming PC Wi-Fi
.50-254 - Available for DHCP/Static
```

**VLANs 10, 20, 30, 40:**
- Not yet assigned IP ranges
- Will be configured in Phase 3 with pfSense

### Cost Savings Tracking

**Phase 2 Contribution:**
- No direct cost savings yet (infrastructure phase)
- Enables future savings:
  - VLAN 20 (Homelab Services): Will host Nextcloud, Immich (replaces Google Drive, iCloud Photos)
  - VLAN 30 (DMZ): Will host game servers (replaces commercial hosting)

**Projected Annual Savings (once services deployed):**
- Google Drive replacement: ~RM 250/year
- iCloud Photos replacement: ~RM 130/year
- Game server hosting: ~RM 400/year
- **Total: ~RM 780/year**

---

## 🚀 Next Steps - Phase 3 Preview

**Phase 3: pfSense Integration & Inter-VLAN Routing**

1. **pfSense VLAN Configuration:**
   - Create VLAN interfaces on pfSense
   - Assign IP addresses per VLAN:
     - VLAN 1: 192.168.100.1 (existing)
     - VLAN 10: 192.168.10.1
     - VLAN 20: 192.168.20.1
     - VLAN 30: 192.168.30.1
     - VLAN 40: 192.168.40.1

2. **Inter-VLAN Routing:**
   - Enable routing between VLANs
   - Configure firewall rules:
     - VLAN 1 → All VLANs (management access)
     - VLAN 10 → Internet + VLAN 20 (client access to services)
     - VLAN 20 → Internet only (services)
     - VLAN 30 → Restricted (DMZ)
     - VLAN 40 → No access (isolated malware lab)

3. **Network Services:**
   - DHCP per VLAN
   - DNS per VLAN
   - NTP configuration
   - pfBlockerNG (ad blocking)

4. **Testing:**
   - Move Gaming PC to VLAN 10
   - Verify inter-VLAN routing
   - Test firewall rules
   - Confirm isolation of VLAN 40

---

## 📁 Files and Backups

**Created during Phase 2:**
- ✅ Switch configuration backup: `TP-Link-SG108E-VLAN-Config-2025-01-XX.bin`
- ✅ Proxmox network config backup: `/etc/network/interfaces.backup.20250XXX`
- ✅ Network topology documentation: `network-topology-phase2.md`
- ✅ Phase 2 completion documentation: This file

**Storage location:**
- Local: `/home/user/homelab-backups/phase2/`
- Git repository: `github.com/muzakkir97/homelab-infrastructure`

---

## 🎓 Skills Developed

### Technical Skills
- ✅ VLAN configuration on managed switches
- ✅ 802.1Q VLAN tagging understanding
- ✅ Trunk port vs access port configuration
- ✅ PVID concepts and application
- ✅ Proxmox networking with VLAN-aware bridges
- ✅ Network troubleshooting methodology
- ✅ Layer 2 switching fundamentals

### Soft Skills
- ✅ Systematic troubleshooting approach
- ✅ Documentation while building
- ✅ Patience during trial-and-error
- ✅ Research and learning from errors
- ✅ Asking for help when stuck

---

## 📸 Screenshots and Evidence

*Screenshots to include in GitHub repository:*
1. ✅ Switch VLAN configuration summary
2. ✅ Switch Port 2 (trunk) configuration
3. ✅ Switch PVID settings
4. ✅ Proxmox VLAN-aware bridge settings
5. ✅ Bridge VLAN output (`bridge vlan show`)
6. ✅ Successful ping tests
7. Network topology diagram

---

## 🙏 Acknowledgments

**Resources used:**
- Proxmox VE Documentation: https://pve.proxmox.com/wiki/Network_Configuration
- TP-Link Switch Manual: https://www.tp-link.com/
- VLAN tutorials and guides from various networking blogs
- Reddit r/homelab community

**AI Assistance:**
- Claude (Anthropic) for troubleshooting and guidance
- ChatGPT for concept clarification

---

## ✅ Phase 2 Sign-Off

**Completed by:** Muzakkir  
**Date:** January 2025  
**Status:** ✅ **COMPLETE AND VERIFIED**

**Checklist:**
- [x] Switch VLANs created (1, 10, 20, 30, 40)
- [x] Trunk port configured (Port 2 to Proxmox)
- [x] PVID assignments set correctly
- [x] Switch configuration backed up
- [x] Proxmox VLAN-aware bridge enabled
- [x] Bridge VLAN filtering verified (enabled)
- [x] Network connectivity tested and working
- [x] Documentation completed
- [x] Ready for Phase 3

**Network is stable and ready for Phase 3 implementation!** 🎉

---

*Next: [Phase 3 - pfSense Integration & Inter-VLAN Routing](../phase-3-pfsense-routing/README.md)*
