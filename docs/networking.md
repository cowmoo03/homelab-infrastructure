# Networking and Segmentation

The documented design uses a managed switch, Intel multi-port NIC, Proxmox Linux bridges, and OPNsense. Network labels describe intended roles; inspect routes and firewall behavior before relying on isolation.

## Documented Bridge Roles

| Bridge | Intended role |
|---|---|
| `vmbr0` | Management / upstream connectivity |
| `vmbr1` | Security lab |
| `vmbr2` | Public-service DMZ |

Current physical NIC mapping, switch ports, VLAN membership, PVIDs, guest interfaces, and subnet assignments need live verification. Operational addresses and device identifiers are deliberately omitted.

## Intended Policy

| Source | Destination | Policy |
|---|---|---|
| Security lab | Internet | Permitted through firewall policy |
| Security lab | Management | Blocked |
| DMZ | Internet | Required services permitted |
| DMZ | Management and security lab | Blocked |
| Internet | Published game services | Required forwarded ports only |
| Authorized remote storage client | NAS | Explicit Tailscale and Samba access |
| Proposed Docker segment | NAS, management, other private networks | Blocked; not yet implemented |

## Separate Remote-Access Path

Tailscale runs on the NAS and authorized clients. Off-site SMB access was tested previously. Tailscale policy must be reviewed independently of OPNsense: broad member permissions or subnet routes can bypass the intended restrictions.

For an additional storage recipient, the proposed starting permission is NAS TCP 445 only. The HTTPS password portal is a separate permission if required. Do not assume an existing membership or shared-device rule is sufficiently restrictive.

## Validation Before Access Changes

Check IPv4 and IPv6, direct same-bridge communication, upstream routes, VPN routes, firewall rule order, and return paths. Test the actual application ports from the recipient's identity; ping alone does not prove that SMB or management access is blocked.

Keep administrative connectivity available during changes and preserve private configuration backups. The future Docker VM must be isolated by controls outside the guest, since a guest administrator can change guest firewall settings.
