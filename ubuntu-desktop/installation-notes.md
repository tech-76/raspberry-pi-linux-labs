# Ubuntu Desktop Installation Notes

## 1. Installation Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Raspberry Pi model | |
| RAM | |
| Storage | |
| Display | |
| Keyboard/mouse | |
| Network | Ethernet/Wi-Fi |
| OS image | Ubuntu Desktop release |
| Imaging tool | |
| Intended purpose | |

## 2. Hardware Preparation

- Use a supported Raspberry Pi model.
- Use a reliable power supply.
- Use storage with sufficient capacity and acceptable performance.
- Verify cooling requirements for sustained workloads.
- Connect display and peripherals before first boot when practical.
- Confirm the selected image supports the hardware.

## 3. Imaging

1. Open the supported imaging tool.
2. Select the correct Ubuntu Desktop image.
3. Select the correct target storage device.
4. Review advanced settings.
5. Write and verify the image.
6. Safely eject the storage.
7. Insert it into the Raspberry Pi and boot.

Double-check the target device to avoid overwriting another disk.

## 4. First-Boot Configuration

Complete:

- Language and keyboard.
- Timezone.
- User account.
- Device name.
- Network.
- Privacy settings.
- Display resolution and scaling.
- Update prompts.

Record only non-sensitive information.

## 5. System Update

Open a terminal:

```bash
sudo apt update
sudo apt full-upgrade -y
sudo apt autoremove --purge -y
sudo reboot
```

After reboot:

```bash
hostnamectl
cat /etc/os-release
systemctl --failed
journalctl -p warning -b
```

## 6. Hardware Validation

### Display

```bash
xrandr 2>/dev/null
```

Check:

- Native resolution.
- Scaling.
- Refresh rate.
- Overscan or cropped edges.
- HDMI reconnect behavior.

### Input Devices

```bash
lsusb
```

Test keyboard shortcuts, pointer behavior, and USB reconnection.

### Audio

```bash
pactl info 2>/dev/null
aplay -l 2>/dev/null
```

Confirm correct output device in desktop settings.

### Network

```bash
ip -br address
ip route
resolvectl status
ping -c 4 192.168.10.1
getent hosts example.com
```

### Storage

```bash
lsblk -f
df -hT
```

## 7. Application Baseline

Record installed applications and why they are needed. Example categories:

- Browser.
- Text editor.
- Terminal.
- Remote support tool.
- Office/productivity.
- Screenshot utility.
- Monitoring utility.

Avoid installing unnecessary software.

## 8. User Support Baseline

Test common user tasks:

- Log in and log out.
- Connect to Wi-Fi.
- Open a browser.
- Download and locate a file.
- Connect a USB device.
- Adjust display settings.
- Adjust audio.
- Install updates.
- Restart and shut down correctly.

## 9. Completion Checklist

- [ ] Installation details recorded.
- [ ] System updated.
- [ ] Display validated.
- [ ] Input devices validated.
- [ ] Network and DNS validated.
- [ ] Audio validated where required.
- [ ] Storage reviewed.
- [ ] Applications documented.
- [ ] Shutdown and reboot tested.
- [ ] Evidence sanitized.
