# LinkedIn Post: Phase 7A Backup Strategy

---

💾 **Homelab Update: Building a Resilient Backup Strategy**

Just completed implementing a comprehensive backup system for my enterprise homelab — and learned some valuable lessons about storage protocols along the way!

**The Challenge:**
With 10 LXC containers running critical services (Nextcloud, monitoring stack, game servers), I needed automated backups that could handle everything from 200MB configs to 17GB data stores.

**The Plot Twist:**
My initial SMB/CIFS approach hit a wall — literally. Large file transfers (>10GB) caused kernel-level crashes in the Linux CIFS driver. The backup would hang for hours, freeze terminals, and eventually corrupt.

**The Solution — Hybrid Backup Strategy:**
```
Small Containers (8) → NFS → NAS @ 02:00
Large Containers (2) → Local HDD @ 02:30
```

This approach plays to each storage type's strengths:
• NFS handles small-to-medium containers reliably
• Local storage provides maximum throughput for large containers (248 MB/s vs 62 MB/s)

**Complete Backup Coverage:**
• ✅ 10 LXC containers — Automated daily with 7/4/2 retention
• ✅ pfSense config — Manual XML + auto config history
• ✅ Pi-hole config — Teleporter export

**Bonus: Enhanced Pi-hole Blocking**
While configuring Pi-hole backups, I added more blocklists:
• 81K → 488K blocked domains (6x increase!)
• Added Easyprivacy, AdguardDNS, and Prigent-Malware lists

**Key Takeaway:**
Don't assume protocols are interchangeable. SMB might be convenient for Windows shares, but NFS is often the better choice for Linux infrastructure backups. When one approach fails, understand *why* before trying alternatives — the root cause informed my entire strategy.

📚 Full documentation: https://github.com/muzakkir97

#Homelab #Backup #Proxmox #NFS #DevOps #CloudEngineering #DisasterRecovery #PiHole #AdBlocking #LearningInPublic #Infrastructure #Linux

---

**Character Count:** ~1,700 (within LinkedIn's 3,000 limit)
