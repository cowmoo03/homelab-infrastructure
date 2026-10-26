# NAS Storage and Backups

**Status:** Core NAS functionality was implemented and tested. The latest owner report says the original production arrangement was restored after a temporary expansion-drive test. Current health and configuration must be inspected before adding access.

All user names and share labels below are anonymous examples.

## Storage Layout

A dedicated Ubuntu Server VM provides Samba storage. Two 2 TB enterprise disks are mapped into the VM by stable host device identity, formatted as ext4, and mounted persistently through `/etc/fstab`.

| Filesystem label | Mount | Role |
|---|---|---|
| `STORAGE` | `/srv/storage` | Primary user files |
| `BACKUPS` | `/srv/backups` | Separate versioned backup target |

This documented layout is a primary disk plus a backup disk, **not RAID**. It does not establish whether unrelated host or guest storage uses redundancy.

| Example share | Example path | Access |
|---|---|---|
| `User1` | `/srv/storage/users/user1` | First private storage account |
| `User2` | `/srv/storage/users/user2` | Second private storage account |
| `Shared` | `/srv/storage/shared` | Authorized shared-storage group |

Private directories use owner-only filesystem access and Samba restrictions. Storage users are documented as non-login accounts, separate from administrator access. Shared uploads use a dedicated service account for quota accounting.

## Quotas

Per-user and shared-area quotas are enforced on the primary storage volume, with headroom reserved on the disk. Verify live quota enforcement, current usage, and free space before assigning another quota. Primary quotas do not guarantee sufficient backup space for retained versions.

## Access and Validation

Previously completed tests include Windows file creation, rename and deletion; cross-user private-share denial; authenticated shared-folder access; persistent mapped drives; and access through a mobile hotspot using Tailscale. A router DHCP reservation was configured for stable LAN access. Android access was reported working; macOS validation was deferred.

Remote SMB uses Tailscale rather than public SMB forwarding. A new recipient's access must be tested with that recipient's own identity and credentials. Existing owner access does not prove restrictions for other users.

## Versioned File Backups

rsnapshot stores backups on the second disk, with documented retention of seven daily and four weekly snapshots. Cron runs the daily and weekly jobs on a fixed schedule; verify the timezone and actual cron configuration before changing schedules.

A manual backup and a restore of a deleted test file were verified, including ownership and permissions. Scheduled jobs use a wrapper that sends a Discord alert on failure and stays silent on success. Both a synthetic failure and a real successful backup run were tested.

These are local file backups. They do not establish full-VM backups or offsite disaster recovery. Check the latest successful run, retention behavior, target mount, and space before relying on recovery.

## Password Self-Service

A private HTTPS portal is served through Tailscale Serve. It verifies the current Samba password before allowing a change. An end-to-end password change followed by a fresh Windows SMB login was tested.

For a forgotten password, an administrator sets a temporary Samba password and the user replaces it through the portal. The earlier SSH-based workaround was removed. Portal access for an additional account requires explicit authorization and a separate test; it is not implied by SMB access.

## Storage Transport and Monitoring

The unreliable dual-bay USB dock was replaced by an EMC KTN-STL3 shelf connected through an external SAS HBA. Existing filesystem labels and UUIDs were retained. Stable host disk mappings were updated, both mounts were verified, and Windows share access was tested.

The migration record states that SMART monitoring was updated to stable SAS device mappings and re-enabled. Older USB Prolific/by-path monitoring instructions are obsolete. Inspect the current `smartd` configuration and each monitored device before claiming present coverage.

A synthetic SMART warning was previously delivered through the alert chain. That test proves the notification path at the time; it does not independently prove current monitoring of every disk. Credentials and webhook URLs remain private.

## Reboot Validation and Next Change

A prior NAS reboot test verified both mounts, Samba, Tailscale, the password portal, HTTPS serving, and Windows share access. Repeat relevant checks after storage or network changes.

Before adding another user: inspect Samba configuration, mounted filesystems, ownership/ACLs, quotas, backup health, and Tailscale policy. Preserve a rollback copy of affected configuration privately. Add only the new private account/share, verify allowed and denied access, and retest existing users. See [roadmap](roadmap.md).
