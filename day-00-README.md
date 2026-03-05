# Day 0 — Planning & Architecture

---

## Objective

Plan the full homelab architecture before touching any hardware. Document everything and set up the repo.

## What I Did

- Documented hardware specs (HP EliteDesk 800 G4 Mini — i5-8500T, 32 GB RAM, 512 GB NVMe)
- Designed the network architecture with five segmented zones behind a virtual firewall
- Defined the IP addressing scheme
- Set up this GitHub repo with folder structure
- Published Day 0 LinkedIn post with architecture diagram

## Architecture

The lab runs on a single Proxmox hypervisor (EliteDesk). All VMs sit behind a pfSense firewall that routes between five network segments:

| Segment | Bridge | Subnet | Purpose |
|---------|--------|--------|---------|
| Management | vmbr1 | 10.0.1.0/24 | Wazuh SIEM, vulnerability scanner, DNS logging |
| Servers | vmbr2 | 10.0.2.0/24 | Active Directory Domain Controller |
| Endpoints | vmbr3 | 10.0.3.0/24 | Windows 10, Ubuntu Server |
| Attack | vmbr4 | 10.0.4.0/24 | Kali Linux (controlled access) |
| DMZ | vmbr5 | 10.0.5.0/24 | Honeypot |

Full architecture diagram: [homelab-architecture.svg](../../diagrams/homelab-architecture.svg)

## Key Decisions

- **pfSense first**: The firewall goes up before any endpoints so every VM is segmented from day one.
- **Headless Proxmox**: EliteDesk runs headless. All management via the MacBook over Proxmox Web UI (:8006) and SSH.
- **Thin provisioning**: With 512 GB storage, VMs only consume actual disk used. Keeps things manageable.
- **Core stack vs. on-demand**: Always-on VMs (pfSense, Wazuh, Windows 10, Ubuntu, AD) stay within ~15-19 GB RAM. Specialized VMs (Kali, OpenVAS, FlareVM) rotate in as needed.

## ISOs Ready

- Proxmox VE
- pfSense CE
- Windows 10
- Ubuntu Server 22.04/24.04
- Kali Linux
- VirtIO drivers

## Next

Day 1 — Proxmox Installation & Config. Wiping the EliteDesk and installing Proxmox VE bare metal.
