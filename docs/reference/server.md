## 🖥️ `server.md` – Server Specifications: `doc-box-53`

### Server Overview

| Property       | Value              |
|----------------|-------------------|
| **Hypervisor** | Proxmox VE 8.0.4  |
| **vCPU**       | 2                 |
| **RAM**        | 1024 GiB          |
| **Storage**    | 50 GiB SSD        |
| **OS**         | Debian 12         |
| **IP**         | 10.10.10.53      |
| **Hostname**   | doc-box-53        |
| **Domain**     | internal.lab.vm   |

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

- Weekly auto-update of images (Sunday @ 2 AM)
- Git-push triggers for rebuild and restart
- Backups stored at `/srv/backups` (daily)
