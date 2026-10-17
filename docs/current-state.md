# Current State and Verification

**Review date: October 17, 2026.** Based on the project documentation and the latest owner-reported hardware and recovery results. No live host inspection was performed during this documentation review.

## Evidence Levels

- **Documented completed:** implemented and tested previously; this is not a guarantee of present health.
- **Latest reported:** a recent owner update or recorded diagnostic result.
- **Needs verification:** inspect the live system before relying on the detail or making changes.
- **Planned:** not established as deployed.

## Status

| Component | Status | Remaining verification |
|---|---|---|
| Proxmox host | Documented Dell Precision / i7; latest report confirms 32 GB RAM and 1 TB SSD | VM allocations, free resources, host storage layout |
| OPNsense and segmentation | Documented completed | Interfaces, rules, IPv4/IPv6 paths, current isolation tests |
| Samba NAS | Documented completed; reported restored to normal after temporary disk test | Mounts, share configuration, permissions, quotas |
| Primary and backup disks | Two 2 TB ext4 volumes documented | Stable device mappings and current health |
| SAS shelf migration | Documented completed | Monitoring paths after hardware changes |
| NAS remote access | Tailscale SMB tested off-site previously | Current policy and each recipient's effective access |
| Password portal | HTTPS and password-change test completed | Service health, authorized accounts, current exposure |
| File backups | rsnapshot schedule, failure alert, and restore test completed | Latest successful backup and available backup space |
| SMART alerts | Synthetic alert test completed | Actual device coverage and real warning delivery |
| Four expansion HDDs | Purchased; one capacity repair verified | Caddies, individual health/capacity checks, array design |
| Additional storage user | Planned | Identity, quota, private share, off-site access tests |
| Dedicated Docker VM | Planned | Workload requirements and isolation design |

## Corrections Applied in This Review

- Remote NAS access and scheduled NAS file backups are completed work, not initial roadmap tasks.
- Full-VM backups and offsite recovery remain separate future work.
- SAS shelf attachment supersedes the old USB transport; USB-specific SMART settings are historical.
- The architecture includes the NAS and distinguishes VM hosting from network routing.
- Four purchased 4 TB disks are expansion inventory, not an installed production array.
- Minecraft tuning examples are illustrative and must not be treated as a live configuration export.

## Updating This Record

Record the date, evidence, outcome, and any remaining uncertainty after each verified change. Summarize terminal results with anonymous role labels; keep raw outputs, identities, addresses, disk identifiers, and credentials private. Plans become completed work only after implementation and validation.
