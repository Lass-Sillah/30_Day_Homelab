# Day 1 — Proxmox Installation & Config

---

## Objective

Install Proxmox VE on bare metal and prepare the hypervisor for the full lab build. Create the network bridge infrastructure for segmentation before any VMs go up.

## What I Did

- Installed Proxmox VE 9.1.1 on the HP EliteDesk 800 G4 Mini (wiped Windows 11 Pro)
- Verified BIOS: VT-x and VT-d enabled
- Set static management IP at 192.168.1.50
- Fixed repository config — changed suite from bookworm to trixie to match Proxmox 9.x/Debian base
- Disabled enterprise repo, enabled no-subscription repo
- Ran full system update — all packages current
- Created Linux bridges for network segmentation:

| Bridge | Segment | Subnet | Purpose |
|--------|---------|--------|---------|
| vmbr0 | WAN | Home network (DHCP) | Internet access, Proxmox management |
| vmbr1 | Management | 10.0.1.0/24 | Wazuh, scanners, admin tools |
| vmbr2 | Servers | 10.0.2.0/24 | Active Directory DC |
| vmbr3 | Endpoints | 10.0.3.0/24 | Windows 10, Ubuntu |
| vmbr4 | Attack | 10.0.4.0/24 | Kali Linux |

- Uploaded all ISOs (pfSense CE, Windows 10, Ubuntu Server, Kali Linux, VirtIO drivers)
- Configured weekly backup schedule (Sundays at 01:00, ZSTD compression, snapshot mode)
- Confirmed headless management from MacBook via web UI (:8006) and SSH

## Key Takeaways

- Proxmox 9.x runs on Debian Trixie, not Bookworm — the default no-subscription repo pointed to the wrong suite and had to be corrected manually. Small fix but would have caused update failures down the line.
- Bridges vmbr1-4 have no IPs and no physical ports assigned. They're internal-only — pfSense will handle all routing between segments starting Day 2.
- Set up backups before building anything. Lesson from my previous lab: protect your work before you start breaking things.

## Next

Day 2 — pfSense Firewall VM. First VM on the network, routing between all five segments.
