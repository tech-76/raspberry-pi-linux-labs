# Wireless Configuration

## Objective

Configure reliable, secure Wi-Fi for a headless Raspberry Pi Zero 2 W and document how to validate and recover the connection.

## 1. Wireless Baseline

```bash
ip -br link
ip -br address
iw dev 2>/dev/null
rfkill list
```

Depending on the operating system, Wi-Fi may be managed by NetworkManager, `wpa_supplicant`, or another supported service. Identify the active manager before editing files.

## 2. NetworkManager Method

Check:

```bash
nmcli general status
nmcli device status
nmcli connection show
```

List visible networks:

```bash
nmcli device wifi list
```

Connect interactively or using a protected method. Avoid placing the passphrase in shell history or committed scripts.

Validate:

```bash
nmcli connection show --active
ip -br address
ip route
getent hosts example.com
```

## 3. Raspberry Pi Imager Method

For a fresh build, configure:

- SSID.
- Security type.
- Passphrase.
- Wireless country.
- Hostname.
- User.
- SSH method.

This is generally safer and more repeatable than manually editing boot files without confirming current operating-system behavior.

## 4. DHCP Reservation

Create a reservation on the router using the device's wireless MAC address. Do not publish the full MAC address unless needed.

Validate after reboot:

```bash
ip -br address
ip route
```

## 5. Signal and Link Quality

```bash
iw dev wlan0 link 2>/dev/null
```

Record:

- Frequency.
- Signal.
- Bitrate.
- Access point.
- Reconnect behavior.

Do not publish the access point MAC address or exact location unless appropriate.

## 6. Connectivity Test Order

```bash
ping -c 4 192.168.10.1
ping -c 4 1.1.1.1
getent hosts example.com
```

Interpretation:

- Gateway failure: local Wi-Fi, DHCP, or VLAN issue.
- External IP failure: gateway or upstream issue.
- DNS failure only: resolver configuration issue.

## 7. Common Problems

### Wireless interface blocked

```bash
rfkill list
sudo rfkill unblock wifi
```

Investigate why it was blocked before automating a workaround.

### Wrong country or channel compatibility

Confirm the correct regulatory country and access-point channel.

### Connects but loses access after reboot

Review:

```bash
nmcli connection show
journalctl -u NetworkManager -b 2>/dev/null
journalctl -u wpa_supplicant -b 2>/dev/null
```

### Works near the router only

Investigate:

- Enclosure placement.
- Interference.
- Access-point band and channel.
- Power quality.
- Distance and obstacles.
- 2.4 GHz coverage.

### DNS only fails

```bash
resolvectl status 2>/dev/null
cat /etc/resolv.conf
```

## 8. Security Checklist

- [ ] WPA2 or WPA3 used.
- [ ] Default router credentials changed.
- [ ] Passphrase not committed to Git.
- [ ] Guest or IoT segmentation considered.
- [ ] SSH restricted to trusted sources.
- [ ] Device patched.
- [ ] Unused services disabled.
- [ ] Recovery path documented.
