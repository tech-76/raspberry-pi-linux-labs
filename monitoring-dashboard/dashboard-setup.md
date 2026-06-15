# Dashboard Setup

## 1. Design Record

| Item | Value |
|---|---|
| Date | `YYYY-MM-DD` |
| Device | Raspberry Pi model |
| OS | |
| Dashboard software | |
| Display | |
| Data source | Local/remote |
| URL or local path | Redact if sensitive |
| Startup method | systemd/autostart |
| Authentication | |
| Intended viewers | |

## 2. Define the Dashboard Purpose

Select a focused objective. Example:

> Provide a read-only overview of Raspberry Pi system health and critical lab services on an always-on display.

Avoid placing unrelated widgets on one screen.

## 3. Baseline System Checks

```bash
hostnamectl
ip -br address
df -hT
free -h
uptime
systemctl --failed
```

## 4. Example Local Metrics Script

Create a simple script:

```bash
sudo mkdir -p /opt/lab-dashboard
sudoedit /opt/lab-dashboard/system-status.sh
```

Example:

```bash
#!/usr/bin/env bash
set -euo pipefail

echo "Hostname: $(hostname)"
echo "Updated:  $(date --iso-8601=seconds)"
echo "Uptime:   $(uptime -p)"
echo "Load:     $(cut -d' ' -f1-3 /proc/loadavg)"
echo "Memory:"
free -h
echo "Storage:"
df -h /
echo "Failed services:"
systemctl --failed --no-legend || true
```

Secure and test:

```bash
sudo chmod 750 /opt/lab-dashboard/system-status.sh
sudo /opt/lab-dashboard/system-status.sh
```

This script can feed a local page, terminal dashboard, or scheduled data file.

## 5. Example Browser Kiosk Concept

Install a supported browser and configure it to launch only the approved dashboard URL. Exact package and desktop autostart methods vary by distribution.

A conceptual launch command may include:

```bash
browser-command --kiosk --no-first-run https://dashboard.example
```

Do not copy browser flags without reviewing what they do. Avoid disabling certificate validation or security features.

## 6. systemd Service Pattern

For a custom dashboard service:

```ini
[Unit]
Description=Lab Monitoring Dashboard
After=network-online.target
Wants=network-online.target

[Service]
Type=simple
User=dashboard
Group=dashboard
WorkingDirectory=/opt/lab-dashboard
ExecStart=/usr/bin/python3 /opt/lab-dashboard/app.py
Restart=on-failure
RestartSec=5
NoNewPrivileges=true
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```

Save as:

```text
/etc/systemd/system/lab-dashboard.service
```

Then:

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now lab-dashboard
sudo systemctl status lab-dashboard --no-pager
```

Only use this template after creating the referenced user and application.

## 7. Validation

Test:

- Dashboard opens after reboot.
- Data refreshes.
- Date/time is accurate.
- Failed data sources show a clear status.
- Browser recovers after network interruption.
- Service restarts after a controlled failure.
- Screen does not reveal sensitive information.
- Admin exit method works.

Commands:

```bash
systemctl status lab-dashboard --no-pager
journalctl -u lab-dashboard --since "30 minutes ago"
sudo ss -lntup
```

## 8. Screenshot Checklist

Capture:

1. Full dashboard overview.
2. System health section.
3. Service status section.
4. Startup service status.
5. Reboot validation.

Store in:

```text
monitoring-dashboard/screenshots/
```

Use names such as:

```text
2026-06-14_dashboard_overview.png
```

## 9. Troubleshooting

### Dashboard does not start

```bash
systemctl status lab-dashboard --no-pager
journalctl -u lab-dashboard -b
```

### Browser opens before network is ready

Use a network-online dependency and confirm the operating system actually provides a working wait-online service.

### Blank screen

Check:

- Display cable and input.
- Browser process.
- Dashboard URL.
- DNS.
- Certificate.
- Authentication session.
- GPU/display logs.

### Stale metrics

Check:

- Data collection service.
- System time.
- File permissions.
- API or socket access.
- Scheduled job.
- Dashboard refresh interval.

## 10. Change Record

```text
Date:
Change:
Reason:
Risk:
Rollback:
Validation:
Result:
```
