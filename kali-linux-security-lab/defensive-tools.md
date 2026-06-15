# Defensive Tools

This document focuses on safe, non-destructive use inside the authorized lab.

## 1. System and Network Inventory

### `ip`

```bash
ip -br address
ip route
ip neigh
```

Purpose:

- Confirm local interfaces.
- Confirm the selected lab route.
- Review directly observed neighbours.

### `ss`

```bash
sudo ss -lntup
```

Purpose:

- Identify services listening on the Kali host.
- Validate that unnecessary services are not exposed.

## 2. DNS Tools

```bash
dig example.com
nslookup example.com
resolvectl status 2>/dev/null
```

Purpose:

- Validate DNS.
- Compare configured resolvers.
- Review response records.

## 3. Controlled Service Discovery

Use only against listed lab targets.

```bash
nmap -sT -sV 192.168.56.20
```

This performs a TCP connect scan and basic service-version detection against one authorized host.

Safer documentation practices:

- Scan one host first.
- Avoid aggressive timing.
- Avoid scripts you have not reviewed.
- Record the command, target, time, and authorization.
- Stop if the target becomes unstable.

## 4. Packet Analysis

### `tcpdump`

Capture only tester-generated traffic on the isolated interface:

```bash
sudo tcpdump -i eth0 -nn host 192.168.56.20
```

Write a short, bounded capture when required:

```bash
sudo timeout 60 tcpdump -i eth0 -nn host 192.168.56.20 -w lab-test.pcap
```

Do not commit packet captures to Git.

### Wireshark

Use display filters to reduce exposed data:

```text
ip.addr == 192.168.56.20
dns
tcp.port == 22
```

Analyze:

- DNS request and response.
- TCP handshake.
- Expected service ports.
- Retransmissions.
- Cleartext versus encrypted protocols.

## 5. Log Review

```bash
sudo journalctl -p warning -b
sudo journalctl -u ssh --since "1 hour ago"
sudo tail -n 100 /var/log/auth.log 2>/dev/null
```

Review authentication events, failed services, and repeated warnings.

## 6. File Integrity Demonstration

Create a synthetic file:

```bash
printf 'authorized lab test\n' > sample.txt
sha256sum sample.txt
```

Modify it and compare the hash:

```bash
printf 'authorized lab test - changed\n' > sample.txt
sha256sum sample.txt
```

Purpose:

- Demonstrate integrity checking.
- Avoid using real confidential files.

## 7. Permission Review

```bash
find ./lab-data -maxdepth 2 -type f -printf '%m %u %g %p\n' 2>/dev/null
namei -l ./lab-data/sample.txt 2>/dev/null
```

Review ownership and access for synthetic lab files.

## 8. Firewall Review

```bash
sudo ufw status verbose 2>/dev/null
sudo nft list ruleset 2>/dev/null
```

Do not modify firewall policy without a recovery path.

## 9. Update and Exposure Review

```bash
sudo apt update
apt list --upgradable
sudo ss -lntup
systemctl --failed
```

The goal is to reduce known risk, not merely collect tool output.

## 10. Findings Template

```text
Finding:
Asset:
Evidence:
Risk:
Likelihood:
Impact:
Recommendation:
Change applied:
Retest result:
Status:
```
