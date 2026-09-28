# Minecraft Server

## Overview

A dedicated Ubuntu Server VM hosts a Paper Minecraft server for a small private group. The server is intentionally treated as a public-facing workload and placed in its own DMZ.

## Service Design

- Paper server software
- Java runtime
- systemd service for automatic startup and restart
- Whitelist enabled
- Online authentication enabled
- RCON disabled
- Query disabled
- GrimAC anti-cheat installed
- Dedicated DMZ network

## Performance Tuning

The VM is sized for a small group and tuned for stability rather than maximum view distance.

Representative settings:

```properties
max-players=10
view-distance=8
simulation-distance=6
online-mode=true
white-list=true
enable-rcon=false
enable-query=false
```

## Security Architecture

```text
Internet
   |
Upstream Router
   |
OPNsense
   |
Public-Service DMZ
   |
Minecraft VM
```

The DMZ is blocked from reaching both the management network and the security lab. Only the ports required by the game service are published externally.

## Anti-Cheat

GrimAC is configured in a monitor-first mode so alerts can be reviewed before enabling aggressive automatic punishments. External webhook and proxy-sharing features are disabled.

## Planned Enhancement

Proximity voice chat is planned using a server-side plugin plus the required client mod, with its voice traffic handled separately from the normal Minecraft game connection.
