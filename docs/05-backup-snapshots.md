# 05 – Snapshot, Backup & Restore

**Status:** ✅ erledigt (14.–16.10.2026)

> 🇬🇧 **In short:** Tested a snapshot rollback, then created a `vzdump` backup of CT 100 and restored it as a new container (CT 101). A marker file proved the backup contained the real data.

## Was und warum

- **Snapshot:** Zustand schnell sichern, z. B. vor einem Update.
- **Backup:** Kopie auf einem anderen Speicher – schützt auch, wenn die Disk ausfällt.
- **Restore-Test:** Ein Backup ist nur gut, wenn man es auch zurückspielen kann.

## Teil 1 – Snapshot und Rollback

1. Snapshot `vor_test` von CT 100 erstellt.
2. Testdatei angelegt: `echo "Testdatei" > /root/test.txt`
3. Rollback auf `vor_test` → `ls /root` ist leer. Der alte Zustand ist zurück.

## Teil 2 – Backup mit vzdump

1. Marker-Datei in CT 100: `echo "Backup-Test" > /root/backup_marker.txt`
2. CT 100 → Backup → **Backup now**: Storage `local`, Mode `Snapshot`, Compression `ZSTD`.
3. Ergebnis: `TASK OK` nach 22 Sekunden.

```text
/var/lib/vz/dump/vzdump-lxc-100-2026_10_16-00_40_02.tar.zst   204M
```

## Teil 3 – Restore als neuer Container

1. Backup in `local` → **Restore** als **CT 101**, Storage `local-lvm`, Option **Unique** (neue MAC-Adresse).
2. IP von CT 101 auf `192.168.100.21` geändert – sonst IP-Konflikt mit CT 100.
3. Kontrolle in CT 101:

```text
root@ct-debian01:~# cat /root/backup_marker.txt
Backup-Test
root@ct-debian01:~# hostname -I
192.168.100.21
```

Ergebnis: Die Marker-Datei ist da – das Backup enthält die echten Daten. CT 100 blieb unverändert. CT 101 ist danach gestoppt.

## Troubleshooting

| Meldung | Bedeutung | Maßnahme |
|---------|-----------|----------|
| `Sum of all thin volume sizes (24.00 GiB) exceeds the size of thin pool` | **Thin Provisioning:** Den Disks sind mehr GB zugesagt, als der Pool hat. Belegt wird nur, was wirklich geschrieben wird. | Warnung, kein Fehler. Belegung mit `lvs` beobachten. Nicht benötigte Snapshots und Test-Container löschen. |

## Screenshots

![Rollback](../screenshots/06-snapshot-rollback.png)
![Backup TASK OK](../screenshots/07-backup-task-ok.png)
![Restore CT 101](../screenshots/08-restore-ct101.png)

## Was ich gelernt habe

- **Snapshot ≠ Backup.** Ein Snapshot liegt auf demselben Speicher, ein Backup nicht.
- Ein Backup immer mit einem **Restore-Test** prüfen.
- Bei einem Restore auf eine neue ID: IP-Adresse ändern, sonst gibt es einen Konflikt.
- Thin Provisioning spart Platz, braucht aber Überwachung.
