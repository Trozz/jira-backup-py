---
layout: default
title: Azure Blob Storage
parent: Cloud Storage Setup
nav_order: 3
---

# Azure Blob Storage Setup
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Prerequisites

- An Azure subscription
- A storage account
- A container for storing backups
- Access keys or Azure AD authentication

## Creating Azure Storage Resources

### Create Storage Account

1. Log in to [Azure Portal](https://portal.azure.com)
2. Click "Create a resource" → "Storage account"
3. Configure:
   - Choose a unique storage account name
   - Select region close to your instances
   - Performance: Standard (recommended)
   - Redundancy: LRS, ZRS, GRS based on needs

### Create Container

1. Navigate to your storage account
2. Go to "Containers" under "Data storage"
3. Click "+ Container"
4. Set name and access level (Private recommended)

## Authentication Methods

### Using Access Keys

1. Go to your storage account
2. Navigate to "Access keys" under "Security + networking"
3. Copy one of the access keys

```yaml
storage:
  provider: "azure"
  azure:
    container_name: "jira-backups"
    account_name: "mystorageaccount"
    account_key: "your-access-key-here"
```

### Using Connection String

```yaml
storage:
  provider: "azure"
  azure:
    container_name: "jira-backups"
    connection_string: "DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey;EndpointSuffix=core.windows.net"
```

### Using Azure AD (Managed Identity)

For Azure VMs or Azure Kubernetes Service:

```yaml
storage:
  provider: "azure"
  azure:
    container_name: "jira-backups"
    account_name: "mystorageaccount"
    use_managed_identity: true
```

### Using SAS Token

```yaml
storage:
  provider: "azure"
  azure:
    container_name: "jira-backups"
    account_name: "mystorageaccount"
    sas_token: "?sv=2021-06-08&ss=b&srt=sco&sp=rwdlacx&se=2024-12-31T23:59:59Z&..."
```

## Advanced Configuration

### Access Tiers

Configure blob access tiers for cost optimization:

```yaml
storage:
  azure:
    access_tier: "Hot"  # Options: Hot, Cool, Archive
```

### Encryption

Azure Storage encrypts all data at rest by default. For customer-managed keys:

```yaml
storage:
  azure:
    encryption:
      key_vault_url: "https://myvault.vault.azure.net/"
      key_name: "my-encryption-key"
      key_version: "key-version-id"
```

### Blob Metadata

Add custom metadata to backups:

```yaml
storage:
  azure:
    metadata:
      environment: "production"
      retention: "30days"
```

## RBAC Configuration

### Create Custom Role

For least-privilege access, create a custom role:

```json
{
  "Name": "Jira Backup Contributor",
  "Description": "Can upload and manage Jira backups",
  "Actions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/read",
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write",
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete"
  ],
  "DataActions": [
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/read",
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/write",
    "Microsoft.Storage/storageAccounts/blobServices/containers/blobs/delete"
  ],
  "AssignableScopes": [
    "/subscriptions/{subscription-id}/resourceGroups/{resource-group}/providers/Microsoft.Storage/storageAccounts/{storage-account}"
  ]
}
```

### Assign Role

1. Go to your storage account
2. Click "Access control (IAM)"
3. Add role assignment
4. Select your custom role or "Storage Blob Data Contributor"
5. Assign to user, group, or service principal

## Lifecycle Management

Configure lifecycle rules to optimize costs:

1. Go to your storage account
2. Navigate to "Lifecycle management"
3. Add rules to:
   - Move blobs to Cool tier after 30 days
   - Move to Archive tier after 90 days
   - Delete after retention period

Example lifecycle rule:
```json
{
  "rules": [
    {
      "name": "archiveOldBackups",
      "type": "Lifecycle",
      "definition": {
        "filters": {
          "blobTypes": ["blockBlob"],
          "prefixMatch": ["jira-backup"]
        },
        "actions": {
          "baseBlob": {
            "tierToCool": {
              "daysAfterModificationGreaterThan": 30
            },
            "tierToArchive": {
              "daysAfterModificationGreaterThan": 90
            },
            "delete": {
              "daysAfterModificationGreaterThan": 365
            }
          }
        }
      }
    }
  ]
}
```

## Testing Configuration

Run a test backup:

```bash
python backup.py --test --service jira
```

## Monitoring

### Enable Diagnostics

1. Go to your storage account
2. Navigate to "Diagnostic settings"
3. Add diagnostic setting
4. Select metrics and logs to collect
5. Send to Log Analytics, Event Hub, or Archive

### Set Up Alerts

1. Navigate to "Alerts" under "Monitoring"
2. Create alert rules for:
   - Failed authentication attempts
   - Storage capacity usage
   - Transaction anomalies

## Troubleshooting

### Authentication Failures

- Verify account name and key
- Check firewall rules on storage account
- Ensure correct Azure AD permissions if using managed identity

### Network Issues

- Check storage account firewall settings
- Verify network connectivity
- Enable "Allow trusted Microsoft services" if needed

### Performance Issues

- Use appropriate access tier
- Consider using premium storage for large backups
- Enable large file shares if needed

## Cost Optimization

1. **Use Lifecycle Management**: Automatically move old backups to cheaper tiers
2. **Choose Correct Redundancy**: LRS is cheapest, GRS for geo-redundancy
3. **Monitor Usage**: Use Azure Cost Management to track spending
4. **Reserved Capacity**: Consider reserved capacity for predictable workloads
5. **Delete Unnecessary Backups**: Implement retention policies