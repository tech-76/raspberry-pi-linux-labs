# Ubuntu Server Setup

## 1. Deployment Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Hardware | Raspberry Pi model |
| Storage | Type and capacity |
| Power supply | Rating/model |
| Network | Ethernet or Wi-Fi |
| OS image | Ubuntu Server release |
| Hostname | `ubuntu-pi01` |
| Administrator | Non-default user |
| Intended role | Linux administration lab |

## 2. Image and First Boot

Use the supported imaging tool and verify the selected device before writing the image.

Recommended first-boot tasks:

```bash
hostnamectl
cat /etc/os-release
uname -a
ip -br address
ip route
```

Change the hostname if required:

```bash
sudo hostnamectl set-hostname ubuntu-pi01
```

Verify `/etc/hosts` contains a suitable local hostname mapping.

## 3. Update Packages

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove --purge -y
sudo reboot
```

After reboot:

```bash
uptime
systemctl --failed
journalctl -p warning -b
```

Review warnings rather than deleting or hiding them.

## 4. Time, Locale, and Synchronization

```bash
timedatectl
locale
```

Set the timezone as required:

```bash
sudo timedatectl set-timezone America/Toronto
```

Validate time synchronization:

```bash
timedatectl status
```

Accurate time is important for logs, certificates, package management, and authentication.

## 5. User and Privilege Review

```bash
id
groups
getent group sudo
```

Create a separate administrative account when appropriate:

```bash
sudo adduser labadmin
sudo usermod -aG sudo labadmin
```

Verify access before disabling or changing any existing account. Do not use the root account for routine work.

## 6. Baseline Packages

Install only what is needed:

```bash
sudo apt install -y curl wget git vim htop tree dnsutils net-tools lsof
```

Optional utilities should be justified in the lab notes.

## 7. Firewall Baseline

Check current status:

```bash
sudo ufw status verbose
```

A simple SSH-only baseline may be:

```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status numbered
```

Do not enable a firewall remotely until the required management rule is confirmed.

## 8. Storage Baseline

```bash
lsblk -f
df -hT
sudo du -xhd1 / 2>/dev/null | sort -h
```

Record:

- Root filesystem type.
- Total and available capacity.
- Boot filesystem.
- External storage.
- Mount points.
- Any warnings from kernel or journal logs.

## 9. Memory and CPU Baseline

```bash
free -h
lscpu
uptime
top
```

Temperature may be available through platform-specific tools or:

```bash
cat /sys/class/thermal/thermal_zone0/temp
```

Divide the raw value by 1000 when the interface reports millidegrees Celsius.

## 10. Service Baseline

```bash
systemctl --type=service --state=running
systemctl --failed
sudo ss -lntup
```

Document every intentionally exposed listening service.

## 11. Log Review

```bash
journalctl -b
journalctl -p warning -b
dmesg --level=err,warn
```

Sanitize logs before publishing.

## 12. Backup Plan

At minimum, record:

- Configuration files to back up.
- User data paths.
- Backup destination.
- Backup frequency.
- Restore procedure.
- Last restore test.

A backup is not proven until a restore test succeeds.

## 13. Completion Checklist

- [ ] Hostname configured.
- [ ] Timezone and clock correct.
- [ ] Packages updated.
- [ ] Administrative user verified.
- [ ] SSH secured.
- [ ] Stable network configuration validated.
- [ ] Firewall policy documented.
- [ ] Listening services reviewed.
- [ ] Storage and memory baseline captured.
- [ ] Backup plan documented.
- [ ] Sensitive evidence sanitized.
