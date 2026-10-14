# Proxmox VE Homelab – Virtualisierungsprojekt

> 🔄 Projekt im Aufbau / Work in progress – Stand: 14.10.2026

## 🇩🇪 Projektbeschreibung

Dieses Repository dokumentiert meinen Aufbau einer praxisnahen Virtualisierungsumgebung mit **Proxmox VE**. Ich mache eine Umschulung zur Fachinformatikerin für Systemintegration und nutze dieses Projekt, um Virtualisierung, Netzwerk und Datensicherung praktisch zu üben – als Open-Source-Alternative zu VMware.

Die Umgebung läuft als verschachtelte Virtualisierung (Nested Virtualization) in einer Lab-Umgebung: Proxmox VE ist als Hyper-V-VM auf einem Windows Server 2019 installiert und hat ein eigenes, isoliertes Netzwerk mit NAT.

## 🇬🇧 Project Description

This repository documents my hands-on build of a virtualization lab with **Proxmox VE**. I am retraining as an IT Specialist for System Integration (Fachinformatikerin für Systemintegration) and use this project to practice virtualization, networking and backup as an open-source alternative to VMware.

The lab runs as nested virtualization: Proxmox VE is installed as a Hyper-V VM on Windows Server 2019 and uses its own isolated network with NAT.

## 📋 Projektstatus / Project Status

| Phase | Thema / Topic | Status |
|-------|---------------|--------|
| 1 | Installation & Web-GUI | ✅ erledigt / done |
| 2 | Erster LXC-Container (Debian 13) | ✅ erledigt / done |
| 3 | Templates & Klonen | ⏳ geplant / planned |
| 4 | Netzwerk (isoliertes Netz, NAT, `vmbr0`) | 🔄 in Arbeit / in progress |
| 5 | Snapshots & Backup (`vzdump`) | 🔄 Snapshot ✅, Backup geplant |

Legende: ⏳ geplant · 🔄 in Arbeit · ✅ erledigt

## 🏗️ Aufbau / Architecture

```text
Physischer Lab-Host (Hyper-V)
└── Windows Server 2019 (VM, Hyper-V-Rolle aktiv)
    ├── vSwitch "Proxmox-Intern" (intern) – 192.168.100.1/24 + NAT ins Internet
    └── PVE01 – Proxmox VE 9.2 (Hyper-V-VM, Generation 1)
        │   IP: 192.168.100.10/24 – Web-GUI: https://192.168.100.10:8006
        └── CT 100 "ct-debian01" – LXC-Container, Debian 13
                IP: 192.168.100.20/24
```

## 🛠️ Technik / Technology

- **Hypervisor:** Proxmox VE 9.2 (Debian-basiert, KVM/LXC)
- **Unterbau:** Windows Server 2019 mit Hyper-V (Nested Virtualization)
- **Verwaltung:** Proxmox Web-GUI (HTTPS, Port 8006), PowerShell (Hyper-V)
- **Netzwerk:** interner Hyper-V-Switch, NAT, Linux Bridge `vmbr0`
- **Speicher:** local / local-lvm

## 📂 Dokumentation / Documentation

| Datei | Inhalt |
|-------|--------|
| [01-installation.md](docs/01-installation.md) | Hyper-V-VM, Installation, Web-GUI, Probleme & Lösungen |
| [02-erste-vm.md](docs/02-erste-vm.md) | LXC-Template und erster Container |
| [03-templates-klonen.md](docs/03-templates-klonen.md) | geplant |
| [04-netzwerk.md](docs/04-netzwerk.md) | isoliertes Netz mit NAT, Ping-Test |
| [05-backup-snapshots.md](docs/05-backup-snapshots.md) | Snapshot und Rollback |

Screenshots: Ordner [`screenshots/`](screenshots/)
