# Pi-3in1

A Raspberry Pi 5 home server combining three self-hosted services:

1. NAS
2. Private Cloud
3. Pi-hole

## Project Goals

The goal is to build a low-cost, self-hosted server using a Raspberry Pi 5 and NVMe storage.

## Architecture

Raspberry Pi 5
│
├── NAS
│   └── Samba
│
├── Private Cloud
│   └── Nextcloud
│
└── Network-wide Ad Blocking
    └── Pi-hole

## Hardware

- Raspberry Pi 5
- Geekworm X1001 NVMe HAT
- 256GB M.2 2280 NVMe SSD
- 27W 5V/5A USB-C power supply
- Cooling system
- 3D-printed enclosure
- Ethernet

## Progress

- [x] Raspberry Pi OS
- [x] NVMe detection
- [x] NVMe partitioning
- [x] EXT4 filesystem
- [x] Persistent NVMe mounting
- [ ] Samba NAS
- [ ] Private cloud
- [ ] Pi-hole
- [ ] Final enclosure
- [ ] Remote access
