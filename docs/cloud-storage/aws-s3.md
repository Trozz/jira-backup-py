---
layout: default
title: AWS S3
parent: Cloud Storage Setup
nav_order: 1
---

# AWS S3 Setup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

- An AWS account
- An S3 bucket for storing backups
- AWS credentials (access key or IAM role)

## Creating an S3 Bucket

1. Log in to the AWS Console
2. Navigate to S3
3. Click "Create bucket"
4. Choose a unique bucket name and region
5. Configure settings:
   - Enable versioning (recommended)
   - Enable encryption
   - Block public access

## IAM Permissions

Create an IAM policy with the minimum required permissions:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "s3:PutObject",
        "s3:GetObject",
        "s3:DeleteObject",
        "s3:ListBucket"
      ],
      "Resource": [
        "arn:aws:s3:::your-backup-bucket/*",
        "arn:aws:s3:::your-backup-bucket"
      ]
    }
  ]
}
```

## Configuration

### Using Access Keys

```yaml
storage:
  provider: "aws"
  aws:
    bucket_name: "my-jira-backups"
    region: "us-east-1"
    access_key_id: "AKIAIOSFODNN7EXAMPLE"
    secret_access_key: "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

### Using IAM Roles (Recommended)

If running on EC2 or ECS, you can use IAM roles:

```yaml
storage:
  provider: "aws"
  aws:
    bucket_name: "my-jira-backups"
    region: "us-east-1"
    # No credentials needed - will use instance role
```

### Using AWS Profile

```yaml
storage:
  provider: "aws"
  aws:
    bucket_name: "my-jira-backups"
    region: "us-east-1"
    profile: "backup-profile"
```

## Advanced Configuration

### S3 Storage Classes

Configure lifecycle rules to transition old backups to cheaper storage:

```yaml
storage:
  aws:
    storage_class: "STANDARD_IA"  # Options: STANDARD, STANDARD_IA, GLACIER, DEEP_ARCHIVE
```

### Server-Side Encryption

```yaml
storage:
  aws:
    encryption: "AES256"  # or "aws:kms" for KMS encryption
    kms_key_id: "arn:aws:kms:region:account:key/key-id"  # If using KMS
```

### Custom Endpoint (S3-Compatible)

For S3-compatible storage (MinIO, Wasabi, etc.):

```yaml
storage:
  aws:
    endpoint_url: "https://s3.wasabisys.com"
    bucket_name: "my-backups"
    region: "us-east-1"
```

## Testing Your Configuration

Run a test backup to verify your configuration:

```bash
python backup.py --test --service jira
```

## Troubleshooting

### Access Denied Errors

- Verify IAM permissions
- Check bucket policy
- Ensure correct region

### Connection Errors

- Check network connectivity
- Verify endpoint URL (if using custom endpoint)
- Check AWS credentials

### Region Errors

- Ensure bucket region matches configuration
- Use the correct region code (e.g., "us-east-1", not "US East")

## Cost Optimization

1. Enable S3 Intelligent-Tiering
2. Set up lifecycle policies
3. Use compression in backup settings
4. Consider using S3 Glacier for long-term retention