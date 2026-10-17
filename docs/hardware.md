# Hardware Inventory

**Latest reconciliation: October 17, 2026.** Component models are suitable for this public portfolio; serial numbers, WWNs, MAC addresses, and operational addresses are omitted.

| Component | Latest recorded configuration | Status |
|---|---|---|
| Host | Dell Precision 3440 SFF, Intel Core i7-10700 | Documented host |
| Memory | Four 8 GB DIMMs, 32 GB total | Latest owner confirmation; previously reported about 31 GiB usable at 2400 MT/s |
| SSD | 1 TB T-FORCE SSD | Latest confirmed connected SSD; host/VM allocation needs inspection |
| NAS disks | Two 2 TB Seagate enterprise HDDs | Primary data and separate backup target |
| Disk shelf | EMC KTN-STL3, 15 bays | In use for existing NAS storage |
| HBA | LSI 9200-8e / SAS2008 external SAS HBA | Connected to shelf over SFF-8088 |
| Network adapter | Intel i350-based four-port NIC | Documented; current interface mapping needs verification |
| Switch | TP-Link TL-SG108E managed switch | Documented; current port/VLAN assignment needs verification |
| Expansion disks | Four purchased 4 TB HDDs | Awaiting deployment and individual validation |

## Storage History

The two production NAS drives previously passed extended SMART self-tests without reported sector errors. They were later migrated from a USB dock that disconnected during I/O to the SAS shelf. Filesystems and data were retained, and NAS access was tested after migration.

One newly purchased 4 TB SAS drive initially exposed only 3 TB. Its reported user capacity was restored and verified at approximately 4 TB decimal (3.6 TiB). This result applies to that drive only and does not establish the health or capacity of the other three drives. The original production disk arrangement was restored after the test.

## Capacity and Expansion

Owned HDD capacity totals 20 TB nominal before formatting or redundancy: two 2 TB drives plus four 4 TB drives. Only the two-drive primary/backup arrangement is documented as deployed. Backup capacity is not additional primary storage.

Older references to smaller SSDs are historical; their continued presence is unconfirmed. No spare PCIe slot is reported with the HBA and network adapter installed. Expansion decisions must account for physical space, cooling, power, and available host resources.
