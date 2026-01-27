# Phase 3: Network Topology

## Current Network Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                              INTERNET                                        │
│                           ([ISP_NAME])                                       │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                     ISP ROUTER: [ISP_ROUTER_MODEL]                          │
│                           IP: [ISP_ROUTER_IP]                               │
├─────────────────────────────────────────────────────────────────────────────┤
│  WiFi (SSID: [WIFI_SSID])           │  LAN Ports                            │
│  ├── Shared Devices                 │  ├── Port 1: TP-Link Switch (Mgmt)   │
│  ├── Work Laptop                    │  ├── Port 2: [Available]             │
│  └── Mobile Devices                 │  ├── Port 3: → pfSense WAN           │
│      ([ISP_DHCP_RANGE])             │  └── Port 4: [Available]             │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      │ Ethernet Cable
                                      │
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    pfSense ROUTER: AC8F Intel N100                          │
│                        pfSense 2.7.2-RELEASE                                │
├─────────────────────────────────────────────────────────────────────────────┤
│  HARDWARE PORTS:                                                            │
│  ┌─────────┬─────────┬─────────┬─────────┬─────────┐                       │
│  │Port 1010│  LAN4   │  LAN3   │  LAN2   │  LAN1   │                       │
│  │  igc0   │  igc1   │  igc2   │  igc3   │   ?     │                       │
│  │   ❌    │   ❌    │   ✅    │   ✅    │   ?     │                       │
│  │(broken) │(broken) │  LAN    │  WAN    │(unused) │                       │
│  └─────────┴─────────┴────┬────┴────┬────┴─────────┘                       │
│                           │         │                                       │
│  INTERFACES:              │         └── WAN: [WAN_IP]/24                    │
│  ├── WAN (igc3)          │              (DHCP from ISP Router)              │
│  │   └── 2.5 Gbps        │              Gateway: [ISP_ROUTER_IP]            │
│  └── LAN (igc2)          │                                                  │
│      └── 1 Gbps Trunk    └── LAN Trunk (802.1Q VLAN Tagging)               │
│                                                                             │
│  VLAN SUBINTERFACES:                                                        │
│  ├── igc2 (untagged)  → VLAN 1:  192.168.1.1/24  (Management)              │
│  ├── igc2.10 (tagged) → VLAN 10: 192.168.10.1/24 (Main Network)            │
│  ├── igc2.20 (tagged) → VLAN 20: 192.168.20.1/24 (Homelab Services)        │
│  ├── igc2.30 (tagged) → VLAN 30: 192.168.30.1/24 (DMZ)                     │
│  └── igc2.40 (tagged) → VLAN 40: 192.168.40.1/24 (Malware Lab) [ISOLATED]  │
└─────────────────────────────────────────────────────────────────────────────┘
                                      │
                                      │ 802.1Q VLAN Trunk
                                      │ (VLANs 1,10,20,30,40)
                                      ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                    MANAGED SWITCH: TP-Link TL-SG108E                        │
│                        Management IP: [SWITCH_MGMT_IP]                      │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌────────┬────────┬────────┬────────┬────────┬────────┬────────┬────────┐ │
│  │ Port 1 │ Port 2 │ Port 3 │ Port 4 │ Port 5 │ Port 6 │ Port 7 │ Port 8 │ │
│  │ TRUNK  │ TRUNK  │        │ ACCESS │        │        │        │        │ │
│  │pfSense │Proxmox │  ---   │Worksta-│  ---   │  ---   │  ---   │  ---   │ │
│  │        │        │        │  tion  │        │        │        │        │ │
│  └────┬───┴────┬───┴────────┴────┬───┴────────┴────────┴────────┴────────┘ │
│       │        │                 │                                         │
│  PORT CONFIGURATIONS:                                                       │
│  ├── Port 1 (pfSense Trunk):                                               │
│  │   └── VLAN 1: Untagged, PVID 1                                         │
│  │   └── VLAN 10,20,30,40: Tagged                                         │
│  ├── Port 2 (Proxmox Trunk):                                               │
│  │   └── VLAN 1: Untagged, PVID 1                                         │
│  │   └── VLAN 10,20,30,40: Tagged                                         │
│  └── Port 4 (Workstation):                                                 │
│      └── VLAN 1: Untagged, PVID 1 (Access Port)                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
          │                │                     │
          │                │                     │
          ▼                ▼                     ▼
┌─────────────────┐ ┌─────────────────┐ ┌─────────────────┐
│    PROXMOX      │ │   WORKSTATION   │ │    FUTURE       │
│    SERVER       │ │                 │ │    DEVICES      │
├─────────────────┤ ├─────────────────┤ ├─────────────────┤
│ Ryzen 5 5600X   │ │ VLAN 1          │ │ Pi-hole         │
│ 32GB RAM        │ │ 192.168.1.x     │ │ UGREEN NAS      │
│ ZFS Mirror      │ │ (DHCP)          │ │ Game Servers    │
│                 │ │                 │ │ VMs             │
│ Can host VMs on:│ │ Access pfSense: │ │                 │
│ - VLAN 10       │ │ https://        │ │ To be deployed  │
│ - VLAN 20       │ │ 192.168.1.1     │ │ in Phase 4+     │
│ - VLAN 30       │ │                 │ │                 │
│ - VLAN 40       │ │                 │ │                 │
└─────────────────┘ └─────────────────┘ └─────────────────┘
```

---

## VLAN Summary Table

| VLAN ID | Name | Subnet | Gateway | DHCP Range | Firewall |
|---------|------|--------|---------|------------|----------|
| 1 | Management | 192.168.1.0/24 | 192.168.1.1 | .51-.250 | Allow Internet |
| 10 | Main_Network | 192.168.10.0/24 | 192.168.10.1 | .51-.250 | Allow Internet |
| 20 | Homelab_Services | 192.168.20.0/24 | 192.168.20.1 | .51-.250 | Allow Internet |
| 30 | DMZ | 192.168.30.0/24 | 192.168.30.1 | .51-.250 | Allow Internet |
| 40 | Malware_Lab | 192.168.40.0/24 | 192.168.40.1 | .51-.250 | **BLOCKED** |

---

## Physical Cable Connections

| Connection | From | To | Cable Length |
|------------|------|-----|--------------|
| WAN | ISP Router LAN Port | AC8F LAN2 (igc3) | ~15 meters |
| LAN Trunk | AC8F LAN3 (igc2) | Switch Port 1 | ~1 meter |
| Proxmox | Proxmox NIC | Switch Port 2 | ~1 meter |
| Workstation | Workstation NIC | Switch Port 4 | ~2 meters |

---

## Traffic Flow Examples

### Example 1: Workstation to Internet
```
Workstation (192.168.1.x, VLAN 1)
    │
    ▼ [Untagged frame]
Switch Port 4
    │
    ▼ [Tagged as VLAN 1... but VLAN 1 is untagged on trunk]
Switch Port 1 → pfSense LAN (igc2)
    │
    ▼ [Routed through pfSense]
pfSense WAN (igc3)
    │
    ▼ [NAT to WAN IP]
ISP Router
    │
    ▼
INTERNET
```

### Example 2: Future VM on VLAN 20 to Internet
```
VM (192.168.20.x, VLAN 20)
    │
    ▼ [Tagged frame: VLAN 20]
Proxmox vSwitch
    │
    ▼ [Tagged frame: VLAN 20]
Switch Port 2 → Switch Port 1 (trunk)
    │
    ▼ [Tagged frame: VLAN 20]
pfSense LAN (igc2.20)
    │
    ▼ [Routed through pfSense]
pfSense WAN (igc3)
    │
    ▼ [NAT]
INTERNET
```

### Example 3: Malware Lab (BLOCKED)
```
Malware VM (192.168.40.x, VLAN 40)
    │
    ▼ [Tagged frame: VLAN 40]
Switch → pfSense (igc2.40)
    │
    ▼ [Firewall: BLOCK rule]
    ❌ DROPPED
    
(Can reach pfSense at 192.168.40.1 for DNS only)
```

---

## Access Reference

| Resource | IP Address | Access Method |
|----------|------------|---------------|
| pfSense Web UI | 192.168.1.1 | https://192.168.1.1 from VLAN 1 |
| ISP Router | [ISP_ROUTER_IP] | Via WiFi or manual IP |
| TP-Link Switch | [SWITCH_MGMT_IP] | Via WiFi or manual IP |
| Proxmox | TBD | Will be on VLAN 1 or 20 |

---

## Placeholder Reference

| Placeholder | Description |
|-------------|-------------|
| [ISP_NAME] | Your internet service provider |
| [ISP_ROUTER_MODEL] | ISP router model number |
| [ISP_ROUTER_IP] | ISP router gateway IP (e.g., 192.168.x.1) |
| [WIFI_SSID] | Your WiFi network name |
| [ISP_DHCP_RANGE] | DHCP range from ISP router |
| [WAN_IP] | pfSense WAN IP from DHCP |
| [SWITCH_MGMT_IP] | Switch management IP address |

---

*Diagram created: January 2026*
