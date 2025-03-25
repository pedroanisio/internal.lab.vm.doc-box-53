## 🖥️ Server Specifications: `doc-box-53`

### Overview

| Property       | Value              |
|----------------|-------------------|
| **Hypervisor** | Proxmox VE 8.3.5 (pve-box-2)  |
| **vCPU**       | 2                 |
| **RAM**        | 1024 GiB          |
| **Storage**    | 64 GiB SSD        |
| **OS**         | Debian 12.10         |
| **IP**         | 10.10.10.53      |
| **Hostname**   | doc-box-53        |
| **Domain**     | internal.lab.vm   |
| **POOL**     | mngmt   |
| **Backup**     | Daily @ 11:37 UTC  |

### Description

This VM (`doc-box-53`) acts as the **documentation hub** for the homelab. It
runs Docker containers, each serving a different MkDocs instance for specific
topics, mapped to subdomains or internal routes.

### Services

- **MkDocs Material**: Serving multiple docs (ports 8001–8010)
- **Traefik**: Reverse proxy for centralized routing (ports 80/443)
- **Watchtower**: Docker image updater

### Networking

- Static IP: `10.10.10.53`
- Exposed ports: `80`, `443`, `8001-8010`

### Maintenance Notes

- Git-push triggers for rebuild and restart
- Backups stored at `pbs-box-73`
