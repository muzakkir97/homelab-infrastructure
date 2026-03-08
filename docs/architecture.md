# Target Architecture

> **Last Updated:** March 8, 2026

---

## Design Principles

1. **Defense in Depth** — Multiple security layers
2. **Segmentation** — Isolate different traffic types
3. **One Service Per Container** — Clearer boundaries, independent management
4. **Documentation First** — Everything documented before and after
5. **Learn by Doing** — Prefer complex approaches for learning value

---

## VLAN Design

| VLAN | Purpose | Security Level |
|------|---------|----------------|
| 10 - Management | Infrastructure devices | Highest — admin only |
| 20 - Main Network | Daily-use clients | Medium — trusted users |
| 30 - Services | All hosted services | Medium — controlled access |
| 40 - DMZ | Public-facing services | Low trust — internet exposed |
| 50 - Malware Lab | Security research | Air-gapped — no connectivity |

## Traffic Flow Rules (Target)

| From → To | Allowed | Method |
|-----------|---------|--------|
| VLAN 20 → VLAN 30 | Yes | Specific service ports only |
| VLAN 30 → Internet | Yes | For updates, external APIs |
| VLAN 10 → All | Yes | Management access |
| VLAN 50 → Any | No | Completely isolated |
| Internet → VLAN 30 | Limited | Via NPM reverse proxy or Cloudflare Tunnel |

---

## Container Architecture

### Container Inventory (10 Total)

| CTID | Name | IP | Boot Order | Purpose |
|------|------|----|------------|---------|
| 201 | nginx-proxy-manager | 192.168.30.201 | 1 | Reverse proxy, SSL termination |
| 207 | network-ddns | 192.168.30.207 | 2 | Cloudflare DDNS updates |
| 202 | monitoring-prometheus | 192.168.30.202 | 3 | Metrics collection |
| 204 | monitoring-loki | 192.168.30.204 | 4 | Log aggregation |
| 205 | monitoring-alertmanager | 192.168.30.205 | 5 | Alert routing |
| 203 | monitoring-grafana | 192.168.30.203 | 6 | Dashboards |
| 206 | monitoring-uptime | 192.168.30.206 | 7 | Availability monitoring |
| 220 | nextcloud | 192.168.30.220 | 8 | Cloud storage |
| 300 | gaming-panel | 192.168.30.210 | 9 | Pterodactyl Panel |
| 302 | gaming-wings-1 | 192.168.30.212 | 10 | Pterodactyl Wings |

### Resource Allocation

| Container Type | CPU | RAM | Disk |
|----------------|-----|-----|------|
| Monitoring (Prometheus, Loki) | 1-2 cores | 1-2 GB | 10-20 GB |
| Web Services (NPM, Grafana) | 1 core | 512 MB - 1 GB | 8-10 GB |
| Nextcloud | 2 cores | 4 GB | 20 GB |
| Gaming (Wings) | 2 cores | 10 GB | 50 GB |

---

## External Access Architecture

### Access Methods

| Method | Services | Authentication |
|--------|----------|----------------|
| Cloudflare Access | Grafana | Email OTP (Zero Trust) |
| Cloudflare Tunnel | Nextcloud | Nextcloud native login |
| Port Forward + NPM | Pterodactyl Panel | Panel login |
| Port Forward (Direct) | Game servers (Terraria, MC) | None (game auth) |
| Tailscale VPN | Admin interfaces (Proxmox, pfSense, Pi-hole) | Tailscale auth |

### External Access Flow

```
Internet
    │
    ├── Cloudflare (Proxied) ──► NPM ──► Grafana
    │                                    (+ CF Access OTP)
    │
    ├── Cloudflare Tunnel ──► Nextcloud (CT 220)
    │
    ├── DNS Only + Port Forward ──► NPM ──► Pterodactyl Panel
    │
    └── DNS Only + Port Forward ──► Game Servers (7777, 25570)

Tailscale VPN
    │
    └── pfSense Subnet Router ──► All internal services
```

---

## Architecture Decision Records

### ADR-001: LXC over VMs

**Decision:** Use LXC containers for most services instead of full VMs.

**Rationale:** 32GB RAM constraint. LXC uses ~256-512MB vs VM 2-4GB per instance. Enables 10+ isolated services on single host.

**Trade-off:** Shared kernel reduces isolation. Use VMs for Malware Lab (VLAN 50).

### ADR-002: One Container Per Service

**Decision:** Deploy each service in its own LXC container.

**Rationale:** Mirrors microservices architecture. Independent backup, update, and resource management. Cleaner firewall rules.

### ADR-003: Router-on-a-Stick

**Decision:** Single-NIC pfSense with 802.1Q VLAN subinterfaces.

**Rationale:** Cost-effective for homelab scale. Single managed switch handles Layer 2 segmentation.

**Trade-off:** All inter-VLAN traffic on single link. Acceptable at current scale.

### ADR-004: Gaming on VLAN 30

**Decision:** Place gaming services on Services VLAN instead of dedicated Gaming VLAN.

**Rationale:** Reduces complexity. Same access patterns as other services. Original VLAN 50 repurposed for Malware Lab.

### ADR-005: Cloudflare for External Access

**Decision:** Use Cloudflare for DNS, CDN, and Zero Trust access.

**Rationale:** Free tier provides DDoS protection, SSL termination, and Zero Trust authentication. No additional cost.

### ADR-006: Tailscale for Admin Access

**Decision:** Use Tailscale VPN for all administrative access.

**Rationale:** No exposed SSH or admin ports. WireGuard-based mesh VPN works through NAT. Zero configuration on clients.

### ADR-007: Cloudflare Tunnel for Nextcloud

**Decision:** Use Cloudflare Tunnel instead of port forwarding for Nextcloud.

**Rationale:** 
- No ISP router configuration needed
- No pfSense NAT rules required
- Works with mobile apps (unlike Cloudflare Access)
- Automatic DDoS protection

**Trade-off:** Dependency on Cloudflare infrastructure.

---

## Network Topology

```
                         ┌─────────────┐
                         │  INTERNET   │
                         └──────┬──────┘
                                │
         ┌──────────────────────┼──────────────────────┐
         │                      │                      │
    Cloudflare             Cloudflare              ISP Router
    (Proxied)              Tunnel                  (Port Fwd)
         │                      │                      │
         └──────────────────────┼──────────────────────┘
                                │
                         ┌──────┴──────┐
                         │  pfSense    │
                         │  AC8F N100  │
                         │  (igc2/igc3)│
                         └──────┬──────┘
                                │ 802.1Q Trunk
                         ┌──────┴──────┐
                         │  TP-Link    │
                         │  TL-SG108E  │
                         └──────┬──────┘
              ┌─────────┬───────┼───────┬─────────┐
              │         │       │       │         │
           VLAN10   VLAN20   VLAN30  VLAN40   VLAN50
            Mgmt     Main   Services  DMZ    Malware
              │         │       │       │         │
           Proxmox   Clients  10 CTs  Future  Air-gap
           pfSense   Gaming   Pi-hole
           NAS       Work
           Switch
```

---

## Monitoring Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Prometheus (CT 202)                   │
│                    Scrapes all targets                   │
└─────────────────────────┬───────────────────────────────┘
                          │
        ┌─────────────────┼─────────────────┐
        │                 │                 │
        ▼                 ▼                 ▼
   Node Exporters    Container       Service Endpoints
   (Pi-hole, CTs)    Metrics         (Grafana, NPM, etc.)
                          │
                          ▼
              ┌───────────────────────┐
              │  Alertmanager (CT 205) │
              └───────────┬───────────┘
                          │
              ┌───────────┴───────────┐
              │                       │
              ▼                       ▼
         Telegram                  Discord
         (Critical)               (Warnings)
```

---

## Storage Architecture

| Storage | Type | Location | Purpose |
|---------|------|----------|---------|
| local | Directory | /var/lib/vz | ISOs, templates |
| local-lvm | LVM-Thin | pve/data | VM/CT disks |
| kinmoon-smb | SMB/CIFS | 192.168.10.15 | Container backups |

### Backup Strategy (Planned - Phase 7A)

- Scheduled LXC backups to NAS
- Configuration snapshots
- Backup rotation policy
- Recovery testing

---

*Last updated: March 8, 2026*
