# LinkedIn Post: Phase 6F Firewall Hardening

---

🔒 **Homelab Update: Network Segmentation Done Right**

Just completed a critical security milestone — implementing proper inter-VLAN firewall rules on my pfSense firewall.

**The Problem:**
When I first set up my 5-VLAN architecture, I used "allow-all" rules to get things working. Classic mistake! VLANs existed at Layer 2, but the firewall wasn't enforcing boundaries at Layer 3. A compromised device on my client network could reach my Proxmox management interface directly.

**The Solution:**
• Created firewall aliases for cleaner rule management (RFC1918, service ports, DNS servers)
• Designed explicit allow rules for each VLAN based on actual traffic requirements
• Added RFC1918 block rules to prevent lateral movement
• Configured Tailscale VPN as the only admin access path to management infrastructure

**Traffic Flow After Hardening:**
```
Client VLAN → Management: ❌ BLOCKED
Client VLAN → Services: ✅ Specific ports only
Client VLAN → Tailscale → Management: ✅ ALLOWED
Malware Lab → Anywhere: ❌ Air-gapped
```

**Key Takeaway:**
Network segmentation without firewall enforcement is just security theater. The real value comes from Layer 3 rules that control what traffic can actually cross VLAN boundaries.

This is part of my ongoing homelab project as I transition from Customer Service Engineering to Cloud/DevOps. Every phase teaches enterprise concepts through hands-on implementation.

📚 Full documentation: https://github.com/muzakkir97

#Homelab #NetworkSecurity #pfSense #Firewall #VLAN #DevOps #CloudEngineering #Tailscale #ZeroTrust #LearningInPublic #CareerTransition

---

**Character Count:** ~1,450 (within LinkedIn's 3,000 limit)
