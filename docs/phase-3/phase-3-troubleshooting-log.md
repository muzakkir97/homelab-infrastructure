# Phase 3: Troubleshooting Log

## Overview

This document records all issues encountered during the pfSense deployment and their solutions. This serves as a reference for future troubleshooting and demonstrates systematic problem-solving skills.

---

## Issue #1: Intel i226-V Network Ports Not Working

### Problem Description
Multiple ethernet ports on the AC8F Intel N100 Mini PC showed "no carrier" status in pfSense, even when physical link lights were illuminated on both the switch and the AC8F.

### Affected Hardware
- **Device:** AC8F Intel N100 Mini PC
- **Chipset:** Intel i226-V 2.5GbE (all 4 ports)
- **OS:** pfSense 2.7.2-RELEASE (FreeBSD 14.0)

### Symptoms

**Port 1010 (igc0):**
```
igc0: flags=8802<BROADCAST,SIMPLEX,MULTICAST>
status: no carrier
media: Ethernet autoselect
```
- No link light on port
- DHCP requests timed out with "no link ... giving up"

**LAN4 (igc1):**
```
igc1: flags=1008802<BROADCAST,SIMPLEX,MULTICAST,LOWER_UP>
status: no carrier
media: Ethernet autoselect
```
- Link light visible (orange)
- LOWER_UP flag indicated physical layer detected
- But driver reported "no carrier"

### Diagnostic Steps Performed

1. **Verified cable functionality:**
   - Same cable worked with test PC on ISP Router ✅
   - Cable confirmed good

2. **Verified ISP router ports:**
   - Multiple ports tested: Working ✅
   - DHCP server confirmed active

3. **Tested multiple AC8F ports:**
   - Port 1010 (igc0): No carrier ❌
   - LAN4 (igc1): No carrier ❌
   - LAN3 (igc2): **WORKING** ✅
   - LAN2 (igc3): **WORKING** ✅

4. **Attempted driver fixes:**
   ```bash
   # Force media speed
   ifconfig igc1 media 1000baseT mediaopt full-duplex up
   ifconfig igc1 media 2500Base-T mediaopt full-duplex up
   
   # Disable hardware offloading
   ifconfig igc1 -rxcsum -txcsum -tso -lro up
   
   # Manual DHCP request
   dhclient -v igc1
   ```
   All attempts failed with "no link ... giving up"

### Root Cause Analysis

The Intel i226-V 2.5GbE chipset has known compatibility issues with FreeBSD's igc driver. This is a documented issue in the pfSense/FreeBSD community.

**Evidence:**
- All 4 interfaces correctly detected by FreeBSD:
  ```
  igc0: Intel(R) Ethernet Controller I226-V @ [MAC_ADDRESS_0]
  igc1: Intel(R) Ethernet Controller I226-V @ [MAC_ADDRESS_1]
  igc2: Intel(R) Ethernet Controller I226-V @ [MAC_ADDRESS_2]
  igc3: Intel(R) Ethernet Controller I226-V @ [MAC_ADDRESS_3]
  ```
- Hardware not defective (2 of 4 ports work)
- Driver initialization issue on some ports

### Solution

**Working Configuration:**
- **WAN:** igc3 (LAN2 physical port) - 2.5 Gbps connection ✅
- **LAN:** igc2 (LAN3 physical port) - 1 Gbps connection ✅

**Non-Working Ports (Documented):**
- igc0 (Port 1010): Not used
- igc1 (LAN4): Not used

### Impact
- Two ports are sufficient for router-on-a-stick configuration
- WAN + single LAN trunk is all that's needed
- No functional limitation for project goals

### Lessons Learned
1. Always verify cable/port functionality with known-working device
2. Test all available ports systematically when encountering issues
3. Intel i226-V has known FreeBSD driver issues - not all ports may work
4. Document MAC addresses and interface mappings for reference

---

## Issue #2: Cannot Access Switch After VLAN Configuration

### Problem Description
After configuring pfSense and moving workstation to VLAN 1 (192.168.1.x network), the TP-Link switch web interface became inaccessible.

### Symptoms
- Browser timeout when accessing switch management IP
- Workstation on 192.168.1.x cannot reach ISP network subnet

### Root Cause
Network segmentation working as designed:
- Workstation: 192.168.1.x (pfSense network)
- Switch: On ISP router network
- Different subnets = no direct communication

### Solution

**Option 1: Connect via ISP Router WiFi**
1. Connect computer to ISP WiFi
2. Computer gets IP on ISP network
3. Access switch management interface
4. Make changes
5. Disconnect WiFi, reconnect ethernet

**Option 2: Temporary Manual IP**
1. Set computer IP to ISP network range
2. Access switch
3. Revert to DHCP after

### Resolution Status
✅ Resolved - Access methods documented

### Lessons Learned
1. This is expected behavior with proper network segmentation
2. Document access methods for devices on different networks
3. Switch management doesn't need to be on same network as clients

---

## Issue #3: DHCP Not Assigning IP on First Boot

### Problem Description
After initial pfSense installation, WAN interface showed no IP address.

### Symptoms
```
WAN (wan) -> igc0 -> v4: [blank]
```

### Diagnostic Steps

1. **Check interface status:**
   ```bash
   ifconfig igc0
   # Output showed: status: no carrier
   ```

2. **Manual DHCP request:**
   ```bash
   dhclient -v igc0
   # Output: igc0: no link ... giving up
   ```

3. **Verified cable connected:**
   - Cable in Port 1010 ✅
   - ISP Router port ✅

### Root Cause
Port 1010 (igc0) driver issue - same as Issue #1

### Solution
Reassigned WAN to working port (igc3):
1. Console option 1 (Assign Interfaces)
2. WAN = igc3
3. LAN = igc2
4. Moved physical cable to correct port

### Resolution Status
✅ Resolved

---

## Issue #4: Switch Port VLAN Configuration

### Problem Description
Initial confusion about which ports needed VLAN tagging for router-on-a-stick.

### Understanding Required

**Trunk Port (to pfSense):**
- Must carry ALL VLANs
- VLAN 1: Untagged (native VLAN)
- VLANs 10, 20, 30, 40: Tagged

**Access Ports (to devices):**
- Single VLAN only
- Traffic untagged
- PVID determines VLAN membership

### Correct Configuration

**Trunk Port (pfSense):**
```
VLAN 1:  Untagged, PVID 1
VLAN 10: Tagged
VLAN 20: Tagged
VLAN 30: Tagged
VLAN 40: Tagged
```

**Access Port (Workstation - VLAN 1):**
```
VLAN 1:  Untagged, PVID 1
```

### Resolution Status
✅ Resolved - All VLANs routing correctly

---

## Hardware Reference

### AC8F Port to Interface Mapping

| Physical Label | Interface | MAC Address | Status |
|----------------|-----------|-------------|--------|
| Port 1010 | igc0 | [MAC_0] | ❌ No carrier |
| LAN4 | igc1 | [MAC_1] | ❌ No carrier |
| LAN3 | igc2 | [MAC_2] | ✅ Working |
| LAN2 | igc3 | [MAC_3] | ✅ Working |

### Recommended AC8F Configuration
- **WAN:** Use LAN2 port (igc3) - supports 2.5 Gbps
- **LAN:** Use LAN3 port (igc2) - supports 1 Gbps
- **Unused:** Port 1010, LAN4 (driver issues)

---

## Commands Reference

### Check Interface Status
```bash
ifconfig igc0
ifconfig -a | grep "^igc"
```

### Check All Detected NICs
```bash
dmesg | grep -i "igc.*Ethernet"
pciconf -lv | grep -A 3 "network"
```

### Manual DHCP Request
```bash
dhclient -r igc0    # Release
dhclient igc0       # Request
dhclient -v igc0    # Verbose mode
```

### Check Routing Table
```bash
netstat -rn
```

### Ping Test
From pfSense console menu, option 7 (Ping host)

---

## Summary

| Issue | Root Cause | Resolution |
|-------|------------|------------|
| Ports not working | Intel i226-V driver bug | Use igc2/igc3 |
| Switch inaccessible | Network segmentation | Access via WiFi |
| No WAN IP | Wrong port assigned | Reassign to igc3 |
| VLAN confusion | Configuration learning | Documented properly |

**Key Takeaway:** Systematic troubleshooting and documentation enabled successful deployment despite hardware compatibility challenges.

---

*Document created: January 2026*
