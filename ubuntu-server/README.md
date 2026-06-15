# Ubuntu Server Lab

## Overview

This lab documents the installation and administration of Ubuntu Server on Raspberry Pi hardware. It emphasizes repeatable deployment, secure remote access, network configuration, maintenance, service management, logging, and recovery.

## Learning Objectives

- Install a server operating system on ARM hardware.
- Configure hostname, users, timezone, storage, and network access.
- Administer the system through SSH.
- Apply least-privilege and patching practices.
- Validate network, DNS, storage, memory, and services.
- Maintain a structured operational checklist.
- Troubleshoot issues using logs and command-line tools.

## Suggested Environment

| Item | Example |
|---|---|
| Hardware | Raspberry Pi 4 |
| OS | Ubuntu Server |
| Hostname | `ubuntu-pi01` |
| IP address | `192.168.10.20/24` |
| Gateway | `192.168.10.1` |
| Access | Ethernet and SSH |
| Role | General Linux administration lab |

## Lab Workflow

1. Image the storage device.
2. Complete first boot and account setup.
3. Update packages and firmware where supported.
4. Configure hostname and time.
5. Configure stable networking.
6. Secure SSH.
7. Install only required services.
8. Validate logs, storage, memory, and network.
9. Create a backup and recovery record.
10. Capture sanitized evidence.

## Core Validation Commands

```bash
hostnamectl
cat /etc/os-release
ip -br address
ip route
resolvectl status
timedatectl
lsblk
df -h
free -h
systemctl --failed
journalctl -p warning -b
```

## Included Documents

- [`server-setup.md`](server-setup.md)
- [`ssh-configuration.md`](ssh-configuration.md)
- [`network-configuration.md`](network-configuration.md)
- [`maintenance-checklist.md`](maintenance-checklist.md)

## Completion Evidence

Add:

- First-boot system information.
- Network configuration validation.
- Successful SSH key login.
- Service status output.
- Disk and memory baseline.
- Maintenance checklist with a real date.
- One troubleshooting example.
