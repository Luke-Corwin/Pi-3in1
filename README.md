# Raspberry Pi 3-in-1 Home Server

A Raspberry Pi 5 home server combining a NAS, private cloud, and DNS filtering system into one device.

This project was built to gain hands-on experience with Linux, networking, storage, databases, DNS, remote access, and integrating multiple services on one Raspberry Pi.

## System Overview

```text
                    Raspberry Pi 5
                          |
                     256GB NVMe
                          |
          +---------------+---------------+
          |               |               |
        Samba         Nextcloud        Pi-hole
          |               |               |
      Local NAS       Private Cloud    DNS Filtering
                          |
                   Apache + MariaDB
                          |
                      Tailscale
```

## Hardware

- Raspberry Pi 5
- Geekworm X1001 NVMe adapter
- SUNEAST 256GB NVMe SSD
- Raspberry Pi 27W power supply
- Active cooling
- Raspberry Pi OS

<!-- IMAGE: Add a photo of the complete Raspberry Pi server here -->

## 1. Network Attached Storage

The first part of the project turns the Raspberry Pi into a Network Attached Storage server using Samba.

The 256GB NVMe SSD is mounted at:

```bash
/mnt/nas
```

Samba allows computers on the local network to access the storage directly through Windows File Explorer.

The NAS was tested from a Windows PC by creating, opening, editing, renaming, and deleting files.

**Status: Complete and tested**

<!-- IMAGE: Add screenshot of Samba share open in Windows -->

[View Pi-NAS](Pi-NAS/README.md)

## 2. Private Cloud

The second part of the project uses Nextcloud to create a self-hosted private cloud.

Nextcloud runs through Apache and uses MariaDB to store information including user accounts, permissions, file metadata, folder structure, and application settings.

The actual Nextcloud files are stored on the NVMe SSD.

Tailscale was also configured to provide secure remote connectivity to the Raspberry Pi.

The cloud was tested by creating folders and uploading, downloading, renaming, and deleting files through Nextcloud.

**Status: Complete and tested**

<!-- IMAGE: Add screenshot of your Nextcloud dashboard -->

[View Pi-Cloud](Pi-Cloud/README.md)

## 3. DNS Filtering

The third part of the project uses Pi-hole to provide DNS-level filtering.

Pi-hole listens for DNS requests on port 53 and forwards permitted requests to Cloudflare while blocking domains included in its filtering lists.

Pi-hole was configured to use the Raspberry Pi's `wlan0` interface.

DNS functionality was verified with:

```bash
dig @192.168.5.26 google.com
```

The Raspberry Pi successfully returned DNS records through port 53.

**Status: Complete and tested**

<!-- IMAGE: Add screenshot of Pi-hole dashboard or query log -->

[View Pi-hole](Pi-hole/README.md)

## Troubleshooting

One of the most useful parts of this project was troubleshooting the network configuration.

During Pi-hole setup, I attempted to move the server from Wi-Fi to Ethernet through an existing network switch. The Ethernet interface received an address on a different upstream network instead of the private LAN used by the mesh network.

I diagnosed the issue using:

```bash
ip -br addr
ip route
ip route get 1.1.1.1
```

After identifying the routing difference, I returned the server to the private Wi-Fi network and configured Pi-hole to use `wlan0`.

This reinforced the importance of understanding network interfaces, routing, private and public IP addressing, and how physical network topology affects services.

## Technologies Used

- Raspberry Pi
- Linux
- Samba
- Nextcloud
- Apache
- MariaDB
- SQL
- Pi-hole
- DNS
- Tailscale
- NVMe storage
- TCP/IP networking
- Linux systemd services
- SSH
- Git/GitHub

## What I Learned

This project gave me experience building and troubleshooting a multi-service Linux server rather than configuring each service independently.

I learned how network storage, web servers, databases, DNS, remote access, file permissions, network interfaces, and persistent storage interact on a single system.

The project also required validating each service individually and troubleshooting real networking problems instead of relying only on a successful installation.

## Future Improvements

- Configure Pi-hole as the network-wide DNS server
- Add automated server backups
- Configure HTTPS for Nextcloud
- Add system monitoring
- Add additional storage
- Integrate backup power and safe shutdown functionality