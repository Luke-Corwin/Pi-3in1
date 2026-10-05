# Pi-hole Setup

This document covers the Pi-hole setup used for the Pi-3in1 Raspberry Pi home server.

Pi-hole is the third major service in the project alongside Samba network storage and Nextcloud private cloud storage.

## 1. Check the Network

Before installing Pi-hole, I checked the Raspberry Pi's network interfaces:

```bash
ip -br addr
```

The Raspberry Pi's private LAN connection is:

```text
Interface: wlan0
LAN IP during setup: 192.168.5.26
```

I also checked the routing table:

```bash
ip route
```

## 2. Check Port 53

Pi-hole needs DNS port 53.

Before installation, I checked whether another service was already using the port:

```bash
sudo ss -lntup | grep ':53'
```

No conflicting DNS service was found.

## 3. Update Raspberry Pi OS

The package lists and installed packages were updated before installing Pi-hole:

```bash
sudo apt update
sudo apt upgrade -y
```

## 4. Install Pi-hole

The Pi-hole installer was started with:

```bash
curl -sSL https://install.pi-hole.net | bash
```

The installation wizard was then used to configure the DNS server.

## 5. Installation Configuration

The following options were selected during setup:

```text
Upstream DNS: Cloudflare
Query Logging: Enabled
Privacy Mode: Show Everything
```

Cloudflare acts as the upstream DNS resolver for requests that are not blocked by Pi-hole.

The DNS flow is:

```text
Client
   |
   v
Pi-hole
   |
   +---- Blocked domain -> Blocked
   |
   +---- Allowed domain -> Cloudflare -> Internet
```

## 6. Configure the Correct Network Interface

Pi-hole was initially configured while testing Ethernet.

Further network testing showed that the Ethernet connection was connected to a different upstream network instead of the private mesh LAN.

The network configuration was investigated using:

```bash
ip -br addr
```

```bash
ip route
```

```bash
ip route get 1.1.1.1
```

The private home network was accessible through:

```text
wlan0
```

Pi-hole was therefore configured to use `wlan0`:

```bash
sudo pihole-FTL --config dns.interface wlan0
```

Pi-hole was restarted to apply the change:

```bash
sudo systemctl restart pihole-FTL
```

## 7. Verify Pi-hole

Pi-hole's status was checked with:

```bash
sudo pihole status
```

The successful status check confirmed:

```text
FTL is listening on port 53
UDP IPv4 enabled
TCP IPv4 enabled
UDP IPv6 enabled
TCP IPv6 enabled
Pi-hole blocking enabled
```

## 8. Test Local DNS

Pi-hole was first tested directly from the Raspberry Pi:

```bash
dig @127.0.0.1 google.com
```

The request returned a valid DNS response.

This confirmed that the Pi-hole DNS service was functioning locally.

## 9. Test DNS Through the LAN

Pi-hole was then tested using the Raspberry Pi's LAN address:

```bash
dig @192.168.5.26 google.com
```

The request successfully returned DNS records.

The response identified the Raspberry Pi as the DNS server:

```text
SERVER: 192.168.5.26#53
```

This confirmed that Pi-hole was listening for DNS requests through the Raspberry Pi's network interface.

## 10. Monitor DNS Queries

Pi-hole queries can be monitored in real time with:

```bash
sudo pihole -t
```

During testing, the log displayed DNS requests being processed, cached, and forwarded to Cloudflare.

Press:

```text
Ctrl+C
```

to exit the live log.

## Final Configuration

The completed Pi-hole configuration is:

```text
Device: Raspberry Pi 5
Interface: wlan0
LAN IP during testing: 192.168.5.26
DNS Port: 53
Upstream DNS: Cloudflare
Query Logging: Enabled
Blocking: Enabled
```

Pi-hole now operates alongside the other Pi-3in1 services:

```text
Raspberry Pi 5
|
+-- Samba       -> Network Attached Storage
|
+-- Nextcloud   -> Private Cloud
|
+-- Pi-hole     -> DNS Filtering
```

## Future Improvement

The next major improvement would be configuring the home router or mesh system to advertise the Raspberry Pi as the DNS server automatically.

This would allow supported devices across the local network to use Pi-hole without configuring DNS individually on each client.