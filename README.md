# Raspberry Pi 3-in-1

A Raspberry Pi project combining three useful services into a single system: **Pi-NAS, PiCloud, and Pi-hole**.

The goal is to make one Raspberry Pi capable of handling local storage, private cloud access, and network-wide ad blocking, while keeping the system practical and expandable.

## Pi-NAS

The NAS portion provides local network storage for files and other data.

**Current setup includes:**

* Raspberry Pi 5
* NVMe SSD
* Samba file sharing
* Local network access
* Remote access through Tailscale
* Mobile file uploads

Documentation for the NAS setup, commands, and troubleshooting can be found in the [`Pi-NAS`](./Pi-NAS) directory.

## Pi-Cloud

The Pi-Cloud portion is intended to provide private cloud storage that can be accessed remotely without relying entirely on third-party cloud storage providers.

Planned functionality includes:

* Remote file access
* File uploads and downloads
* Mobile access
* Private user storage
* Integration with the NAS storage

## Pi-hole

The Pi-hole portion will provide network-wide DNS-based ad and tracker blocking.

It will be configured to run alongside the NAS and cloud services on the same Raspberry Pi.

Planned functionality includes:

* Network-wide ad blocking
* Tracker blocking
* DNS management
* Usage statistics
* Local network configuration

## Expansion

The 3-in-1 setup is designed to be expandable. Additional services and projects can be added to the system as the hardware and storage configuration allow.

The focus is on building a single Raspberry Pi system that can provide multiple useful services while remaining easy to maintain, document, and expand.
