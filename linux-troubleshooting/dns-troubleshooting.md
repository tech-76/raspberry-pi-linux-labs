# DNS Troubleshooting

## Symptom Examples

- Websites fail by name but work by IP.
- One domain does not resolve.
- Internal names fail.
- Pi-hole queries do not appear.
- DNS becomes slow.
- VPN changes name resolution.

## Layered Test Process

### 1. Confirm IP Connectivity

```bash
ip -br address
ip route
ping -c 4 192.168.10.1
ping -c 4 1.1.1.1
```

If public IP connectivity fails, resolve network or routing first.

### 2. Check Assigned Resolver

```bash
resolvectl status
cat /etc/resolv.conf
```

Identify whether DNS came from DHCP, static configuration, VPN, Pi-hole, or a local stub resolver.

### 3. Test System Resolution

```bash
getent hosts example.com
resolvectl query example.com
```

`getent` tests the system's configured name-service path.

### 4. Test a Specific Resolver

```bash
dig @192.168.10.10 example.com
nslookup example.com 192.168.10.10
```

Compare with an approved alternate resolver only for diagnosis.

### 5. Review Response Details

```bash
dig example.com
dig example.com A
dig example.com AAAA
dig example.com MX
dig example.com TXT
```

Use record types relevant to the issue.

### 6. Check Port Reachability

DNS commonly uses UDP and TCP port 53.

```bash
nc -vz 192.168.10.10 53
```

A TCP test does not fully validate UDP, but it can help identify listening or firewall issues.

### 7. Check Local DNS Service

On the resolver host:

```bash
sudo ss -lntup | grep ':53'
systemctl --failed
journalctl -u resolver-service --since "30 minutes ago"
```

## Common Causes

- Wrong DNS address from DHCP.
- Stale lease.
- VPN override.
- Browser encrypted DNS.
- Secondary resolver bypass.
- IPv6 DNS path differs from IPv4.
- Resolver service stopped.
- Port conflict.
- Firewall or VLAN rule.
- Typographical error in record.
- Expired or incorrect local zone.
- DNS filtering rule.
- Time synchronization problem affecting secure DNS.

## Cache Considerations

Caches can exist in:

- Application.
- Operating system.
- Local resolver.
- Pi-hole.
- Router.
- Upstream resolver.

Flush only the relevant cache and record the command used. Restarting an entire system should not be the first response to a single cached record.

## DNS Incident Template

```text
Date:
Affected clients:
Affected names:
IP connectivity result:
Assigned DNS:
Direct resolver test:
Record type:
Response:
Root cause:
Correction:
Cache action:
Validation:
```
