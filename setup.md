# NAS Setup

## 1. Hardware

The NAS uses:

- Raspberry Pi 5
- Geekworm X1001 PCIe to M.2 NVMe HAT
- SUNEAST 256GB M.2 2280 PCIe 3.0 NVMe SSD
- 27W 5V/5A USB-C power supply
- Cooling fan
- Ethernet

## 2. NVMe Configuration

The NVMe SSD was detected as:

`/dev/nvme0n1`

A GPT partition table was created and the primary partition was formatted as EXT4.

The partition is:

`/dev/nvme0n1p1`

The drive is mounted at:

`/mnt/nas`

Persistent mounting was configured through `/etc/fstab`.

## 3. Samba

Samba was installed and configured to provide network file sharing.

The NAS share points to:

`/mnt/nas/storage`

Authentication is handled through a Samba user account.

## 4. Testing

The NAS was connected from a Windows computer and successfully tested for file access and read/write functionality.

## 5. Remote Access

Tailscale can be used as an optional remote-access layer to securely reach the Raspberry Pi/NAS when away from the local network.