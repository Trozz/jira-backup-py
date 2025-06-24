---
layout: home
title: Home
nav_order: 1
description: "Jira Backup Python is a comprehensive backup solution for Jira and Confluence with support for multiple cloud storage providers."
permalink: /
---

# Jira Backup Python Documentation
{: .fs-9 }

A Python-based tool for backing up Jira and Confluence to multiple cloud storage providers.
{: .fs-6 .fw-300 }

[Get started now](#getting-started){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 .mr-2 }
[View it on GitHub](https://github.com/yourusername/jira-backup-py){: .btn .fs-5 .mb-4 .mb-md-0 }

---

## Overview

Jira Backup Python is a powerful and flexible backup solution designed to help organizations protect their Jira and Confluence data. With support for multiple cloud storage providers including AWS S3, Google Cloud Storage, and Azure Blob Storage, you can ensure your critical project management and documentation data is safely backed up according to your organization's requirements.

### Key Features

- **Multi-Cloud Support**: Back up to AWS S3, Google Cloud Storage, or Azure Blob Storage
- **Automated Scheduling**: Set up automated backups using cron expressions
- **Flexible Configuration**: Configure backup retention policies and storage options
- **Secure**: Supports various authentication methods for each cloud provider
- **Easy to Use**: Simple command-line interface and configuration file

## Getting Started

### Prerequisites

- Python 3.7 or higher
- Access to Jira/Confluence instances
- Cloud storage account (AWS, GCP, or Azure)

### Quick Start

1. Install the package:
   ```bash
   pip install -r requirements.txt
   ```

2. Copy and configure the settings:
   ```bash
   cp config.example.yaml config.yaml
   ```

3. Run your first backup:
   ```bash
   python backup.py
   ```

## Documentation

- [Installation Guide]({% link docs/installation.md %})
- [Configuration]({% link docs/configuration.md %})
- [Cloud Storage Setup]({% link docs/cloud-storage.md %})
- [Scheduling Backups]({% link docs/scheduling.md %})
- [API Reference]({% link docs/api-reference.md %})

## About the Project

Jira Backup Python is &copy; 2024 by [Your Name].

### License

Jira Backup Python is distributed by an [MIT license](https://github.com/yourusername/jira-backup-py/blob/master/LICENSE).

### Contributing

We welcome contributions! Please see our [Contributing Guidelines]({% link docs/contributing.md %}) for details.