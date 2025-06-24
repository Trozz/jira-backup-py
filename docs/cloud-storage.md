---
layout: default
title: Cloud Storage Setup
nav_order: 4
has_children: true
---

# Cloud Storage Setup
{: .no_toc }

Jira Backup Python supports multiple cloud storage providers for storing your backups securely in the cloud.
{: .fs-6 .fw-300 }

## Supported Providers

The tool currently supports three major cloud storage providers:

1. **Amazon Web Services (AWS) S3** - Industry-leading object storage service
2. **Google Cloud Storage (GCS)** - Google's unified object storage solution  
3. **Azure Blob Storage** - Microsoft's object storage solution

Each provider offers different features, pricing models, and regional availability. Choose the one that best fits your organization's needs and existing infrastructure.

## General Considerations

### Storage Costs

- Storage costs vary by provider and region
- Consider lifecycle policies to move old backups to cheaper storage tiers
- Enable compression in the backup settings to reduce storage usage

### Security

- Use IAM roles/service accounts instead of API keys when possible
- Enable encryption at rest for your storage buckets/containers
- Implement proper access controls and audit logging

### Performance

- Choose a storage region close to your Jira/Confluence instances
- Consider using multipart uploads for large backups
- Monitor backup times and adjust settings as needed

## Next Steps

Choose your cloud storage provider:

- [AWS S3 Setup]({% link docs/cloud-storage/aws-s3.md %})
- [Google Cloud Storage Setup]({% link docs/cloud-storage/gcp.md %})
- [Azure Blob Storage Setup]({% link docs/cloud-storage/azure.md %})