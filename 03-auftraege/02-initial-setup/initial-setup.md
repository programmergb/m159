# Initial Setup – dc1, client1, adminctr1

Dokumentation der Grundkonfiguration der drei Windows-Server in AWS. Aufgabenstellung siehe
[Modul-Repository](https://gitlab.com/ch-tbz-it/Stud/m159/-/tree/main/03-auftraege/02-initial-setup).

## Umgebung

- AWS Academy Learner Lab (Vocareum), Region us-east-1
- Server: `dc1`, `client1`, `adminctr1` (Windows Server auf EC2)
- Zugriff per RDP (`mstsc`) auf die öffentliche IPv4-Adresse, Benutzer `Administrator`.
  Das Passwort wird in der EC2-Konsole über "Windows-Passwort abrufen" mit dem Schlüssel
  (.pem) entschlüsselt.

Hinweis zum Lab: Läuft die Lab-Session ab, verweigert die AWS-Konsole plötzlich alles
(Fehlermeldung mit `explicit deny`, Rolle `voclabs`). Das ist kein Serverfehler. Lösung: auf
der Vocareum-Seite das Lab neu starten ("Start Lab"), warten bis es aktiv ist und die
Konsole neu öffnen. Die Instanzen bleiben dabei bestehen.

## Umgesetzte Einstellungen (auf allen drei Servern)

| Einstellung | Umsetzung |
| --- | --- |
| Hostname | `Rename-Computer -NewName "<name>"` |
| Ping erlauben | Firewall-Regel für ICMPv4 Echo Request (eingehend) |
| Tastaturlayout | Deutsch (Schweiz) über `Set-WinUserLanguageList de-CH -Force` |
| IPv6 deaktivieren | `Disable-NetAdapterBinding -Name "*" -ComponentID ms_tcpip6` |
| IE Enhanced Security | im Server Manager (Local Server) für Administratoren und Benutzer auf Off |
| Explorer-Optionen | Dateiendungen anzeigen, versteckte Dateien und geschützte Systemdateien anzeigen, Sharing Wizard aus |
| Desktop | Verknüpfungen zu CMD und PowerShell, Symbole This PC / Control Panel / Network eingeblendet |
| Abschluss | Neustart, damit Hostname und Tastaturlayout sicher übernommen sind |

Befehle (PowerShell als Administrator):

```powershell
Rename-Computer -NewName "client1"   # bzw. dc1 / adminctr1

New-NetFirewallRule -DisplayName "Allow ICMPv4 Ping" `
  -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow

Set-WinUserLanguageList de-CH -Force
Disable-NetAdapterBinding -Name "*" -ComponentID ms_tcpip6

$ws = New-Object -ComObject WScript.Shell
$cmd = $ws.CreateShortcut("$env:USERPROFILE\Desktop\CMD.lnk")
$cmd.TargetPath = "$env:SystemRoot\System32\cmd.exe"
$cmd.Save()
$ps = $ws.CreateShortcut("$env:USERPROFILE\Desktop\PowerShell.lnk")
$ps.TargetPath = "$env:SystemRoot\System32\WindowsPowerShell\v1.0\powershell.exe"
$ps.Save()

Restart-Computer
```

## Kontrolle

IPv6 ist deaktiviert, wenn diese Abfrage bei `Enabled` überall `False` liefert:

```powershell
Get-NetAdapterBinding -ComponentID ms_tcpip6
```

Auf adminctr1 wurde das per Screenshot bestätigt. Die Screenshots der drei Server lege ich in
`resources/` ab.

## Offene Punkte

- Screenshots als Nachweis pro Server (Hostname, IPv6-Kontrolle, Desktop) in `resources/` ablegen
- Administrator-Passwort von dc1 ändern, da es in einem KI-Chat geteilt wurde
- Danach weiter mit Auftrag 03 (Domain Controller und Client)
