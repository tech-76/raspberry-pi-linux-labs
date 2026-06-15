# Ubuntu Desktop Troubleshooting

## General Process

1. Reproduce the issue.
2. Identify whether it affects one user, one application, or the whole system.
3. Record the exact error.
4. Review recent changes.
5. Check resources and logs.
6. Test the smallest safe correction.
7. Validate the user workflow.
8. Document the outcome.

## Desktop Does Not Load

Switch to a text console if available and authorized:

```text
Ctrl + Alt + F3
```

Then check:

```bash
systemctl --failed
systemctl status display-manager --no-pager
journalctl -b -p warning
df -h
free -h
```

Possible causes:

- Full filesystem.
- Failed display manager.
- Incomplete update.
- Unsupported display configuration.
- Damaged user profile.
- Power or storage instability.

## Black Screen or Incorrect Resolution

```bash
xrandr 2>/dev/null
journalctl -b | grep -iE 'drm|hdmi|display|gpu'
```

Check:

- HDMI cable and port.
- Display input selection.
- Power cycle order.
- Supported resolution.
- Scaling.
- Firmware or kernel updates.
- Alternate monitor.

Do not edit boot configuration without creating a backup.

## Wi-Fi Does Not Connect

```bash
nmcli device status
nmcli device wifi list
ip -br address
rfkill list
journalctl -u NetworkManager --since "20 minutes ago"
```

Check:

- Correct SSID and passphrase.
- Regulatory country.
- Signal strength.
- Airplane mode or radio block.
- DHCP lease.
- Time and certificate issues.
- 2.4 GHz versus 5 GHz compatibility.

Never publish Wi-Fi credentials.

## Internet Works by IP but Not by Name

```bash
ping -c 4 1.1.1.1
getent hosts example.com
resolvectl status
resolvectl query example.com
```

This points to DNS rather than general connectivity.

## Package Manager Is Locked

Identify the process:

```bash
ps aux | grep -E 'apt|dpkg'
sudo lsof /var/lib/dpkg/lock-frontend 2>/dev/null
```

Do not delete lock files blindly. Allow legitimate updates to finish. If an operation was interrupted, repair carefully:

```bash
sudo dpkg --configure -a
sudo apt --fix-broken install
sudo apt update
```

## Application Will Not Launch

Run it from a terminal to capture errors:

```bash
application-name
```

Also check:

```bash
journalctl --user --since "20 minutes ago"
df -h
free -h
```

Possible causes include missing dependencies, profile corruption, permissions, display issues, and unsupported architecture.

## No Audio

```bash
pactl info 2>/dev/null
pactl list short sinks 2>/dev/null
aplay -l 2>/dev/null
```

Check desktop sound settings, selected output, mute state, HDMI routing, and application-specific output.

## USB Device Not Detected

```bash
lsusb
dmesg --follow
```

Reconnect the device and observe new kernel messages. Test another port, cable, power source, or computer.

## Slow Performance

```bash
top
free -h
df -h
uptime
```

Investigate:

- Thermal throttling.
- Slow or failing storage.
- Memory pressure.
- Browser tabs and extensions.
- Background updates.
- Desktop effects.
- Insufficient power.

## Troubleshooting Record

```text
Date:
User impact:
Exact symptom:
Steps to reproduce:
Recent changes:
Diagnostics:
Root cause:
Resolution:
Validation:
Prevention:
```
