# Phase 3 Summary: pfSense Router-on-a-Stick Deployment

## Document Purpose
This summary provides context for future Claude sessions to continue the homelab infrastructure project. It contains the current state of the project, what was accomplished, and what's planned next.

---

## Project Overview

**Owner:** [OWNER_NAME]  
**Role:** Customer Service Engineer at [COMPANY], Malaysia  
**Project:** Enterprise-grade homelab infrastructure  
**Repository:** github.com/[USERNAME]/homelab-infrastructure  
**Current Phase:** Phase 3 COMPLETE ✅  

### Project Goals
1. Replace expensive cloud subscriptions with self-hosted alternatives
2. Develop hands-on enterprise networking and cybersecurity skills
3. Create isolated malware testing environment for Red Team practice
4. Document everything for career portfolio (GitHub + LinkedIn)

---

## Phase 3 Completion Summary

### What Was Deployed
- **pfSense 2.7.2-RELEASE** on AC8F Intel N100 Mini PC
- **Router-on-a-stick architecture** with 802.1Q VLAN trunking
- **5 VLANs** routed through single physical interface
- **Centralized firewall** with inter-VLAN routing

### Hardware Configuration

#### AC8F Mini PC (pfSense Router)
| Port | Interface | Status | Function | Speed |
|------|-----------|--------|----------|-------|
| Port 1010 | igc0 | ❌ NOT WORKING | - | - |
| LAN4 | igc1 | ❌ NOT WORKING | - | - |
| LAN3 | igc2 | ✅ WORKING | LAN Trunk | 1 Gbps |
| LAN2 | igc3 | ✅ WORKING | WAN | 2.5 Gbps |

**Known Issue:** Intel i226-V driver compatibility with FreeBSD/pfSense. Ports igc0 and igc1 show "no carrier" even with physical link light. This is a known driver bug, not hardware defect.

**Working Solution:** Use igc2 (LAN3) for LAN trunk and igc3 (LAN2) for WAN.

#### Network Topology
```
Internet ([ISP_NAME])
        ↓
[ISP Router: [ISP_ROUTER_MODEL]]
├── IP: [ISP_ROUTER_IP]
├── WiFi: Shared network (unchanged)
└── LAN Port → AC8F WAN
        ↓
[AC8F pfSense: [WAN_IP] (WAN)]
├── WAN: igc3 (LAN2 port) @ 2.5 Gbps
└── LAN: igc2 (LAN3 port) @ 1 Gbps (VLAN Trunk)
        ↓ (802.1Q Tagged)
[TP-Link TL-SG108E Switch]
├── Port 1: pfSense Trunk (VLAN 1 Untagged, VLANs 10-40 Tagged)
├── Port 2: Proxmox Trunk
├── Port 4: Workstation (VLAN 1)
└── Ports 5-8: Available
```

### VLAN Configuration

| VLAN ID | Name | Subnet | Gateway | DHCP Range | Purpose |
|---------|------|--------|---------|------------|---------|
| 1 | Management | 192.168.1.0/24 | 192.168.1.1 | .51-.250 | Management, default |
| 10 | Main_Network | 192.168.10.0/24 | 192.168.10.1 | .51-.250 | General devices |
| 20 | Homelab_Services | 192.168.20.0/24 | 192.168.20.1 | .51-.250 | Pi-hole, NAS, VMs |
| 30 | DMZ | 192.168.30.0/24 | 192.168.30.1 | .51-.250 | Public-facing services |
| 40 | Malware_Lab | 192.168.40.0/24 | 192.168.40.1 | .51-.250 | Isolated malware analysis |

**Static IP Range:** .2-.50 reserved on each VLAN for servers/infrastructure

### Firewall Rules Summary
- **VLAN 1, 10, 20, 30:** Allow all traffic to internet (NAT)
- **VLAN 40 (Malware Lab):** 
  - ✅ Allow to pfSense (for DNS resolution)
  - ❌ Block all internet access (isolated)

### pfSense Access
- **URL:** https://192.168.1.1
- **Username:** admin
- **Password:** [CHANGED_FROM_DEFAULT]

### Switch Configuration
- **Switch IP:** [SWITCH_MGMT_IP] (on ISP router network)
- **Access Method:** Connect to ISP WiFi or manually set PC to ISP network range
- **Port 1:** VLAN 1 Untagged, VLANs 10/20/30/40 Tagged (pfSense trunk)
- **Port 2:** Trunk for Proxmox

---

## Previous Phases

### Phase 1: Hardware Procurement ✅
- Proxmox server (Ryzen 5 5600X, 32GB RAM, ZFS mirror)
- AC8F Intel N100 Mini PC for pfSense
- TP-Link TL-SG108E managed switch
- Raspberry Pi 4 (for Pi-hole - not yet deployed)
- UGREEN NASync NAS (not yet deployed)

### Phase 2: VLAN Planning & Switch Configuration ✅
- Created VLANs 1, 10, 20, 30, 40 on switch
- Configured trunk ports (tagged) and access ports (untagged)
- Documented network segmentation strategy

---

## Next Phases (Planned)

### Phase 4: Self-Hosted Services
- Deploy Pi-hole on VLAN 20 for DNS filtering
- Configure UGREEN NAS on VLAN 20
- Set up Tailscale VPN on pfSense
- Nginx Proxy Manager for reverse proxy

### Phase 5: Game Servers
- Minecraft server on VLAN 20
- Project Zomboid server
- Terraria server
- Domain: [DOMAIN_NAME]

### Phase 6: Security Lab
- Malware analysis VMs on VLAN 40
- Completely isolated from internet
- Red Team practice environment

---

## Key Technical Concepts Used

### Router-on-a-Stick
Single physical interface (igc2) carries traffic for all 5 VLANs using 802.1Q VLAN tagging. Each VLAN has a virtual subinterface on pfSense:
- igc2 (untagged) → VLAN 1
- igc2.10 (tagged) → VLAN 10
- igc2.20 (tagged) → VLAN 20
- igc2.30 (tagged) → VLAN 30
- igc2.40 (tagged) → VLAN 40

### 802.1Q VLAN Tagging
4-byte tag inserted into Ethernet frames to identify VLAN membership:
- **Tagged traffic:** Frame includes VLAN ID (for trunk ports)
- **Untagged traffic:** No VLAN tag (for access ports)
- **PVID:** Default VLAN for untagged incoming traffic

### Inter-VLAN Routing
Traffic between VLANs routes through pfSense firewall:
1. Device on VLAN 10 sends packet to VLAN 20
2. Switch sends tagged frame to pfSense (trunk)
3. pfSense routes packet (Layer 3)
4. pfSense checks firewall rules
5. If allowed, pfSense sends frame back to switch with VLAN 20 tag
6. Switch forwards to destination on VLAN 20

---

## Troubleshooting Reference

### Intel i226-V "No Carrier" Issue
**Symptoms:** Interface shows "no carrier" despite link light being on
**Affected Ports:** igc0 (Port 1010), igc1 (LAN4)
**Root Cause:** FreeBSD igc driver incompatibility with Intel i226-V 2.5GbE chipset
**Solution:** Use igc2 and igc3 which work correctly

### Accessing pfSense After Setup
**From Workstation (VLAN 1):** https://192.168.1.1
**Workstation should get:** IP from DHCP (192.168.1.51-250)

### Accessing TP-Link Switch
**Issue:** Switch on different subnet
**Solution 1:** Connect to ISP WiFi, then access switch management IP
**Solution 2:** Temporarily set PC IP to ISP network range, access switch, then revert

---

## Important Notes for Future Sessions

1. **Working ports only:** Use igc2 (LAN) and igc3 (WAN) on AC8F
2. **Switch access:** Requires ISP WiFi or manual IP change
3. **Shared network unaffected:** WiFi and ISP router unchanged
4. **Documentation focus:** Update GitHub after each phase
5. **Security compliance:** Hide sensitive IPs in public documentation

---

## Files to Update in Repository

```
homelab-infrastructure/
├── README.md                    # Update with Phase 3 completion
├── docs/
│   ├── phase-1/                 # Hardware procurement docs
│   ├── phase-2/                 # VLAN planning docs
│   └── phase-3/
│       ├── 00-pre-flight-guide.md
│       ├── 01-installation-guide.md
│       ├── 02-vlan-configuration.md
│       ├── 03-firewall-rules.md
│       ├── 04-troubleshooting-log.md
│       └── network-topology.md
└── diagrams/
    └── phase-3-topology.png
```

---

## Placeholder Reference (For Private Use Only)

Replace these placeholders with actual values in your private documentation:

| Placeholder | Description |
|-------------|-------------|
| [OWNER_NAME] | Your name |
| [COMPANY] | Your employer |
| [USERNAME] | GitHub username |
| [ISP_NAME] | Internet service provider |
| [ISP_ROUTER_MODEL] | ISP router model number |
| [ISP_ROUTER_IP] | ISP router gateway IP |
| [WAN_IP] | pfSense WAN IP from DHCP |
| [SWITCH_MGMT_IP] | Switch management IP |
| [DOMAIN_NAME] | Your domain name |
| [CHANGED_FROM_DEFAULT] | New admin password |

---

*Document generated: January 2026*
*Phase 3 completed: January 2026*
