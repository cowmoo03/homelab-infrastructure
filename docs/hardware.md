# Hardware

## Hypervisor Host

The primary lab host is a Dell Precision 3440 SFF with an Intel Core i7-10700. It runs Proxmox VE and hosts the virtual firewall, Linux servers, client VMs, and public-service workloads.

### Memory

The host was upgraded to 32 GB of RAM using four 8 GB DIMMs. Proxmox reports approximately 31 GiB of usable memory, and all four modules are detected and configured at 2400 MT/s.

This additional memory provides more headroom for running multiple virtual machines concurrently, including firewall, server, desktop-lab, and future cybersecurity workloads.

## Networking

- TP-Link Easy Smart managed switch
- Intel i350-based multi-port network adapter
- Separate physical connectivity for management and segmented lab traffic

## Storage

The lab uses a mix of local SSD storage for virtual machines and additional disks for service data, file-storage experiments, backups, and future NAS work.

Two 2 TB Seagate Exos enterprise HDDs were added for the NAS/storage project. Before use, both drives were checked with SMART data and completed extended self-tests without error. Neither drive showed reallocated, pending, or uncorrectable sectors during validation.

The planned role is:

- Primary 2 TB HDD: multi-user file, photo, and video storage
- Secondary 2 TB HDD: automated backup target for important data and selected services

## Design Constraints

A small-form-factor business desktop was intentionally used to keep the project inexpensive, power-efficient, and representative of hardware that can be repurposed for a practical home IT lab.
