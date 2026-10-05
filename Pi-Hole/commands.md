# Pi-hole Commands

Useful commands used to configure, test, and troubleshoot Pi-hole on the Pi-3in1 server.

## Check Pi-hole Status

```bash
sudo pihole status
```

Confirms that Pi-hole FTL is running, listening for DNS requests, and that blocking is enabled.

## Restart Pi-hole

```bash
sudo systemctl restart pihole-FTL
```

Restarts the Pi-hole FTL service after configuration changes.

## View Live DNS Queries

```bash
sudo pihole -t
```

Displays DNS requests being processed by Pi-hole in real time.

Press `Ctrl+C` to exit.

## Test Pi-hole Locally

```bash
dig @127.0.0.1 google.com
```

Tests whether Pi-hole can resolve a DNS request directly on the Raspberry Pi.

## Test Pi-hole Through the LAN

```bash
dig @192.168.5.26 google.com
```

Tests whether Pi-hole responds through the Raspberry Pi's LAN address.

A successful response should identify the Raspberry Pi as the DNS server:

```text
SERVER: 192.168.5.26#53
```

## Test Using nslookup

```bash
nslookup google.com 192.168.5.26
```

Another way to verify that the Raspberry Pi is responding to DNS requests.

## View Network Interfaces

```bash
ip -br addr
```

Displays the Raspberry Pi's network interfaces and their IP addresses.

The Pi-3in1 server currently uses:

```text
wlan0
```

for its private LAN connection.

## View Routing Table

```bash
ip route
```

Displays the Raspberry Pi's current network routes and default gateway.

## Check Which Route Is Being Used

```bash
ip route get 1.1.1.1
```

Shows which interface and gateway the Raspberry Pi would use to reach the internet.

This command was useful when troubleshooting the Ethernet and Wi-Fi connections.

## Configure Pi-hole Interface

```bash
sudo pihole-FTL --config dns.interface wlan0
```

Configures Pi-hole to use the `wlan0` network interface.

## Check DNS Ports

```bash
sudo ss -lntup | grep ':53'
```

Shows services listening on DNS port 53.

Pi-hole FTL should appear in the output.

## Check Pi-hole Service

```bash
sudo systemctl status pihole-FTL
```

Displays detailed information about the Pi-hole FTL service.

## Raspberry Pi IP Addresses

```bash
hostname -I
```

Displays the IP addresses currently assigned to the Raspberry Pi.