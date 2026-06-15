# Ubuntu Server Maintenance Checklist

Use this checklist on a defined schedule. Record actual results instead of checking boxes without evidence.

## Maintenance Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Administrator | Name or initials |
| Hostname | `ubuntu-pi01` |
| Uptime before maintenance | |
| Change window | |
| Backup verified | Yes/No |
| Reboot required | Yes/No |
| Issues found | |

## Before Maintenance

- [ ] Confirm current system time.
- [ ] Confirm stable power and network connectivity.
- [ ] Review recent changes.
- [ ] Verify a current backup exists.
- [ ] Confirm local or alternate recovery access.
- [ ] Record baseline service status.
- [ ] Notify affected users if applicable.

Commands:

```bash
date
uptime
systemctl --failed
df -hT
free -h
ip -br address
ip route
```

## Package Maintenance

```bash
sudo apt update
apt list --upgradable
sudo apt upgrade
sudo apt autoremove --purge
```

Review package changes before accepting them on important systems.

- [ ] Package index refreshed.
- [ ] Upgrade list reviewed.
- [ ] Security updates applied.
- [ ] Obsolete packages reviewed.
- [ ] Package errors resolved.
- [ ] Reboot requirement checked.

Check:

```bash
test -f /var/run/reboot-required && cat /var/run/reboot-required
```

## Service Health

```bash
systemctl --failed
systemctl --type=service --state=running
sudo ss -lntup
journalctl -p warning --since "7 days ago"
```

- [ ] Failed services investigated.
- [ ] Intended services running.
- [ ] Listening ports reviewed.
- [ ] Unexpected services documented or removed.
- [ ] Repeated warnings investigated.

## Storage Health

```bash
lsblk -f
df -hT
df -i
sudo du -xhd1 /var 2>/dev/null | sort -h
```

- [ ] Filesystem capacity acceptable.
- [ ] Inode usage acceptable.
- [ ] Log growth reviewed.
- [ ] Temporary files reviewed.
- [ ] External mounts present.
- [ ] Backup destination accessible.
- [ ] Kernel storage errors reviewed.

## Performance Baseline

```bash
uptime
free -h
top
cat /sys/class/thermal/thermal_zone0/temp 2>/dev/null
```

- [ ] Load average reviewed.
- [ ] Memory and swap reviewed.
- [ ] Temperature within expected range.
- [ ] Processes with abnormal use investigated.
- [ ] Power or throttling warnings checked where supported.

## Network and DNS

```bash
ip -br address
ip route
ping -c 4 192.168.10.1
getent hosts example.com
resolvectl status
```

- [ ] Expected address assigned.
- [ ] Default route correct.
- [ ] Gateway reachable.
- [ ] DNS resolution works.
- [ ] Remote administration works.
- [ ] Firewall rules reviewed.
- [ ] No unexplained address changes.

## Security Review

- [ ] Administrative users reviewed.
- [ ] SSH keys reviewed.
- [ ] Root remote login remains disabled.
- [ ] Firewall policy reviewed.
- [ ] Unused accounts or services reviewed.
- [ ] Authentication failures reviewed.
- [ ] Sensitive files have correct permissions.
- [ ] Secrets are not stored in the repository.

Useful checks:

```bash
getent group sudo
sudo lastlog
sudo journalctl -u ssh --since "7 days ago"
sudo ufw status verbose
```

## Backup and Restore

- [ ] Latest backup completed.
- [ ] Backup logs reviewed.
- [ ] Backup destination has space.
- [ ] Configuration backup updated.
- [ ] Restore procedure documented.
- [ ] Restore test completed according to schedule.

## After Maintenance

```bash
systemctl --failed
uptime
df -hT
free -h
ip -br address
getent hosts example.com
```

- [ ] Required reboot completed.
- [ ] Services returned successfully.
- [ ] SSH access retested.
- [ ] DNS and internet access retested.
- [ ] Application function retested.
- [ ] Maintenance record completed.
- [ ] New issue or change documented.

## Summary

```text
Work completed:
Updates installed:
Problems found:
Problems resolved:
Outstanding risks:
Next action:
Next maintenance date:
```
