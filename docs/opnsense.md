# OPNsense Firewall

## Role

OPNsense is deployed as a virtual firewall/router inside Proxmox. It provides routing and policy enforcement between the upstream network, the isolated security lab, and the public-service DMZ.

## Interfaces

OPNsense has separate virtual interfaces for:

- Upstream/WAN connectivity
- Security-lab LAN
- Public-service DMZ

## Firewall Design

### Security Lab

- Internet access: allowed
- Management-network access: blocked
- Firewall access: limited to services required for administration and networking

### Public-Service DMZ

- Internet access: allowed as required
- Management-network access: blocked
- Security-lab access: blocked
- Firewall web-management access: blocked
- Inbound access: limited to explicitly published application ports

## NAT

Inbound traffic for the Minecraft service is forwarded through the upstream router to OPNsense, then translated to the isolated game-server VM.

No hypervisor, SSH, or firewall-management interface is intentionally exposed to the public internet.

## Troubleshooting Experience

During the build, I worked through issues involving interface assignment, default routes, DHCP interface selection, firewall rule ordering, and NAT validation.
