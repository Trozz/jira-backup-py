---
layout: default
title: Contributing
nav_order: 7
---

# Contributing Guidelines
{: .no_toc }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

---

## How to Contribute

We welcome contributions to Jira Backup Python! This document provides guidelines for contributing to the project.

## Getting Started

1. Fork the repository
2. Clone your fork:
   ```bash
   git clone https://github.com/yourusername/jira-backup-py.git
   cd jira-backup-py
   ```
3. Create a virtual environment:
   ```bash
   python -m venv venv
   source venv/bin/activate
   ```
4. Install development dependencies:
   ```bash
   pip install -r requirements-dev.txt
   ```

## Development Process

### 1. Create a Feature Branch

```bash
git checkout -b feature/your-feature-name
```

Use descriptive branch names:
- `feature/add-azure-support`
- `fix/authentication-timeout`
- `docs/update-installation-guide`

### 2. Make Your Changes

- Write clean, readable code
- Follow existing code style
- Add comments for complex logic
- Update documentation as needed

### 3. Write Tests

- Add unit tests for new functionality
- Ensure all tests pass:
  ```bash
  pytest
  ```
- Maintain or improve code coverage

### 4. Commit Your Changes

Use conventional commits:
```bash
git commit -m "feat: add support for Azure Blob Storage"
git commit -m "fix: resolve timeout issue during large backups"
git commit -m "docs: update configuration examples"
```

### 5. Submit a Pull Request

1. Push your branch to GitHub
2. Open a pull request against the `main` branch
3. Fill out the PR template
4. Wait for review and address feedback

## Code Style

### Python Style Guide

- Follow PEP 8
- Use type hints where appropriate
- Maximum line length: 88 characters (Black default)

### Formatting

Use Black for code formatting:
```bash
black .
```

### Linting

Run linters before submitting:
```bash
flake8 .
mypy .
```

## Testing

### Unit Tests

- Place tests in the `tests/` directory
- Use pytest for testing
- Mock external dependencies
- Aim for >80% code coverage

### Integration Tests

- Test actual cloud storage operations
- Use test accounts/buckets
- Clean up test data after runs

### Running Tests

```bash
# Run all tests
pytest

# Run with coverage
pytest --cov=backup --cov-report=html

# Run specific test file
pytest tests/test_s3.py
```

## Documentation

### Code Documentation

- Add docstrings to all functions and classes
- Use Google-style docstrings:
  ```python
  def upload_to_s3(file_path: str, bucket: str) -> bool:
      """Upload a file to S3.
      
      Args:
          file_path: Path to the file to upload
          bucket: Name of the S3 bucket
          
      Returns:
          True if upload successful, False otherwise
          
      Raises:
          S3UploadError: If upload fails
      """
  ```

### User Documentation

- Update relevant documentation in `docs/`
- Include examples for new features
- Keep language clear and concise

## Pull Request Guidelines

### PR Title

Use conventional commit format:
- `feat: ` for new features
- `fix: ` for bug fixes
- `docs: ` for documentation updates
- `refactor: ` for code refactoring
- `test: ` for test additions/changes
- `chore: ` for maintenance tasks

### PR Description

Include:
- Summary of changes
- Related issue number (if applicable)
- Testing performed
- Screenshots (if UI changes)

### PR Checklist

- [ ] Tests pass locally
- [ ] Code follows style guidelines
- [ ] Documentation updated
- [ ] Commit messages follow conventions
- [ ] Branch is up to date with main

## Reporting Issues

### Bug Reports

Include:
- Python version
- Operating system
- Configuration (sanitized)
- Steps to reproduce
- Expected vs actual behavior
- Error messages/logs

### Feature Requests

Describe:
- Use case
- Proposed solution
- Alternative solutions considered
- Impact on existing functionality

## Community

### Code of Conduct

- Be respectful and inclusive
- Welcome newcomers
- Provide constructive feedback
- Focus on what is best for the community

### Getting Help

- Check existing issues first
- Ask questions in discussions
- Join our community chat (if available)

## Release Process

1. Update version in `__version__.py`
2. Update CHANGELOG.md
3. Create release PR
4. Tag release after merge
5. Build and publish to PyPI

## License

By contributing, you agree that your contributions will be licensed under the project's MIT License.