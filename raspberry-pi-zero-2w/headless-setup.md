# Headless Setup

## 1. Deployment Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Device | Raspberry Pi Zero 2 W |
| OS | Raspberry Pi OS Lite |
| Storage | |
| Hostname | `pi-zero01` |
| Wi-Fi network | Redacted |
| IP plan | DHCP reservation |
| Admin workstation | |
| Power supply | |
| Intended role | |

## 2. Image the Device

Using Raspberry Pi Imager:

- Select Raspberry Pi Zero 2 W.
- Select a supported Lite image.
- Choose the correct storage target.
- Set a unique hostname.
- Create a non-default user.
- Configure the correct Wi-Fi SSID, security, and country.
- Enable SSH.
- Prefer public-key authentication where supported.
- Write and verify the image.

Do not place reusable credentials in repository files.

## 3. First Boot

Allow sufficient time for first boot. Discover the device through:

- Router DHCP lease list.
- Local DNS or mDNS, if available.
- A controlled scan of your own subnet.
- Serial console, if configured and understood.

Example:

```bash
ping pi-zero01.local
ssh labadmin@pi-zero01.local
```

If mDNS is unavailable, use the assigned private address.

## 4. Baseline Validation

```bash
hostnamectl
cat /etc/os-release
ip -br address
ip route
getent hosts example.com
timedatectl
df -hT
free -h
systemctl --failed
```

## 5. Update Carefully

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove --purge -y
sudo reboot
```

Confirm free space before large upgrades.

## 6. Secure SSH

- Verify key-based login.
- Protect the private key.
- Disable root login.
- Disable password login only after key access works.
- Restrict SSH to trusted networks when practical.
- Keep a local recovery method.

## 7. Resource Baseline

```bash
uptime
free -h
df -h
top
cat /sys/class/thermal/thermal_zone0/temp 2>/dev/null
```

Record idle and working-state values.

## 8. Reboot Validation

```bash
sudo reboot
```

Confirm:

- Device rejoins Wi-Fi.
- Reserved address remains correct.
- SSH is reachable.
- Required service starts.
- Time synchronizes.
- No failed services appear.

## 9. Recovery Plan

If network access fails:

1. Check power and activity indicators.
2. Review router DHCP leases.
3. Test another access point or 2.4 GHz network.
4. Use a serial console if previously configured.
5. Mount the storage on an admin computer only when necessary.
6. Back up files before editing offline.
7. Restore from a known-good image if configuration cannot be recovered.

## 10. Evidence

Add:

- Hardware photo.
- Sanitized imager settings.
- First successful SSH session.
- System baseline.
- Reboot validation.
- Resource usage.
