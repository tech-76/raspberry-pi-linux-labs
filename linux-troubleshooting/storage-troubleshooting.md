# Storage Troubleshooting

## Warning

Storage commands can cause data loss. Confirm the device name, mount state, filesystem, backups, and recovery plan before making changes.

## 1. Inventory

```bash
lsblk -f
findmnt
df -hT
df -i
```

Record device, partition, filesystem, UUID, mount point, capacity, and usage.

## 2. Identify Space Use

```bash
sudo du -xhd1 / 2>/dev/null | sort -h
sudo du -xhd1 /var 2>/dev/null | sort -h
```

Use `-x` to stay on one filesystem.

## 3. Review Logs

```bash
dmesg --level=err,warn
journalctl -k -p warning
```

Search for:

- I/O errors.
- Filesystem errors.
- Read-only remounts.
- USB disconnects.
- Voltage or power warnings.
- MMC or NVMe errors.

## 4. Full Filesystem

Common areas:

- Logs.
- Package cache.
- Container data.
- Downloads.
- Old kernels.
- Backup files.
- Temporary files.

Review before deleting.

```bash
sudo journalctl --disk-usage
sudo du -sh /var/cache/apt 2>/dev/null
```

A safer package cleanup:

```bash
sudo apt autoremove --purge
sudo apt clean
```

## 5. Inode Exhaustion

```bash
df -i
```

A filesystem can have free capacity but no free inodes because of many small files.

## 6. Mount Problems

```bash
findmnt
cat /etc/fstab
sudo mount -av
```

Back up `/etc/fstab` before editing. Use UUIDs where appropriate and understand `nofail` and timeout options for removable devices.

## 7. Filesystem Check

Do not run a repair check on a mounted filesystem unless the tool and filesystem explicitly support it.

General process:

1. Back up recoverable data.
2. Boot from alternate media or unmount the filesystem.
3. Identify the filesystem type.
4. Use the correct filesystem-specific tool.
5. Record output.
6. Reboot and validate.

## 8. microSD Considerations

- Unexpected power loss can corrupt storage.
- Low-quality cards can fail early.
- Write-heavy logging reduces endurance.
- Keep a known-good image.
- Use proper shutdown.
- Consider SSD storage for sustained workloads.
- Use a reliable power supply.

## 9. External USB Storage

Check:

```bash
lsusb
lsblk -f
dmesg --follow
```

Investigate cable, power, enclosure, USB mode, filesystem, and mount options.

## 10. Storage Incident Template

```text
Date:
Device:
Filesystem:
Mount point:
Symptom:
Capacity:
Inodes:
Kernel/log evidence:
Recent change:
Backup status:
Action:
Result:
Data loss:
Preventive action:
```
