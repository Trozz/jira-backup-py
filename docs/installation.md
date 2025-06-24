---
layout: default
title: Installation
nav_order: 2
---

# Installation Guide
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## System Requirements

Before installing Jira Backup Python, ensure your system meets the following requirements:

- Python 3.7 or higher
- pip (Python package installer)
- Git (for cloning the repository)
- Access to Jira and/or Confluence instances
- Cloud storage account (AWS, GCP, or Azure)

## Installation Steps

### 1. Clone the Repository

```bash
git clone https://github.com/yourusername/jira-backup-py.git
cd jira-backup-py
```

### 2. Create a Virtual Environment

It's recommended to use a virtual environment to avoid conflicts with other Python packages:

```bash
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
```

### 3. Install Dependencies

```bash
pip install -r requirements.txt
```

### 4. Verify Installation

```bash
python backup.py --help
```

You should see the help message with available commands and options.

## Configuration

After installation, you need to configure the tool:

1. Copy the example configuration file:
   ```bash
   cp config.example.yaml config.yaml
   ```

2. Edit `config.yaml` with your settings. See the [Configuration Guide]({% link docs/configuration.md %}) for details.

## Docker Installation (Optional)

If you prefer using Docker:

```bash
docker build -t jira-backup-py .
docker run -v $(pwd)/config.yaml:/app/config.yaml jira-backup-py
```

## Next Steps

- [Configure your backup settings]({% link docs/configuration.md %})
- [Set up cloud storage]({% link docs/cloud-storage.md %})
- [Schedule automated backups]({% link docs/scheduling.md %})