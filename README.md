# Homelab

A home server project on a ThinkPad T440.

## Hardware and OS

| Item | Details |
|------|---------|
| Machine | Lenovo ThinkPad T440 |
| CPU | Intel Core i5-4300U, 2 cores / 4 threads |
| RAM | 8 GB |
| Storage | ~457 GB |
| OS | Ubuntu 26.04 LTS |
| Network | Wi-Fi, rented apartment (no router admin access) |

## Constraints

- The router is managed by someone else, so there is no port forwarding or DHCP reservation.
- All work happens inside the local network.

## Progress

### Stage 1: Linux fundamentals
- [x] Baseline system documentation: [docs/01-linux-basics.md](docs/01-linux-basics.md)
- [x] SSH server and first remote login: [docs/02-ssh-setup.md](docs/02-ssh-setup.md)
- [x] SSH key authentication and firewall: [docs/03-ssh-keys-and-firewall.md](docs/03-ssh-keys-and-firewall.md)

### Stage 2: Networking
- [x] IP addresses, DHCP, DNS, ports: [docs/04-networking.md](docs/04-networking.md)
- [x] Stable address on Wi-Fi [docs/05-reliable-access.md](docs/05-reliable-access.md)
- [x] Remote access with Tailscale

### Stage 3: Services
- [ ] File server
- [ ] Small website
- [ ] Database
- [ ] Services starting automatically on boot

### Stage 4: Administration
- [ ] Backups
- [ ] Monitoring (CPU, RAM, disk)
- [ ] Log inspection
- [ ] Hardening and updates

## Repository layout

```
docs/      step-by-step write-ups for each task
configs/   copies of configuration files I edited
scripts/   scripts I wrote
images/    screenshots and diagrams
```

## Changelog

- 2026-11-15: Set up Tailscale for remote access without port forwarding.
- 2026-11-15: Fixed Wi-Fi power saving delay, disabled suspend on lid close, hostname access working.
- 2026-11-14: SSH key login and firewall set up.
- 2026-11-13: Project started. Baseline documented.


