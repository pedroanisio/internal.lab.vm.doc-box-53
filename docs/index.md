# `doc-box-53` — Documentation Hub VM

Welcome to the documentation for **`doc-box-53`**, part of the `internal.lab.vm` homelab infrastructure.

---

## 📌 Overview

`doc-box-53` acts as the central **documentation hub** for the homelab. It hosts multiple MkDocs Material instances via Docker, with each container dedicated to a specific VM or service.

📍 Access: [https://docs.local.arc4d3.io](https://docs.local.arc4d3.io)

---

## ⚙️ Quick Facts

| Key           | Value                        |
|----------------|------------------------------|
| **Hostname**   | `doc-box-53`                 |
| **IP Address** | `10.10.10.53`                |
| **OS**         | Debian 12                    |
| **Services**   | MkDocs Material, Watchtower  |

---

## 🔗 Useful Links

| Description                | Link                                                                 |
|----------------------------|----------------------------------------------------------------------|
| **PBS Dashboard**          | [https://192.168.199.73:8007/#pbsDashboard](https://192.168.199.73:8007/#pbsDashboard) |
| **PBS VM View**            | [https://192.168.199.2:8006/#v1:0:=qemu%2F102:4:::::::](https://192.168.199.2:8006/#v1:0:=qemu%2F102:4:::::::) |
| **Internal Access**        | [http://10.10.10.53:8000](http://10.10.10.53:8000)                   |
| **MkDocs Material Docs**   | [https://squidfunk.github.io/mkdocs-material/](https://squidfunk.github.io/mkdocs-material/) |
| **Traefik Documentation**  | [https://doc.traefik.io/traefik/](https://doc.traefik.io/traefik/)   |
| **Public Access (via Traefik)** | [https://docs.local.arc4d3.io](https://docs.local.arc4d3.io)       |

---

## 🧭 Navigation

Use the MkDocs sidebar to browse:

- **Reference** – Specs, configs, and base setup
- **Services** – Running services and container documentation
- **Maintenance** – Backup plans, update policies, and health checks

---

## 📅 Recent Changes

- **2025-03** – Initial documentation system deployed
- **2025-03** – Traefik reverse proxy configured
- **2025-03** – Watchtower enabled for automated updates

---

## 📬 Contact

For issues, requests, or contributions related to this VM:

📧 `admin@internal.lab.vm`
