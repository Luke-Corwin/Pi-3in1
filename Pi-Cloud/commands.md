# PiCloud Commands

Useful commands for managing and troubleshooting the PiCloud setup.

## System Information

Check the Pi's IP address:

```bash
hostname -I
```

Check disk usage:

```bash
df -h
```

Check the NVMe drive:

```bash
lsblk
```

## NVMe / SSD

Check the SSD mount:

```bash
df -h /mnt/nas
```

Check the PiCloud directory:

```bash
ls -lh /mnt/nas/nextcloud
```

Check the Nextcloud data directory:

```bash
ls -lh /mnt/nas/nextcloud/data
```

Check directory permissions:

```bash
ls -ld /mnt/nas/nextcloud
```

## Mounting

Mount everything configured in `/etc/fstab`:

```bash
sudo mount -a
```

View the filesystem table:

```bash
cat /etc/fstab
```

Check mounted drives:

```bash
findmnt
```

## Nextcloud Permissions

Set Nextcloud ownership:

```bash
sudo chown -R www-data:www-data /mnt/nas/nextcloud
```

Check ownership:

```bash
ls -ld /mnt/nas/nextcloud
```

## MariaDB

Open MariaDB:

```bash
sudo mariadb
```

Select the Nextcloud database:

```sql
USE nextcloud;
```

Show databases:

```sql
SHOW DATABASES;
```

Show database users:

```sql
SELECT User, Host FROM mysql.user;
```

Exit MariaDB:

```sql
EXIT;
```

## Tailscale

Check Tailscale status:

```bash
sudo tailscale status
```

Start Tailscale:

```bash
sudo tailscale up
```

Check the Pi's Tailscale IP:

```bash
tailscale ip
```

## Power

Safely shut down the Raspberry Pi:

```bash
sudo poweroff
```

Reboot:

```bash
sudo reboot
```

## Nextcloud Access

Local network address:

```text
http://PI-IP/nextcloud
```

Example:

```text
http://192.168.5.26/nextcloud
```

## Storage Test

Upload a file through Nextcloud, then run:

```bash
ls -lh /mnt/nas/nextcloud/data
```

Check storage usage:

```bash
df -h /mnt/nas
```

This confirms that PiCloud is using the NVMe SSD for its storage.

## Important

Never commit passwords, API keys, private keys, Tailscale authentication information, or other secrets to GitHub.

Use placeholders such as:

```text
YOUR_PASSWORD
PI-IP
```

instead of publishing your actual credentials.
