# OPNsense Firewall

OPNsense is deployed as a Proxmox VM for routing, DHCP, NAT, and policy enforcement between the upstream network, security lab, and public-service DMZ.

## Documented Interfaces and Policy

| Zone | Intended access |
|---|---|
| Upstream/WAN | Upstream connectivity and specifically forwarded services |
| Security lab | Internet access; management access blocked |
| Public-service DMZ | Required internet access; management and security-lab access blocked |

Firewall administration is restricted to authorized management paths. Service networks should reach only the firewall services they require. Current interface assignments and rule enforcement need live verification; no raw rule export or operational address is published here.

## Public Services

The upstream router forwards the published Minecraft services to OPNsense, which translates them to the game VM. Gameplay uses TCP; proximity voice uses its required UDP service. Hypervisor management, firewall management, NAS SMB, and SSH are not intentionally forwarded publicly.

## Validation Lessons

Previous work resolved a missing default route, incorrect DHCP interface selection, and a broad allow rule evaluated before an intended block. The lesson is to verify rule order and actual traffic paths after every network change.

OPNsense only filters traffic that traverses it. Same-bridge traffic and separate overlay-network paths require their own controls. The proposed Docker segment is future work; its policy will need IPv4/IPv6, DNS, updates, egress, and management-access tests before use.
