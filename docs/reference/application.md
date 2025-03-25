# Application

Runs on docker and is present at /srv/docs
user is non root (`pals`)

```text
pals@doc-box-53:/srv$ tree
.
└── docs
    ├── docker-compose.yml
    └── repos
        ├── arc4d3
        │   ├── docs
        │   │   ├── assets
        │   │   │   └── logo.png
        │   │   ├── index.md
        │   │   └── reference
        │   │       ├── docker.md
        │   │       ├── dod.md
        │   │       ├── llms.md
        │   │       ├── python.md
        │   │       ├── soft-eng.md
        │   │       ├── structure.md
        │   │       └── tests.md
        │   ├── mkdocs.yml
        │   └── overrides
        │       └── main.html
        └── internal.lab.vm.doc-box-53
            ├── docs
            │   ├── assets
            │   │   └── debian.png
            │   ├── index.md
            │   └── reference
            │       ├── guidelines.md
            │       ├── purpose.md
            │       ├── server.md
            │       └── templates.md
            ├── mkdocs.yml
            └── README.md
```

## Docker Setup

The documentation is served via Docker containers using the official MkDocs Material image:

```yaml
services:
  doc-box-53:
    image: squidfunk/mkdocs-material
    container_name: docs-doc-box-53
    ports:
      - "8000:8000"
    volumes:
      - ./repos/internal.lab.vm.doc-box-53:/docs
    restart: unless-stopped
    labels:
      - "traefik.enable=true"
      - "traefik.http.routers.doc-box-53.rule=Host(`docs.internal.lab.vm`) && PathPrefix(`/doc-box-53`)"
      - "traefik.http.routers.doc-box-53.entrypoints=web,websecure"
      - "traefik.http.services.doc-box-53.loadbalancer.server.port=8000"
```

## Development Workflow

1. Content is written in Markdown within the appropriate repository structure
2. Changes are committed to the Git repository
3. Watchtower automatically detects changes and rebuilds the documentation container
4. Documentation is updated and available via the Traefik reverse proxy

## Access Points

The documentation can be accessed via:

- Direct port: `http://10.10.10.53:8000`
- Traefik proxy: `https://docs.internal.lab.vm/doc-box-53`