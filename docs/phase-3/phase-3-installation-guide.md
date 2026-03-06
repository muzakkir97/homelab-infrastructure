# Phase 3: pfSense Installation Guide

## Overview

This document provides a complete walkthrough of deploying pfSense on an AC8F Intel N100 Mini PC in a router-on-a-stick configuration with 802.1Q VLAN trunking.

**Deployment Date:** January 2026  
**Duration:** Approximately 4-6 hours  
**Difficulty:** Intermediate to Advanced  

---

## Prerequisites

### Hardware Required
- AC8F Intel N100 Mini PC (or similar x86-64 device)
- Monitor with HDMI connection
- USB keyboard
- Bootable USB drive (8GB minimum)
- Ethernet cables (at least 2)
- Managed switch with VLAN support (TP-Link TL-SG108E or similar)

### Software Required
- pfSense CE 2.7.2 or later (AMD64 USB Memstick Installer)
- Rufus (for creating bootable USB on Windows)

### Network Requirements
- Working internet connection via ISP router
- Access to switch management interface
- DHCP-enabled network for WAN interface

---

## Part 1: pfSense Installation

### Step 1: Create Bootable USB Drive

1. Download pfSense CE from https://www.pfsense.org/download/
   - Architecture: AMD64 (64-bit)
   - Installer: USB Memstick Installer
   - File: `pfSense-CE-2.7.2-RELEASE-amd64.img.gz`

2. Download and run Rufus from https://rufus.ie/

3. Configure Rufus:
   - Device: Select your USB drive
   - Boot selection: Select the pfSense .img.gz file
   - Partition scheme: MBR
   - Target system: BIOS or UEFI

4. Click START and wait for completion

5. Safely eject USB drive

### Step 2: Boot from USB

1. Connect monitor and keyboard to AC8F Mini PC
2. Insert pfSense USB drive
3. Power on the device
4. Press DEL or F2 to enter BIOS if needed
5. Set boot order to USB first
6. Save and reboot

### Step 3: pfSense Installer

When the pfSense installer loads, you'll see detected network interfaces.

**VLAN Setup Prompt:**
```
Should VLANs be set up now [y|n]?
```
**Answer:** `n` (We'll configure VLANs later via web interface)

**WAN Interface Assignment:**
```
Enter the WAN interface name or 'a' for auto-detection:
```
- For AC8F: Enter `igc3` (LAN2 port - see troubleshooting section)
- Other devices: Select appropriate interface for WAN

**LAN Interface Assignment:**
```
Enter the LAN interface name:
```
- For AC8F: Enter `igc2` (LAN3 port)
- This will be your VLAN trunk interface

**Optional Interfaces:**
Press Enter to skip all optional interface prompts.

**Confirmation:**
```
WAN  -> igc3
LAN  -> igc2

Do you want to proceed [y|n]?
```
**Answer:** `y`

### Step 4: Installation to SSD

pfSense will install to the internal storage. Wait for completion (5-10 minutes).

When prompted:
```
Remove the installation media (USB drive) and press Enter to reboot.
```
1. Remove USB drive
2. Press Enter

### Step 5: First Boot

After reboot, you'll see the pfSense console menu:

```
*** Welcome to pfSense 2.7.2-RELEASE (amd64) on pfSense ***

WAN (wan)       -> igc3        -> v4: [IP from DHCP]
LAN (lan)       -> igc2        -> v4: 192.168.1.1/24

0) Logout (SSH only)
1) Assign Interfaces
2) Set interface(s) IP address
...
```

---

## Part 2: Physical Connections

### WAN Connection
Connect your WAN cable:
- **From:** ISP router LAN port
- **To:** AC8F port assigned as WAN (LAN2/igc3 for AC8F)

Verify WAN connectivity:
1. At console menu, check if WAN shows IP address
2. Press `7` (Ping host)
3. Enter `8.8.8.8`
4. Successful pings = WAN working

### LAN Connection
Connect your LAN cable:
- **From:** AC8F port assigned as LAN (LAN3/igc2 for AC8F)
- **To:** Switch trunk port (will configure later)

---

## Part 3: Initial Web Interface Access

### Option A: Direct Connection

1. Connect a computer directly to the same switch port as pfSense LAN
2. Computer should receive DHCP address (192.168.1.x)
3. Open browser: https://192.168.1.1
4. Accept security warning (self-signed certificate)

### Option B: Manual IP Configuration

If DHCP doesn't work:
1. Set computer IP: 192.168.1.50
2. Subnet: 255.255.255.0
3. Gateway: 192.168.1.1
4. DNS: 192.168.1.1
5. Access https://192.168.1.1

### Default Login
- **Username:** admin
- **Password:** pfsense

---

## Part 4: Setup Wizard

Complete the pfSense setup wizard:

### Step 1: General Information
- Hostname: `pfsense-homelab`
- Domain: `homelab.local`
- Primary DNS: `8.8.8.8`
- Secondary DNS: `8.8.4.4`
- Override DNS: ☑️ Checked

### Step 2: Time Server
- Time server: `pool.ntp.org`
- Timezone: Select your timezone

### Step 3: WAN Configuration
- Should show DHCP configuration
- No changes needed if WAN is working

### Step 4: LAN Configuration
- IP Address: 192.168.1.1
- Subnet Mask: 24
- No changes needed

### Step 5: Admin Password
**IMPORTANT:** Change from default!
- Set a strong password
- Remember this password

### Step 6: Reload
Click "Reload" to apply settings.

---

## Part 5: VLAN Configuration

### Create VLAN Interfaces

Navigate to: **Interfaces → Assignments → VLANs**

Create each VLAN:

| VLAN Tag | Parent Interface | Description |
|----------|------------------|-------------|
| 10 | igc2 (LAN) | Main_Network |
| 20 | igc2 (LAN) | Homelab_Services |
| 30 | igc2 (LAN) | DMZ |
| 40 | igc2 (LAN) | Malware_Lab |

For each VLAN:
1. Click "+ Add"
2. Select Parent Interface: `igc2` (or your LAN interface)
3. Enter VLAN Tag
4. Enter Description
5. Click "Save"

### Assign VLAN Interfaces

Navigate to: **Interfaces → Assignments**

In the "Available network ports" dropdown:
1. Select "VLAN 10 on igc2"
2. Click "+ Add"
3. Repeat for VLANs 20, 30, 40

Click "Save"

### Configure Each VLAN Interface

For each OPT interface, configure:

**OPT1 (VLAN 10 - Main Network):**
- Enable: ☑️ Checked
- Description: `VLAN10_MAIN`
- IPv4 Configuration: Static IPv4
- IPv4 Address: `192.168.10.1/24`
- Save and Apply Changes

**OPT2 (VLAN 20 - Homelab Services):**
- Enable: ☑️ Checked
- Description: `VLAN20_SERVICES`
- IPv4 Address: `192.168.20.1/24`

**OPT3 (VLAN 30 - DMZ):**
- Enable: ☑️ Checked
- Description: `VLAN30_DMZ`
- IPv4 Address: `192.168.30.1/24`

**OPT4 (VLAN 40 - Malware Lab):**
- Enable: ☑️ Checked
- Description: `VLAN40_MALWARE`
- IPv4 Address: `192.168.40.1/24`

---

## Part 6: DHCP Server Configuration

Navigate to: **Services → DHCP Server**

For each VLAN interface tab:

| Interface | DHCP Range Start | DHCP Range End |
|-----------|------------------|----------------|
| LAN | 192.168.1.51 | 192.168.1.250 |
| VLAN10_MAIN | 192.168.10.51 | 192.168.10.250 |
| VLAN20_SERVICES | 192.168.20.51 | 192.168.20.250 |
| VLAN30_DMZ | 192.168.30.51 | 192.168.30.250 |
| VLAN40_MALWARE | 192.168.40.51 | 192.168.40.250 |

For each:
1. ☑️ Enable DHCP server on interface
2. Set range from and to addresses
3. Click "Save"

---

## Part 7: Firewall Rules

Navigate to: **Firewall → Rules**

### Allow Internet Access (VLANs 1, 10, 20, 30)

For each VLAN that needs internet access:

1. Click on interface tab (e.g., VLAN10_MAIN)
2. Click "+ Add" (with up arrow)
3. Configure:
   - Action: Pass
   - Interface: [Current VLAN]
   - Address Family: IPv4
   - Protocol: Any
   - Source: [VLAN] subnets
   - Destination: any
   - Description: Allow [VLAN] to Internet
4. Save and Apply Changes

### Block Internet for Malware Lab (VLAN 40)

Navigate to: **Firewall → Rules → VLAN40_MALWARE**

**Rule 1 - Allow to pfSense:**
- Action: Pass
- Source: VLAN40_MALWARE subnets
- Destination: This Firewall (self)
- Description: Allow VLAN40 to pfSense

**Rule 2 - Block Internet:**
- Action: Block
- Source: VLAN40_MALWARE subnets
- Destination: any
- Description: Block VLAN40 from Internet

**Important:** Rule 1 must be ABOVE Rule 2 (rules processed top to bottom)

---

## Part 8: Switch Configuration

### Configure Trunk Port

Access your managed switch and configure the pfSense port as a trunk:

**Port Configuration for pfSense Trunk:**

| VLAN | Port Setting |
|------|--------------|
| VLAN 1 | Untagged, PVID 1 |
| VLAN 10 | Tagged |
| VLAN 20 | Tagged |
| VLAN 30 | Tagged |
| VLAN 40 | Tagged |

### Physical Connection

After configuring the trunk port:
1. Move pfSense LAN cable to the configured trunk port
2. Verify link lights on both switch and pfSense
3. Wait 30 seconds for link establishment

---

## Verification

### Check Dashboard

Navigate to: **Status → Dashboard**

Verify all interfaces show:
- Green up arrows
- Correct IP addresses
- Active status

### Test Connectivity

From a device on VLAN 1:
1. Ping pfSense: `ping 192.168.1.1`
2. Ping internet: `ping 8.8.8.8`
3. Test DNS: `ping google.com`

### Test Inter-VLAN Routing

From pfSense console (option 7):
1. Ping each VLAN gateway (192.168.10.1, 192.168.20.1, etc.)
2. All should respond

---

## Summary

After completing this guide, you will have:

- ✅ pfSense installed and configured
- ✅ WAN with internet connectivity
- ✅ 5 VLANs (1, 10, 20, 30, 40) with separate subnets
- ✅ DHCP servers on all VLANs
- ✅ Firewall rules for internet access
- ✅ Isolated malware lab (VLAN 40)
- ✅ Router-on-a-stick architecture with VLAN trunking

---

## Appendix: AC8F Port Mapping

| Physical Port | Interface | Recommended Use |
|---------------|-----------|-----------------|
| Port 1010 | igc0 | ⚠️ May not work (driver issue) |
| LAN4 | igc1 | ⚠️ May not work (driver issue) |
| LAN3 | igc2 | ✅ LAN / VLAN Trunk |
| LAN2 | igc3 | ✅ WAN |
| LAN1 | igc4 | Unknown / Not tested |

**Note:** Intel i226-V chipset has known compatibility issues with FreeBSD. If a port doesn't work, try the next one.

---

*Document created: January 2026*
