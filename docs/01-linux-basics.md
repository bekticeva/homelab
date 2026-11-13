# Baseline: ThinkPad T440 starting state

## Goal
Record the hardware and software state of the server before I change anything, so I can show what I configured later.

## System summary

| Item | Value |
|------|-------|
| Machine | Lenovo ThinkPad T440 |
| OS | Ubuntu 26.04 LTS (Resolute Raccoon) |
| Kernel | 7.0.0-27-generic |
| CPU | Intel Core i5-4300U @ 1.90 GHz, 2 cores / 4 threads |
| RAM | 7.1 GiB total (about 4.8 GiB available at idle) |
| Swap | 4.0 GiB |
| Disk | 457 GB root partition (`/dev/sda2`), 3% used |
| Boot | UEFI (`/dev/sda1` mounted at `/boot/efi`) |
| Network | Wi-Fi (`wlp3s0`), IP from DHCP |

## Commands used

```bash
cat /etc/os-release     # OS name and version
uname -r                # kernel version
free -h                 # memory and swap usage
df -h                   # disk usage per filesystem
lscpu | head -n 15      # CPU details
```

## What the output tells me
- **OS:** Ubuntu 26.04 LTS is a long-term support release, so it gets security updates for years.
- **Memory:** `free` shows "available" (about 4.8 GiB), which is more meaningful than "free", because Linux uses spare RAM for disk cache (`buff/cache`).
- **Disk:** the real storage is `/dev/sda2`. The `tmpfs` lines are in-memory filesystems, not disks.
- **CPU:** a 2-core mobile CPU from 2013 is enough for SSH, file sharing, a small website, and a database. It won't suit heavy workloads or many virtual machines.

## Problems and fixes

## Next steps
- Set up SSH key authentication.
- Update the system with `sudo apt update && sudo apt full-upgrade`.
- Enable the firewall (`ufw`).