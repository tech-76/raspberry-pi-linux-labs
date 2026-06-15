# Orange Pi Compatibility Notes

## Purpose

Track the exact hardware and software combinations tested. Compatibility claims should be based on observed results, not assumptions.

## Test Matrix

| Component | Status | Evidence | Notes |
|---|---|---|---|
| Boot from microSD | Not tested / Pass / Partial / Fail | | |
| Boot from eMMC | | | |
| Boot from NVMe | | | |
| HDMI display | | | |
| Ethernet | | | |
| Wi-Fi | | | |
| Bluetooth | | | |
| USB 2.0 | | | |
| USB 3.x | | | |
| Audio | | | |
| GPIO | | | |
| Camera | | | |
| Hardware video decode | | | |
| Suspend/resume | | | |
| Thermal sensors | | | |
| Fan control | | | |

## Software Matrix

| Image | Release | Kernel | Desktop | Network | Notes |
|---|---|---|---|---|---|
| Vendor image | | | | | |
| Armbian | | | | | |
| Ubuntu/Debian image | | | | | |

## Diagnostic Commands

```bash
hostnamectl
cat /etc/os-release
uname -a
lscpu
lsusb
lspci 2>/dev/null
lsblk -f
ip -br address
rfkill list
systemctl --failed
journalctl -p warning -b
dmesg --level=err,warn
```

## Issue Record

```text
Issue ID:
Hardware model/revision:
Image:
Kernel:
Component:
Symptom:
Reproduction steps:
Expected result:
Actual result:
Logs:
Workaround:
Permanent fix:
Retest:
Status:
```

## Common Compatibility Areas

### Boot Media

- Some images support microSD but require extra steps for eMMC or NVMe.
- Bootloader changes can make recovery more difficult.
- Keep known-good media available.

### Wi-Fi and Bluetooth

- Confirm the onboard chipset.
- Check whether firmware is loaded.
- Review radio blocks and regulatory settings.
- Test reconnect after reboot.

### Display

- Test native resolution.
- Test cold boot and warm reboot.
- Record whether HDMI audio works.
- Review device-tree overlays only from trusted documentation.

### GPIO

Pin layouts and voltage limits can differ from Raspberry Pi. Never assume header compatibility. Verify the exact board schematic and pinout before connecting hardware.

### Power and Thermals

- Use a supply that meets the board specification.
- Record idle and load temperature.
- Check for throttling or instability.
- Use cooling appropriate to the enclosure and workload.

## Comparison Summary

```text
Most stable image:
Best server image:
Best desktop image:
Working network interfaces:
Known hardware limitations:
Recommended use case:
Not recommended for:
```
