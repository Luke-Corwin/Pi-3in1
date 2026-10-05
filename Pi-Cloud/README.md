# Pi-Cloud

Pi-Cloud is the self-hosted private cloud portion of my Raspberry Pi 3-in-1 Home Server.

The goal was to host my own cloud storage using Nextcloud rather than relying entirely on a third-party cloud provider.

<!-- IMAGE: Add screenshot of Nextcloud dashboard -->

## Architecture

```text
                 Client
                    |
                Nextcloud
                    |
                  Apache
                    |
          +---------+---------+
          |                   |
       Files               MariaDB
          |                   |
     NVMe Storage          Metadata
```

## Software

The cloud server uses:

- Nextcloud
- Apache
- MariaDB
- PHP
- Tailscale
- Raspberry Pi OS

## Nextcloud

Nextcloud provides the user interface and file-management system.

The installation is stored under:

```bash
/mnt/nas/nextcloud
```

This keeps the cloud storage on the NVMe SSD.

Apache serves the Nextcloud web application to clients on the network.

Its status can be checked using:

```bash
sudo systemctl status apache2
```

## MariaDB

Nextcloud requires more than a location to store files.

MariaDB provides the database used to keep track of information including:

- Users
- File metadata
- Folder structure
- Permissions
- Sharing settings
- Application settings

The database and the actual files therefore serve different purposes.

```text
Nextcloud
   |
   +---- Actual files ----> NVMe storage
   |
   +---- File information -> MariaDB
```

MariaDB can be checked using:

```bash
sudo systemctl status mariadb
```

## Remote Access

Tailscale was installed to provide secure remote connectivity to the Raspberry Pi.

The connection can be checked using:

```bash
tailscale status
```

This gives the Raspberry Pi a separate Tailscale address in addition to its local network address.

## Testing

Nextcloud was tested through its web interface.

I successfully:

- Logged into Nextcloud
- Created a folder
- Uploaded a file
- Opened the file
- Downloaded the file
- Renamed the file
- Deleted the file

<!-- IMAGE: Add screenshot showing files inside Nextcloud -->

This verified that the Nextcloud, Apache, MariaDB, and storage components were functioning together.

## Result

The Raspberry Pi successfully functions as a self-hosted private cloud server while simultaneously operating the Samba NAS and Pi-hole DNS service.

## Skills Demonstrated

- Linux server administration
- Nextcloud
- Apache
- MariaDB
- SQL
- Web server configuration
- Database configuration
- File permissions
- NVMe storage
- Tailscale
- Remote networking
- Service troubleshooting