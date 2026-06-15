# Raspberry Pi Zero 2 W Lab

## Overview

This lab documents a lightweight, headless Raspberry Pi Zero 2 W deployment. It focuses on imaging, Wi-Fi, SSH, remote administration, low-resource operation, power considerations, and recovery.

## Learning Objectives

- Prepare a headless Raspberry Pi OS installation.
- Configure secure wireless access.
- Discover and connect to the device.
- Validate hostname, addressing, DNS, time, storage, and services.
- Apply updates without exhausting storage.
- Monitor resource and temperature limits.
- Recover from failed network or boot configuration.

## Example Use Cases

- Lightweight DNS or utility service.
- Remote sensor gateway.
- Small status endpoint.
- Portable Linux administration practice.
- Low-power network tool.
- Headless automation node.

## Constraints

- Limited memory and CPU compared with larger Pi models.
- Wi-Fi-only networking unless an adapter is added.
- microSD performance and endurance.
- Power stability.
- Thermal and enclosure considerations.
- Fewer practical concurrent services.

## Included Documents

- [`headless-setup.md`](headless-setup.md)
- [`wireless-configuration.md`](wireless-configuration.md)

## Completion Checklist

- [ ] OS imaged and verified.
- [ ] Unique hostname configured.
- [ ] Non-default user configured.
- [ ] Wi-Fi credentials protected.
- [ ] SSH key access tested.
- [ ] Address recorded or reserved.
- [ ] DNS and time validated.
- [ ] Storage and memory baseline captured.
- [ ] Reboot reconnect tested.
- [ ] Recovery process documented.
