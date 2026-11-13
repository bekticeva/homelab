# Setting up SSH on the ThinkPad T440

## Goal
Enable remote administration of the T440 from my main laptop over the local Wi-Fi network, so the T440 can run as a headless server.

## Environment
- Server: Lenovo ThinkPad T440, Ubuntu 26.04 LTS (codename "resolute"), hostname `<hostname>`
- Client: main laptop, Windows PowerShell
- Network: home Wi-Fi, router managed by landlord (no router admin access)
- Server interface: `wlp3s0` (Wi-Fi), IP assigned by DHCP (`<server-ip>/24`)

## Steps

### 1. Install and enable the SSH server (on the T440)
```bash
sudo apt install -y openssh-server
sudo systemctl enable ssh
sudo systemctl status ssh
```
- `apt install` installs the OpenSSH server package.
- `systemctl enable ssh` makes the service start automatically on boot.
- `systemctl status ssh` checks that it is `active (running)`.

### 2. Find the server's IP address
```bash
ip a
```
The Wi-Fi interface `wlp3s0` showed an `inet` address of `<server-ip>/24`, assigned dynamically by DHCP (`scope global dynamic`).

### 3. Connect from the main laptop
```powershell
ssh <user>@<server-ip>
```
On the first connection SSH showed the host key fingerprint (ED25519) and asked whether to trust it. I typed `yes`, which saved the key in `known_hosts`. Later connections skip this prompt.

## Result
[Connected successfully and got a shell prompt on the T440.]


## Problems and fixes

## Next steps
- Set up SSH key authentication and disable password login.
- Reserve or fix the server's IP, or use a hostname.
- Add a firewall rule with `ufw`.