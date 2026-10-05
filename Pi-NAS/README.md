# Pi-NAS

Pi-NAS is the Network Attached Storage portion of my Raspberry Pi 3-in-1 Home Server.

The goal was to use an NVMe SSD connected to a Raspberry Pi 5 as network-accessible storage for computers on my local network.

## Hardware

- Raspberry Pi 5
- Geekworm X1001 NVMe adapter
- SUNEAST 256GB NVMe SSD
- Raspberry Pi 27W power supply
- Active cooling

## Storage

The NVMe SSD was formatted and mounted at:

```bash
/mnt/nas
```

The drive can be verified using:

```bash
lsblk -f
```

and:

```bash
df -h /mnt/nas
```

This provides persistent storage for the server without relying on the Raspberry Pi's microSD card for file storage.

## Samba

Samba was installed to allow Windows computers to access the Raspberry Pi storage using SMB.

The Samba service can be checked using:

```bash
sudo systemctl status smbd
```

Samba accounts can be viewed using:

```bash
sudo pdbedit -L
```

The NAS can then be accessed from Windows using:

```text
\\PI-IP-ADDRESS
```

## Testing

The NAS was tested from a Windows computer.

I successfully:

- Connected to the Samba share
- Created a folder
- Created a file
- Opened the file
- Modified the file
- Renamed the file
- Deleted the file
- Deleted the folder

This verified both read and write access between the Windows client and Raspberry Pi.

## Result

The Raspberry Pi successfully functions as a local Network Attached Storage server using NVMe storage and Samba.

This service continues running alongside Nextcloud and Pi-hole as part of the complete Pi-3in1 system.

## Skills Demonstrated

- Linux file systems
- NVMe storage
- Samba/SMB
- Network file sharing
- Linux permissions
- Windows/Linux interoperability
- Linux service management
- Network troubleshooting