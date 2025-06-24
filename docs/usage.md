---
layout: default
title: Usage Guide
nav_order: 6
---

# Usage Guide
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Basic Usage

### Running a Backup

To perform a manual backup of both Jira and Confluence:

```bash
python backup.py
```

### Backup Specific Service

Backup only Jira:
```bash
python backup.py --service jira
```

Backup only Confluence:
```bash
python backup.py --service confluence
```

### Test Mode

Run in test mode to verify configuration without uploading:
```bash
python backup.py --test
```

## Command Line Options

| Option | Description |
|--------|-------------|
| `--service` | Specify service to backup (jira, confluence, or both) |
| `--test` | Run in test mode without uploading |
| `--config` | Path to configuration file (default: config.yaml) |
| `--verbose` | Enable verbose logging |
| `--dry-run` | Show what would be done without executing |

## Backup Process

1. **Authentication**: Connects to Jira/Confluence using API credentials
2. **Export Request**: Initiates backup export via Atlassian API
3. **Monitoring**: Polls export status until complete
4. **Download**: Downloads backup file to temporary directory
5. **Compression**: Optionally compresses the backup
6. **Upload**: Uploads to configured cloud storage
7. **Cleanup**: Removes local temporary files

## Output and Logs

### Console Output

```
2024-01-15 10:00:00 INFO: Starting backup process...
2024-01-15 10:00:01 INFO: Initiating Jira backup...
2024-01-15 10:05:23 INFO: Backup complete, size: 1.2GB
2024-01-15 10:05:24 INFO: Uploading to AWS S3...
2024-01-15 10:08:45 INFO: Upload complete
2024-01-15 10:08:46 INFO: Backup successful!
```

### Log Files

Logs are written to:
- Default: `./logs/backup_YYYYMMDD.log`
- Configure in `config.yaml`:
  ```yaml
  backup:
    log_directory: "/var/log/jira-backup"
  ```

## Backup Verification

### List Recent Backups

```bash
python backup.py --list-backups
```

### Verify Backup Integrity

```bash
python backup.py --verify --backup-id <backup-id>
```

## Restore Process

### Download Backup

```bash
python restore.py --backup-id <backup-id> --output /path/to/restore
```

### List Available Backups

```bash
python restore.py --list
```

## Best Practices

1. **Regular Testing**: Perform test restores monthly
2. **Monitor Logs**: Check logs for warnings or errors
3. **Verify Uploads**: Confirm backups exist in cloud storage
4. **Document Process**: Keep restore procedures updated
5. **Alert on Failures**: Set up notifications for failed backups

## Troubleshooting

### Common Issues

**Backup Timeout**
- Increase timeout in configuration
- Check network connectivity
- Verify Atlassian API limits

**Authentication Failed**
- Verify API token is valid
- Check username/email is correct
- Ensure user has backup permissions

**Upload Failed**
- Verify cloud storage credentials
- Check network connectivity
- Ensure sufficient storage space

**Out of Memory**
- Increase system memory
- Enable compression
- Use streaming upload for large files

### Debug Mode

Enable detailed debugging:
```bash
python backup.py --debug
```

## Performance Tips

1. **Schedule Off-Peak**: Run backups during low usage
2. **Compression**: Enable to reduce transfer time
3. **Parallel Uploads**: Use multipart uploads for large files
4. **Incremental Backups**: Consider incremental strategy for large instances

## Security Considerations

1. **Secure Credentials**: Never commit credentials to version control
2. **Encryption**: Enable encryption in cloud storage
3. **Access Control**: Limit who can access backups
4. **Audit Logs**: Monitor access to backup files
5. **Retention**: Delete old backups per policy