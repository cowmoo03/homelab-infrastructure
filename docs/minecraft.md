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
- Simple Voice Chat proximity voice plugin installed
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

## Proximity Voice Chat

Simple Voice Chat is installed on the Paper server and configured for proximity-based in-game voice communication.

Voice traffic is handled separately from normal Minecraft gameplay traffic:

- Minecraft gameplay uses TCP
- Voice chat uses a dedicated UDP service
- OPNsense performs destination NAT for the voice service
- The upstream router forwards only the required UDP port to OPNsense
- Clients use Fabric with the Simple Voice Chat mod installed

This keeps the voice feature isolated to the Minecraft service without exposing additional management interfaces.
