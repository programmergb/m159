# Umsetzungsvorschlag - Modul 159

Der Umsetzungsvorschlag ist eine Orientierungshilfe für die Lehrperson. Die Aufträge werden von
den Lernenden selbstständig bearbeitet und **einzeln abgegeben** (Stufen 1–2); der mündliche
Nachweis (Stufen 3–4) folgt gebündelt in der Schlussbesprechung. Details in
[Kompetenzmatrix](../08-kompetenznachweise/lb2/kompetenzmatrix-lb2.md) und
[Instruktionen](../01-instruktionen).

| Tag | Themen | Unterlagen / Übungen | Beschreibung |
| --- | --- | --- | --- |
| 1 | **Einführung**<br>- Was ist AD (On-Prem, Entra ID)<br>- Objekte im AD<br>- Domain Controller<br>- Überblick Aufträge 01–13<br>- Rahmen für den KI-Einsatz | [01-instruktionen](../01-instruktionen)<br>[ki-nutzung.md](../ki-nutzung.md)<br>[Visualisierung von Modul 159](https://www.canva.com/design/DAF0Bg-_BRY/N6D1WCLgKWYgKOAf0l4bgA/edit?utm_content=DAF0Bg-_BRY&utm_campaign=designshare&utm_medium=link2&utm_source=sharebutton)<br>[History](../02-unterrichtsressourcen/03-fachliteratur-tutorials/history)<br>[05-umfrage](../02-unterrichtsressourcen/05-umfrage) | Modulvorstellung, Begriffe klären, Zusammenhänge von On-Prem AD, Entra ID und Cloud-Integration erklären. Ablauf der Abgabe inkl. mündlichem Nachweis und Fragenkatalog erläutern. |
| 2 | **Auftrag 01 – Planung**<br>- DNS-Namenskonzept (privat & öffentlich)<br>- DC-DNS-Einstellungen<br>- **Cloud-Bereitschaft prüfen** | [nslookup.md](../02-unterrichtsressourcen/04-uebungen/nslookup.md)<br>[dns-names.md](../02-unterrichtsressourcen/03-fachliteratur-tutorials/dns/dns-names.md) | Lernende erstellen ihre Planung. **Wichtig:** Azure for Students und Entra ID werden heute aktiviert und der Nachweis erbracht. Wer scheitert, wechselt sofort auf die Varianten B. |
| 3 | **Aufträge 02 & 03 – Initial Setup & erster DC**<br>- DC installieren & promoten<br>- DNS in AD<br>- Reverse Lookup Zone<br>- NSLOOKUP | [04-uebungen](../02-unterrichtsressourcen/04-uebungen) | Live-Demo DC-Promotion und DNS-Einrichtung. Lernende setzen die Konfiguration in der eigenen Umgebung um. |
| 4 | **Aufträge 04 & 05 – Freigaben, Berechtigungen, ABE, AGDLP, AWS Managed Microsoft AD** | [04-uebungen](../02-unterrichtsressourcen/04-uebungen) | Unterschied Freigabe- und NTFS-Berechtigungen erklären, AGDLP vorstellen und anwenden, AWS Managed AD einrichten, Trust konfigurieren, Ports & Sicherheit dokumentieren. |
| 5 | **Aufträge 06 & 07 – RSAT, Admin Center, DIT & GPOs** | [02-praesentationen](../02-unterrichtsressourcen/02-praesentationen) | RSAT-Tools installieren, Admin Center konfigurieren, OU-Struktur im DIT darstellen, DN ableiten, GPO-Aufgaben umsetzen und Fehlerbehebung durchführen. |
| 6 | **Auftrag 08 – Suche im Directory**<br>- LDAP-Bind und LDAP-Suche<br>- Suchfilter, Scope, verschachtelte Bedingungen<br>- LDAP vs. LDAPS | [Theorie LDAP](../02-unterrichtsressourcen/03-fachliteratur-tutorials/ldap)<br>[08-suche-im-directory](../03-auftraege/08-suche-im-directory) | Bind und Suche an `ldp.exe` demonstrieren, Aufbau eines Suchfilters erklären. Lernende bauen ihre Filter gegen die eigene Struktur und pflegen sie in Anwendungen ein. |
| 7 | **Auftrag 09 – Identity Management & PowerShell Debugging** | [09-automation-und-debugging](../03-auftraege/09-automation-und-debugging) | Automatisierter Benutzerimport aus CSV, Fehlerbilder in PowerShell lesen, Bezug zu User-Lifecycle-Prozessen herstellen. |
| 8 | **Auftrag 10 – MS Entra ID & Connect** | [02-praesentationen](../02-unterrichtsressourcen/02-praesentationen) | Entra ID mit On-Prem AD verbinden, Unterschiede der Sync-Methoden erklären, Demo einer Synchronisation durchführen. |
| 9 | **Aufträge 11 & 12 – Profile & Netzlaufwerke** | [02-praesentationen](../02-unterrichtsressourcen/02-praesentationen) | Servergespeicherte Benutzerprofile und FS-Logix, Azure Storage-Account einrichten und verbinden. Für Lernende auf Variante B: AWS S3 und Backup-Automation. |
| 10 | **Auftrag 13 – SSO**<br>- SSO-Konzepte (Kerberos, NTLM, OAuth)<br>- Einschränkungen bei nicht-hybriden Clients<br>- Abschluss & offene Punkte | [sso.md](../02-unterrichtsressourcen/04-uebungen/sso.md) | SSO-Prinzipien erklären und direkt am Auftrag anwenden. Login-Tests mit manueller Anmeldung, Token-basiertem SSO und Kerberos/WIA. Restabgaben. |

> [!NOTE]
>
> Gegenüber der Vorversion wurde der eigenständige SSO-Theorietag aufgelöst und in Auftrag 13
> integriert. Der freigewordene Tag geht an die neuen Aufträge 08 und 09.
>
> **Offen:** Diese Tagesübersicht bildet noch das alte Modell ab (Inputs bis Tag 10). Im
> getrennten Modell sind Tag 9–10 der Schlussbesprechung vorbehalten; die Input-/Auftragstage
> müssen auf Tag 1–8 verdichtet werden.
