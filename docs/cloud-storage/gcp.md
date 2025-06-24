---
layout: default
title: Google Cloud Storage
parent: Cloud Storage Setup
nav_order: 2
---

# Google Cloud Storage Setup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

- A Google Cloud Platform account
- A GCS bucket for storing backups
- Service account with appropriate permissions

## Creating a GCS Bucket

1. Go to the [Google Cloud Console](https://console.cloud.google.com)
2. Navigate to Cloud Storage
3. Click "Create bucket"
4. Configure your bucket:
   - Choose a unique name
   - Select a location close to your Jira/Confluence instances
   - Choose storage class (Standard recommended)
   - Set access control to "Uniform"

## Service Account Setup

### Creating a Service Account

1. Navigate to IAM & Admin → Service Accounts
2. Click "Create Service Account"
3. Provide a name and description
4. Grant the following roles:
   - Storage Object Admin
   - Storage Legacy Bucket Reader

### Generate Service Account Key

1. Click on your service account
2. Go to "Keys" tab
3. Add Key → Create new key
4. Choose JSON format
5. Save the key file securely

## Configuration

### Using Service Account Key File

```yaml
storage:
  provider: "gcp"
  gcp:
    bucket_name: "my-jira-backups"
    project_id: "my-project-123456"
    credentials_path: "/path/to/service-account-key.json"
```

### Using Application Default Credentials

When running on Google Cloud infrastructure:

```yaml
storage:
  provider: "gcp"
  gcp:
    bucket_name: "my-jira-backups"
    project_id: "my-project-123456"
    # No credentials_path needed - uses ADC
```

### Using Environment Variable

```yaml
storage:
  provider: "gcp"
  gcp:
    bucket_name: "my-jira-backups"
    project_id: "my-project-123456"
    credentials_path: ${GOOGLE_APPLICATION_CREDENTIALS}
```

## Advanced Configuration

### Storage Classes

Configure storage class for cost optimization:

```yaml
storage:
  gcp:
    storage_class: "STANDARD"  # Options: STANDARD, NEARLINE, COLDLINE, ARCHIVE
```

### Customer-Managed Encryption Keys (CMEK)

```yaml
storage:
  gcp:
    encryption_key: "projects/PROJECT_ID/locations/LOCATION/keyRings/RING/cryptoKeys/KEY"
```

### Lifecycle Rules

Set up lifecycle rules in the GCS console to:
- Move old backups to cheaper storage classes
- Delete backups after retention period

## IAM Best Practices

### Minimum Required Permissions

Create a custom role with only necessary permissions:

```json
{
  "title": "Jira Backup Storage",
  "description": "Permissions for Jira backup operations",
  "stage": "GA",
  "includedPermissions": [
    "storage.objects.create",
    "storage.objects.delete",
    "storage.objects.get",
    "storage.objects.list",
    "storage.buckets.get"
  ]
}
```

### Workload Identity (GKE)

If running on GKE, use Workload Identity:

1. Enable Workload Identity on your cluster
2. Create a Kubernetes service account
3. Bind it to the GCP service account
4. No credentials file needed in configuration

## Testing Your Configuration

Verify your setup with a test backup:

```bash
python backup.py --test --service confluence
```

## Troubleshooting

### Authentication Errors

- Verify service account key file path
- Check GOOGLE_APPLICATION_CREDENTIALS environment variable
- Ensure service account has correct permissions

### Permission Denied

- Check IAM roles assigned to service account
- Verify bucket permissions
- Ensure project_id is correct

### Bucket Not Found

- Confirm bucket name is correct
- Check you're using the right project
- Verify bucket exists in the specified project

## Cost Optimization

1. Use appropriate storage classes:
   - STANDARD for recent backups
   - NEARLINE for 30-day old backups
   - COLDLINE for 90-day old backups
   - ARCHIVE for long-term retention

2. Enable Object Lifecycle Management
3. Use regional buckets instead of multi-regional
4. Monitor storage usage with Cloud Monitoring