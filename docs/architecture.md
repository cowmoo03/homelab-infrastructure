# Architecture Overview

This homelab is built around a Proxmox VE host used to practice virtualization, Linux administration, networking, firewalling, and cybersecurity.

## Core Components

- Proxmox VE hypervisor
- OPNsense virtual firewall/router
- Ubuntu Server virtual machines
- Ubuntu Desktop security-lab client
- Dedicated Minecraft server VM
- Managed Ethernet switch
- Multi-port Intel NIC for segmented lab networking

## Logical Layout

```text
Internet
   |
Upstream Router
   |
Proxmox Host
   |
   +-- Management Network
   |
   +-- OPNsense
       |
       +-- Security Lab Network
       |   +-- Ubuntu Desktop Lab VM
       |
       +-- Minecraft DMZ
           +-- Minecraft Server VM
```

The design separates management, security-lab, and public-facing workloads so they do not share the same trust boundary.

## Design Goals

- Practice realistic network segmentation
- Learn firewall policy and NAT
- Isolate public-facing services from management systems
- Build repeatable VM and service deployments
- Document troubleshooting and operational decisions
