# PiCloud

PiCloud is the private cloud storage portion of my **Pi-3in1** Raspberry Pi project. It uses a Raspberry Pi 5 with NVMe SSD storage and Nextcloud to provide personal file storage that can be accessed from a web browser or the Nextcloud mobile app.

## Hardware

* Raspberry Pi 5
* NVMe HAT
* NVMe SSD
* Ethernet connection
* Raspberry Pi power supply
* Custom 3D-printed case

The case was designed in **Blender** and 3D printed specifically to fit the Raspberry Pi 5, NVMe HAT, and SSD together in one compact enclosure.

## Software

* Raspberry Pi OS
* Nextcloud
* Apache
* MariaDB
* PHP
* Tailscale *(optional for remote access)*

## Storage

The NVMe SSD is mounted at:

```bash
/mnt/nas
```

Nextcloud stores its data on the SSD rather than the Raspberry Pi's microSD card.

## Accessing PiCloud

From a device on the same network, PiCloud can be accessed through:

```text
http://PI-IP/nextcloud
```

For example:

```text
http://192.168.5.26/nextcloud
```

The official Nextcloud app can also be used on a phone by entering the PiCloud server address.

## Project Goal

The goal of PiCloud is to create a low-cost private cloud that gives me control over my own storage instead of relying entirely on third-party cloud storage services.

PiCloud is one of three systems included in my **Pi-3in1** project:

* **Pi-NAS** — Local network storage
* **PiCloud** — Private cloud storage with Nextcloud
* **PiHole** — Network-wide ad blocking

## More Documentation

* [Setup Guide](setup.md)
* [Commands Reference](commands.md)

## Setup

![3D Printed Case](images/3DCase.jpg)
