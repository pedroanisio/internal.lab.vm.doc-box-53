## 🧠 `purpose.md` – Purpose of This Documentation Node

## Primary Function
This VM serves as the centralized documentation hub for our entire homelab
infrastructure. It hosts multiple MkDocs instances via Docker, providing a clean
and scalable documentation system.

## Business Justification
Maintaining comprehensive documentation for our homelab helps track status,
configuration, and connections between services. The documentation server provides:
- A single source of truth for all homelab infrastructure
- Easy access to server configurations and maintenance procedures
- Knowledge preservation for future reference and troubleshooting

## Key Applications/Services
- **MkDocs Material**: The main documentation framework that renders Markdown
  as beautiful documentation sites
- **Traefik**: Reverse proxy that provides subdomain routing to each documentation
  instance
- **Watchtower**: Automatic Docker image updates to ensure documentation containers
  stay current

## Dependencies
- **Depends on**: Network infrastructure, DNS server
- **Depended on by**: All homelab services (for documentation access)

## Lifecycle Information
- **Created**: March 2025
- **Planned retirement**: None (core infrastructure)
- **Maintenance window**: Sundays, 2 AM - 4 AM
