Day 2 — pfSense Firewall & Network Segmentation

Day 2 focused on building the core network infrastructure of the homelab using pfSense as the primary firewall and router.

This step establishes the segmented network architecture that all future systems will rely on.

pfSense Deployment

pfSense was deployed as a virtual machine inside Proxmox with the following configuration:

CPU: 2 cores
Memory: 1 GB
Disk: 8 GB
Network Interfaces: 5 VirtIO adapters

Each interface was mapped to a Proxmox bridge to create isolated network segments.

Network Segmentation

The lab network is divided into four internal segments plus WAN:

Network	Subnet	Purpose
WAN	192.168.1.0/24	Internet / home network
Management	10.0.1.0/24	Admin tools and SIEM
Servers	10.0.2.0/24	Domain Controller and infrastructure
Endpoints	10.0.3.0/24	Windows and Linux workstations
Attack	10.0.4.0/24	Kali attacker machine

Each subnet is routed through pfSense and receives IP addresses through DHCP.

DHCP Configuration

Each internal network has its own DHCP range:

Management: 10.0.1.100 – 10.0.1.200
Servers: 10.0.2.100 – 10.0.2.200
Endpoints: 10.0.3.100 – 10.0.3.200
Attack: 10.0.4.100 – 10.0.4.200

Firewall Rules

Firewall rules were created to simulate enterprise-style segmentation.

Key rule implemented:

Attack Network (10.0.4.0/24)
→ Allowed to reach Servers and Endpoints
→ Blocked from reaching Management network (10.0.1.0/24)

This prevents attackers from directly accessing administrative infrastructure.

DNS Configuration

pfSense DNS Resolver was enabled to provide internal DNS resolution across all lab networks.

Management Access

A WAN firewall rule allows secure HTTPS management of pfSense from the host network.

Source: MacBook IP
Destination: This Firewall
Port: 443 (HTTPS)

Outcome

At the end of Day 2 the lab has:

• A functioning firewall/router
• Segmented internal networks
• DHCP services for each segment
• Firewall rules enforcing network boundaries
• Internet connectivity for internal systems

This architecture mirrors real-world enterprise network segmentation used in SOC environments.

Next Steps

Day 3 will introduce the first endpoint systems:

• Windows 10 endpoint VM
• Sysmon logging configuration
• Initial log forwarding to SIEM
