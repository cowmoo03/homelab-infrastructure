# Security Controls

## Security Model

The homelab uses network segmentation and least privilege rather than placing every workload on one flat network.

## Trust Zones

### Management

Reserved for infrastructure administration such as the hypervisor and network-management interfaces.

### Security Lab

Used for experimentation and cybersecurity practice. It can reach the internet but is isolated from management systems.

### Public-Service DMZ

Used for internet-facing services. It is isolated from both management and the security lab.

## Service Hardening

Current controls include:

- Default-deny thinking between trust zones
- Explicit firewall rules for required traffic
- No direct public exposure of Proxmox management
- No direct public exposure of OPNsense management
- No public SSH forwarding to the Minecraft VM
- Minecraft whitelist enabled
- Minecraft online authentication enabled
- RCON and query disabled
- Anti-cheat monitoring enabled

## Portfolio Sanitization

This public repository intentionally excludes operational secrets and personally identifying infrastructure details, including:

- Public IP addresses
- MAC addresses
- Passwords
- API keys and tokens
- SSH private keys
- Exact physical location
- Raw configuration exports that may contain secrets
- Personal account identifiers not required for the project

## Principle

The core design principle is to reduce unnecessary trust between systems. A compromise of a public-facing or experimental workload should not automatically provide access to infrastructure-management systems.
