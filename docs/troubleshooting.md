# Troubleshooting Notes

This project involved several real-world troubleshooting scenarios rather than a single clean deployment path.

## Network Interface Renaming

After a Linux interface-name change, a Proxmox bridge referenced an interface that no longer existed under its previous name.

Approach:

1. Enumerated current network interfaces.
2. Matched the physical NIC ports to their new Linux names.
3. Updated the Proxmox bridge configuration.
4. Verified link state and connectivity.

## OPNsense Default Route

The firewall initially had WAN connectivity but lacked a usable default route.

Approach:

1. Inspected the routing table.
2. Validated the upstream gateway.
3. Added a temporary route to confirm the diagnosis.
4. Corrected the persistent gateway configuration.
5. Rebooted and verified the route survived.

## DHCP on a New DMZ Interface

A newly created DMZ interface did not initially receive DHCP service.

Approach:

1. Verified the interface was enabled and addressed.
2. Checked the DHCP/Dnsmasq interface selection.
3. Added the DMZ interface to DHCP service.
4. Verified a client received an address and gateway.

## Firewall Rule Ordering

Traffic was not being blocked as expected because a broader allow rule was evaluated before the intended restriction.

Approach:

1. Reviewed rule order and source/destination objects.
2. Replaced an overly broad source alias with an explicit network object where appropriate.
3. Moved block rules above general allow rules.
4. Retested connectivity between trust zones.

## Duplicate Linux Plugin Files

A Minecraft plugin was loaded twice because two filenames differed only by capitalization.

Approach:

1. Inspected the plugin directory.
2. Identified the duplicate JAR files.
3. Stopped the service before modification.
4. Removed the redundant copy.
5. Restarted and verified a clean plugin load.

These incidents helped reinforce a repeatable workflow: observe, isolate the layer involved, change one variable at a time, and verify the result.

## NAS USB Dock Disconnections

Both NAS disks disappeared during I/O through a dual-bay USB dock, leaving the VM in an I/O error state. The disks were moved to the external SAS shelf, their identities and filesystems were checked, and stable host mappings were updated. Both NAS mounts and client access were verified afterward. USB-specific monitoring settings became obsolete and must not be reused for the SAS connection.

## Expansion Disk Reported Reduced Capacity

One newly purchased nominal 4 TB SAS disk reported 3 TB. After confirming its identity and that it contained no production data, its reported capacity was restored and independently checked. The production disk arrangement was then restored and normal operation reported.

This is a historical result, not a reusable disk command recipe. Identify each disk individually, check VM ownership and mounted data, and obtain explicit approval before destructive storage operations. Temporary device letters are not stable identifiers. Capacity restoration alone does not establish drive health.
