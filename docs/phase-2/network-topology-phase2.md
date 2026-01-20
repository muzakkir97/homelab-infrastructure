# Homelab Network Topology - Phase 2 Complete

## Physical Network Layout

```
                    Internet
                       │
                       ↓
            ┌──────────────────────┐
            │  ISP Router/Modem    │
            │  (Bridge Mode)       │
            └──────────┬───────────┘
                       │
                       ↓
            ┌──────────────────────┐
            │  Main Router         │
            │  192.168.100.1       │
            │  (VLAN 1)            │
            └──────────┬───────────┘
                       │ Port 1
                       ↓
    ┌──────────────────────────────────────────────┐
    │     TP-Link TL-SG108E Managed Switch         │
    │           192.168.100.2 (VLAN 1)             │
    │                                               │
    │  Port 1: Router (VLAN 1 Untagged)           │
    │  Port 2: Proxmox TRUNK (VLAN 1 Untagged +   │
    │          VLAN 10,20,30,40 Tagged)            │
    │  Port 3: Work Laptop (VLAN 10 Untagged)     │
    │  Port 4: Gaming PC (VLAN 1 Untagged)        │
    │  Port 5-8: Available                         │
    └───────┬──────────────────────────────────────┘
            │ Port 2 (Trunk)
            ↓
    ┌──────────────────────────────────────────────┐
    │         Proxmox VE Server                     │
    │         192.168.100.25 (VLAN 1)              │
    │                                               │
    │  Hardware:                                    │
    │  • CPU: AMD Ryzen 5 5600X                    │
    │  • RAM: 32GB                                  │
    │  • NIC: nic0 (eno1) - 1Gbps                  │
    │                                               │
    │  Network Bridge:                              │
    │  • vmbr0 (VLAN-aware)                        │
    │    ├─ VLAN 1 (Untagged) - Host              │
    │    └─ VLAN 10,20,30,40 (Tagged) - VMs       │
    └──────────────────────────────────────────────┘
```

## VLAN Configuration

### VLAN 1 - Management Network
**Subnet:** 192.168.100.0/24  
**Gateway:** 192.168.100.1  
**Purpose:** Management and main network

| Device | IP Address | Switch Port | VLAN Config |
|--------|-----------|-------------|-------------|
| Router | 192.168.100.1 | Port 1 | Untagged |
| Switch Management | 192.168.100.2 | N/A | VLAN 1 |
| Proxmox Host | 192.168.100.25 | Port 2 | Untagged (PVID 1) |
| Gaming PC | 192.168.100.49 | Port 4 | Untagged |
| Available IPs | .50-.254 | Various | DHCP/Static |

### VLAN 10 - Client Network (Main_Netwo)
**Subnet:** TBD (Future: 192.168.10.0/24)  
**Gateway:** TBD (Phase 3)  
**Purpose:** Client devices and workstations

| Device | IP Address | Switch Port | VLAN Config |
|--------|-----------|-------------|-------------|
| Work Laptop | TBD | Port 3 | Untagged (PVID 10) |
| Future VMs | TBD | Port 2 (via Proxmox) | Tagged |

### VLAN 20 - Homelab Services
**Subnet:** TBD (Future: 192.168.20.0/24)  
**Gateway:** TBD (Phase 3)  
**Purpose:** Self-hosted services (Nextcloud, Immich, etc.)

| Service | IP Address | Location | VLAN Config |
|---------|-----------|----------|-------------|
| Nextcloud VM | TBD | Proxmox | Tagged VLAN 20 |
| Immich VM | TBD | Proxmox | Tagged VLAN 20 |
| Game Servers | TBD | Proxmox | Tagged VLAN 20 |

### VLAN 30 - DMZ (Isolated Services)
**Subnet:** TBD (Future: 192.168.30.0/24)  
**Gateway:** TBD (Phase 3)  
**Purpose:** Public-facing services with restricted access

| Service | IP Address | Location | VLAN Config |
|---------|-----------|----------|-------------|
| Public Web Server | TBD | Proxmox | Tagged VLAN 30 |
| VPN Endpoint | TBD | Proxmox | Tagged VLAN 30 |

### VLAN 40 - Malware Lab
**Subnet:** TBD (Future: 192.168.40.0/24)  
**Gateway:** TBD (Phase 3)  
**Purpose:** Isolated environment for malware analysis

| Device | IP Address | Location | VLAN Config |
|--------|-----------|----------|-------------|
| Analysis VM | TBD | Proxmox | Tagged VLAN 40 |
| Sandbox | TBD | Proxmox | Tagged VLAN 40 |

## Switch Port Configuration Summary

| Port | Device | PVID | VLAN Membership | Link Type |
|------|--------|------|-----------------|-----------|
| 1 | Router | 1 | VLAN 1 (Untagged) | Access |
| 2 | Proxmox | 1 | VLAN 1 (Untagged)<br>VLAN 10,20,30,40 (Tagged) | Trunk |
| 3 | Work Laptop | 10 | VLAN 10 (Untagged) | Access |
| 4 | Gaming PC | 1 | VLAN 1 (Untagged) | Access |
| 5 | Available | 1 | VLAN 1 (Untagged) | Access |
| 6 | Available | 1 | VLAN 1 (Untagged) | Access |
| 7 | Available | 1 | VLAN 1 (Untagged) | Access |
| 8 | Available | 1 | VLAN 1 (Untagged) | Access |

## Proxmox Bridge Configuration

### vmbr0 (VLAN-aware Bridge)

```
Bridge: vmbr0
├─ Physical Port: nic0 (eno1)
├─ IP Address: 192.168.100.25/24
├─ Gateway: 192.168.100.1
├─ VLAN Filtering: Enabled
└─ VLAN Configuration:
   ├─ VLAN 1: PVID, Egress Untagged (Proxmox host)
   ├─ VLAN 10: Tagged (Available for VMs)
   ├─ VLAN 20: Tagged (Available for VMs)
   ├─ VLAN 30: Tagged (Available for VMs)
   └─ VLAN 40: Tagged (Available for VMs)
```

## Traffic Flow Examples

### Example 1: Proxmox Host Management
```
Gaming PC → Switch (VLAN 1) → Proxmox (VLAN 1 Untagged)
└─ Both on same VLAN, direct communication
```

### Example 2: Future VM on VLAN 10
```
VM (VLAN 10) → vmbr0 (tags packet with VLAN 10)
             → nic0 (sends tagged packet)
             → Switch Port 2 (receives VLAN 10 tagged)
             → Switch Port 3 (forwards to Work Laptop, removes tag)
             → Work Laptop receives untagged packet
```

### Example 3: Inter-VLAN Communication (Phase 3)
```
Device VLAN 10 → Switch → pfSense/Router → Switch → Device VLAN 20
└─ Requires routing/firewall rules (Phase 3)
```

## Network Security

### Current Segmentation
- **VLAN 1 (Management):** Full access to all infrastructure
- **VLAN 10 (Clients):** Isolated, requires routing for inter-VLAN
- **VLAN 20 (Services):** Isolated, will host services
- **VLAN 30 (DMZ):** Will be heavily firewalled (Phase 3)
- **VLAN 40 (Malware Lab):** Completely isolated (Phase 3)

### Planned Security Rules (Phase 3)
- VLAN 40 (Malware Lab): No internet, no access to other VLANs
- VLAN 30 (DMZ): Restricted outbound, limited inbound
- VLAN 20 (Services): Controlled access from VLAN 1 and 10
- VLAN 10 (Clients): Internet access, limited inter-VLAN access
- VLAN 1 (Management): Full access to all VLANs (admin only)

## Phase 2 Achievements

✅ **Infrastructure:**
- Managed switch configured with 5 VLANs
- Trunk port established between switch and Proxmox
- VLAN-aware bridge configured on Proxmox
- Network connectivity verified

✅ **Learning Outcomes:**
- Understanding of VLAN tagging vs untagged traffic
- Trunk port configuration and PVID concepts
- Switch management and configuration
- Network troubleshooting methodology

✅ **Documentation:**
- Switch configuration backed up
- Network topology documented
- IP address allocation planned
- Security segmentation designed

## Next Phase Preview

**Phase 3: pfSense Integration & Inter-VLAN Routing**
- Configure pfSense with VLAN interfaces
- Set up inter-VLAN routing rules
- Implement firewall policies per VLAN
- Configure DNS and DHCP per VLAN
- Set up VPN for remote access (Tailscale)

---

*Last Updated: Phase 2 Completion - January 2025*
