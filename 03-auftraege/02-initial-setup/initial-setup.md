# Initial Setup – dc1, client1, adminctr1

Dokumentation der Grundkonfiguration der drei Windows-Server in AWS. Aufgabenstellung siehe
[Modul-Repository](https://gitlab.com/ch-tbz-it/Stud/m159/-/tree/main/03-auftraege/02-initial-setup).

Autor: Giacomo Betsch, Klasse Pe24d

## 1. Umgebung

- AWS Academy Learner Lab (Vocareum), Region us-east-1
- Drei Windows-Server (EC2): `dc1`, `client1`, `adminctr1`
- Geplante Adressen (siehe [Planung](../01-planung/planung.md)): dc1 `10.0.0.10`, client1 `10.0.0.20`
- Zugriff per Remotedesktop (`mstsc`) auf die öffentliche IPv4-Adresse, Benutzer `Administrator`.
  Das Passwort wird in der EC2-Konsole über "Windows-Passwort abrufen" mit dem Schlüssel
  (.pem) entschlüsselt. Passwörter stehen nicht in diesem Repository.

Hinweis zum Lab: Läuft die Lab-Session ab, verweigert die AWS-Konsole plötzlich alles
(Fehlermeldung mit `explicit deny`, Rolle `voclabs`). Das ist kein Serverfehler. Lösung: auf
der Vocareum-Seite das Lab neu starten ("Start Lab"), warten bis es aktiv ist und die
Konsole neu öffnen. Die Instanzen bleiben dabei bestehen.

## 2. Sicherheitsgruppen

Für die Server habe ich zwei Sicherheitsgruppen erstellt, jeweils mit den Ports, die
Active Directory braucht (RDP, DNS, LDAP/LDAPS, Kerberos, SMB, RPC, Global Catalog, ICMP).

Domain Controller (`m159-dc-sg`, 16 eingehende Regeln):

![Sicherheitsgruppe m159-dc-sg](resources/sg-dc-eingehende-regeln.png)

Client (`m159-client-sg`, 9 eingehende Regeln):

![Sicherheitsgruppe m159-client-sg](resources/sg-client-eingehende-regeln.png)

## 3. Grundkonfiguration der Server

Die folgenden Einstellungen habe ich auf den Servern vorgenommen. Befehle liefen in einer
PowerShell mit Administratorrechten.

### 3.1 Hostname

`Rename-Computer -NewName "<name>"`, danach Neustart. dc1 im Server Manager (Local Server)
mit dem Computernamen `dc1` in der Arbeitsgruppe `WORKGROUP`; das Betriebssystem ist
Windows Server 2025 Datacenter auf einer EC2-Instanz `t3.micro`:

![dc1 im Server Manager](resources/dc1-server-manager-local-server.png)

Auf dem Desktop von dc1 zeigt die Bildschirmeinblendung Hostname `dc1`, die private Adresse
`10.0.0.10` und die Availability Zone `us-east-1a`. Das belegt, dass es meine Umgebung ist:

![dc1 Desktop](resources/dc1-desktop-und-powershell.png)

### 3.2 Ping erlauben

Eine Firewallregel erlaubt eingehende ICMPv4-Echo-Anfragen:

```powershell
New-NetFirewallRule -DisplayName "Allow ICMPv4 Ping" `
  -Protocol ICMPv4 -IcmpType 8 -Direction Inbound -Action Allow
```

![Firewallregel erstellt](resources/ping-firewallregel-erstellt.png)

### 3.3 Tastaturlayout Deutsch (Schweiz)

```powershell
Set-WinUserLanguageList de-CH -Force
```

Windows weist darauf hin, dass die Anzeigesprache erst nach der nächsten Anmeldung
wirksam wird (Warnung im PowerShell-Fenster auf den Bildern unten).

### 3.4 IPv6 deaktivieren

```powershell
Disable-NetAdapterBinding -Name "*" -ComponentID ms_tcpip6
Get-NetAdapterBinding -ComponentID ms_tcpip6
```

Die Kontrolle zeigt `Enabled = False`:

![IPv6 deaktiviert](resources/ipv6-deaktiviert.png)

### 3.5 CMD und PowerShell auf dem Desktop

Zwei Verknüpfungen habe ich per Skript angelegt:

```powershell
$ws = New-Object -ComObject WScript.Shell
$cmd = $ws.CreateShortcut("$env:USERPROFILE\Desktop\CMD.lnk")
$cmd.TargetPath = "$env:SystemRoot\System32\cmd.exe"
$cmd.Save()
$ps = $ws.CreateShortcut("$env:USERPROFILE\Desktop\PowerShell.lnk")
$ps.TargetPath = "$env:SystemRoot\System32\WindowsPowerShell\v1.0\powershell.exe"
$ps.Save()
```

Ablauf dieser Befehle inklusive IPv6-Kontrolle auf zwei weiteren Servern:

![Befehle, Server 100.53.86.185](resources/rdp-100-53-86-185-befehle-ipv6.png)

![Befehle und IPv6-Kontrolle](resources/befehle-und-ipv6-kontrolle.png)

### 3.6 Explorer-Optionen

Im Explorer unter *Options → View* habe ich versteckte Dateien und Ordner eingeblendet und
die Dateiendungen bekannter Dateitypen angezeigt. Beim Einblenden der geschützten
Systemdateien fragt Windows nach einer Bestätigung:

![Warnung geschützte Systemdateien](resources/explorer-warnung-systemdateien.png)

![Ordneroptionen](resources/explorer-optionen-ansicht.png)

### 3.7 Desktop-Symbole

Über *Personalize → Themes → Desktop icon settings* habe ich Computer (This PC),
Control Panel und Network eingeblendet:

![Desktop icon settings](resources/desktop-icon-settings.png)

### 3.8 Neustart

Nach den Einstellungen habe ich jeden Server neu gestartet, damit Hostname und Tastaturlayout
sicher übernommen sind.

## 4. Kontrolle client1

Auf client1 habe ich die Einstellungen mit Abfragen geprüft: Hostname `client1`, Sprache
`de-CH`, IPv6 `Enabled = False`, private Adresse `10.0.0.20`, Firewallregel
`Allow ICMPv4 Ping` aktiv. Ein Ping auf dc1 (`10.0.0.10`) wurde mit 3 von 3 Paketen
beantwortet:

![client1: Ping auf dc1](resources/client1-ping-und-firewallregel.png)

## 5. Offene Punkte

- **IE Enhanced Security Configuration** stand auf dc1 zum Zeitpunkt des Bildes 3.1 noch auf
  `On`. Die Vorgabe ist `Off` (Server Manager → Local Server). Nachweis nach der Änderung
  fehlt noch.
- Nachweisbilder für Hostname, Desktop-Symbole (This PC, Control Panel, Network) und
  "Use Sharing Wizard" aus für client1 und adminctr1 fehlen noch.
- AWS-Konsole: Bilder der EC2-Instanzliste und der VPC folgen.
- Administrator-Passwörter von dc1 und client1 ändern, da sie ausserhalb des Passwort-Managers
  sichtbar waren.
- Danach weiter mit Auftrag 03 (Domain Controller und Client).
