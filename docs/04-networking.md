# Networking basics on the T440

## Goal
Understand how the server is connected to the network: addressing, DHCP, DNS, routing and open ports, and learn the basic troubleshooting flow.

## Network overview

| Item | Value |
|------|-------|
| Local subnet | 192.168.0.0/24 |
| Server address | 192.168.0.81 (DHCP, dynamic) |
| Default gateway | 192.168.0.1 (router) |
| DNS server | 192.168.0.1 (router) |
| Interface | `wlp3s0` (Wi-Fi, 5 GHz network) |
| Ethernet | `enp0s25`, no cable connected |
| IPv6 | Provided by ISP (global addresses omitted here) |

## Concepts 
- **IP address / subnet:** unique address of my device on the internet. Subnet bits define the network - the rest defines the device.
                            Meaning all devices in 192.168.0.0/24 share the first 24 bits and can talk to eachother directly.
                            All communication beyond that goes through the router.
- **Default gateway:** is the addres of the router which just forwards packets.
- **DHCP:** The router automatically assigns addresses to devices that join the network. These addresses are leased, meaning they are temporary and might change.
- **DNS:** system that resolves names / ip addresses
- **Ports:** A port identifies a specific service on a device. An IP address gets traffic to the machine, and the port gets it to the right program. 
            SSH listens on port 22, and the firewall blocks everything else coming in.

## Commands and findings

### Addressing and routing
```bash
ip a
ip route
nmcli device show wlp3s0
```
The server has a DHCP address with a lease of about 24 hours. The routing table has a local route for the subnet and a default route via the router.

### DNS
```bash
resolvectl status
resolvectl query ubuntu.com
dig ubuntu.com
```
Name resolution goes through the local stub resolver (`127.0.0.53`), which forwards to the router. `dig` returned the A records in about 0 ms (cached).

### Connectivity tests
```bash
ping -c 4 <gateway>
ping -c 4 1.1.1.1
ping -c 4 ubuntu.com
traceroute ubuntu.com
```
All pings had 0% loss. Gateway tests the local link, `1.1.1.1` tests internet routing, and `ubuntu.com` tests DNS plus routing. If they fail in that order, I know which layer is broken.

### Listening ports and firewall
```bash
sudo ss -tulpn
sudo ufw status verbose
```
Reachable from the network: SSH on port 22 only. Other services (DNS stub, chrony, CUPS) listen on localhost only. The firewall denies all other incoming traffic.

### Other devices
```bash
ip neigh
```
Shows devices the server has talked to recently: the router and my main laptop.

## Problems and fixes
- The first ping after idle took about 490 ms, then about 2 ms. Likely Wi-Fi power saving. 


## Next steps
- Reach the server by hostname
- Set a stable address on the server itself, since I can't reserve one on the router.
- Install Tailscale for remote access without port forwarding.
- Draw a network diagram.