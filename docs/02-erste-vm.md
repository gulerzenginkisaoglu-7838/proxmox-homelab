# 02 – Erster LXC-Container

**Status:** ✅ erledigt (14.10.2026) – eine vollständige KVM-VM ist geplant

## Ziel

Einen ersten Linux-Container (LXC) in Proxmox erstellen und testen.

## Durchgeführte Schritte

1. Template heruntergeladen: `local (pve01)` → CT Templates → Templates → **debian-13-standard 13.6-1**
2. Container erstellt („Create CT“):
   - CT ID `100`, Hostname `ct-debian01`
   - 8 GB Disk, 1 Core, 512 MB RAM, 512 MB Swap
   - Netzwerk: Bridge `vmbr0`, statische IP `192.168.100.20/24`, Gateway `192.168.100.1`
3. Container gestartet und in der Konsole getestet (`ping`, `apt update`).

## Probleme & Lösungen

**Problem: Template-Download blieb mehrmals stehen**
Die Verbindung wurde während des Downloads zweimal getrennt (`Connection closed ... Retrying`).
Lösung: Download **nicht** abgebrochen. `wget` hat automatisch an der gleichen Stelle weitergemacht (`206 Partial Content`). Am Ende: `TASK OK`, Prüfsumme korrekt.
Fortschritt kontrolliert mit: `ls -lh /var/lib/vz/template/cache/`

## Screenshots

![Template-Download](../screenshots/04-ct-template-download.png)

## Was ich gelernt habe

- Unterschied **Container (LXC)** und **VM (KVM)**: Ein Container teilt sich den Kernel mit dem Host, startet schneller und braucht weniger Ressourcen.
- In verschachtelten Umgebungen sind Container besonders praktisch, weil sie keine Hardware-Virtualisierung brauchen.
- Ein abgebrochener Download ist nicht immer verloren – HTTP kann Teildownloads fortsetzen.
