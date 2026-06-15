# Monitoring Dashboard Lab

## Overview

This lab documents the creation of an always-on Raspberry Pi monitoring dashboard. The dashboard can display system health, service status, network information, environmental data, or approved web-based monitoring panels.

The emphasis is on reliable startup, least privilege, secure display practices, validation, and recovery.

## Possible Implementations

- Local HTML dashboard.
- Grafana display.
- Netdata overview.
- Prometheus-backed metrics.
- Custom Python or shell-generated status page.
- Kiosk browser showing an internal monitoring URL.

Choose one implementation and document the exact software used.

## Learning Objectives

- Collect system metrics.
- Present status information clearly.
- Configure a service or kiosk to start automatically.
- Restrict access to sensitive dashboards.
- Validate recovery after reboot.
- Troubleshoot display, service, network, and authentication failures.

## Example Metrics

- Host uptime.
- CPU load.
- Memory use.
- Storage use.
- Temperature.
- Network reachability.
- DNS service status.
- Backup status.
- Failed services.
- Last update time.

## Security Considerations

- Do not display secrets, tokens, internal IP inventories, or personal data on a publicly visible screen.
- Restrict the dashboard to a trusted LAN or authenticated access.
- Use a read-only account where supported.
- Keep the browser and dashboard software updated.
- Disable unnecessary browser features in kiosk mode.
- Document how to exit kiosk mode for administration.

## Files

- [`dashboard-setup.md`](dashboard-setup.md)
- `screenshots/` for sanitized evidence.

## Completion Checklist

- [ ] Dashboard purpose defined.
- [ ] Metrics selected.
- [ ] Data source documented.
- [ ] Startup behavior configured.
- [ ] Reboot recovery tested.
- [ ] Full-screen or kiosk behavior tested.
- [ ] Access controls reviewed.
- [ ] Sensitive information removed.
- [ ] Failure and recovery procedure documented.
