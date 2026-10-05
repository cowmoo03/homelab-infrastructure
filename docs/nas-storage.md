# NAS Storage

## Overview

A dedicated Ubuntu Server VM provides multi-user network storage for the homelab. Two validated 2 TB enterprise HDDs are attached directly to the VM:

- Primary storage mounted at `/srv/storage`
- Backup storage mounted at `/srv/backups`

Both drives use ext4 and mount persistently through `/etc/fstab`.

## Storage Layout

The primary data disk is organized into private user areas and a shared area:

```text
/srv/storage/
├── users/
│   ├── moemin/
│   │   ├── Files/
│   │   ├── Photos/
│   │   └── Videos/
│   └── abdullah/
│       ├── Files/
│       ├── Photos/
│       └── Videos/
└── shared/
    ├── Files/
    ├── Photos/
    └── Videos/
```

Private directories are restricted to their owners. The shared directory uses a dedicated group for authorized users.

## Quotas

Filesystem quotas are enabled on the primary storage volume.

Current allocation:

- Moemin: 550 GiB
- Abdullah: 550 GiB
- Shared pool: 400 GiB
- Remaining capacity is intentionally left unallocated as reserve

The shared pool is owned by a dedicated service account so shared files count against the shared quota instead of either user's personal quota.

## SMB Access

Samba is installed and configured with three authenticated shares:

- `Moemin` — private to the Moemin account
- `Abdullah` — private to the Abdullah account
- `Shared` — available to authorized members of the shared NAS group

Windows SMB access to the Moemin share was tested successfully through File Explorer. Read/write operations were validated by creating, renaming, and deleting a test file.

Cross-user isolation was also tested successfully: an authenticated Moemin session was denied access to the Abdullah private share.

The Shared SMB area was also tested successfully from Windows with authenticated read/write access.

The Windows client now maps the SMB shares as persistent network drives with reconnect-at-sign-in enabled, so the private and shared storage remain available directly under This PC.

The NAS also has a DHCP reservation on the home router so its LAN address remains consistent and mapped SMB paths do not break after lease changes or router reboots.

## Remote Access

Tailscale is installed on the NAS and client devices to provide encrypted remote access without exposing SMB directly to the public internet.

Remote SMB access was tested successfully from a Windows client while connected through a mobile hotspot, confirming that both the private and shared NAS shares are reachable from outside the home network.

Local clients continue to use the LAN address for direct access, while remote clients use the Tailscale path.

## Backups

The secondary 2 TB disk is used as a versioned backup target with rsnapshot.

Current retention policy:

- 7 daily snapshots
- 4 weekly snapshots

A manual daily snapshot was completed successfully and verified to contain the expected storage tree, including the private user directories and shared data.

A full restore test was also completed successfully: a test file was snapshotted, deleted from the live SMB share, restored from `daily.0`, and confirmed back on the primary storage with the correct ownership and permissions.

Backup automation is configured with cron:

- Daily snapshot at 3:00 AM
- Weekly snapshot every Sunday at 4:00 AM

## Password Self-Service

A private web-based Samba password portal is available through Tailscale Serve over HTTPS. It is not exposed to the public internet.

The portal requires the user's current Samba password before allowing a change. Current-password verification is performed against the local Samba service, and only a successful verification permits the restricted password-reset operation.

A full end-to-end test was completed successfully: the password was changed through the portal, Windows SMB sessions were cleared, and the new credential was confirmed to open the user's network share.

If the user forgets the password entirely, the administrator can assign a temporary Samba password. The user then signs in to the portal with that temporary password and replaces it privately with a new one.

The earlier SSH-based password-change workaround was removed after the web portal was validated, leaving the browser-based workflow as the supported self-service method.

## Access-Control Design

- Private user directories are owner-only
- Shared storage is accessible only to members of the shared NAS group
- Administrative access is separate from storage-user access
- Storage users use non-login system accounts
- SMB authentication maps users to their intended private or shared storage
- Shared uploads are forced to a dedicated shared-storage account so they count against the shared quota
- Remote access is provided through an encrypted overlay network rather than public SMB exposure

## SMART Monitoring and Discord Alerts

The Proxmox host now monitors all three attached drives with `smartd`:

- Proxmox system SSD
- Primary NAS 2 TB HDD
- Backup NAS 2 TB HDD

The two NAS HDDs are monitored through their stable `/dev/disk/by-path` device paths using the USB Prolific bridge type, so monitoring does not depend on temporary `/dev/sdX` names.

A root-only Discord webhook is stored locally and used by a custom `smartd-runner` script. SMART warnings are sent to a dedicated Discord NAS alerts channel.

The complete alert chain was tested successfully with a synthetic SMART test event:

`smartd -> smartd-runner -> Discord alert script -> Discord channel`

The test alert identified the Proxmox host, affected device, alert type, and SMART message.

## Reboot Validation

A reboot validation was completed successfully. After restarting the NAS VM:

- Both storage filesystems remounted successfully
- Samba returned to an active state
- Tailscale reconnected automatically
- The password portal service returned to an active state
- Tailscale Serve restored the private HTTPS proxy
- Windows mapped shares opened normally
- The password portal remained reachable

## Status

Core NAS functionality is complete for the current scope. macOS Finder access is intentionally not being tested at this time.
