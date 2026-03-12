# Day 3 — Windows 10 Endpoint VM

**SY0-701 Domain:** 4.1 - Security for Hosts

---

## Objective

Deploy the first endpoint on the segmented network behind pfSense. This Windows 10 VM will be the primary user workstation — the system we'll be defending and attacking throughout the challenge.

## What I Did

- Created Windows 10 Pro VM on the Endpoints VLAN (vmbr3)
- VM specs: 2 CPU cores, 4 GB RAM, 40 GB VirtIO disk
- Loaded VirtIO storage driver during install to detect the virtual disk
- Used local account setup (bypassed Microsoft account requirement)
- Installed VirtIO network driver post-install through Device Manager
- Installed VirtIO balloon driver and guest agent for Proxmox integration
- Added firewall rule in pfSense to allow Endpoints VLAN (OPT2) outbound traffic — without this rule, the VM had no internet since pfSense blocks all traffic on OPT interfaces by default
- Verified network connectivity:

| Test | Result |
|------|--------|
| IP address | 10.0.3.x via DHCP from pfSense |
| Ping gateway (10.0.3.1) | Success |
| Ping internet (8.8.8.8) | Success |
| DNS resolution (google.com) | Success |

- Ran Windows Update to fully patch the system
- Downloaded and installed Sysinternals Suite to C:\Tools
- Took clean baseline snapshot in Proxmox

## Key Takeaways

- Windows doesn't include VirtIO drivers out of the box. You need the VirtIO drivers ISO mounted as a second CD during install, otherwise Windows can't see the virtual disk at all. Network driver also needs manual install after setup.
- pfSense blocks all traffic on OPT interfaces by default — even if DHCP is handing out addresses, the endpoints can't reach anything until you create a pass rule. This is secure by default, but easy to miss if you expect it to work like the LAN interface.
- The bypassnro trick for skipping Microsoft account during setup doesn't work on all Windows 10 builds. Disconnecting the network adapter or switching to a bridge with no connectivity is the simpler workaround.

## Screenshots

- [ipconfig showing 10.0.3.x address]
- [Successful ping tests]
- [Device Manager with all drivers installed]
- [Proxmox snapshot]

## Next

Day 4 — Ubuntu Server VM on the Endpoints VLAN.
