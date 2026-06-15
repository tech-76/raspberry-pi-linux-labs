# Pi-hole DNS Filtering Lab

## Overview

This lab documents the deployment of Pi-hole as a network-level DNS filtering service on a Raspberry Pi. The objective is to demonstrate Linux administration, DNS fundamentals, IP configuration, service validation, client configuration, logging, and troubleshooting.

## Learning Objectives

- Install and update a supported Linux operating system.
- Assign a stable IP address to the Pi-hole host.
- Install Pi-hole using the official installation method.
- Configure network clients or the router to use Pi-hole for DNS.
- Validate name resolution and filtering.
- Review query logs without exposing private browsing data.
- Maintain allowlists, blocklists, backups, and updates.
- Troubleshoot DNS failures methodically.

## Example Environment

| Component | Example |
|---|---|
| Hardware | Raspberry Pi 4 |
| Operating system | Raspberry Pi OS Lite or Ubuntu Server |
| Hostname | `pihole01` |
| LAN | `192.168.10.0/24` |
| Pi-hole address | `192.168.10.10` |
| Router | `192.168.10.1` |
| Admin access | SSH from a trusted workstation |
| Upstream DNS | Selected during Pi-hole configuration |

Replace all examples with values from your own lab.

## High-Level Workflow

1. Prepare the operating system.
2. Update the system and set the hostname.
3. Reserve or assign a stable IP address.
4. Install Pi-hole.
5. Configure upstream DNS and interface settings.
6. Point one test client to Pi-hole.
7. Validate DNS resolution and filtering.
8. Expand to router-level DNS only after successful testing.
9. Document exceptions, maintenance, and recovery steps.

## Validation Checklist

- [ ] Pi-hole host responds to ping from the admin network.
- [ ] SSH access works from a trusted device.
- [ ] Pi-hole DNS service is listening on port 53.
- [ ] The web administration page is reachable.
- [ ] A test client resolves normal domains through Pi-hole.
- [ ] A known test domain is blocked according to policy.
- [ ] Query logs show the test client.
- [ ] Critical business or household services still work.
- [ ] Configuration backup has been exported.
- [ ] Sensitive screenshots have been sanitized.

## Evidence to Add

- Sanitized dashboard overview.
- DNS query test from a client.
- Service status output.
- Network diagram.
- Before-and-after client DNS settings.
- Short troubleshooting example.

## Files in This Lab

- [`installation-notes.md`](installation-notes.md)
- [`network-diagram.md`](network-diagram.md)
- [`troubleshooting.md`](troubleshooting.md)

## Security Notes

- Restrict the admin interface to trusted networks.
- Use a strong administrative password.
- Keep the host and Pi-hole software updated.
- Do not expose DNS or the web interface directly to the internet.
- Review logs carefully before publishing screenshots.
- Back up configuration before major changes.
