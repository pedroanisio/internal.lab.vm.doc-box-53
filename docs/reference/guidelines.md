Claro! Aqui estão todos os arquivos separados em codeblocks distintos, com seus conteúdos completos conforme o que você compartilhou.

---

### 📄 `docs/reference/purpose.md`

```markdown
## 🧠 `purpose.md` – Purpose of This Documentation Node

## Primary Function
This VM serves as the centralized documentation hub for our entire homelab infrastructure. It hosts multiple MkDocs instances via Docker, providing a clean and scalable documentation system.

## Business Justification
Maintaining comprehensive documentation for our homelab helps track status, configuration, and connections between services. The documentation server provides:
- A single source of truth for all homelab infrastructure
- Easy access to server configurations and maintenance procedures
- Knowledge preservation for future reference and troubleshooting

## Key Applications/Services
- **MkDocs Material**: The main documentation framework that renders Markdown as beautiful documentation sites
- **Traefik**: Reverse proxy that provides subdomain routing to each documentation instance
- **Watchtower**: Automatic Docker image updates to ensure documentation containers stay current

## Dependencies
- **Depends on**: Network infrastructure, DNS server
- **Depended on by**: All homelab services (for documentation access)

## Lifecycle Information
- **Created**: March 2025
- **Planned retirement**: None (core infrastructure)
- **Maintenance window**: Sundays, 2 AM - 4 AM

## Notes
The documentation system follows a hierarchical structure with reversed hierarchy naming (internal.lab.vm.{vm-name}) to maintain consistency and traceability across all documentation sets. Each VM has its own documentation container, with a planned central entry point to connect all documentation instances.
```

---

### 📄 `docs/reference/templates.md`

```markdown
# VM Documentation Template

This template provides a standardized structure for documenting virtual machines in your homelab. Follow this structure for each VM to maintain consistency across your documentation.

## Project Structure

```
/srv/docs/repos/internal.lab.vm.{vm-name}/
├── docs/
│   ├── index.md              # Home page / Overview
│   ├── reference/
│   │   ├── server.md         # Server specifications
│   │   ├── purpose.md        # Purpose and usage documentation
│   │   ├── network.md        # Network configuration
│   │   └── maintenance/
│   │       └── backups.md    # Backup procedures
│   ├── services/
│   │   └── docker.md         # Docker containers overview
│   └── assets/
│       └── logo.png          # Logo for documentation
└── mkdocs.yml                # MkDocs configuration file
```

## server.md Template

```markdown
# Server Specifications - {vm-name}

## Server Specifications

| Property     | Value                  |
|--------------|------------------------|
| **Hypervisor** | {hypervisor}           |
| **vCPU**     | {vcpu}                 |
| **RAM**      | {ram}                  |
| **Storage**  | {storage}              |
| **OS**       | {os}                   |
| **IP**       | {ip}                   |
| **Hostname** | {hostname}             |
| **Domain**   | internal.lab.vm        |

## Description
{description of the VM's purpose and role in the homelab}

## Services Running
- **{Service 1}**: {description and ports}
- **{Service 2}**: {description and ports}
- **{Service 3}**: {description and ports}

## Networking Configuration
- Network configured with static IP {ip}
- {Ports exposed}
- {Other networking details}

## Maintenance Notes
- {Backup schedule}
- {Update schedule}
- {Other maintenance information}

## Related Documentation
- [Network Configuration](network.md)
- [Backup Procedures](maintenance/backups.md)
- [Docker Containers Overview](services/docker.md)
```

## purpose.md Template

```markdown
# Purpose - {vm-name}

## Primary Function
{Describe the primary function and purpose of this VM}

## Business Justification
{Explain why this VM exists and what business/homelab needs it addresses}

## Key Applications/Services
- **{Service 1}**: {purpose and importance}
- **{Service 2}**: {purpose and importance}
- **{Service 3}**: {purpose and importance}

## Dependencies
- **Depends on**: {list of VMs/services this VM depends on}
- **Depended on by**: {list of VMs/services that depend on this VM}

## Lifecycle Information
- **Created**: {creation date}
- **Planned retirement**: {if applicable}
- **Maintenance window**: {regular maintenance time}

## Notes
{Any additional notes about the purpose or usage of this VM}
```

## mkdocs.yml Template

```yaml
site_name: VM {vm-name}
nav:
  - Home: index.md
  - Reference:
    - Server Specifications: reference/server.md
    - Purpose: reference/purpose.md
    - Network Configuration: reference/network.md
    - Maintenance:
      - Backup Procedures: reference/maintenance/backups.md
  - Services:
    - Docker Containers: services/docker.md

theme:
  name: material
  features:
    - content.code.copy
    - navigation.instant
    - navigation.tracking
    - navigation.expand
    - navigation.indexes
    - toc.follow
  logo: assets/logo.png
  palette:
    primary: indigo
    accent: indigo

markdown_extensions:
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.details
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.keys
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
```

## Docker Compose Template

```yaml
version: '3'

services:
  {vm-name}:
    image: squidfunk/mkdocs-material
    container_name: docs-{vm-name}
    ports:
      - "{port}:8000"
    volumes:
      - ./repos/internal.lab.vm.{vm-name}:/docs
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.{vm-name}.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/{vm-name}`)"
      - "traefik.http.routers.{vm-name}.entrypoints=web,websecure"
      - "traefik.http.services.{vm-name}.loadbalancer.server.port=8000"
```

---

### 📄 `docs/reference/server.md`

```markdown
## 🖥️ `server.md` – Server Specifications: `doc-box-53`

### Server Overview

| Property       | Value              |
|----------------|-------------------|
| **Hypervisor** | Proxmox VE 8.0.4  |
| **vCPU**       | 2                 |
| **RAM**        | 1024 GiB          |
| **Storage**    | 50 GiB SSD        |
| **OS**         | Debian 12         |
| **IP**         | 10.10.10.53        |
| **Hostname**   | doc-box-53        |
| **Domain**     | internal.lab.vm   |

### Description

This VM (`doc-box-53`) acts as the **documentation hub** for the homelab. It runs Docker containers, each serving a different MkDocs instance for specific topics, mapped to subdomains or internal routes.

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
```

---

### 📄 `docs/reference/guidelines.md`

```markdown
# Implementation Guide for VM Documentation

This guide explains how to implement the documentation template for all your homelab VMs.

## Setup Process

### 1. Create the Base Directory Structure

```bash
mkdir -p /srv/docs/repos/internal.lab.vm.{vm-name}/docs/reference/maintenance
mkdir -p /srv/docs/repos/internal.lab.vm.{vm-name}/docs/services
mkdir -p /srv/docs/repos/internal.lab.vm.{vm-name}/docs/assets
```

### 2. Create Base Documentation Files

- `index.md`
- `reference/server.md`
- `reference/purpose.md`
- `reference/network.md`
- `reference/maintenance/backups.md`
- `services/docker.md`
- `mkdocs.yml`

### 3. Initialize Git Repository

```bash
cd /srv/docs/repos/internal.lab.vm.{vm-name}
git init
git add .
git commit -m "Initial documentation structure"
```

### 4. Update Docker Compose

```yaml
services:
  {vm-name}:
    image: squidfunk/mkdocs-material
    container_name: docs-{vm-name}
    ports:
      - "{port}:8000"
    volumes:
      - ./repos/internal.lab.vm.{vm-name}:/docs
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.{vm-name}.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/{vm-name}`)"
      - "traefik.http.routers.{vm-name}.entrypoints=web,websecure"
      - "traefik.http.services.{vm-name}.loadbalancer.server.port=8000"
```

### 5. Start the New Documentation Container

```bash
docker-compose up -d {vm-name}
```

## Maintenance & Best Practices

- Commit changes to docs regularly
- Link related documentation
- Use `git pull && docker restart` for automation
- Use `watchtower` or update manually

## Central Documentation Portal (Future)

```yaml
nav:
  - Virtual Machines:
    - doc-box-53: http://docs.internal.lab.vm/doc-box-53/
```
```

---


---

### 📄 `mkdocs.yml`

```yaml
site_name: VM doc-box-53

nav:
  - Home: index.md
  - Reference:
    - Server Specifications: reference/server.md
    - Purpose: reference/purpose.md
    - Network Configuration: reference/network.md
    - Maintenance:
      - Backup Procedures: reference/maintenance/backups.md
  - Services:
    - Docker Containers: services/docker.md

theme:
  name: material
  features:
    - content.code.copy
    - navigation.instant
    - navigation.tracking
    - navigation.expand
    - navigation.indexes
    - toc.follow
  logo: assets/logo.png
  palette:
    primary: indigo
    accent: indigo

markdown_extensions:
  - abbr
  - admonition
  - attr_list
  - def_list
  - footnotes
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.arithmatex:
      generic: true
  - pymdownx.betterem:
      smart_enable: all
  - pymdownx.caret
  - pymdownx.details
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg
  - pymdownx.highlight:
      anchor_linenums: true
  - pymdownx.inlinehilite
  - pymdownx.keys
  - pymdownx.magiclink
  - pymdownx.mark
  - pymdownx.smartsymbols
  - pymdownx.superfences:
      custom_fences:
        - name: mermaid
          class: mermaid
          format: !!python/name:pymdownx.superfences.fence_code_format
  - pymdownx.tabbed:
      alternate_style: true
  - pymdownx.tasklist:
      custom_checkbox: true
  - pymdownx.tilde
```
