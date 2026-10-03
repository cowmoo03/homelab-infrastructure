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

Windows SMB access to the Moemin share was tested successfully through File Explorer, showing the expected `Files`, `Photos`, and `Videos` directories.

## Access-Control Design

- Private user directories are owner-only
- Shared storage is accessible only to members of the shared NAS group
- Administrative access is separate from storage-user access
- Storage users use non-login system accounts
- SMB authentication maps users to their intended private or shared storage
- Shared uploads are forced to a dedicated shared-storage account so they count against the shared quota

## Next Steps

- Test write/delete access
- Test cross-user isolation
- Test the Shared share
- Test macOS Finder access
- Configure automated backups to the secondary disk
- Add secure remote access without exposing SMB directly to the internet
