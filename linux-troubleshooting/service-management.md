# Linux Service Management

## systemd Concepts

- **Unit** — a managed object such as a service, socket, timer, or mount.
- **Active** — currently running or completed successfully, depending on unit type.
- **Enabled** — configured to start through a target dependency.
- **Failed** — entered a failure state.
- **Journal** — centralized logs managed by `systemd-journald`.

## Check Service Status

```bash
systemctl status service-name --no-pager
systemctl is-active service-name
systemctl is-enabled service-name
```

## Start, Stop, Restart, Reload

```bash
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name
sudo systemctl reload service-name
```

Use reload when supported to apply configuration without a full restart.

## Enable or Disable Startup

```bash
sudo systemctl enable service-name
sudo systemctl disable service-name
```

Enable and start:

```bash
sudo systemctl enable --now service-name
```

## Find Failed Services

```bash
systemctl --failed
```

## Review Logs

```bash
journalctl -u service-name
journalctl -u service-name --since "30 minutes ago"
journalctl -u service-name -b
journalctl -u service-name -f
```

## Validate Configuration

Many services provide a configuration-test command. Examples include:

```bash
sudo sshd -t
sudo nginx -t
sudo apachectl configtest
```

Use the command specific to the installed service before reloading.

## Inspect Unit Definition

```bash
systemctl cat service-name
systemctl show service-name
```

## Edit Safely

Use a drop-in override:

```bash
sudo systemctl edit service-name
```

After modifying unit files:

```bash
sudo systemctl daemon-reload
sudo systemctl restart service-name
```

## Common Failure Causes

- Invalid configuration.
- Missing file or directory.
- Wrong permissions.
- Port already in use.
- Dependency unavailable.
- Network not ready.
- Full filesystem.
- Missing package or executable.
- Incorrect user/group.
- Environment variable absent.
- Security policy restriction.
- Repeated crash triggering start limits.

## Port Conflict

```bash
sudo ss -lntup
sudo lsof -i :PORT
```

## Service Failure Record

```text
Service:
Host:
Symptom:
Status output:
Log evidence:
Configuration test:
Port check:
Dependency check:
Root cause:
Correction:
Restart/reload:
Validation:
Preventive action:
```

## Validation Checklist

- [ ] Configuration test passes.
- [ ] Service active.
- [ ] Service starts after reboot when required.
- [ ] Expected port is listening.
- [ ] Client function works.
- [ ] Logs show no repeated errors.
- [ ] Permissions are least privilege.
- [ ] Change and rollback documented.
