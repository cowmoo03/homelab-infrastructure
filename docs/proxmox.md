# Proxmox Virtualization

Proxmox VE hosts workloads separated by function and trust level.

| Documented VM role | Purpose |
|---|---|
| OPNsense | Routing, DHCP, NAT, firewall policy |
| Ubuntu Server | General Linux administration and storage practice |
| Ubuntu Desktop | Client for isolated security-lab testing |
| Minecraft Server | Public-facing game service in the DMZ |
| NAS Server | Samba shares, quotas, backups, and private password portal |

VM IDs, current power state, CPU/RAM allocations, virtual disks, and network interfaces must be inspected before provisioning or modifying a VM. They are not inferred from this role list.

## Storage

The latest owner report identifies a connected 1 TB SSD. Exact host/guest allocations and free space need verification.

The NAS has an SSD-backed OS disk and two individually mapped 2 TB HDDs from the external SAS shelf. Stable disk identities are used rather than temporary Linux device letters. Inside the NAS, ext4 labels persistently mount the primary filesystem at `/srv/storage` and the backup filesystem at `/srv/backups`.

Earlier Linux practice used a separate ext4 virtual volume at `/srv/labdata` for backups, evidence, logs, packet captures, scripts, and tools. Its present attachment and use are not confirmed by that historical exercise.

## Operational Practices

- Verify device identity and VM ownership before changing storage mappings.
- Keep production primary and backup disks intact during expansion work.
- Check available RAM and disk capacity before allocating new guests.
- Validate persistence of mounts and networking after reboot.
- Keep management access private and separate from guest access.
- Distinguish NAS file backups from full-VM recovery; automated VM backups remain planned.

## Proposed Docker VM

A separate Ubuntu VM is planned for an additional user's containers. An initial proposal is 2 vCPU, 4 GiB RAM, and 40 GiB disk, subject to workload and host capacity checks. The VM is not yet deployed.

Docker administrative access is treated as guest-root capability. Isolation must be enforced outside the VM, with no production NAS mounts, host devices, or hypervisor administration granted to that user.
