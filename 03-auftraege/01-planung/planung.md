# Planung – AD/Cloud-Umgebung von Giacomo Betsch

Eigene Planung für Auftrag 01. Aufgabenstellung und Fragenkatalog dazu stehen im
[Modul-Repository](https://gitlab.com/ch-tbz-it/Stud/m159/-/tree/main/03-auftraege/01-planung)
und werden hier bewusst nicht dupliziert.

## Stand

| Punkt | Status |
| --- | --- |
| Azure for Students aktiviert | offen |
| Entra-ID-Tenant erreichbar | offen |
| AWS-Account bereit | offen |

Nachweis-Screenshots (Azure-Aktivierung, Entra-Tenant) folgen hier in `resources/`, sobald die
Accounts aktiviert sind. Passwörter werden grundsätzlich nicht in diesem Repository abgelegt,
sondern separat in einem Passwort-Manager.

## Namenskonzept

| Zweck | Wert | Begründung |
| --- | --- | --- |
| Öffentliche Domain (UPN, über dynv6.com) | `m159giacomo.dynv6.net` | eigener, registrierbarer Name |
| Interne AD-Domain (On-Prem) | `corp.m159giacomo.dynv6.net`, NetBIOS `CORP` | eigene Sub-Domain statt Duplikat der öffentlichen Domain – vermeidet Konflikte zwischen interner und externer DNS-Auflösung; kein `.local`, da das mit mDNS/Bonjour kollidiert und für Zertifikate nicht mehr taugt |
| AWS-Managed-AD-Domain | `aws.m159giacomo.dynv6.net`, NetBIOS `AWS` | eigene Sub-Domain, damit On-Prem-AD und AWS Managed AD als getrennte Gesamtstrukturen erkennbar bleiben, verbunden über einen Tree-Root Trust |

## Netzwerkplanung (AWS VPC)

VPC `10.0.0.0/16`, aufgeteilt auf vier Subnetze über zwei Availability Zones:

| Subnetz | CIDR | Zweck |
| --- | --- | --- |
| public1 (us-east-1a) | 10.0.0.0/20 | EC2-Instanzen (DC, Client) |
| public2 (us-east-1b) | 10.0.16.0/20 | EC2-Instanzen (Admin Center) |
| private1 (us-east-1a) | 10.0.128.0/20 | AWS Managed AD |
| private2 (us-east-1b) | 10.0.144.0/20 | AWS Managed AD |

Zwei private Subnetze in unterschiedlichen AZs sind Pflicht für AWS Managed Microsoft AD. Die
EC2-Instanzen liegen in öffentlichen Subnetzen, damit während des Moduls RDP-Zugriff von
ausserhalb möglich ist; alle übrigen Dienste (LDAP, Kerberos, SMB, DNS, RPC, Global Catalog)
werden in den Security Groups auf das VPC-CIDR beschränkt, RDP bleibt der einzige von aussen
offene Port.

## Geplante Server

| Rolle | FQDN | Private IP | Subnetz |
| --- | --- | --- | --- |
| Domain Controller | dc1.corp.m159giacomo.dynv6.net | 10.0.0.10 | public1 |
| Client | client1.corp.m159giacomo.dynv6.net | 10.0.0.20 | public1 |
| Admin Center | adminctr1.corp.m159giacomo.dynv6.net | 10.0.16.10 | public2 |

Der DC bekommt eine statische IP, weil Client und Admin-Center-Server ihre DNS-Anfragen fest
an diese Adresse richten – ändert sie sich, laufen alle Namensauflösungen ins Leere. IPs und
Hostnamen werden deshalb jetzt vorab festgelegt statt erst beim Aufsetzen ad hoc vergeben, um
spätere Nacharbeit und Adresskonflikte zu vermeiden.

Umsetzung in Auftrag 02 (siehe [Dokumentation](../02-initial-setup/initial-setup.md)): Die VPC
heisst `m159-vpc` (`vpc-04e7c91259eea14e7`), die vier Subnetze wurden mit den geplanten
Adressbereichen angelegt. Abweichung von der Planung: Es wurden keine Elastic IPs vergeben,
die öffentlichen Adressen wechseln daher nach einem Lab-Neustart.

## Testbenutzer für spätere Berechtigungstests

Für Auftrag 04 (Freigaben/Berechtigungen) plane ich je einen fiktiven Benutzer pro Abteilung:

| Abteilung | Benutzername | Name | Bereich |
| --- | --- | --- | --- |
| Sekretariat | jmueller | Jasmin Müller | intern |
| Buchhaltung | tbucher | Tom Bucher | intern |
| Geschäftsleitung | slehmann | Sara Lehmann | intern |
| Promotion | kpromoter | Kevin Promoter | extern |

## Offene Punkte

- Azure for Students / AWS-Account aktivieren, Nachweis-Screenshots ablegen
- Entra-ID-Tenant-Domain und Global-Administrator-Konto nach Tenant-Erstellung ergänzen
- Python-App-Registrierung (Tenant/Client-ID) folgt erst mit Auftrag 13
