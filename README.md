# Raspberry Pi NAS

The NAS component of **Pi-3in1** is a self-hosted network storage server built using a Raspberry Pi 5 and NVMe storage. Files can be accessed over the local network through Samba.

## Hardware

- Raspberry Pi 5
- Geekworm X1001 PCIe to M.2 NVMe HAT
- SUNEAST 256GB M.2 2280 PCIe 3.0 NVMe SSD
- 27W 5V/5A USB-C power supply
- Cooling fan
- Ethernet
- 3D-printed enclosure

## Software

- Raspberry Pi OS
- Linux
- EXT4
- Samba
- SSH

## Storage

The NVMe SSD is configured with an EXT4 filesystem and mounted at:

`/mnt/nas`

Persistent mounting is configured through `/etc/fstab`.

## Network Storage

Samba is configured to provide authenticated network file sharing. The NAS has been successfully tested from Windows with file read/write functionality.

## Status

**Complete and operational.**

This is the first completed component of the **Pi-3in1** project.

### Project Roadmap

- [x] NAS
- [ ] Private Cloud
- [ ] Pi-hole