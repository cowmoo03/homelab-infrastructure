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

## Backups

The secondary 2 TB disk is used as a versioned backup target with rsnapshot.

Current retention policy:

- 7 daily snapshots
- 4 weekly snapshots

A manual daily snapshot was completed successfully and verified to contain the expected storage tree, including the private user directories and shared data.

Backup automation is configured with cron:

- Daily snapshot at 3:00 AM
- Weekly snapshot every Sunday at 4:00 AM

## Access-Control Design

- Private user directories are owner-only
- Shared storage is accessible only to members of the shared NAS group
- Administrative access is separate from storage-user access
- Storage users use non-login system accounts
- SMB authentication maps users to their intended private or shared storage
- Shared uploads are forced to a dedicated shared-storage account so they count against the shared quota

## Next Steps

- Test macOS Finder access
- Add secure remote access without exposing SMB directly to the internet
- Test a file restore from an older snapshot
