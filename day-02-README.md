# Day 2 — pfSense Firewall & Network Segmentation

---

## Objective

Deploy pfSense as the first VM on the network. Establish segmented network architecture that all future systems will rely on.

## What I Did

### pfSense Deployment

Deployed pfSense CE as a virtual machine inside Proxmox:

- CPU: 2 cores
- Memory: 1 GB
- Disk: 8 GB
- Network Interfaces: 5 VirtIO adapters (one per segment)
- Installed via Netgate online installer — the standalone ISO is no longer available from pfSense.org, so the installer fetches packages over the network during setup

### Network Segmentation

The lab network is divided into four internal segments plus WAN. Each interface is mapped to a Proxmox bridge to create isolated zones:

| Network | Subnet | Gateway | Purpose |
|---------|--------|---------|---------|
| WAN | 192.168.1.0/24 | DHCP from router | Internet / home network |
| Management | 10.0.1.0/24 | 10.0.1.1 | Admin tools and SIEM |
| Servers | 10.0.2.0/24 | 10.0.2.1 | Domain Controller and infrastructure |
| Endpoints | 10.0.3.0/24 | 10.0.3.1 | Windows and Linux workstations |
| Attack | 10.0.4.0/24 | 10.0.4.1 | Kali attacker machine |

### DHCP Configuration

Each internal network has its own DHCP range:

| Segment | Range |
|---------|-------|
| Management | 10.0.1.100 – 10.0.1.200 |
| Servers | 10.0.2.100 – 10.0.2.200 |
| Endpoints | 10.0.3.100 – 10.0.3.200 |
| Attack | 10.0.4.100 – 10.0.4.200 |

### Firewall Rules

Firewall rules enforce enterprise-style segmentation. Key rule: the Attack network (10.0.4.0/24) can reach Servers and Endpoints but is blocked from reaching Management (10.0.1.0/24). Attackers should never have direct access to administrative infrastructure.

A WAN rule allows secure HTTPS management of pfSense from the MacBook (port 443).

### DNS

pfSense DNS Resolver enabled across all lab networks for internal DNS resolution.

## Key Takeaways

- The pfSense ISO download process has changed significantly — the standalone ISO from pfsense.org turned out to be a zip archive disguised as an ISO. The Netgate online installer is now the official method.
- Accessing the pfSense web UI required temporarily bridging the LAN interface to vmbr0 (the home network) since internal bridges have no physical NIC. After configuration, the interface was moved back to its proper segment.
- VirtIO network adapters work natively with pfSense — no driver issues unlike Windows VMs.


## Next

Day 3 — Windows 10 Endpoint VM on the segmented network.
