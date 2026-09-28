# Networking and Segmentation

## Overview

The lab uses a managed switch, a multi-port Intel NIC, Proxmox Linux bridges, and OPNsense to separate workloads into distinct trust zones.

## Logical Networks

| Network | Purpose |
|---|---|
| Management | Hypervisor and infrastructure administration |
| Security Lab | Isolated environment for cybersecurity testing |
| Public-Service DMZ | Hosts internet-facing services such as the Minecraft server |

## Proxmox Bridges

| Bridge | Role |
|---|---|
| `vmbr0` | Management / upstream connectivity |
| `vmbr1` | Security lab |
| `vmbr2` | Public-service DMZ |

## Segmentation Policy

The security lab can reach the internet but is blocked from directly accessing the management network. The public-service DMZ is separately isolated from both the management network and the security lab.

The intent is to limit lateral movement if an experimental or public-facing workload is compromised.

## Managed Switching

A TP-Link Easy Smart managed switch is used to practice VLAN membership, PVID configuration, and access-port behavior. A dedicated Intel multi-port NIC provides additional physical interfaces for lab segmentation.

## Skills Practiced

- VLAN concepts
- Access-port configuration
- Linux bridges
- DHCP
- Routing
- NAT
- Firewall policy
- Network isolation
- Connectivity validation with ping and route inspection
