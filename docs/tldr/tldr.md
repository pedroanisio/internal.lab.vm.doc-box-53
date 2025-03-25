## Documentation VM (doc-box-53) Summary

**Purpose**: Central documentation hub for the entire homelab infrastructure using MkDocs Material.

**Key Details**:
- **IP**: 10.10.10.53
- **Hostname**: doc-box-53.internal.lab.vm
- **OS**: Debian 12.10
- **Resources**: 2 vCPU, 1024 GiB RAM, 64 GiB SSD
- **Hypervisor**: Proxmox VE 8.3.5 (pve-box-2)

**Services**:
- Multiple MkDocs Material instances (ports 8001-8010)
- Traefik for reverse proxy (ports 80/443)
- Watchtower for automatic Docker updates

**Architecture**:
- Documentation is organized in Git repositories
- Each VM has its own documentation container
- Consistent template structure for all VM documentation
- Accessible via https://docs.internal.lab.vm/{vm-name}

**Maintenance**:
- Daily backups @ 11:37 UTC to pbs-box-73
- Weekly system updates (security)
- Monthly full system updates
- Maintenance window: Sundays, 2 AM - 4 AM

**Documentation Structure**:
- Follows a standard template for all VMs
- Includes purpose, server specs, services, and maintenance docs
- Uses consistent naming convention: internal.lab.vm.{vm-name}

**Contact**: admin@internal.lab.vm