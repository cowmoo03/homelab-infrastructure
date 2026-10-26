# Hardware Inventory

**Latest reconciliation: October 17, 2026.** This inventory describes capability classes rather than exact models. Specific component models, serial numbers, WWNs, MAC addresses, and operational addresses are intentionally omitted from this public portfolio.

| Component | Class / capability | Status |
|---|---|---|
| Host | Small-form-factor business desktop, 8-core/16-thread CPU | Hypervisor host |
| Memory | 32 GB | Confirmed |
| SSD | ~1 TB SSD for host and VM storage | In use |
| NAS disks | Two 2 TB enterprise HDDs | Primary data and separate backup target |
| Disk shelf | 15-bay external SAS disk shelf | In use for NAS storage |
| HBA | External SAS HBA (IT mode) | Connects the host to the shelf |
| Network adapter | Multi-port server NIC | Provides interfaces for segmentation |
| Switch | Managed, VLAN-capable Gigabit switch | In use |
| Expansion disks | Four 4 TB HDDs | Awaiting deployment and individual validation |

## Storage History

The two production NAS drives previously passed extended SMART self-tests without reported sector errors. They were later migrated from a USB dock that disconnected during I/O to the external SAS shelf. Filesystems and data were retained, and NAS access was tested after migration.

One newly purchased 4 TB SAS drive initially exposed only 3 TB. Its reported user capacity was restored and verified at approximately 4 TB decimal (3.6 TiB). This result applies to that drive only and does not establish the health or capacity of the other drives.

## Capacity and Expansion

Owned HDD capacity totals 20 TB nominal before formatting or redundancy: two 2 TB drives plus four 4 TB drives. Only the two-drive primary/backup arrangement is documented as deployed. Backup capacity is not additional primary storage.

No spare PCIe slot is available with the HBA and network adapter installed. Expansion decisions must account for physical space, cooling, power, and available host resources.
