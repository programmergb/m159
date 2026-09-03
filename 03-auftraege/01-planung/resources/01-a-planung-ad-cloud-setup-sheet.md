# M159 - Projekt-Setup-Sheet

In diesem Dokument werden sämtliche Angaben sowie Passwörter, welche Sie für die Installation Ihrer Umgebung benötigen, festgehalten.

![Modul_159_Architekturdiagramm](modul-159-architekturdiagramm.drawio.svg)

---

## 0. Status

| Punkt | Status |
| --- | --- |
| Azure for Students aktiviert | ⬜ offen |
| Entra-ID-Tenant erreichbar | ⬜ offen |
| AWS-Account bereit | ⬜ offen |

> **Cloud-Bereitschaft (siehe [readme.md](../readme.md)):** Sobald Azure for Students /
> Entra ID aktiviert sind, hier je einen Screenshot als Nachweis ablegen
> (`resources/nachweis-azure-for-students.png`, `resources/nachweis-entra-tenant.png`).
> Scheitert die Aktivierung: sofort der Lehrperson melden und auf Varianten B wechseln,
> Fehlermeldung als Screenshot beilegen.

> **Passwörter:** Aus Sicherheitsgründen (siehe [fragen.md](../fragen.md), Frage 7) werden in
> diesem Dokument **keine echten Passwörter** eingetragen. Sie werden ausserhalb des
> Git-Repositories in einem Passwort-Manager abgelegt. Die Zeilen bleiben als Platzhalter
> stehen, damit klar ist, wo ein Kennwort benötigt wird.

---

## 1. Übersicht Umgebung

Diese Umgebung umfasst:

- **1x Windows Server (DC)** auf AWS EC2
- **1x Windows Server (Client)** auf AWS EC2 (da AWS keine Windows Clients anbietet)
- **1x Windows Server (Admin Center)** auf AWS EC2 zur Verwaltung der AWS Managed AD
- **AWS Managed AD** mit Trust zur On-Premises(EC2) AD
- **Entra Connect** zur Synchronisation mit Entra ID sowie Entra AD
- **Lokale AD-Domain** (zu Beginn), später **öffentliche Domain als UPN**

---

## 2. Allgemeine Angaben

| Feld                                | Wert |
| ----------------------------------- | ---- |
| Vorname                             | Giacomo |
| Nachname                            | Betsch |
| Klasse                              | Pe24d |
| Dokumentation (GIT-Repository-Link) | https://github.com/programmergb/m159 |

---

## 3. Ressourcen

| Feld                                                         | Wert                                            |
| ------------------------------------------------------------ | ----------------------------------------------- |
| Active Directory Second-Level-Domäne                         | `m159giacomo.dynv6.net` (fiktiv, Second-Level unter dynv6.net) |
| Geplante öffentliche Domain (UPN) -> Registrieren Sie einen Namen unter https://dynv6.com/ | `m159giacomo.dynv6.net` |
| Azure Education Account mit 80$ ([Anleitung für Freischaltung neuer Azure for Students Account (nicht TBZ-E-Mail)](../../../02-unterrichtsressourcen/03-fachliteratur-tutorials/azure/Azure-for-Students-Anleitung/Anleitung-Azure-for-Students.md))<br />(Wenn Sie Ihre private E-Mail-Adresse nicht verwenden möchten, können Sie beispielsweise eine Gmail-Adresse erstellen.) | ⬜ noch nicht aktiviert – geplant mit privater Gmail-Adresse |
| ![Azure.png](azure.png)                                      | – |
| Azure Education Account Passwort                             | *(nicht im Repo – Passwort-Manager)* |

---

## 4. AWS VPC Setup

**Hinweis:**
Alle Instanzen liegen in einem öffentlichen Subnetz und sind über RDP (Port 3389) von außen erreichbar.
Alle weiteren Ports sind nur innerhalb des VPCs offen.

| Komponente                      | VPC-ID                | CIDR         | Name |
| -------------------------------- | ---------------------- | ------------ | ---- |
| VPC                              | *(wird bei Erstellung vergeben)* | 10.0.0.0/16 | m159-giacomo-vpc |
| M159-subnet-private1-us-east-1a  | *(TODO nach Erstellung)* | 10.0.128.0/20 | AWS Managed AD – AZ a |
| M159-subnet-private2-us-east-1b  | *(TODO nach Erstellung)* | 10.0.144.0/20 | AWS Managed AD – AZ b |
| M159-subnet-public1-us-east-1a   | *(TODO nach Erstellung)* | 10.0.0.0/20  | EC2-Instanzen – AZ a |
| M159-subnet-public2-us-east-1b   | *(TODO nach Erstellung)* | 10.0.16.0/20 | EC2-Instanzen – AZ b |

*Begründung:* Die zwei privaten Subnetze in unterschiedlichen Availability Zones sind Pflicht
für AWS Managed Microsoft AD (Multi-AZ-Anforderung). Die EC2-Instanzen (DC, Client, Admin
Center) liegen in den öffentlichen Subnetzen, damit während des Moduls RDP-Zugriff von aussen
möglich ist; der Zugriff wird über die Sicherheitsgruppen (Abschnitt 5) auf Port 3389
beschränkt.

---

## 5. AWS Sicherheitsgruppen

### Sicherheitsgruppe für Domain Controller

| Regeltyp                     | Port(e)                 | Quelle  |
| ----------------------------- | ------------------------ | ------- |
| RDP                           | 3389  (TCP)               | 0.0.0.0/0 |
| LDAP                          | 389 (TCP/UDP)             | 10.0.0.0/16 |
| LDAPS                         | 636 (TCP)                 | 10.0.0.0/16 |
| Kerberos                      | 88 (TCP/UDP)              | 10.0.0.0/16 |
| SMB                           | 445  (TCP)                | 10.0.0.0/16 |
| DNS                           | 53 (TCP/UDP)              | 10.0.0.0/16 |
| RPC                           | 135, 49152-65535  (TCP)   | 10.0.0.0/16 |
| ICMP                          | Alle                      | 10.0.0.0/16 |
| Global Catalog                | 3268 (TCP)                | 10.0.0.0/16 |
| Global Catalog SSL            | 3269 (TCP)                | 10.0.0.0/16 |
| Kerberos Password Change/Set  | 464 (TCP/UDP)             | 10.0.0.0/16 |

*Begründung:* Nur RDP ist von aussen (0.0.0.0/0) erreichbar. Sämtliche AD-Dienste (LDAP,
Kerberos, SMB, DNS, RPC, Global Catalog) sind auf das VPC-CIDR `10.0.0.0/16` beschränkt – so
kann der DC ausschliesslich von Systemen innerhalb der eigenen Umgebung angesprochen werden.

### Sicherheitsgruppe für Clients

| Regeltyp | Port(e)     | Beschreibung                             | Quelle                                           |
| -------- | ----------- | ----------------------------------------- | ------------------------------------------------- |
| RDP      | 3389        | Remote Desktop                            | 0.0.0.0/0                                          |
| TCP      | 88          | Kerberos Authentication                   | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| TCP      | 135         | RPC Endpoint Mapper                       | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| TCP      | 139         | NetBIOS Session Service                   | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| TCP      | 389         | LDAP                                      | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| UDP      | 53          | DNS                                       | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| TCP      | 445         | SMB/CIFS (Dateifreigabe, AD-Operationen)  | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| TCP      | 49152-65535 | RPC Ephemeral Ports                       | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |
| ICMP     | Alle        | Ping etc.                                 | 10.0.0.0/20 <br/>10.0.128.0/20<br/>10.0.144.0/20   |

---

## 6. Active Directory Umgebung

### On-Premises Active Directory (AWS EC2)

| Feld                                  | Wert                          |
| -------------------------------------- | ------------------------------ |
| Active Directory Third-Level-Domäne-1 | `corp.m159giacomo.dynv6.net` (NetBIOS: `CORP`) |
| Öffentlicher UPN-Suffix (später)      | `m159giacomo.dynv6.net` |
| Domänenadministrator                  | Administrator |
| Kennwort Domänenadministrator         | *(nicht im Repo – Passwort-Manager)* |
| Kennwort-Demote (Herunterstufen)      | *(nicht im Repo – Passwort-Manager)* |

*Begründung:* Die interne AD-Domäne (`corp.m159giacomo.dynv6.net`) ist bewusst **nicht
identisch** mit der öffentlichen Domain (`m159giacomo.dynv6.net`), sondern eine eigene
Sub-Domain – so entsteht kein Namenskonflikt zwischen interner und öffentlicher
DNS-Auflösung. `.local` wird nicht verwendet, da es mit mDNS/Bonjour kollidiert und für
öffentliche Zertifikate nicht nutzbar ist. `CORP` als NetBIOS-Name ist kurz (≤15 Zeichen),
eindeutig und folgt einer in der Praxis gängigen Konvention.

### Azure AD (Entra ID)

| Feld                             | Wert |
| ---------------------------------- | ---- |
| Entra ID Domain                  | `m159giacomo.onmicrosoft.com` (initial, bis eigene Domain verifiziert ist) |
| Azure Global Administrator       | *(TODO nach Tenant-Erstellung)* |
| Kennwort Azure Administrator     | *(nicht im Repo – Passwort-Manager)* |
| Entra Connect Server (DC aus AD) | dc1.corp.m159giacomo.dynv6.net |

### AWS Managed AD

| Feld                                  | Wert                                                 |
| -------------------------------------- | ----------------------------------------------------- |
| Active Directory Third-Level-Domäne-2 | `aws.m159giacomo.dynv6.net` (NetBIOS: `AWS`) |
| Trust-Typ                             | Tree-Root Trust |
| AWS Managed Admin User                | admin |
| AWS Managed Admin Passwort            | *(nicht im Repo – Passwort-Manager)* |
| DNS-Server 1                          | *(TODO nach Erstellung)* |
| DNS-Server 2                          | *(TODO nach Erstellung)* |
| Trust Passwort                        | *(nicht im Repo – Passwort-Manager)* |
| Subnetz 1                             | M159-subnet-private1-us-east-1a (10.0.128.0/20) |
| Subnetz 2                             | M159-subnet-private2-us-east-1b (10.0.144.0/20) |

*Begründung:* Zwei getrennte Third-Level-Domänen (`corp.` für On-Prem, `aws.` für AWS Managed
AD) unter derselben Second-Level-Domäne machen die beiden Gesamtstrukturen eindeutig
unterscheidbar und der Tree-Root Trust dazwischen nachvollziehbar.

---

## 7. EC2-Instanzen

| Komponente                  | FQDN                              | Elastic IP           | Private IP (CIDR) | Subnetz                        | DNS-Server 1 | DNS-Server 2 | Lokaler Admin | Kennwort |
| ---------------------------- | ---------------------------------- | ---------------------- | -------------------- | -------------------------------- | -------------- | -------------- | --------------- | -------- |
| IaaS/OnPrem AD DC            | dc1.corp.m159giacomo.dynv6.net    | *(TODO nach Launch)* | 10.0.0.10             | M159-subnet-public1-us-east-1a  | 127.0.0.1      | –              | Administrator   | *(nicht im Repo)* |
| Windows Server (Client)      | client1.corp.m159giacomo.dynv6.net | *(TODO nach Launch)* | 10.0.0.20             | M159-subnet-public1-us-east-1a  | 10.0.0.10      | –              | Administrator   | *(nicht im Repo)* |
| Windows Server Admin Center  | adminctr1.corp.m159giacomo.dynv6.net | *(TODO nach Launch)* | 10.0.16.10           | M159-subnet-public2-us-east-1b  | 10.0.0.10      | –              | Administrator   | *(nicht im Repo)* |

*Begründung:* Die IP-Adressen und Hostnamen werden vorab fest geplant, statt sie beim
Aufsetzen ad hoc zu vergeben – der DC braucht zwingend eine statische IP, da Client und
Admin-Center-Server ihre DNS-Anfragen fest an diese Adresse richten. Client und Admin Center
verweisen daher als primären DNS-Server auf die geplante DC-IP `10.0.0.10`.

---

## 8. Abteilungen & Benutzer

Definieren Sie je einen Benutzer dieser 3 Abteilungen

| Abteilung | Name der Abteilung | Benutzername | Vorname | Nachname | Kennwort | Bereiche |
| --------- | ------------------- | -------------- | --------- | ---------- | -------- | -------- |
| 1         | Sekretariat         | jmueller       | Jasmin    | Müller     | *(nicht im Repo)* | intern |
| 2         | Buchhaltung         | tbucher        | Tom       | Bucher     | *(nicht im Repo)* | intern |
| 3         | GL                  | slehmann       | Sara      | Lehmann    | *(nicht im Repo)* | intern |
| 4         | Promoter            | kpromoter      | Kevin     | Promoter   | *(nicht im Repo)* | extern |

## 09. Python-App-Registration (Entra-ID)

| Name                    | Wert |
| ------------------------ | ---- |
| Directory (tenant) ID   | *(wird in Auftrag 13 ergänzt)* |
| Application (client) ID | *(wird in Auftrag 13 ergänzt)* |
| Client Secret ID        | *(nicht im Repo – wird in Auftrag 13 im Passwort-Manager abgelegt)* |

---

## 10. Hinweise

- Beginnen Sie mit der lokalen Domain-Umgebung und konfigurieren Sie **später den UPN-Suffix** mit der öffentlichen Domain.
- Achten Sie auf **richtige Portfreigaben** in den AWS Sicherheitsgruppen, insbesondere für RDP, SMB und AD-Dienste.
- Dokumentieren Sie alle IP-Adressen, Benutzernamen und Kennwörter konsequent in dieser Vorlage.
- Diese Vorlage wurde für das Schuljahr 2025/26 komplett neu erstellt. Falls Ihnen etwas fehlt, ist die Lehrperson dankbar, wenn Sie ihr dies mitteilen.
