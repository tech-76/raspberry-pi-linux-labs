# Network Troubleshooting

## Troubleshooting Order

1. Physical or radio link.
2. Interface state.
3. IP address.
4. Local subnet.
5. Default gateway.
6. Routing.
7. DNS.
8. Port/service.
9. Firewall.
10. Application.

## 1. Link and Interface

```bash
ip -br link
ip -br address
```

For Ethernet:

```bash
ethtool eth0 2>/dev/null
```

For Wi-Fi:

```bash
iw dev wlan0 link 2>/dev/null
rfkill list
nmcli device status 2>/dev/null
```

## 2. Addressing

Check:

- Correct subnet.
- Correct prefix length.
- No duplicate address.
- Expected DHCP/static method.
- IPv4 and IPv6 behavior.

```bash
ip address show
ip route
```

## 3. Gateway

```bash
ping -c 4 192.168.10.1
ip neigh
```

If the gateway is unreachable, investigate VLAN, Wi-Fi isolation, cable, switch port, address, and subnet.

## 4. Routing

```bash
ip route
ip route get 1.1.1.1
```

Confirm the expected egress interface and source address.

## 5. External Connectivity

```bash
ping -c 4 1.1.1.1
```

Some hosts block ping, so use additional approved tests when needed.

## 6. DNS

```bash
getent hosts example.com
resolvectl status
```

If IP connectivity works but names fail, follow the DNS guide.

## 7. Service Reachability

```bash
nc -vz server-address 22
curl -I http://server-address
```

Use only against systems you are authorized to test.

## 8. Listening Services

On the server:

```bash
sudo ss -lntup
systemctl status service-name --no-pager
```

## 9. Firewall

```bash
sudo ufw status verbose
sudo nft list ruleset
```

Review client, server, router, and VLAN policies.

## 10. Packet Capture

For difficult lab issues, capture only authorized traffic:

```bash
sudo tcpdump -i eth0 -nn host 192.168.10.20
```

Stop after collecting enough evidence.

## Common Symptoms

| Symptom | Likely Area |
|---|---|
| Interface down | Cable, radio, driver, power |
| No IP | DHCP or static config |
| Self-assigned address | DHCP failure |
| Gateway fails | Local network or subnet |
| Public IP fails | Routing or upstream |
| DNS fails only | Resolver path |
| One port fails | Service or firewall |
| Intermittent loss | Wi-Fi, power, cable, overload |
| Works locally, not remotely | Routing, NAT, firewall, binding |

## Incident Record

```text
Topology:
Affected path:
Source:
Destination:
Protocol/port:
Symptom:
Link status:
Address:
Gateway test:
Route:
DNS:
Service status:
Firewall:
Packet evidence:
Root cause:
Resolution:
Validation:
```
