# Lab Hardware

## Proxmox host

- **Memory:** 32 GB installed; the node reports 31.02 GiB usable.
- **Storage:** 1 TB SSD; the captured local-lvm thin pool reports 875.49 GB capacity.
- **Processor:** Intel Core i3-14100T, with 8 logical CPUs shown in Proxmox.
- **Platform:** Proxmox VE 9.2.2 in the captured node overview.

The node screenshot verifies these CPU and resource details. It does not identify the chassis model, so I removed the earlier unverified HP EliteDesk 800 G6 model entry. [See the resource and VM screenshots](setup/windows-endpoint.md).

## Virtual machines

| VM ID | Name | Purpose |
|---|---|---|
| 100 | `opnsense-fw` | Lab firewall and gateway |
| 101 | `lab-desktop` | Debian endpoint for Linux investigations |
| 102 | `soc-wazuh` | Ubuntu server running Wazuh manager, indexer, and dashboard |
| 103 | `lab-windows` | Windows endpoint for Windows Security event investigations |

The Windows VM hardware screenshot confirms 4 CPU cores, 8 GiB RAM, an 80 GiB disk, OVMF UEFI, TPM 2.0, and a VirtIO NIC on `vmbr1`. Its Wazuh enrollment and Windows event collection are verified in the setup notes.

## Existing server

My separate Debian server runs Nextcloud and is outside this SOC project.

## Planned additions

Windows Server, Active Directory, and Sysmon are still planned. They are not part of the completed environment yet.
