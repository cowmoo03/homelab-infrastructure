# Roadmap

**Reviewed: October 17, 2026.** Items below remain planned unless explicitly marked completed in [current state](current-state.md).

## 1. Verify Production NAS State

Inspect live Samba configuration, mount identity, folder permissions/ACLs, quotas, backup freshness, and current Tailscale policy. Recheck SMART monitoring after the SAS migration and temporary drive test. Preserve affected configuration privately before changes.

## 2. Additional Private NAS Access

Create a separate non-login storage account and private share, with a quota based on available capacity. Limit remote permissions, test allowed file operations and denied access, and retest existing users. Establish the recipient's intended use before granting access.

## 3. Isolated Docker VM

Provision a separate Ubuntu VM after resource checks. Enforce network restrictions outside the guest, allow only required connectivity, and prove that production storage and administration are unreachable. Set resource limits and a recovery plan. Keep security exercises on disposable targets.

## 4. Storage Expansion

Validate all four purchased 4 TB drives individually after caddies arrive. One drive's capacity repair is complete; the others are not assumed validated. Select a redundancy layout only after reviewing capacity, failure tolerance, backup coverage, and migration/rollback needs. Retain the working primary and backup disks throughout preparation.

## 5. Recovery and Monitoring

- UPS sizing and tested graceful shutdown
- Automated full-VM and additional application backups
- Offsite recovery and more restore exercises
- Centralized logs, SIEM, service health monitoring, and dashboard
- Updated recovery runbooks and sanitized diagrams

Scheduled NAS file backups, local restore testing, remote SMB access, and basic Discord alerts are already documented accomplishments; expansion of those capabilities remains future work.

## Longer-Term Learning

- Windows Server and Active Directory
- Ansible automation
- Additional isolated security targets
- Optional Nextcloud and duplicate-photo workflows
- Self-hosted mail with deliverability and recovery planning
- CPU-based local AI experiments within available resources

These interests are not claims of installed services.
