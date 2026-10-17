# Security Controls

The lab uses workload separation, network segmentation, authenticated services, and least privilege. These controls reduce risk; documented intent does not substitute for current verification.

## Implemented and Previously Tested

- Separate security-lab and public-service DMZ networks behind OPNsense
- Restricted public forwarding for game services, without intentional public management exposure
- Samba authentication, private-directory permissions, shared-group restrictions, and quotas
- Cross-user private-share denial tested from Windows
- Remote storage access through Tailscale
- Private HTTPS password portal with current-password verification
- Versioned local file backups and a successful restore test
- Backup failure and SMART alert notification tests
- Minecraft whitelist and online authentication, with RCON/query disabled in the documented configuration

## Verification Gaps

Recheck live firewall rules, bridge placement, IPv6, overlay permissions, service patch levels, backup freshness, and SMART device coverage before granting new access. A Tailscale connection alone does not establish least privilege, and a synthetic alert does not prove present disk health.

## Additional Storage User — Planned

Use a dedicated non-login account, private directory, explicit Samba authorization, and an enforced quota. Do not add the account to existing shared-storage groups by default. Proposed remote access is NAS TCP 445 only; password-portal access is separate if needed. Verify denied access to other shares and administration, as well as successful use of the private share.

## Docker Environment — Planned

Treat Docker administration as control of the guest. Use a separate VM and externally enforced network restrictions, including protection of the NAS, hypervisor, upstream router, other guests, and overlay resources. Do not mount production NAS data into the test environment.

Define authorized test targets and permitted outbound traffic before security testing. Active exploitation exercises belong on disposable targets with dummy data. No design guarantees impossible compromise.

## Publishing and Secret Handling

Follow [CONTRIBUTING.md](../CONTRIBUTING.md). Public documentation uses generic account labels and omits personal names, addresses, tailnet identifiers, credentials, private keys, webhook URLs, disk serials/WWNs, and raw operational exports.

Review file contents, filenames, images, diffs, and commit messages before publishing. Deleting a value from the latest file does not remove it from Git history. If a credential is exposed, revoke or rotate it promptly; history cleanup alone cannot invalidate a copied secret. Coordinate any history rewrite and account for forks, cached views, and existing clones.
