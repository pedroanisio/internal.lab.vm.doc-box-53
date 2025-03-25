## VM Documentation Template

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

### 📄 `docs/reference/purpose.md`

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

### 📄 `docs/reference/server.md`

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

### 📄 `mkdocs.yml`

```yaml
site_name: VM {vm-name}
nav:
  - Home: index.md
  - Reference:
    - Server Specifications: reference/server.md
    - Purpose: reference/purpose.md
    - Network Configuration: reference/network.md
    - Any aditional section: section{...}.md
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

## index.md Template

```markdown
# {vm-name} Documentation

Welcome to the documentation for {vm-name}, part of the internal.lab.vm homelab infrastructure.

## Overview

This VM serves as {brief description of purpose}. This documentation covers the server specifications, configuration, maintenance procedures, and services running on this machine.

## Quick Facts

- **Hostname**: {hostname}
- **IP Address**: {ip}
- **Primary Services**: {main services}
- **OS**: {os}

## Navigation

Use the sidebar to navigate through the documentation:

- **Reference** - Technical specifications and configuration details
- **Services** - Information about services running on this VM
- **Maintenance** - Backup and maintenance procedures

## Recent Changes

- {date}: {change description}
- {date}: {change description}
- {date}: {change description}

## Contact

For issues or questions regarding this VM, contact {contact information}.
```

## Docker Compose Template for MkDocs Hosting

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