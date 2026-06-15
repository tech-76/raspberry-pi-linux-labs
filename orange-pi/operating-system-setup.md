# Orange Pi Operating System Setup

## 1. Hardware Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Exact model | |
| Board revision | |
| SoC | |
| RAM | |
| Boot storage | microSD/eMMC/NVMe |
| Network | Ethernet/Wi-Fi |
| Power supply | |
| Cooling | |
| Display | |
| Intended purpose | |

Photograph both sides of the board and redact serial identifiers before publishing.

## 2. Select an Image

Potential sources include:

- Official vendor image.
- Ubuntu or Debian community image.
- Armbian image supporting the exact model.

Record:

- Download source.
- Image filename.
- Release date.
- Kernel version.
- Desktop/server edition.
- Checksum.
- Known limitations.

Do not assume an image for a similar board will boot safely.

## 3. Verify the Download

When a checksum is provided:

```bash
sha256sum downloaded-image.img.xz
```

Compare the complete value with the publisher's checksum.

## 4. Write the Image

Use a supported imaging utility. Confirm the target device carefully.

After writing:

- Safely eject.
- Insert into the Orange Pi.
- Connect display or serial console where appropriate.
- Connect Ethernet for the first boot when supported.
- Apply power with a suitable supply.

## 5. First-Boot Record

```bash
hostnamectl
cat /etc/os-release
uname -a
lscpu
ip -br address
ip route
lsblk -f
free -h
```

Record kernel and device-tree details:

```bash
tr -d '\0' </proc/device-tree/model 2>/dev/null; echo
dmesg | head -n 50
```

## 6. User and Hostname

Create a unique administrative user and hostname. Replace any default credentials immediately.

```bash
sudo hostnamectl set-hostname orange-pi01
```

Review `/etc/hosts` after changing the hostname.

## 7. Update Strategy

Before a major upgrade:

- Save a bootable backup image.
- Review image-provider documentation.
- Check free storage.
- Record current kernel.
- Confirm local recovery access.

Then:

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
```

Avoid forcing firmware or kernel packages from a different board family.

## 8. Network Validation

```bash
ip -br address
ip route
ping -c 4 192.168.10.1
ping -c 4 1.1.1.1
getent hosts example.com
```

For Wi-Fi:

```bash
nmcli device status 2>/dev/null
rfkill list
```

## 9. Hardware Validation

### USB

```bash
lsusb
dmesg --follow
```

### Storage

```bash
lsblk -f
df -hT
sudo dmesg | grep -iE 'mmc|nvme|sd|error'
```

### Temperature

```bash
cat /sys/class/thermal/thermal_zone0/temp 2>/dev/null
```

### Audio and display

Use distribution-supported tools and record whether HDMI, analog output, or onboard codecs are functional.

## 10. Service and Port Review

```bash
systemctl --failed
sudo ss -lntup
journalctl -p warning -b
```

## 11. Backup

Create a recovery image or file-level backup appropriate to the boot medium. Record restoration steps and test when possible.

## 12. Completion Summary

```text
Image used:
Boot result:
Working features:
Partially working features:
Unsupported features:
Updates applied:
Problems resolved:
Recovery method:
Next steps:
```
