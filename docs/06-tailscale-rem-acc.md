# Tailscale

## Goal
Access the server from anywhere, not just the local network

## Environment
- Server: ThinkPad T440, Ubuntu 26.04 LTS, Wi-Fi only, remote access via SSH (key login, password login disabled)
- Client: main laptop, Windows with PowerShell
- Network: rented apartment, router managed by someone else (no admin access, no port forwarding)
- Software: Tailscale [1.104.7], installed on both devices
- Account: Tailscale free personal plan, signed in with Google, two-factor authentication not enabled


## Steps

### 1. Create the account

### 2. Install Tailscale on the server (T440)
```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### 3. Connect the server to the tailnet
```bash
sudo tailscale up
```
prints a login URL - sign in, approve the device.

### 4. Install Tailscale on the laptop
```powershell
winget install --id Tailscale.Tailscale -e
```
Signed in with the same account, and both devices appeared as connected.

### 5. Test from the home network and from outside the home network
```powershell
ping <tailscale-ip>
ssh <user>@<tailscale-ip>
```

### 6. Check it starts on boot
```bash
systemctl is-enabled tailscaled
sudo reboot
```
After the reboot wait few minutes and reconnected over Tailscale without touching the server.

## Result
The server is reachable from anywhere using its Tailscale address, with no port forwarding or router changes. SSH still requires my key, and password login stays disabled.



## Next steps
- host a real service
