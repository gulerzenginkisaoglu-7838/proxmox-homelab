# 04 – Netzwerk

**Status:** ✅ erledigt (14.10.2026)

> 🇬🇧 **In short:** Created an internal Hyper-V switch with NAT so Proxmox has internet access without touching the lab network. Ping to the Windows gateway fails because of the firewall – not a network error.

## Was und warum

Ein eigenes Netz für Proxmox. Grund: Im Lab-Netz vergibt ein DHCP-Server die Adressen. Feste IPs könnten kollidieren. Das interne Netz ist getrennt und hat trotzdem Internet.

## Durchgeführte Schritte

```powershell
New-VMSwitch -Name "Proxmox-Intern" -SwitchType Internal
New-NetIPAddress -IPAddress 192.168.100.1 -PrefixLength 24 -InterfaceAlias "vEthernet (Proxmox-Intern)"
New-NetNat -Name "Proxmox-NAT" -InternalIPInterfaceAddressPrefix 192.168.100.0/24
```

| Gerät | IP |
|-------|----|
| Windows Server (Gateway, NAT) | 192.168.100.1 |
| Proxmox PVE01 | 192.168.100.10 |
| CT 100 ct-debian01 | 192.168.100.20 |
| CT 101 (Restore-Test) | 192.168.100.21 |

## Tests und Ergebnis

| Test | Ergebnis | Bedeutung |
|------|----------|-----------|
| `ping 192.168.100.10` | 0 % Verlust | Container erreicht Proxmox |
| `ping 192.168.100.1` | 100 % Verlust | Windows-Firewall blockiert ICMP |
| `apt update` | Pakete geladen | Gateway, NAT und DNS funktionieren |

## Troubleshooting

| Problem | Ursache | Lösung |
|---------|---------|--------|
| Ping zum Gateway schlägt fehl | Windows-Firewall blockiert eingehendes ICMP Echo | Kein Fehler. Mit `apt update` und Ping zu Proxmox gegengeprüft |

## Screenshots

![Ping und apt update](../screenshots/05-ct-netzwerk-ping.png)

## Was ich gelernt habe

- Interner Switch + NAT = isoliertes Netz mit Internetzugang.
- Ein fehlgeschlagener Ping heißt nicht automatisch „keine Verbindung“.
- Netzwerkprobleme mit mehreren Tests eingrenzen.
