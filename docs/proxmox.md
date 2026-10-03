# Proxmox Virtualization

## Overview

Proxmox VE is the central virtualization platform for the homelab. It hosts firewall, Linux server, desktop lab, game-server, and NAS workloads.

## VM Roles

| Workload | Purpose |
|---|---|
| OPNsense | Routing, DHCP, NAT, and firewalling |
| Ubuntu Server | General Linux administration and storage practice |
| Ubuntu Desktop | Client system for testing the isolated security lab |
| Minecraft Server | Public-facing service placed in a dedicated DMZ |
| NAS Server | Multi-user file storage, permissions, quotas, and backups |

## Storage Practice

A secondary virtual disk was attached to an Ubuntu Server VM, partitioned, formatted with ext4, mounted under `/srv/labdata`, and configured for persistence with `/etc/fstab`.

Example organization:

```text
/srv/labdata/
├── backups/
├── evidence/
├── logs/
├── pcaps/
├── scripts/
└── tools/
```

A dedicated NAS VM was also created with a small SSD-backed operating-system disk. Two validated 2 TB enterprise HDDs were attached directly to the VM using stable host device paths so the guest can manage the data and backup disks separately.

Planned storage roles:

- Primary 2 TB HDD: multi-user files, photos, and videos
- Secondary 2 TB HDD: automated backup target

## Operational Practices

- Separate workloads into VMs based on function and trust level
- Allocate CPU and memory according to service needs
- Use persistent Linux mounts for service data
- Keep management interfaces off public-facing networks
- Use systemd for long-running Linux services
- Validate networking and storage after reboot
