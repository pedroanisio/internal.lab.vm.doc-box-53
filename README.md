# 📚 `doc-box-53` Documentation VM

This repository contains the MkDocs-based documentation site for the `doc-box-53` virtual machine. It is part of the `internal.lab.vm` homelab and acts as the central documentation hub for all infrastructure components.

---

## 🧠 Purpose

`doc-box-53` hosts multiple MkDocs Material instances using Docker. Each instance serves documentation for a different VM or service, following a standardized structure for traceability, clarity, and scalability.

---

## 📁 Structure

```
/srv/docs/repos/internal.lab.vm.doc-box-53/
├── docs/
│   ├── index.md
│   ├── reference/
│   │   ├── purpose.md
│   │   ├── server.md
│   │   ├── application.md
│   │   ├── templates.md
│   │   └── guidelines.md
│   └── assets/
│       └── debian.png
├── mkdocs.yml
└── README.md
```

---

## 🚀 Services Running

- **MkDocs Material** – Multiple documentation instances
- **Traefik** – Reverse proxy with path-based routing
- **Watchtower** – Automatic Docker image updates

---

## 🌐 Access

Available locally via reverse proxy:

```
https://docs.internal.lab.vm/{vm-name}
```

---

## 🛠️ How to Add a New VM Documentation Set

1. Copy the template directory structure to a new `repos/internal.lab.vm.{vm-name}` folder.
2. Edit `mkdocs.yml`, `index.md`, and files inside `reference/`.
3. Add a new service entry to the main `docker-compose.yml` on the host VM.
4. Push changes to Git – this triggers the rebuild via `watchtower`.

---

## 🤝 Contributing

If you're a contributor within the homelab team:
- Follow the provided templates.
- Use consistent naming (`internal.lab.vm.{vm-name}`).
- Update the sidebar (`mkdocs.yml`) as needed.

---

## 📬 Contact

For issues or questions regarding this documentation server:

📧 `admin@internal.lab.vm`
```
