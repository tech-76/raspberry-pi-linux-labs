# Ubuntu Server Network Configuration

## Objective

Configure and validate reliable IPv4 networking, DNS resolution, routing, and remote access on Ubuntu Server.

## Example Plan

| Setting | Example |
|---|---|
| Interface | `eth0` |
| Address | `192.168.10.20/24` |
| Gateway | `192.168.10.1` |
| DNS | `192.168.10.10`, approved fallback if required |
| Search domain | `lab.example` |
| Method | Router DHCP reservation or Netplan static configuration |

## 1. Discover Current State

```bash
ip -br link
ip -br address
ip route
resolvectl status
networkctl status
```

Identify the correct interface name before editing configuration.

## 2. Prefer a DHCP Reservation When Appropriate

A router-based DHCP reservation provides a stable address while keeping network configuration centralized.

Validation after reservation:

```bash
sudo dhclient -r
sudo dhclient
ip -br address
ip route
```

Command availability varies; a reboot or network service restart may be used instead in a controlled lab.

## 3. Netplan Static Address Example

Back up the existing configuration:

```bash
sudo cp -a /etc/netplan /etc/netplan.backup
```

Edit the actual Netplan YAML file:

```yaml
network:
  version: 2
  ethernets:
    eth0:
      dhcp4: false
      addresses:
        - 192.168.10.20/24
      routes:
        - to: default
          via: 192.168.10.1
      nameservers:
        addresses:
          - 192.168.10.10
        search:
          - lab.example
```

YAML spacing is significant. Validate before applying:

```bash
sudo netplan generate
sudo netplan try
```

`netplan try` provides a rollback window, which is valuable when connected remotely.

Apply after successful validation:

```bash
sudo netplan apply
```

## 4. Connectivity Validation

```bash
ip -br address
ip route
ping -c 4 192.168.10.1
ping -c 4 1.1.1.1
getent hosts example.com
resolvectl query example.com
```

Interpret in order:

1. Interface is up.
2. Correct address is assigned.
3. Default route exists.
4. Gateway responds.
5. External IP connectivity works.
6. DNS name resolution works.

## 5. DNS Validation

```bash
resolvectl status
resolvectl query example.com
dig example.com
dig @192.168.10.10 example.com
```

Check whether DNS is being supplied by:

- Netplan.
- DHCP.
- VPN.
- Local resolver.
- Pi-hole.
- Another network manager.

## 6. Port and Route Checks

```bash
ip route get 1.1.1.1
ss -lntup
sudo ufw status verbose
```

For a remote service:

```bash
nc -vz 192.168.10.20 22
```

Run the client-side test only from an authorized machine.

## 7. Wi-Fi Notes

For headless servers, Ethernet is preferred when practical. If Wi-Fi is required:

- Use WPA2 or WPA3.
- Do not commit plaintext credentials to Git.
- Confirm regulatory country settings.
- Document signal strength and reconnect behavior.
- Test after reboot and access-point restart.
- Maintain a local recovery method.

## 8. Common Problems

| Symptom | Likely Area |
|---|---|
| No address | DHCP, link, YAML, interface name |
| Address but no gateway | Route configuration |
| Gateway works but internet IP fails | Router, upstream, firewall |
| Internet IP works but names fail | DNS |
| SSH works locally only | Firewall, VLAN, routing |
| Address changes | Missing reservation or static config |
| Duplicate address warning | IP conflict |
| Works until reboot | Configuration not persistent |

## 9. Change Record

```text
Date:
Previous configuration:
New configuration:
Reason:
Validation:
Rollback method:
Result:
```
