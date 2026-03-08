# LinkedIn Post — Phase 6F VLAN Migration Complete

---

## Post Content

🔧 **Homelab Update: Enterprise VLAN Migration Complete!**

Just wrapped up a 17-hour infrastructure overhaul on my homelab, migrating from a partially-segmented network to a properly architected 5-VLAN topology.

**The Challenge:**
My homelab had grown organically — services scattered across wrong VLANs, a legacy flat network still active, and inconsistent naming conventions. Time for a clean-up!

**What I Did:**
✅ Migrated 9 LXC containers to their correct VLAN
✅ Rebuilt DNS architecture with pfSense forwarding to Pi-hole
✅ Configured proper trunk/access port assignments on managed switch
✅ Set up secure remote access via Tailscale
✅ Fresh Pi-hole installation (learned NetworkManager replaced dhcpcd!)

**Key Technical Learnings:**

1️⃣ **Trunk vs Access Ports Matter**
Proxmox with VLAN-aware bridges sends tagged traffic. Access ports strip tags → connectivity breaks. Took me an hour to figure out port 2 needed trunk mode!

2️⃣ **DNS Architecture Design**
Direct cross-VLAN DNS queries were timing out despite allow-all firewall rules. Solution: pfSense as DNS proxy → Pi-hole → Cloudflare. More reliable and still get ad-blocking everywhere.

3️⃣ **Always Check Your Network Manager**
Modern Raspberry Pi OS uses NetworkManager, not dhcpcd. Spent 30 minutes editing the wrong config file before checking `systemctl status`.

**Current Architecture:**
- VLAN 10: Management (Proxmox, pfSense, NAS)
- VLAN 20: Client devices
- VLAN 30: All services (13 containers)
- VLAN 40: DMZ (future)
- VLAN 50: Isolated malware lab

**Still To Do:**
- Firewall hardening (replace allow-all rules)
- Container autostart configuration
- NAS static IP assignment

Building this homelab has been an incredible learning experience for my transition into cloud engineering. Every "simple" change reveals layers of complexity that you just don't encounter in tutorials!

📂 Full documentation: github.com/muzakkir97

#Homelab #DevOps #Networking #VLAN #pfSense #Proxmox #InfrastructureAsCode #CloudEngineering #CareerTransition #TechLearning

---

## Post Image Suggestions

Consider including:
1. Network topology diagram showing the 5 VLANs
2. Screenshot of Proxmox showing all containers running
3. Screenshot of Uptime Kuma with green status indicators
4. Before/after comparison of the VLAN structure

---

## Hashtag Strategy

**Primary (always include):**
- #Homelab
- #DevOps
- #CloudEngineering

**Secondary (rotate):**
- #Networking
- #VLAN
- #pfSense
- #Proxmox

**Career-focused:**
- #CareerTransition
- #TechLearning
- #InfrastructureAsCode
