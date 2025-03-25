# 💾 Backup Procedures

This page documents the backup procedures for `doc-box-53` and its documentation repositories.

## VM Backup

The `doc-box-53` virtual machine is backed up automatically through Proxmox:

| Backup Detail | Value |
|---------------|-------|
| **Schedule** | Daily @ 11:37 UTC |
| **Retention** | 7 days of daily backups |
| **Location** | PBS storage on `pbs-box-73` |
| **Type** | Full VM backup (including all disks) |
| **Verification** | Weekly automated backup verification |

## Documentation Content Backup

In addition to the VM-level backup, the documentation content is backed up separately:

### Git Repository Backups

All documentation repositories are version-controlled with Git:

1. Every repository is automatically backed up with each commit
2. Remote repositories are stored on the internal GitLab instance (`git.internal.lab.vm`)
3. Repository backups are included in the GitLab backup procedure

### Manual Content Export

A monthly export of all documentation repositories is performed:

```bash
# Script location: /usr/local/bin/export-docs.sh
cd /srv/docs
tar -czf /backup/doc-exports/docs-export-$(date +%Y%m%d).tar.gz repos/
```

## Restoration Procedure

### VM Restoration

To restore the entire VM from backup:

1. Access the Proxmox web interface
2. Navigate to the Backup section
3. Select the appropriate backup of `doc-box-53`
4. Click "Restore" and select the target node
5. Verify network settings before restoring

### Content Restoration

To restore documentation content:

1. Clone the repository from GitLab:
   ```bash
   git clone http://git.internal.lab.vm/homelab/internal.lab.vm.doc-box-53.git
   ```

2. If GitLab is unavailable, restore from the exported archive:
   ```bash
   tar -xzf /backup/doc-exports/docs-export-YYYYMMDD.tar.gz -C /tmp/
   cp -r /tmp/repos/internal.lab.vm.doc-box-53 /srv/docs/repos/
   ```

3. Restart the documentation container:
   ```bash
   cd /srv/docs
   docker-compose restart doc-box-53
   ```

## Backup Monitoring

Backups are monitored through:

- Daily email reports from Proxmox backup jobs
- Monitoring dashboard alerts for failed backups
- Weekly backup verification tests

## Emergency Recovery Contact

In case of backup or restoration issues, contact:

- Primary: `admin@internal.lab.vm`
- Secondary: Infrastructure team via internal chat