---
layout: default
title: Configuration
nav_order: 3
---

# Configuration Guide
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Configuration File Overview

Jira Backup Python uses a YAML configuration file to manage all settings. The configuration file is divided into several sections:

- **jira**: Jira connection settings
- **confluence**: Confluence connection settings  
- **storage**: Cloud storage provider settings
- **backup**: General backup settings
- **scheduler**: Automated scheduling settings

## Basic Configuration

### Jira Settings

```yaml
jira:
  url: "https://your-domain.atlassian.net"
  username: "your-email@example.com"
  api_token: "your-api-token"
  backup_path: "/backups/jira"
```

### Confluence Settings

```yaml
confluence:
  url: "https://your-domain.atlassian.net/wiki"
  username: "your-email@example.com"
  api_token: "your-api-token"
  backup_path: "/backups/confluence"
```

### Storage Configuration

The storage section supports multiple providers. You only need to configure the one you're using:

#### AWS S3

```yaml
storage:
  provider: "aws"
  aws:
    bucket_name: "my-backup-bucket"
    region: "us-east-1"
    access_key_id: "AKIAIOSFODNN7EXAMPLE"
    secret_access_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

#### Google Cloud Storage

```yaml
storage:
  provider: "gcp"
  gcp:
    bucket_name: "my-backup-bucket"
    project_id: "my-project-id"
    credentials_path: "/path/to/service-account-key.json"
```

#### Azure Blob Storage

```yaml
storage:
  provider: "azure"
  azure:
    container_name: "my-backup-container"
    account_name: "mystorageaccount"
    account_key: "your-account-key"
```

### Backup Settings

```yaml
backup:
  retention_days: 30
  compression: true
  include_attachments: true
  temp_directory: "/tmp/jira-backup"
```

### Scheduler Settings

```yaml
scheduler:
  enabled: true
  jira_schedule: "0 2 * * *"  # Daily at 2 AM
  confluence_schedule: "0 3 * * *"  # Daily at 3 AM
```

## Environment Variables

You can also use environment variables for sensitive information:

```yaml
jira:
  api_token: ${JIRA_API_TOKEN}

storage:
  aws:
    access_key_id: ${AWS_ACCESS_KEY_ID}
    secret_access_key: ${AWS_SECRET_ACCESS_KEY}
```

## Validation

The configuration is validated on startup. Common validation errors include:

- Missing required fields
- Invalid URLs
- Incorrect cloud storage credentials
- Invalid cron expressions

## Security Best Practices

1. Never commit `config.yaml` with real credentials to version control
2. Use environment variables for sensitive data
3. Restrict file permissions: `chmod 600 config.yaml`
4. Rotate API tokens regularly
5. Use IAM roles when running on cloud infrastructure

## Next Steps

- [Set up cloud storage]({% link docs/cloud-storage.md %})
- [Configure automated scheduling]({% link docs/scheduling.md %})
- [Run your first backup]({% link docs/usage.md %})