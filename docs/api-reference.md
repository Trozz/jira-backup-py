---
layout: default
title: API Reference
nav_order: 8
---

# API Reference
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## Core Modules

### backup.py

Main entry point for the backup application.

#### Functions

**main()**
```python
def main(args: Optional[List[str]] = None) -> int:
    """
    Main entry point for the backup application.
    
    Args:
        args: Command line arguments (optional)
        
    Returns:
        Exit code (0 for success, non-zero for failure)
    """
```

**parse_arguments()**
```python
def parse_arguments() -> argparse.Namespace:
    """
    Parse command line arguments.
    
    Returns:
        Parsed arguments namespace
    """
```

### backup_manager.py

Manages the backup process for Jira and Confluence.

#### Classes

**BackupManager**
```python
class BackupManager:
    """Manages backup operations for Jira and Confluence."""
    
    def __init__(self, config: Dict[str, Any]):
        """
        Initialize the backup manager.
        
        Args:
            config: Configuration dictionary
        """
        
    def backup_jira(self) -> str:
        """
        Perform Jira backup.
        
        Returns:
            Backup ID or path
            
        Raises:
            BackupError: If backup fails
        """
        
    def backup_confluence(self) -> str:
        """
        Perform Confluence backup.
        
        Returns:
            Backup ID or path
            
        Raises:
            BackupError: If backup fails
        """
```

### Storage Providers

#### storage/s3.py

**S3Storage**
```python
class S3Storage(StorageProvider):
    """AWS S3 storage provider."""
    
    def __init__(self, config: Dict[str, Any]):
        """
        Initialize S3 storage.
        
        Args:
            config: S3 configuration containing:
                - bucket_name: S3 bucket name
                - region: AWS region
                - access_key_id: AWS access key (optional)
                - secret_access_key: AWS secret key (optional)
        """
        
    def upload(self, file_path: str, remote_path: str) -> bool:
        """
        Upload file to S3.
        
        Args:
            file_path: Local file path
            remote_path: Remote path in bucket
            
        Returns:
            True if successful
            
        Raises:
            S3UploadError: If upload fails
        """
        
    def download(self, remote_path: str, local_path: str) -> bool:
        """
        Download file from S3.
        
        Args:
            remote_path: Remote path in bucket
            local_path: Local file path
            
        Returns:
            True if successful
        """
        
    def list_backups(self, prefix: str = "") -> List[Dict[str, Any]]:
        """
        List backups in bucket.
        
        Args:
            prefix: Path prefix to filter
            
        Returns:
            List of backup metadata
        """
```

#### storage/gcs.py

**GCSStorage**
```python
class GCSStorage(StorageProvider):
    """Google Cloud Storage provider."""
    
    def __init__(self, config: Dict[str, Any]):
        """
        Initialize GCS storage.
        
        Args:
            config: GCS configuration containing:
                - bucket_name: GCS bucket name
                - project_id: GCP project ID
                - credentials_path: Path to service account key (optional)
        """
```

#### storage/azure.py

**AzureStorage**
```python
class AzureStorage(StorageProvider):
    """Azure Blob Storage provider."""
    
    def __init__(self, config: Dict[str, Any]):
        """
        Initialize Azure storage.
        
        Args:
            config: Azure configuration containing:
                - container_name: Container name
                - account_name: Storage account name
                - account_key: Storage account key (optional)
                - connection_string: Connection string (optional)
        """
```

### Atlassian API Clients

#### atlassian/jira_client.py

**JiraClient**
```python
class JiraClient:
    """Client for Jira API operations."""
    
    def __init__(self, url: str, username: str, api_token: str):
        """
        Initialize Jira client.
        
        Args:
            url: Jira instance URL
            username: User email
            api_token: API token
        """
        
    def create_backup(self, include_attachments: bool = True) -> str:
        """
        Create a backup export.
        
        Args:
            include_attachments: Include attachments in backup
            
        Returns:
            Task ID for the backup job
        """
        
    def get_backup_status(self, task_id: str) -> Dict[str, Any]:
        """
        Get backup task status.
        
        Args:
            task_id: Backup task ID
            
        Returns:
            Status information
        """
        
    def download_backup(self, download_url: str, output_path: str) -> None:
        """
        Download backup file.
        
        Args:
            download_url: URL to download backup
            output_path: Local path to save file
        """
```

#### atlassian/confluence_client.py

**ConfluenceClient**
```python
class ConfluenceClient:
    """Client for Confluence API operations."""
    
    def __init__(self, url: str, username: str, api_token: str):
        """
        Initialize Confluence client.
        
        Args:
            url: Confluence instance URL
            username: User email
            api_token: API token
        """
```

### Scheduler

#### scheduler.py

**BackupScheduler**
```python
class BackupScheduler:
    """Manages scheduled backup tasks."""
    
    def __init__(self, config: Dict[str, Any]):
        """
        Initialize scheduler.
        
        Args:
            config: Scheduler configuration
        """
        
    def start(self) -> None:
        """Start the scheduler."""
        
    def stop(self) -> None:
        """Stop the scheduler."""
        
    def add_job(self, service: str, schedule: str) -> None:
        """
        Add a scheduled job.
        
        Args:
            service: Service to backup (jira/confluence)
            schedule: Cron expression
        """
```

## Configuration Schema

### Config Structure

```python
ConfigSchema = {
    "jira": {
        "url": str,
        "username": str,
        "api_token": str,
        "backup_path": str
    },
    "confluence": {
        "url": str,
        "username": str,
        "api_token": str,
        "backup_path": str
    },
    "storage": {
        "provider": str,  # "aws", "gcp", or "azure"
        "aws": {...},     # AWS-specific config
        "gcp": {...},     # GCP-specific config
        "azure": {...}    # Azure-specific config
    },
    "backup": {
        "retention_days": int,
        "compression": bool,
        "include_attachments": bool,
        "temp_directory": str
    },
    "scheduler": {
        "enabled": bool,
        "jira_schedule": str,
        "confluence_schedule": str
    }
}
```

## Exceptions

### Custom Exceptions

```python
class BackupError(Exception):
    """Base exception for backup errors."""

class ConfigurationError(BackupError):
    """Configuration-related errors."""

class AuthenticationError(BackupError):
    """Authentication failures."""

class StorageError(BackupError):
    """Storage operation errors."""

class AtlassianAPIError(BackupError):
    """Atlassian API errors."""
```

## Utilities

### utils/config.py

**load_config()**
```python
def load_config(config_path: str = "config.yaml") -> Dict[str, Any]:
    """
    Load configuration from YAML file.
    
    Args:
        config_path: Path to configuration file
        
    Returns:
        Configuration dictionary
        
    Raises:
        ConfigurationError: If config is invalid
    """
```

**validate_config()**
```python
def validate_config(config: Dict[str, Any]) -> None:
    """
    Validate configuration structure.
    
    Args:
        config: Configuration dictionary
        
    Raises:
        ConfigurationError: If validation fails
    """
```

### utils/logging.py

**setup_logging()**
```python
def setup_logging(level: str = "INFO", log_file: Optional[str] = None) -> None:
    """
    Configure logging.
    
    Args:
        level: Log level (DEBUG, INFO, WARNING, ERROR)
        log_file: Optional log file path
    """
```

## Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `JIRA_API_TOKEN` | Jira API token | None |
| `CONFLUENCE_API_TOKEN` | Confluence API token | None |
| `AWS_ACCESS_KEY_ID` | AWS access key | None |
| `AWS_SECRET_ACCESS_KEY` | AWS secret key | None |
| `GOOGLE_APPLICATION_CREDENTIALS` | GCP credentials path | None |
| `AZURE_STORAGE_CONNECTION_STRING` | Azure connection string | None |