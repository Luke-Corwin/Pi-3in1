# PiCloud Setup Guide

This guide documents how I set up PiCloud using a Raspberry Pi 5, NVMe storage, and Nextcloud.

## 1. Prepare the Hardware

The PiCloud build uses:

* Raspberry Pi 5
* NVMe HAT
* NVMe SSD
* Ethernet
* Raspberry Pi power supply

I also designed and 3D printed a custom case in **Blender**. The case was designed to hold the Raspberry Pi 5, NVMe HAT, and SSD together as a single compact unit.

## 2. Verify the NVMe SSD

After connecting the NVMe HAT and SSD, check that the drive is detected:

```bash
lsblk
```

The SSD should appear as an NVMe device, such as:

```text
nvme0n1
└─nvme0n1p1
```

Check the available storage:

```bash
df -h
```

## 3. Mount the SSD

The SSD is mounted at:

```text
/mnt/nas
```

Verify the mount:

```bash
df -h /mnt/nas
```

The output should show the NVMe drive mounted at `/mnt/nas`.

## 4. Create the Nextcloud Directory

Create the directory used by Nextcloud:

```bash
sudo mkdir -p /mnt/nas/nextcloud
```

Set the ownership so the web server can access it:

```bash
sudo chown -R www-data:www-data /mnt/nas/nextcloud
```

Verify the permissions:

```bash
ls -ld /mnt/nas/nextcloud
```

## 5. Install the Database

PiCloud uses MariaDB as the Nextcloud database.

Open MariaDB:

```bash
sudo mariadb
```

Create the Nextcloud database:

```sql
CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
```

Create the database user:

```sql
CREATE USER 'nextcloud'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD';
```

Grant the required permissions:

```sql
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextcloud'@'localhost';
```

Apply the changes:

```sql
FLUSH PRIVILEGES;
```

Exit MariaDB:

```sql
EXIT;
```

**Do not commit your real database password to GitHub.**

## 6. Install Nextcloud

After installing and configuring the required web server, PHP, and Nextcloud packages, open Nextcloud from another device:

```text
http://PI-IP/nextcloud
```

For example:

```text
http://192.168.5.26/nextcloud
```

During the Nextcloud setup:

### Data folder

Use:

```text
/mnt/nas/nextcloud/data
```

### Database

Select:

```text
MySQL/MariaDB
```

Enter the database information created earlier:

```text
Database user: nextcloud
Database password: YOUR_PASSWORD
Database name: nextcloud
Database host: localhost
```

Create the Nextcloud administrator account.

## 7. Verify Storage

After installation, upload a file through Nextcloud and check the SSD:

```bash
ls -lh /mnt/nas/nextcloud/data
```

You can also check the SSD's available space:

```bash
df -h /mnt/nas
```

The uploaded files should be stored on the NVMe SSD rather than the Raspberry Pi's microSD card.

## 8. Access PiCloud From a Phone

Install the official Nextcloud mobile app.

When asked for the server address, enter:

```text
http://PI-IP/nextcloud
```

For example:

```text
http://192.168.5.26/nextcloud
```

Then log in using the Nextcloud account created during setup.

## 9. Remote Access

Tailscale can optionally be used to access PiCloud remotely without directly exposing Nextcloud to the public internet.

Check the Tailscale status with:

```bash
sudo tailscale status
```

Start Tailscale with:

```bash
sudo tailscale up
```

Remote access should only be configured after confirming that PiCloud works correctly on the local network.

## Final Architecture

```text
                 Raspberry Pi 5
                       │
                  NVMe HAT
                       │
                  NVMe SSD
                       │
                 /mnt/nas
                       │
                  Nextcloud
                       │
              ┌────────┴────────┐
              │                 │
          Web Browser       Mobile App
```

The Raspberry Pi runs Nextcloud while the NVMe SSD provides the primary storage for PiCloud.
