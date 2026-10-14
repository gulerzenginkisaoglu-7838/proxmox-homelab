# Proxmox VE Homelab – Virtualisierungsprojekt

> 🔄 Projekt im Aufbau / Work in progress

## 🇩🇪 Projektbeschreibung

Dieses Repository dokumentiert meinen Aufbau einer praxisnahen Virtualisierungsumgebung mit **Proxmox VE**. Ich mache eine Umschulung zur Fachinformatikerin für Systemintegration und nutze dieses Projekt, um Virtualisierung, Netzwerk und Datensicherung praktisch zu üben – als Open-Source-Alternative zu VMware.

## 🇬🇧 Project Description

This repository documents my hands-on build of a virtualization lab with **Proxmox VE**. I am retraining as an IT Specialist for System Integration (Fachinformatikerin für Systemintegration) and use this project to practice virtualization, networking and backup as an open-source alternative to VMware.

## 📋 Projektstatus / Project Status

| Phase | Thema / Topic | Status |
|-------|---------------|--------|
| 1 | Installation & Web-GUI | ⏳ geplant / planned |
| 2 | Erste VM / Container | ⏳ geplant / planned |
| 3 | Templates & Klonen | ⏳ geplant / planned |
| 4 | Netzwerk & Bridges (`vmbr0`) | ⏳ geplant / planned |
| 5 | Snapshots & Backup (`vzdump`) | ⏳ geplant / planned |

Legende: ⏳ geplant · 🔄 in Arbeit · ✅ erledigt

## 🛠️ Technik / Technology

- **Hypervisor:** Proxmox VE (Debian-basiert, KVM/LXC)
- **Verwaltung:** Proxmox Web-GUI (HTTPS, Port 8006)
- **Netzwerk:** Linux Bridges (`vmbr0`)
- **Speicher:** local / local-lvm

## 📂 Repository-Struktur / Structure

```text
proxmox-homelab/
├── README.md
├── docs/
│   ├── 01-installation.md
│   ├── 02-erste-vm.md
│   ├── 03-templates-klonen.md
│   ├── 04-netzwerk.md
│   └── 05-backup-snapshots.md
└── screenshots/
```
