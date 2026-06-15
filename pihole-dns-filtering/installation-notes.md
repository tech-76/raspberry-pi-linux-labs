# Pi-hole Installation Notes

## 1. Pre-Installation Record

Document the environment before making changes.

| Item | Record |
|---|---|
| Date | `YYYY-MM-DD` |
| Device model | Raspberry Pi model |
| Storage | microSD or SSD model/capacity |
| OS image | Distribution and release |
| Hostname | `pihole01` |
| MAC address | Redact before publishing |
| IP assignment | DHCP reservation or static |
| IP address | `192.168.10.10/24` |
| Gateway | `192.168.10.1` |
| Admin workstation | Operating system |
| Backup location | Secure local path |

## 2. Prepare the Operating System

Use Raspberry Pi Imager or the vendor-supported imaging tool.

Recommended options:

- Set a unique hostname.
- Create a non-default administrative user.
- Enable SSH only when needed.
- Configure Wi-Fi securely if Ethernet is unavailable.
- Select the correct keyboard, locale, and timezone.
- Avoid embedding reusable passwords in screenshots or notes.

After first boot:

```bash
hostnamectl
ip address
ip route
cat /etc/os-release
```

## 3. Update the System

```bash
sudo apt update
sudo apt full-upgrade -y
sudo reboot
```

After reboot:

```bash
uptime
uname -a
systemctl --failed
```

## 4. Configure a Stable Address

A DHCP reservation on the router is usually easier to maintain than configuring a static address independently on the host. Record the reservation and verify it after reboot.

Validation:

```bash
ip -br address
ip route
ping -c 4 192.168.10.1
```

Do not create an address conflict. Confirm that the chosen IP is outside the dynamic DHCP pool or formally reserved.

## 5. Install Pi-hole

Use the installation instructions from the official Pi-hole project. Review the script and documentation before running it.

During installation, record:

- Network interface selected.
- IPv4 address and gateway.
- Upstream DNS provider.
- Blocklist choice.
- Web interface selection.
- Query logging and privacy level.
- Any warning or error messages.

After installation, save the administrative URL securely and change the generated password when appropriate.

## 6. Verify Services

```bash
sudo systemctl status pihole-FTL --no-pager
sudo ss -lntup | grep -E ':(53|80|443)\b'
pihole status
```

Expected result:

- DNS service is active.
- Port 53 is listening on the intended interface.
- The web service is reachable from the trusted LAN.
- No unexpected failed services are present.

## 7. Configure a Test Client

Before changing router-wide DNS, configure one client manually.

Record:

| Setting | Example |
|---|---|
| Client name | `test-laptop` |
| Client address | `192.168.10.50` |
| Primary DNS | `192.168.10.10` |
| Secondary DNS | Leave blank during testing, or use a documented resilient design |
| Test date | `YYYY-MM-DD` |

Flush the client DNS cache where applicable, reconnect, and validate:

```bash
nslookup example.com 192.168.10.10
```

Linux systems may also use:

```bash
dig @192.168.10.10 example.com
resolvectl status
```

## 8. Router-Level DNS

Only proceed after the test client works correctly.

Typical options:

- Advertise Pi-hole through the router's DHCP DNS option.
- Use Pi-hole DHCP only when the existing router cannot provide the required DNS configuration.
- Avoid running two DHCP servers on the same broadcast domain.

After renewing a client lease, verify that Pi-hole is the assigned DNS server.

## 9. Functional Tests

Test more than one category:

- Normal web browsing.
- Software updates.
- Streaming or smart devices.
- Microsoft 365 or other business services.
- Internal hostnames, if used.
- VPN connectivity.
- Guest network behavior.
- Devices that use hard-coded DNS.

Record any required allowlist entries and the reason for each exception.

## 10. Backup

Export a Pi-hole configuration backup using the supported interface or command documented by the project. Store it securely and record:

- Export date.
- Pi-hole version.
- Host operating system.
- Backup location.
- Restore test status.

Do not publish backup archives containing local configuration or identifiable query data.

## 11. Completion Record

```text
Installation completed:
Primary objective achieved:
Issues encountered:
Changes made:
Validation performed:
Evidence captured:
Next maintenance date:
```
