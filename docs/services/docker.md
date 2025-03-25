# 🐋 Docker Containers

This page documents the Docker containers running on `doc-box-53` and their configurations.

## Overview

All containers are defined in a single `docker-compose.yml` file located at `/srv/docs/docker-compose.yml`.

## Container List

| Container | Image | Port | Purpose |
|-----------|-------|------|---------|
| **docs-doc-box-53** | squidfunk/mkdocs-material | 8001:8000 | Documentation for doc-box-53 itself |
| **docs-arc4d3** | squidfunk/mkdocs-material | 8002:8000 | Documentation for arc4d3 project |
| **traefik** | traefik:latest | 80:80, 443:443 | Reverse proxy for all documentation sites |
| **watchtower** | containrrr/watchtower | - | Automatic container updates |

## Docker Compose Configuration

```yaml
version: '3'

services:
  doc-box-53:
    image: squidfunk/mkdocs-material
    container_name: docs-doc-box-53
    ports:
      - "8001:8000"
    volumes:
      - ./repos/internal.lab.vm.doc-box-53:/docs
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.doc-box-53.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/doc-box-53`)"
      - "traefik.http.routers.doc-box-53.entrypoints=web,websecure"
      - "traefik.http.services.doc-box-53.loadbalancer.server.port=8000"
  
  arc4d3:
    image: squidfunk/mkdocs-material
    container_name: docs-arc4d3
    ports:
      - "8002:8000"
    volumes:
      - ./repos/arc4d3:/docs
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.arc4d3.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/arc4d3`)"
      - "traefik.http.routers.arc4d3.entrypoints=web,websecure"
      - "traefik.http.services.arc4d3.loadbalancer.server.port=8000"
  
  traefik:
    image: traefik:latest
    container_name: traefik
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
      - ./traefik/config:/etc/traefik
    restart: unless-stopped
  
  watchtower:
    image: containrrr/watchtower
    container_name: watchtower
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    command: --interval 30 --cleanup
    restart: unless-stopped
```

## Adding New Documentation Containers

To add a new documentation container:

1. Create a new repository folder in `/srv/docs/repos/`
2. Add a new service entry to the docker-compose.yml file
3. Follow the template pattern of existing containers
4. Run `docker compose up -d` to start the new container

## Container Management

Common commands for managing the Docker containers:

```bash
# Start all containers
cd /srv/docs
docker compose up -d

# View container logs
docker logs docs-doc-box-53

# Restart a specific container
docker compose restart doc-box-53

# Update all containers (manual option)
docker compose pull
docker compose up -d
```

## Security Considerations

- All containers run as non-root
- Volume mounts are read-only where possible
- Traefik handles SSL termination
- No sensitive credentials stored in docker-compose.yml