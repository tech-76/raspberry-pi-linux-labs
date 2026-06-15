# Common Linux Troubleshooting Commands

Use commands according to the problem. Do not run destructive commands without understanding scope, backups, and rollback.

## System Identity

```bash
hostnamectl
cat /etc/os-release
uname -a
uptime
date
timedatectl
```

## User and Permissions

```bash
whoami
id
groups
getent passwd username
getent group sudo
ls -la
namei -l /path/to/file
```

## CPU and Memory

```bash
lscpu
free -h
top
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

## Storage

```bash
lsblk -f
df -hT
df -i
du -sh /path
sudo du -xhd1 /var 2>/dev/null | sort -h
mount
findmnt
```

## Network

```bash
ip -br link
ip -br address
ip route
ip neigh
ping -c 4 gateway-address
ss -lntup
```

## DNS

```bash
resolvectl status
getent hosts example.com
dig example.com
nslookup example.com
```

## Services

```bash
systemctl status service-name --no-pager
systemctl --failed
systemctl list-units --type=service --state=running
journalctl -u service-name --since "30 minutes ago"
```

## Logs

```bash
journalctl -b
journalctl -p warning -b
dmesg --level=err,warn
tail -n 100 /var/log/syslog 2>/dev/null
```

## Processes and Files

```bash
ps aux
pgrep -a process-name
lsof /path/to/file
sudo lsof -i
```

## Packages

```bash
sudo apt update
apt list --upgradable
dpkg -l | grep package-name
apt-cache policy package-name
sudo dpkg --configure -a
sudo apt --fix-broken install
```

## Hardware

```bash
lsusb
lspci 2>/dev/null
dmesg --follow
cat /proc/cpuinfo
```

## Firewall

```bash
sudo ufw status verbose
sudo nft list ruleset
```

## Troubleshooting Notes

For every command, record:

- Why it was run.
- What output mattered.
- What hypothesis it supported or rejected.
- What changed next.
- How the final fix was validated.
