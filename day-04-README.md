# Day 4 — Ubuntu Server VM

## Objective

Deploy a Linux endpoint on the segmented network. Harden the basics and establish SSH access.

## What I Did

- Created Ubuntu Server VM on the Endpoints VLAN (vmbr3)
- VM specs: 2 CPU cores, 2 GB RAM, 20 GB VirtIO disk
- Installed Ubuntu Server with OpenSSH enabled
- No driver issues — Linux handles VirtIO natively
- Verified network connectivity on 10.0.3.x subnet through pfSense
- Applied baseline hardening:
  - Disabled root SSH login (PermitRootLogin no)
  - Installed and enabled fail2ban for brute-force protection
  - Configured UFW firewall with SSH allowed
- Installed QEMU Guest Agent for Proxmox integration
- Fully updated all packages
- Took clean baseline snapshot

## Key Takeaways

- Night and day difference from Windows. No driver issues, no Microsoft account workarounds, no manual network adapter setup. Linux on VirtIO just works.
- Basic hardening takes five minutes but makes a real difference — disabling root SSH, enabling fail2ban, and turning on a host firewall are the minimum for any Linux server.
- Both Windows and Ubuntu now sit on the same Endpoints VLAN. Multiple machines can share a subnet — the segment defines the security zone, not the individual host.

## Next

Day 5 — Kali Linux Attack VM on the Attack VLAN.
