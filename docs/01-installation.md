# 01 – Installation & Web-GUI

**Status:** ✅ erledigt (14.10.2026)

## Ziel

Proxmox VE als virtuelle Maschine auf einem Windows Server 2019 (Hyper-V) installieren und die Web-Oberfläche erreichen.

## Durchgeführte Schritte

1. Proxmox VE ISO (Version 9.2-1) von proxmox.com heruntergeladen und die SHA256-Prüfsumme kontrolliert.
2. Hyper-V-VM **PVE01** mit PowerShell erstellt:
   - Generation 1, 2 vCPUs, 6 GB statischer Arbeitsspeicher, 64 GB virtuelle Festplatte
   - Nested Virtualization aktiviert: `Set-VMProcessor -ExposeVirtualizationExtensions $true`
   - MAC-Spoofing aktiviert (nötig für Container/VMs im Proxmox-Netz): `Set-VMNetworkAdapter -MacAddressSpoofing On`
3. Installation im Terminal-UI-Modus:
   - Zeitzone Europe/Berlin, Tastatur Deutsch
   - Hostname `pve01.homelab.local`, IP `192.168.100.10/24`, Gateway `192.168.100.1`
4. Nach dem Neustart Web-GUI geöffnet: `https://192.168.100.10:8006` (Benutzer `root`, Realm Linux PAM).

```powershell
New-VM -Name "PVE01" -Generation 1 -MemoryStartupBytes 6GB -NewVHDSizeBytes 64GB -SwitchName "Proxmox-Intern" ...
Set-VMProcessor -VMName PVE01 -ExposeVirtualizationExtensions $true
Set-VMNetworkAdapter -VMName PVE01 -MacAddressSpoofing On
```

## Probleme & Lösungen

**Problem 1: Installer findet das ISO nicht (Generation-2-VM)**
Meldung: `no device with valid ISO found, please check your installation medium`
Ursache: Bei einer Generation-2-VM hängt das DVD-Laufwerk am virtuellen SCSI-Controller von Hyper-V. Der Installer hat dieses Laufwerk beim Start nicht erkannt.
Lösung: Neue VM mit **Generation 1** erstellt. Dort hängt das DVD-Laufwerk am emulierten IDE-Controller – der Installer hat das ISO sofort gefunden.

**Problem 2: Grafischer Installer startet nicht**
Lösung: Installation im **Terminal-UI-Modus** durchgeführt.

**Problem 3: Fehler beim Neustart nach der Installation**
Meldung: `SQUASHFS error ... Installation aborted`
Ursache: Ich habe das ISO **vor** dem Neustart ausgeworfen. Der Installer lief aber noch vom ISO und konnte seine eigenen Dateien nicht mehr lesen.
Lösung: VM über Hyper-V aus- und wieder eingeschaltet. Die Installation war schon vollständig auf der Festplatte – Proxmox startete normal.
Merke: Erst neu starten, dann das ISO entfernen.

## Screenshots

![Netzwerk-Einstellungen im Installer](../screenshots/01-installer-netzwerk.png)
![Konsole nach der Installation](../screenshots/02-konsole-nach-install.png)
![Web-GUI Übersicht](../screenshots/03-webgui-uebersicht.png)

## Was ich gelernt habe

- Unterschied Hyper-V **Generation 1** (BIOS, IDE) und **Generation 2** (UEFI, Secure Boot, SCSI) – und warum nicht jedes System mit Generation 2 startet.
- Nested Virtualization muss pro VM aktiviert werden.
- Prüfsummen (SHA256) zeigen, ob eine Download-Datei unverändert ist.
- Reihenfolge ist wichtig: Installationsmedium erst nach dem Neustart entfernen.
