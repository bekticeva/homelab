# Reliable access on Wi-Fi

## Goal
Keep the server reachable even though it runs on Wi-Fi, the router is not under my control, and the IP address is assigned by DHCP and can change.

## Constraints
- Rented apartment: no router admin access, so no DHCP reservation or port forwarding.
- The server is Wi-Fi only for now.
- The server runs with the lid closed.




## 1. Hostname access

### Problem
The server's IP is dynamic, so a saved address could stop working after a lease change.

### Solution
`avahi-daemon` was already running (seen listening on UDP 5353 in `ss -tulpn`), which provides mDNS names ending in `.local`.

```powershell
ping <hostname>.local
ssh <user>@<hostname>.local
```




## 2. Lid close behavior

### Problem
By default, closing the lid suspends the laptop and would take the server offline.

### Fix
Edited the systemd-logind configuration and set three options to `ignore`:
```
HandleLidSwitch=ignore
HandleLidSwitchExternalPower=ignore
HandleLidSwitchDocked=ignore
```
command:
sudo nano /etc/systemd/logind.conf

[configs\02-lid.conf](configs\02-lid.conf)




## 3. Wi-Fi power saving

### Problem
The first ping after the connection had been idle was very slow:

| Test | First reply | Average |
|------|-------------|---------|
| Before (idle) | 489 ms | 124 ms |
| After, right after reconnect | 2.20 ms | 2.69 ms |
| After, 10 min idle | 2.20 ms | 2.69 ms |

This suggested the Wi-Fi radio was sleeping and had to wake up for the first packet.

### Fix
```bash
sudo nmcli connection modify <wifi-name> 802-11-wireless.powersave 2
sudo nmcli connection up <wifi-name>
```
- `powersave 2` means "disable".





## What I learned
- [Why a DHCP address can change, and how mDNS avoids depending on it.]
- [What systemd-logind does and why a laptop server needs the lid setting.]
- [How to find a latency problem by comparing first and later ping replies.]

## Next steps
- Remote access from outside the home network with Tailscale.