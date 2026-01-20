# Phase 3: pfSense Integration & Inter-VLAN Routing

## 📋 Phase Overview

**Phase:** 3 of 5  
**Status:** 🟡 Planning  
**Estimated Duration:** 2-3 sessions  
**Prerequisites:** Phase 2 Complete ✅

### Objectives
- Configure pfSense with VLAN interfaces
- Implement inter-VLAN routing
- Set up firewall rules per VLAN
- Configure DHCP and DNS per VLAN
- Establish network security policies
- Enable secure communication between VLANs

---

## 🎯 Phase 3 Goals

### Primary Goals
1. **Enable Inter-VLAN Routing**
   - Devices on VLAN 10 can reach VLAN 1 (management) with restrictions
   - Services on VLAN 20 can access internet
   - DMZ (VLAN 30) has controlled inbound/outbound access
   - Malware Lab (VLAN 40) is completely isolated

2. **Implement Network Services**
   - DHCP per VLAN (dynamic IP assignment)
   - DNS per VLAN (with different policies)
   - NTP (time synchronization)
   - pfBlockerNG (ad blocking & tracking protection)

3. **Security Policies**
   - Firewall rules per VLAN
   - Default deny with explicit allow rules
   - Logging and monitoring
   - Isolated malware lab (no internet, no inter-VLAN)

---

## 🖥️ Hardware Setup

### Current Physical pfSense Machine

**You mentioned you have a physical pfSense machine!**

**Advantages of Physical pfSense:**
- ✅ True network separation (not dependent on Proxmox)
- ✅ Higher reliability (dedicated hardware)
- ✅ Better performance for routing/firewall
- ✅ Can manage Proxmox network independently
- ✅ Easier to troubleshoot network issues

**Required Information (Please Provide):**

1. **What model/specs is your pfSense machine?**
   - CPU:
   - RAM:
   - Network interfaces: How many? (Need at least 2)
   - Current role: Is it already your main router?

2. **Current Network Role:**
   - [ ] Already acting as main router (between ISP and LAN)?
   - [ ] Standalone/not configured yet?
   - [ ] Other role?

3. **Network Interface Configuration:**
   - WAN interface: Connected to ISP/modem?
   - LAN interface: Connected to switch?
   - Additional interfaces: Available for VLANs?

---

## 🔌 Network Architecture Options

### Option A: pfSense as Main Router (Router-on-a-Stick)

**Best if:** Your pfSense is already acting as your main router

```
Internet
   ↓
ISP Modem
   ↓
┌─────────────────────────────────┐
│      pfSense (Physical)          │
│                                  │
│  WAN: ISP connection            │
│  LAN: 192.168.100.1 (VLAN 1)    │
│                                  │
│  VLAN Interfaces:               │
│  ├─ VLAN 1:  192.168.100.1/24   │
│  ├─ VLAN 10: 192.168.10.1/24    │
│  ├─ VLAN 20: 192.168.20.1/24    │
│  ├─ VLAN 30: 192.168.30.1/24    │
│  └─ VLAN 40: 192.168.40.1/24    │
└─────────────┬───────────────────┘
              │ LAN Interface (trunk with VLAN tags)
              ↓
┌─────────────────────────────────┐
│   TP-Link Switch (Port 1)       │
│   Trunk port to pfSense         │
│   Distributes VLANs to devices  │
└─────────────────────────────────┘
```

**How it works:**
- Single physical LAN connection carries all VLANs (802.1Q tagging)
- pfSense LAN interface configured as trunk
- Switch Port 1 configured as trunk (all VLANs tagged)
- pfSense routes between VLANs
- Firewall rules control inter-VLAN traffic

---

### Option B: pfSense with Separate Physical Interfaces per VLAN

**Best if:** Your pfSense has multiple network interfaces (3+)

```
Internet
   ↓
ISP Modem
   ↓
┌─────────────────────────────────┐
│      pfSense (Physical)          │
│                                  │
│  eth0 (WAN): ISP connection     │
│  eth1: VLAN 1  (192.168.100.1)  │
│  eth2: VLAN 10 (192.168.10.1)   │
│  eth3: VLAN 20 (192.168.20.1)   │
│  (Optional: eth4/5 for VLAN 30/40) │
└───┬───┬───┬───┬────────────────┘
    │   │   │   │
    ↓   ↓   ↓   ↓
┌─────────────────────────────────┐
│   TP-Link Switch                │
│   Separate ports per VLAN       │
└─────────────────────────────────┘
```

**How it works:**
- Each VLAN has dedicated physical interface
- No VLAN tagging required (simpler)
- Switch ports configured as access ports (untagged)
- More interfaces = better performance
- Easier to troubleshoot (clear separation)

---

## 🛠️ Configuration Steps (Option A - Router-on-a-Stick)

### Step 1: Current Network Reconfiguration

**Before pfSense VLAN setup:**

1. **Verify Current Setup:**
   - Is pfSense currently acting as 192.168.100.1?
   - Is current router still in use?
   - If yes, we need to migrate to pfSense

2. **Network Migration Plan:**
   ```
   Current:
   ISP → Router (192.168.100.1) → Switch → Devices
   
   Target:
   ISP → pfSense (192.168.100.1) → Switch → Devices
   ```

---

### Step 2: Switch Port 1 Reconfiguration

**Current Port 1 Setup:**
```
Port 1: Router connection
VLAN 1: Untagged
PVID: 1
```

**New Port 1 Setup (Trunk to pfSense):**
```
Port 1: pfSense LAN connection
VLAN 1: Tagged (or Untagged if you prefer native VLAN 1)
VLAN 10: Tagged
VLAN 20: Tagged
VLAN 30: Tagged
VLAN 40: Tagged
PVID: 1
Link Type: Trunk
```

**Configuration Steps:**
1. Login to switch: http://192.168.100.2
2. Navigate to: VLAN → 802.1Q VLAN → Port Config
3. For Port 1:
   - VLAN 1: Tagged (or Untagged as native VLAN)
   - VLAN 10: Tagged
   - VLAN 20: Tagged
   - VLAN 30: Tagged
   - VLAN 40: Tagged
4. Navigate to: VLAN → 802.1Q VLAN → Port PVID
5. Set Port 1 PVID = 1
6. Apply changes

---

### Step 3: pfSense VLAN Interface Configuration

**Access pfSense:**
- Web interface: http://192.168.100.1 (or current IP)
- Default: username `admin`, password `pfsense` (if fresh install)

**Create VLAN Interfaces:**

1. **Navigate to:** Interfaces → Assignments → VLANs

2. **Add VLAN 10 (Clients):**
   ```
   Parent Interface: LAN (or your LAN interface name)
   VLAN Tag: 10
   Description: VLAN10_Clients
   ```
   Click "Save"

3. **Repeat for VLANs 20, 30, 40:**
   ```
   VLAN 20: VLAN20_Services
   VLAN 30: VLAN30_DMZ
   VLAN 40: VLAN40_MalwareLab
   ```

4. **Assign VLAN Interfaces:**
   - Navigate to: Interfaces → Assignments
   - Click "Add" for each VLAN interface
   - Should see: VLAN10_Clients, VLAN20_Services, etc.

5. **Configure Each VLAN Interface:**

   **VLAN 10 (Clients):**
   ```
   Enable: ✓
   Description: VLAN10_Clients
   IPv4 Configuration Type: Static IPv4
   IPv4 Address: 192.168.10.1 / 24
   ```
   
   **VLAN 20 (Services):**
   ```
   Enable: ✓
   Description: VLAN20_Services
   IPv4 Configuration Type: Static IPv4
   IPv4 Address: 192.168.20.1 / 24
   ```
   
   **VLAN 30 (DMZ):**
   ```
   Enable: ✓
   Description: VLAN30_DMZ
   IPv4 Configuration Type: Static IPv4
   IPv4 Address: 192.168.30.1 / 24
   ```
   
   **VLAN 40 (Malware Lab):**
   ```
   Enable: ✓
   Description: VLAN40_MalwareLab
   IPv4 Configuration Type: Static IPv4
   IPv4 Address: 192.168.40.1 / 24
   ```

---

### Step 4: DHCP Server Configuration

**Configure DHCP per VLAN:**

1. **Navigate to:** Services → DHCP Server

2. **For each VLAN, configure:**

   **VLAN 10 (Clients):**
   ```
   Enable: ✓
   Range: 192.168.10.100 to 192.168.10.200
   DNS Servers: 192.168.10.1 (pfSense)
   Gateway: 192.168.10.1
   Domain Name: clients.homelab.local
   ```

   **VLAN 20 (Services):**
   ```
   Enable: ✓
   Range: 192.168.20.100 to 192.168.20.200
   DNS Servers: 192.168.20.1
   Gateway: 192.168.20.1
   Domain Name: services.homelab.local
   ```

   **VLAN 30 (DMZ):**
   ```
   Enable: ✓
   Range: 192.168.30.100 to 192.168.30.150
   DNS Servers: 192.168.30.1
   Gateway: 192.168.30.1
   Domain Name: dmz.homelab.local
   ```

   **VLAN 40 (Malware Lab):**
   ```
   Enable: ✓ (or disabled if you want manual IPs only)
   Range: 192.168.40.100 to 192.168.40.110
   DNS Servers: 192.168.40.1
   Gateway: 192.168.40.1 (or leave blank - no gateway)
   Domain Name: malware.homelab.local
   ```

---

### Step 5: Firewall Rules Configuration

**Security Philosophy:**
- Default DENY all traffic
- Explicitly ALLOW required traffic
- Log all denied attempts for monitoring

**VLAN 1 (Management) Rules:**
```
1. Allow Management → Any (full access for admins)
2. Log all traffic for monitoring
```

**VLAN 10 (Clients) Rules:**
```
1. Allow VLAN10 → Internet (HTTP/HTTPS)
2. Allow VLAN10 → VLAN20 ports 80,443 (access to self-hosted services)
3. Allow VLAN10 → pfSense DNS/DHCP
4. Block VLAN10 → VLAN1 (no management access)
5. Block VLAN10 → VLAN30 (no DMZ access)
6. Block VLAN10 → VLAN40 (no malware lab access)
7. Deny all other traffic (implicit)
```

**VLAN 20 (Services) Rules:**
```
1. Allow VLAN20 → Internet (for updates, external APIs)
2. Allow VLAN20 → pfSense DNS
3. Block VLAN20 → VLAN1 (no management access)
4. Block VLAN20 → other VLANs (services are isolated)
5. Allow inbound from VLAN10 on ports 80,443,8080 (clients accessing services)
6. Deny all other traffic
```

**VLAN 30 (DMZ) Rules:**
```
1. Allow DMZ → Internet (limited ports: 80, 443)
2. Allow inbound from Internet on specific ports (SSH, HTTP, HTTPS) - NAT rules
3. Block DMZ → VLAN1 (no management access)
4. Block DMZ → VLAN10 (no client access)
5. Block DMZ → VLAN20 (no services access)
6. Block DMZ → VLAN40 (no malware lab access)
7. Deny all other traffic
```

**VLAN 40 (Malware Lab) Rules:**
```
1. BLOCK ALL outbound traffic (no internet!)
2. BLOCK ALL inter-VLAN traffic
3. Allow VLAN1 → VLAN40 (management can access lab)
4. Log all connection attempts
```

**Configuration Steps:**

1. **Navigate to:** Firewall → Rules

2. **For each VLAN tab:**
   - Click "Add" (arrow up icon for top of list)
   - Configure rule:
     ```
     Action: Pass/Block
     Interface: [VLAN Interface]
     Protocol: TCP/UDP/Any
     Source: [VLAN network]
     Destination: [Target network/port]
     Description: [Clear description]
     Log: ✓ (for blocked traffic)
     ```
   - Click "Save"

3. **Apply Changes** after all rules configured

---

### Step 6: DNS Configuration

**Navigate to:** Services → DNS Resolver (Unbound)

**Configuration:**
```
Enable DNS Resolver: ✓
Network Interfaces: Select all VLAN interfaces
DHCP Registration: ✓ (allows devices to resolve by hostname)
DNSSEC: ✓ (security)
```

**Advanced Options:**
```
Custom Options:
server:
    private-domain: "homelab.local"
    local-zone: "homelab.local." transparent
```

---

### Step 7: pfBlockerNG (Ad Blocking)

**Install pfBlockerNG:**

1. Navigate to: System → Package Manager → Available Packages
2. Find "pfBlockerNG-devel"
3. Click "Install"
4. Wait for installation

**Configure pfBlockerNG:**

1. Navigate to: Firewall → pfBlockerNG → DNSBL
2. Enable DNSBL
3. Add feed lists:
   - Steven Black's hosts file
   - EasyList
   - AdGuard DNS filter
4. Configure per-VLAN policies:
   - VLAN 10 (Clients): Full ad blocking
   - VLAN 20 (Services): Minimal blocking (might break some services)
   - VLAN 30 (DMZ): No blocking
   - VLAN 40 (Malware Lab): Blocked anyway (no internet)

---

## 🧪 Testing Plan

### Test 1: Basic Connectivity

**From Gaming PC (move to VLAN 10):**

1. Change Switch Port 4:
   - PVID: 10
   - VLAN 10: Untagged
   - Remove from VLAN 1

2. Set Gaming PC to DHCP or static IP:
   ```
   IP: 192.168.10.49
   Subnet: 255.255.255.0
   Gateway: 192.168.10.1
   DNS: 192.168.10.1
   ```

3. Test:
   ```powershell
   # Test pfSense gateway
   ping 192.168.10.1
   
   # Test internet
   ping 8.8.8.8
   ping google.com
   
   # Test VLAN 1 (should fail if firewall rules correct)
   ping 192.168.100.25  # Proxmox - should be blocked
   
   # Test VLAN 20 (will test later with services)
   ```

---

### Test 2: Inter-VLAN Routing

**Create Test VM on Proxmox (VLAN 20):**

1. Create Ubuntu/Debian VM on Proxmox
2. Configure VM network:
   ```
   Bridge: vmbr0
   VLAN Tag: 20
   ```
3. Boot VM and set network:
   ```bash
   # Set static IP or use DHCP
   sudo dhclient  # DHCP
   
   # Or static:
   sudo ip addr add 192.168.20.10/24 dev eth0
   sudo ip route add default via 192.168.20.1
   ```

4. Test from VM:
   ```bash
   # Test pfSense gateway
   ping 192.168.20.1
   
   # Test internet
   ping 8.8.8.8
   
   # Test VLAN 10 (should fail - blocked by firewall)
   ping 192.168.10.49
   ```

5. Test from Gaming PC (VLAN 10):
   ```powershell
   # Test VM on VLAN 20 (should work - firewall allows 10 → 20)
   ping 192.168.20.10
   ```

---

### Test 3: Malware Lab Isolation

**Create VM on VLAN 40:**

1. Create VM with VLAN tag 40
2. Set static IP: 192.168.40.10/24
3. Test complete isolation:
   ```bash
   # Test gateway (should work)
   ping 192.168.40.1
   
   # Test internet (should FAIL - blocked)
   ping 8.8.8.8
   
   # Test other VLANs (should FAIL - blocked)
   ping 192.168.10.1
   ping 192.168.20.1
   ping 192.168.100.25
   ```

4. Test from Management VLAN (should be allowed):
   ```bash
   # From Proxmox or management device
   ping 192.168.40.10  # Should work
   ssh user@192.168.40.10  # Should work
   ```

---

## 📊 IP Address Allocation Plan

### VLAN 1 (Management) - 192.168.100.0/24
```
.1      - pfSense (Gateway)
.2      - Switch Management
.25     - Proxmox Host
.50-.99 - Static assignments (infrastructure)
.100-.254 - DHCP pool
```

### VLAN 10 (Clients) - 192.168.10.0/24
```
.1        - pfSense Gateway
.10-.99   - Static assignments (PCs, laptops)
.100-.200 - DHCP pool
```

### VLAN 20 (Services) - 192.168.20.0/24
```
.1        - pfSense Gateway
.10       - Nextcloud VM
.11       - Immich VM
.12       - Minecraft Server VM
.13       - Project Zomboid Server VM
.14       - Terraria Server VM
.15-20    - Reserved for future game servers
.21       - Pi-hole (if moved from Raspberry Pi)
.100-.200 - DHCP pool (for dynamic service containers)
```

### VLAN 30 (DMZ) - 192.168.30.0/24
```
.1        - pfSense Gateway
.10       - Public web server
.11       - VPN endpoint
.100-.150 - DHCP pool
```

### VLAN 40 (Malware Lab) - 192.168.40.0/24
```
.1        - pfSense Gateway (no internet routing)
.10       - Analysis VM 1
.11       - Analysis VM 2
.12       - Sandbox VM
.100-.110 - DHCP pool (limited)
```

---

## 🔒 Security Considerations

### Network Segmentation Benefits

1. **Containment:**
   - Breach in VLAN 20 (services) cannot access VLAN 1 (management)
   - Malware lab completely isolated

2. **Access Control:**
   - Clients can access services but not infrastructure
   - DMZ isolated from internal network

3. **Monitoring:**
   - Log inter-VLAN traffic attempts
   - Identify suspicious patterns
   - pfBlockerNG provides DNS query logs

4. **Performance:**
   - Broadcast traffic isolated per VLAN
   - Reduced network congestion

---

## ⚠️ Common Issues & Solutions

### Issue 1: pfSense Cannot Tag VLANs on LAN Interface

**Symptoms:**
- VLAN interfaces created but don't work
- No connectivity on VLAN devices

**Root Cause:**
- Some NICs don't support VLAN tagging
- Driver issues

**Solution:**
```
1. Check NIC compatibility:
   System → Advanced → Networking
   Enable: "VLAN Hardware Filtering"
   
2. If doesn't work, use different NIC
3. Or use separate physical interfaces (Option B)
```

---

### Issue 2: VLAN Devices Get IP from Wrong DHCP Server

**Symptoms:**
- Device on VLAN 10 gets 192.168.100.x IP (VLAN 1)

**Root Cause:**
- Old router/DHCP server still running
- DHCP broadcasts crossing VLANs

**Solution:**
```
1. Disable DHCP on old router
2. Verify switch doesn't bridge VLANs
3. Check pfSense DHCP is enabled per VLAN
4. Release/renew IP on client:
   - Windows: ipconfig /release && ipconfig /renew
   - Linux: sudo dhclient -r && sudo dhclient
```

---

### Issue 3: Cannot Access Internet from VLAN 10

**Symptoms:**
- Can ping pfSense gateway (192.168.10.1)
- Cannot ping internet (8.8.8.8)

**Root Cause:**
- Missing NAT outbound rule
- WAN firewall rule blocking

**Solution:**
```
1. Check NAT:
   Firewall → NAT → Outbound
   Mode should be: "Automatic outbound NAT"
   
2. Or manually add:
   Interface: WAN
   Source: 192.168.10.0/24
   Translation: Interface Address
   
3. Check WAN rules (shouldn't normally block outbound)
```

---

## 📋 Phase 3 Checklist

### Pre-Phase Requirements
- [ ] Phase 2 complete (VLANs configured on switch)
- [ ] Proxmox VLAN-aware bridge working
- [ ] pfSense machine accessible and functional
- [ ] Backup of current network configuration

### Configuration Tasks
- [ ] Reconfigure Switch Port 1 as trunk to pfSense
- [ ] Create VLAN interfaces on pfSense (10, 20, 30, 40)
- [ ] Assign IP addresses per VLAN interface
- [ ] Configure DHCP servers per VLAN
- [ ] Set up firewall rules (management, clients, services, DMZ, malware lab)
- [ ] Configure DNS resolver (Unbound)
- [ ] Install and configure pfBlockerNG
- [ ] Set up logging and monitoring

### Testing Tasks
- [ ] Test basic connectivity on each VLAN
- [ ] Test inter-VLAN routing (allowed paths)
- [ ] Test VLAN isolation (blocked paths)
- [ ] Test malware lab complete isolation
- [ ] Test internet access from all VLANs (except VLAN 40)
- [ ] Test DHCP on each VLAN
- [ ] Test DNS resolution on each VLAN
- [ ] Test pfBlockerNG ad blocking

### Documentation Tasks
- [ ] Document pfSense configuration
- [ ] Update network topology diagram
- [ ] Screenshot firewall rules
- [ ] Document IP address allocations
- [ ] Create Phase 3 completion summary
- [ ] Write LinkedIn post

---

## 💰 Cost Savings Enablement

**Phase 3 enables deployment of:**

1. **Nextcloud (VLAN 20):** Replaces Google Drive
   - Saves ~RM 250/year
   
2. **Immich (VLAN 20):** Replaces iCloud Photos
   - Saves ~RM 130/year
   
3. **Game Servers (VLAN 20):** Self-hosted
   - Minecraft, Project Zomboid, Terraria
   - Saves ~RM 400/year in hosting fees

**Total Enabled Savings: ~RM 780/year**

---

## 🚀 Next Steps After Phase 3

**Phase 4: Service Deployment**
- Deploy Nextcloud on VLAN 20
- Deploy Immich on VLAN 20
- Set up game servers on VLAN 20
- Configure Nginx Proxy Manager for reverse proxy
- Set up SSL certificates (Let's Encrypt)
- Implement Tailscale VPN for remote access

**Phase 5: Monitoring & Automation**
- Prometheus + Grafana monitoring
- Uptime Kuma for service health checks
- Automated backups (Proxmox Backup Server)
- Configuration management (Ansible)
- Documentation website (MkDocs)

---

## ❓ Questions to Answer Before Starting Phase 3

**Please provide the following information:**

1. **pfSense Machine Details:**
   - Model/Hardware specs?
   - How many network interfaces?
   - Currently acting as main router? (Y/N)

2. **Current Network:**
   - What's currently acting as 192.168.100.1?
   - ISP connection type (fiber, DSL, cable)?
   - Any specific requirements (port forwarding, etc.)?

3. **Preferred Configuration:**
   - Option A (router-on-a-stick with VLAN tagging)?
   - Option B (separate physical interfaces per VLAN)?
   - Timeline for Phase 3 start?

---

**Once you provide these details, I can create a customized Phase 3 implementation guide specific to your hardware setup!**

---

*Status: Ready for Phase 3 - Pending pfSense Details*
