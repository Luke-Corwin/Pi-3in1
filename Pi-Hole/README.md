# Pi-hole

Pi-hole is the DNS filtering portion of my Raspberry Pi 3-in-1 Home Server.

The goal was to add DNS-level filtering to the same Raspberry Pi already operating as a NAS and private cloud server.

## How It Works

Instead of sending DNS requests directly to an external DNS provider, a client can send them to Pi-hole first.

```text
                Client
                   |
                   v
                Pi-hole
                /     \
               /       \
        Blocked         Allowed
           |               |
         Block         Cloudflare
                           |
                        Internet
```

## Configuration

Pi-hole was configured with:

```text
Network Interface: wlan0
DNS Port: 53
Upstream DNS: Cloudflare
Query Logging: Enabled
```

The Pi-hole service can be checked using:

```bash
sudo pihole status
```

A successful status check showed that Pi-hole FTL was listening on port 53 for both IPv4 and IPv6 DNS traffic and that blocking was enabled.

## Network Configuration

The Raspberry Pi currently reaches the private home network through:

```text
wlan0
```

with the LAN address used during testing:

```text
192.168.5.26
```

During installation, Pi-hole was initially configured for `eth0`.

I later connected the Raspberry Pi to an existing Ethernet switch and discovered that Ethernet was receiving an address from a different upstream network rather than the private mesh LAN.

I diagnosed the network using:

```bash
ip -br addr
```

```bash
ip route
```

and:

```bash
ip route get 1.1.1.1
```

The Raspberry Pi was returned to the private Wi-Fi network and Pi-hole was changed to the correct interface:

```bash
sudo pihole-FTL --config dns.interface wlan0
```

Pi-hole was then restarted:

```bash
sudo systemctl restart pihole-FTL
```

## DNS Testing

I first tested Pi-hole locally using:

```bash
dig @127.0.0.1 google.com
```

Pi-hole successfully returned a valid DNS response.

I then tested through the Raspberry Pi's LAN address:

```bash
dig @192.168.5.26 google.com
```

The response included:

```text
SERVER: 192.168.5.26#53
```

This confirmed that Pi-hole was successfully answering DNS requests through port 53 on the local network.

## Query Monitoring

Pi-hole DNS activity can be monitored live using:

```bash
sudo pihole -t
```

During testing, the log displayed DNS queries, cached results, and requests forwarded to the configured upstream DNS provider.


## Result

Pi-hole successfully runs alongside Samba, Nextcloud, Apache, MariaDB, and Tailscale on the Raspberry Pi 5.

This completed the three primary services of the Pi-3in1 project:

```text
Pi-3in1
|
+-- Samba       -> Network Attached Storage
|
+-- Nextcloud   -> Private Cloud
|
+-- Pi-hole     -> DNS Filtering
```

## Skills Demonstrated

- DNS
- Pi-hole
- TCP/IP networking
- Network interfaces
- Network routing
- IPv4 and IPv6
- Linux service management
- DNS troubleshooting
- Network topology troubleshooting
- Multi-service Linux server configuration