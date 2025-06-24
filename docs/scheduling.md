---
layout: default
title: Scheduling Backups
nav_order: 5
---

# Scheduling Backups
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Overview

Jira Backup Python includes a built-in scheduler that allows you to automate your backup process. The scheduler uses cron expressions for flexible scheduling options.

## Enabling the Scheduler

To enable automated backups, set the scheduler configuration in your `config.yaml`:

```yaml
scheduler:
  enabled: true
  jira_schedule: "0 2 * * *"  # Daily at 2 AM
  confluence_schedule: "0 3 * * *"  # Daily at 3 AM
```

## Cron Expression Format

The scheduler uses standard cron expression format:

```
┌───────────── minute (0 - 59)
│ ┌───────────── hour (0 - 23)
│ │ ┌───────────── day of the month (1 - 31)
│ │ │ ┌───────────── month (1 - 12)
│ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday)
│ │ │ │ │
│ │ │ │ │
* * * * *
```

### Common Schedules

| Schedule | Cron Expression | Description |
|----------|----------------|-------------|
| Daily at 2 AM | `0 2 * * *` | Runs every day at 2:00 AM |
| Weekly on Sunday | `0 2 * * 0` | Runs every Sunday at 2:00 AM |
| Monthly on 1st | `0 2 1 * *` | Runs on the 1st of each month |
| Every 6 hours | `0 */6 * * *` | Runs at 00:00, 06:00, 12:00, 18:00 |
| Weekdays only | `0 2 * * 1-5` | Monday through Friday at 2:00 AM |

## Running the Scheduler

### As a Service

The recommended way to run the scheduler is as a system service:

1. Create a systemd service file:

```ini
[Unit]
Description=Jira Backup Python Scheduler
After=network.target

[Service]
Type=simple
User=backup
WorkingDirectory=/opt/jira-backup-py
ExecStart=/opt/jira-backup-py/venv/bin/python /opt/jira-backup-py/scheduler.py
Restart=always
RestartSec=10

[Install]
WantedBy=multi-user.target
```

2. Enable and start the service:

```bash
sudo systemctl enable jira-backup-scheduler
sudo systemctl start jira-backup-scheduler
```

### Using Docker

Run the scheduler in a Docker container:

```bash
docker run -d \
  --name jira-backup-scheduler \
  --restart unless-stopped \
  -v $(pwd)/config.yaml:/app/config.yaml \
  -v $(pwd)/logs:/app/logs \
  jira-backup-py python scheduler.py
```

### Manual Execution

For testing or development:

```bash
python scheduler.py
```

## Monitoring

### Log Files

The scheduler writes logs to help monitor backup operations:

```yaml
scheduler:
  log_file: "/var/log/jira-backup/scheduler.log"
  log_level: "INFO"  # Options: DEBUG, INFO, WARNING, ERROR
```

### Health Checks

Enable health check endpoint for monitoring:

```yaml
scheduler:
  health_check:
    enabled: true
    port: 8080
    path: "/health"
```

### Notifications

Configure email notifications for backup status:

```yaml
scheduler:
  notifications:
    enabled: true
    smtp_server: "smtp.gmail.com"
    smtp_port: 587
    from_email: "backup@example.com"
    to_email: "admin@example.com"
    on_success: false  # Send email on successful backups
    on_failure: true   # Send email on failed backups
```

## Best Practices

1. **Stagger Schedules**: Don't schedule Jira and Confluence backups at the same time
2. **Off-Peak Hours**: Schedule backups during low-usage periods
3. **Monitor Resources**: Ensure sufficient disk space and memory
4. **Test Restores**: Regularly test your backups by performing restores
5. **Retention Policy**: Configure appropriate retention periods

## Troubleshooting

### Scheduler Not Running

- Check service status: `systemctl status jira-backup-scheduler`
- Review logs for errors
- Verify configuration file syntax

### Backups Not Executing

- Validate cron expressions
- Check system time and timezone
- Ensure proper permissions

### Performance Issues

- Adjust backup schedules to avoid peak hours
- Consider incremental backups for large instances
- Monitor system resources during backups