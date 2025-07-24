# 🔄 Update Process

This document outlines the update procedures for `doc-box-53` and its associated services.

## VM System Updates

The Debian system is updated according to the following schedule:

| Update Type | Schedule | Process |
|-------------|----------|---------|
| **Security Updates** | Weekly (automated) | Unattended-upgrades package |
| **System Updates** | Monthly | Manual during maintenance window |
| **OS Version Upgrade** | As needed | Planned and announced 2 weeks in advance |

### Manual Update Procedure

```bash
# Update package lists
sudo apt update

# Install available updates
sudo apt upgrade -y

# Check if reboot is required
if [ -f /var/run/reboot-required ]; then
  echo "Reboot required - scheduling for maintenance window"
  # Schedule reboot during maintenance window
  sudo shutdown -r 02:00
fi
```

## Docker Container Updates

Docker containers are automatically updated using Watchtower:

- Watchtower checks for new images every 30 minutes
- Updates are performed automatically if new images are available
- All container updates are logged to the system journal

### MkDocs Material Updates

The MkDocs Material container is configured to use the latest version:

```yaml
image: squidfunk/mkdocs-material:latest
```

For specific version pinning needs, edit the docker-compose.yml file and specify the version:

```yaml
image: squidfunk/mkdocs-material:9.1.3
```

## Documentation Content Updates

Documentation content is updated through Git:

1. Make changes to the documentation repository
2. Commit and push changes to the Git repository
3. The MkDocs container automatically detects changes and rebuilds the site

## Maintenance Window

Scheduled maintenance occurs during the following window:

- **Day**: Sundays
- **Time**: 2 AM - 4 AM
- **Frequency**: Weekly

During this window:
- System updates are applied
- Reboots are performed if needed
- Major version upgrades are scheduled

## Update Notifications

System and documentation updates are communicated through:

- Internal notification system for planned maintenance
- Changelog entries in the documentation itself
- System status dashboard updates during maintenance windows

## Rollback Procedure

In case an update causes issues:

### System Rollback

```bash
# If a package update causes issues
sudo apt install ppa-purge
sudo ppa-purge <problematic-ppa>

# For major issues, restore from the most recent VM backup
```

### Docker Rollback

```bash
# Roll back to a specific image version
docker compose down
# Edit docker-compose.yml to specify previous version
docker compose up -d
```

### Documentation Rollback

```bash
# Revert to a previous Git commit
git revert <commit-hash>
git push
```