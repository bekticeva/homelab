# SSH key authentication and firewall

## Goal
Replace password login with key-based authentication and enable a firewall, so the T440 only accepts SSH connections from machines holding my private key.

## Environment
- Server: ThinkPad T440, Ubuntu 26.04 LTS, `<hostname>`, `<server-ip>`
- Client: main laptop, Windows PowerShell with OpenSSH

## Steps

### 1. Update the system (on the T440)
```bash
sudo apt update
sudo apt full-upgrade -y
```

### 2. Generate a key pair (on the main laptop)
```powershell
ssh-keygen -t ed25519 -C "homelab"
```
This creates a private key (stays on my laptop, never shared or committed) and a public key (safe to put on servers).

### 3. Copy the public key to the server
```powershell
type $env:USERPROFILE\.ssh\id_ed25519.pub | ssh <user>@<server-ip> "mkdir -p ~/.ssh && chmod 700 ~/.ssh && cat >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
```
Windows has no `ssh-copy-id`, so I pipe the key over SSH into `authorized_keys`. The `chmod` commands set strict permissions, which SSH requires.

### 4. Test key login
```powershell
ssh <user>@<server-ip>
```
[Logged in without the server password.]

### 5. Disable password login (on the T440)
File: `/etc/ssh/sshd_config.d/01-hardening.conf` (copy in `configs/`)
```
PasswordAuthentication no
PermitRootLogin no
```
```bash
sudo sshd -t
sudo systemctl restart ssh
```
- `sshd -t` checks the config for errors before restarting.
- The `01-` prefix makes the file load first, because SSH uses the first value it finds for each option.
- I kept an existing session open and tested a new login in a second window before closing anything.

### 6. Enable the firewall
```bash
sudo ufw allow ssh
sudo ufw enable
sudo ufw status verbose
```
SSH is allowed before enabling the firewall so I don't lock myself out. Default policy: deny incoming, allow outgoing.



- A firewall should allow the access I need first, then be enabled.


## Next steps
- Understand DHCP, DNS and open ports on the server.
- Make the server's address stable on Wi-Fi.