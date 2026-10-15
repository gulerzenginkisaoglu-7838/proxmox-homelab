# Proxmox VE Homelab – Virtualisierungsprojekt

> Stand: 16.10.2026 · Lernprojekt im Rahmen meiner Umschulung zur Fachinformatikerin für Systemintegration

## 🇩🇪 Projektbeschreibung

Ich habe eine Virtualisierungsumgebung mit **Proxmox VE** aufgebaut und dokumentiert: Installation, Container, eigenes Netzwerk mit NAT, Snapshots sowie Backup und Restore.

Proxmox läuft als Hyper-V-VM auf einem Windows Server 2019 in einer Lab-Umgebung (Nested Virtualization). Alle Probleme und Lösungen sind in [`docs/`](docs/) beschrieben.

## 🇬🇧 In short

I built and documented a Proxmox VE lab: installation, LXC container, isolated network with NAT, snapshots, backup and restore. Proxmox runs as a nested Hyper-V VM on Windows Server 2019. Problems and fixes are documented in `docs/`.

## 📋 Projektstatus

| Phase | Thema | Status |
|-------|-------|--------|
| 1 | Installation & Web-GUI | ✅ erledigt |
| 2 | Erster LXC-Container (Debian 13) | ✅ erledigt |
| 3 | Templates & Klonen | ⏳ geplant |
| 4 | Netzwerk (isoliertes Netz, NAT, `vmbr0`) | ✅ erledigt |
| 5 | Snapshot, Backup (`vzdump`) & Restore | ✅ erledigt |

## 🏗️ Aufbau

```text
Physischer Lab-Host (Hyper-V)
└── Windows Server 2019 (VM, Hyper-V-Rolle aktiv) – 10.100.13.30
    ├── vSwitch "Proxmox-Intern" (intern) – 192.168.100.1/24 + NAT
    └── PVE01 – Proxmox VE 9.2 (Hyper-V-VM, Generation 1) – 192.168.100.10
        ├── CT 100 "ct-debian01" – Debian 13 LXC – 192.168.100.20
        └── CT 101 – Restore-Test aus Backup – 192.168.100.21 (gestoppt)
```

## 🛠️ Technik

- **Hypervisor:** Proxmox VE 9.2 (KVM/LXC), Unterbau: Windows Server 2019 mit Hyper-V
- **Verwaltung:** Proxmox Web-GUI (Port 8006), PowerShell
- **Netzwerk:** interner Hyper-V-Switch, NAT, Linux Bridge `vmbr0`
- **Speicher:** `local` (ISO, Templates, Backups), `local-lvm` (LVM-Thin für Container)
- **Datensicherung:** Snapshots, `vzdump` (ZSTD), Restore auf neue CT-ID

## 🧯 Troubleshooting (Kurzfassung)

| Problem | Ursache | Lösung |
|---------|---------|--------|
| Installer findet ISO nicht | Generation-2-VM: DVD am SCSI-Controller | VM mit Generation 1 |
| Grafischer Installer startet nicht | Grafik in Hyper-V | Terminal UI |
| `SQUASHFS error` beim Neustart | ISO zu früh entfernt | VM neu starten; erst Reboot, dann ISO entfernen |
| Template-Download stockt | Verbindungsabbrüche | Nicht abbrechen – `wget` setzt fort |
| Ping zu 192.168.100.1 schlägt fehl | Windows-Firewall blockiert ICMP | Kein Fehler – anders testen (`apt update`) |
| Thin-Pool-Warnung beim Backup | Thin Provisioning: zugesagt > vorhanden | Belegung beobachten, Unnötiges löschen |

## 📂 Dokumentation

| Datei | Inhalt |
|-------|--------|
| [01-installation.md](docs/01-installation.md) | Hyper-V-VM, Installation, Web-GUI |
| [02-erste-vm.md](docs/02-erste-vm.md) | LXC-Template und erster Container |
| [03-templates-klonen.md](docs/03-templates-klonen.md) | geplant |
| [04-netzwerk.md](docs/04-netzwerk.md) | isoliertes Netz mit NAT, Ping-Test |
| [05-backup-snapshots.md](docs/05-backup-snapshots.md) | Snapshot, Backup, Restore |

Screenshots: [`screenshots/`](screenshots/) (01–08)
