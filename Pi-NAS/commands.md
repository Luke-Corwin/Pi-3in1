# NAS Commands

This document contains the primary commands used to build and configure the Raspberry Pi NAS.

## System Setup

Update the Raspberry Pi:

```bash
sudo apt update
sudo apt full-upgrade -y
```

## Hardware Detection

Check connected storage devices:

```bash
lsblk
```

Check PCIe devices:

```bash
lspci -nn
```

Check for NVMe detection:

```bash
sudo dmesg | grep -i nvme
```

Check PCIe kernel messages:

```bash
sudo dmesg | grep -iE "pcie|pci"
```

## NVMe Partitioning

Open the NVMe drive with `parted`:

```bash
sudo parted /dev/nvme0n1
```

Create a GPT partition table and one partition using the entire drive:

```text
mklabel gpt
mkpart primary ext4 0% 100%
quit
```

Verify the partition:

```bash
lsblk
```

## Format the NVMe

Format the NVMe partition using EXT4:

```bash
sudo mkfs.ext4 -L NAS /dev/nvme0n1p1
```

## Mount the NVMe

Create the mount directory:

```bash
sudo mkdir -p /mnt/nas
```

Mount the NVMe:

```bash
sudo mount /dev/nvme0n1p1 /mnt/nas
```

Verify the mount:

```bash
lsblk
```

Check available storage:

```bash
df -h /mnt/nas
```

## NVMe UUID

Find the filesystem UUID:

```bash
sudo blkid /dev/nvme0n1p1
```

## Persistent Mounting

Edit the filesystem table:

```bash
sudo nano /etc/fstab
```

Add the NVMe using its UUID:

```text
UUID=YOUR-NVME-UUID /mnt/nas ext4 defaults,noatime 0 2
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Test the configuration:

```bash
sudo umount /mnt/nas
sudo mount -a
```

Verify that the NVMe is mounted:

```bash
lsblk
```

## NAS Storage Directory

Create the directory used by the Samba share:

```bash
sudo mkdir -p /mnt/nas/storage
```

Set the current user as the owner:

```bash
sudo chown -R $USER:$USER /mnt/nas/storage
```

## Samba Installation

Install Samba:

```bash
sudo apt update
sudo apt install samba samba-common-bin -y
```

Check the Samba version:

```bash
smbd --version
```

Back up the Samba configuration:

```bash
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.backup
```

Edit the Samba configuration:

```bash
sudo nano /etc/samba/smb.conf
```

Add the NAS share:

```text
[NAS]
   path = /mnt/nas/storage
   browseable = yes
   read only = no
   writable = yes
   valid users = YOUR_USERNAME
   create mask = 0664
   directory mask = 0775
```

Replace `YOUR_USERNAME` with the actual Linux username.

## Samba User

Add the Linux user to Samba:

```bash
sudo smbpasswd -a YOUR_USERNAME
```

Enable the Samba account:

```bash
sudo smbpasswd -e YOUR_USERNAME
```

## Validate Samba

Check the Samba configuration:

```bash
testparm
```

## Start Samba

Restart Samba:

```bash
sudo systemctl restart smbd
```

Enable Samba to start automatically:

```bash
sudo systemctl enable smbd
```

Check Samba status:

```bash
sudo systemctl status smbd
```

## Network Access

Find the Raspberry Pi's local IP address:

```bash
hostname -I
```

From Windows File Explorer, connect using:

```text
\\YOUR-PI-IP\NAS
```

Replace `YOUR-PI-IP` with the Raspberry Pi's local IP address.

Authenticate using the Samba username and password.

## NAS Testing

Create a test file directly on the NAS:

```bash
touch /mnt/nas/storage/test.txt
```

Check the contents:

```bash
ls -l /mnt/nas/storage
```

Files created from Windows should also appear in:

```bash
ls -l /mnt/nas/storage
```

## Remote Access

Tailscale can be used as an optional remote-access layer for accessing the Raspberry Pi/NAS outside the local network.

Check Tailscale status:

```bash
tailscale status
```

Check the Tailscale IP:

```bash
tailscale ip
```

Start Tailscale authentication if required:

```bash
sudo tailscale up
```

## Shutdown

Safely shut down the Raspberry Pi:

```bash
sudo poweroff
```

> **Security:** Never commit passwords, private keys, authentication tokens, Tailscale keys, or personal network information to this repository. Replace usernames, UUIDs, and IP addresses with placeholders when documenting commands publicly.