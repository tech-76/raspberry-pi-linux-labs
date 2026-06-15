# Pi-hole Troubleshooting Guide

## Troubleshooting Method

1. Define the symptom precisely.
2. Confirm whether the issue affects one client or all clients.
3. Check physical connectivity and IP addressing.
4. Test the gateway.
5. Test Pi-hole reachability.
6. Test DNS directly against Pi-hole.
7. Review Pi-hole service status and logs.
8. Compare a blocked and allowed query.
9. Change one variable at a time.
10. Record the root cause and final validation.

## Quick Health Checks

```bash
hostnamectl
ip -br address
ip route
ping -c 4 192.168.10.1
pihole status
sudo systemctl status pihole-FTL --no-pager
sudo systemctl --failed
sudo ss -lntup | grep ':53'
df -h
free -h
```

## Symptom: Clients Have No Internet Access

Internet access may be working while DNS resolution is failing.

From the client:

```bash
ping -c 4 192.168.10.1
ping -c 4 1.1.1.1
nslookup example.com 192.168.10.10
```

Interpretation:

- Gateway ping fails: investigate local network, VLAN, Wi-Fi, cable, or IP settings.
- Public IP ping works but domain lookup fails: investigate DNS.
- Direct query to Pi-hole fails: verify Pi-hole reachability, firewall rules, and service status.
- Direct query works but normal lookup fails: inspect the client-assigned DNS settings.

## Symptom: Pi-hole Web Interface Works but DNS Does Not

Check listening ports:

```bash
sudo ss -lntup | grep ':53'
sudo systemctl status pihole-FTL --no-pager
```

Check for port conflicts:

```bash
sudo ss -lntup
sudo journalctl -u pihole-FTL --since "30 minutes ago"
```

Possible causes:

- DNS service stopped.
- Another resolver is already using port 53.
- Pi-hole is listening on the wrong interface.
- Firewall policy blocks client access.
- Client is on a different VLAN without an allow rule.
- Address changed after reboot.

## Symptom: One Website or Application Is Broken

1. Confirm the problem disappears when using a controlled non-filtered test resolver.
2. Review the Pi-hole query log for the affected time.
3. Identify the exact blocked domain.
4. Research the domain before allowlisting it.
5. Add the narrowest practical exception.
6. Retest and document the business reason.

Do not disable all filtering to resolve one application.

## Symptom: Queries Do Not Appear in Pi-hole

Check the client's DNS configuration:

```bash
# Linux examples
resolvectl status
cat /etc/resolv.conf
```

Common causes:

- Client retained an old DHCP lease.
- Browser or operating system uses encrypted DNS independently.
- VPN overrides DNS.
- A secondary DNS server allows bypass.
- IPv6 DNS is assigned separately.
- Application uses a hard-coded resolver.

## Symptom: Pi-hole Address Changed

```bash
ip -br address
ip route
```

Correct the DHCP reservation or static configuration, renew the lease, and update any router DNS settings. Avoid assigning a duplicate address.

## Symptom: Slow DNS

Check:

```bash
ping -c 10 192.168.10.10
top
free -h
df -h
sudo journalctl -u pihole-FTL --since "1 hour ago"
```

Investigate:

- Weak Wi-Fi or packet loss.
- Slow or unavailable upstream resolver.
- Storage errors or full filesystem.
- Excessive logging or resource pressure.
- Large blocklist changes.
- Time synchronization problems.

## Symptom: Updates Fail

```bash
sudo apt update
sudo apt --fix-broken install
df -h
timedatectl
```

Possible causes:

- DNS already broken.
- Full storage.
- Incorrect date/time.
- Repository problem.
- Interrupted package operation.
- Unsupported operating system release.

## Recovery Checklist

- [ ] Record current configuration.
- [ ] Export a backup if the interface is available.
- [ ] Confirm host address and gateway.
- [ ] Confirm port 53 ownership.
- [ ] Restart only the affected service first.
- [ ] Reboot only when required and documented.
- [ ] Restore from backup only after identifying the failure.
- [ ] Validate from at least two clients.
- [ ] Record root cause and preventive action.

## Incident Record Template

```text
Date/time:
Affected devices:
Symptom:
Recent change:
Initial hypothesis:
Commands/tests performed:
Evidence:
Root cause:
Resolution:
Validation:
Preventive action:
```
