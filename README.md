# Homelab Infrastructure

A practical Proxmox homelab for learning Linux administration, virtualization, network segmentation, storage, backups, and cybersecurity.

This public portfolio explains the design and work completed without publishing personal user identities or operational secrets. All user labels are placeholders. See [publishing rules](CONTRIBUTING.md).

**Documentation reviewed: October 17, 2026.** This is a record of completed work and the latest reported configuration, not live monitoring. [Current state](docs/current-state.md) distinguishes documented results, pending verification, and future plans.

## Completed Work

- Proxmox host with separate firewall, Linux, desktop-lab, game-server, and NAS VM roles
- OPNsense routing, DHCP, NAT, and segmented security-lab and public-service networks
- Ubuntu/Samba NAS with private shares, shared storage, filesystem permissions, and quotas
- Encrypted remote NAS access through Tailscale, tested from outside the home network
- Private HTTPS password-change portal with current-password verification
- Scheduled rsnapshot backups, a successful file-restore test, and failure-only Discord alerts
- Migration of the NAS disks from an unreliable USB dock to an external SAS shelf
- SMART monitoring and alert testing; current device mappings require rechecking after hardware work
- Paper Minecraft service with whitelist, online authentication, anti-cheat monitoring, and proximity voice chat

## Hardware and Storage

| Component | Latest reported state |
|---|---|
| Hypervisor | Dell Precision 3440 SFF, Intel Core i7-10700 |
| Memory | 32 GB across four 8 GB DIMMs |
| Main SSD | 1 TB for host/VM storage; exact allocation needs live verification |
| Production NAS disks | Two 2 TB enterprise HDDs: primary data and separate backup target |
| External storage | EMC KTN-STL3 shelf connected through an LSI 9200-8e / SAS2008 HBA |
| Expansion | Four purchased 4 TB HDDs; deployment and array design pending |

The documented NAS uses **a primary disk and a backup disk**, not a mirrored RAID pair. The purchased expansion disks do not yet represent usable production capacity.

## Architecture

OPNsense routes the security-lab and public-service DMZ networks. Proxmox hosts the VMs; hosting a VM does not itself enforce network isolation. The NAS has its own Tailscale endpoint for remote SMB access. See the [architecture diagram](docs/architecture.md) and [network policy](docs/networking.md).

## Documentation

| Area | Documentation |
|---|---|
| Status and verification | [Current State](docs/current-state.md) |
| Architecture | [Architecture Overview](docs/architecture.md) |
| Hardware | [Hardware Inventory](docs/hardware.md) |
| Virtualization | [Proxmox](docs/proxmox.md) |
| Networking | [Networking and Segmentation](docs/networking.md) |
| Firewall | [OPNsense](docs/opnsense.md) |
| Storage and backups | [NAS Storage](docs/nas-storage.md) |
| Game service | [Minecraft Server](docs/minecraft.md) |
| Security | [Security Controls](docs/security.md) |
| Troubleshooting | [Troubleshooting Notes](docs/troubleshooting.md) |
| Skills | [Skills Demonstrated](docs/skills-demonstrated.md) |
| Next projects | [Roadmap](docs/roadmap.md) |

## Next Priorities

1. Verify the live NAS configuration, permissions, quotas, backups, and remote-access policy.
2. Provision an additional private storage user and prove access restrictions.
3. Build a separate Docker VM with externally enforced network isolation.
4. Validate expansion disks before choosing a storage layout or migrating data.

Windows Server/Active Directory, SIEM, Ansible, full-VM backups, offsite recovery, and local AI remain future projects unless subsequently documented as completed.
