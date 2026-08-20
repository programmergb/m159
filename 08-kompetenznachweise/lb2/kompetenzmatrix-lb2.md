# LB2 - Kompetenzmatrix

Jeder Auftrag wird in **vier Stufen** bewertet.

| Stufe | Nachweis | Wo |
| --- | --- | --- |
| 1 | Grundanforderung umgesetzt | Umgebung / Dokumentation |
| 2 | Vollständig umgesetzt | Umgebung / Dokumentation |
| 3 | Eine Katalogfrage korrekt beantwortet | Schlussbesprechung |
| 4 | Eine zweite Katalogfrage korrekt beantwortet | Schlussbesprechung |

Die Stufen bauen aufeinander auf: Stufe 2 setzt Stufe 1 voraus, Stufe 3 setzt Stufe 2 voraus.

> [!NOTE]
>
> Die Aufträge werden der Reihe nach umgesetzt, beginnend mit Nr. 1. Ausgenommen sind die Aufträge 5 und 6, die gegebenenfalls übersprungen werden können.

---

## Stufen 1 und 2 – Umsetzung

| Nr. | Auftrag | Kompetenzfelder | Stufe 1 | Stufe 2 |
| --- | --- | --- | --- | --- |
| 1 | Planung | A, I | Planung ist vollständig ausgefüllt, alle Felder sind mit eigenen Werten belegt | Planung ist durchdacht und in sich konsistent; die Werte sind begründet und tragen durch alle folgenden Aufträge |
| 2 | Initial Setup | — | AWS ist gemäss Planung aufgesetzt und die Umgebung läuft; IP-Adressen und Hostnames entsprechen der Planung | **Sämtliche** Werte entsprechen der Planung, inkl. Ports in den Security Groups und Standardeinstellungen |
| 3 | Gesamtstruktur (erster DC) & Client | A, B, H | - Active Directory ist eingerichtet<br>- Client ist in der Domain<br>- Hostname des Clients stimmt<br>- NSLOOKUP ist in beide Richtungen erfolgreich | - Alle Zusatzanforderungen aus dem Auftrag sind umgesetzt<br>- Ein RDP-Konzept wurde erstellt und integriert |
| 4 | Freigaben Laufwerke Berechtigungen | A, E | - Alle Gruppen und Benutzer existieren<br>- NTFS- und Freigabeberechtigungen sind vorhanden und getestet<br>- ABE ist aktiviert | Ein Group-Nesting-Konzept (AGDLP) wurde visuell erstellt und auf der Umgebung konfiguriert |
| 5 | AWS Managed Microsoft AD | G, B | - AWS Managed AD ist eingerichtet<br>- Die Ports sind richtig konfiguriert | - Der Trust funktioniert<br>- Sicherheitsvorkehrungen beim Öffnen wurden getroffen und dokumentiert |
| 5 B | Variante B: Moderner IdP (Authentik) | B, C, G | - Authentik läuft auf einer eigenen EC2-Instanz<br>- Die LDAP Source synchronisiert AD-Benutzer und -Gruppen<br>- Login mit einem AD-Konto ist nachgewiesen (Prüfung gegen den DC) | - Die Anbindung läuft über LDAPS mit Zertifikat, die Notwendigkeit ist dokumentiert (inkl. LDAP-Signing ab Server 2025)<br>- Eine App ist per OIDC/SAML oder Proxy mit funktionierendem SSO angebunden, der Zugriff über eine AD-Gruppe gesteuert<br>- Ein least-privilege Bind-Konto wird verwendet und begründet |
| 6 | RSAT & Admin Center V2 | B, I | - RSAT Tools sind installiert und funktionieren<br>- Der Windows Admin Center Server ist für die Administration eingerichtet | - Das Admin Center ist von aussen direkt oder über einen Jump Host via HTTPS oder RDP erreichbar<br>- Sicherheitsüberlegungen sind dokumentiert und die Massnahmen konfiguriert<br>- Der DC des EC2 AD wurde für die Verwaltung hinzugefügt |
| 7 | DIT & GPOs | A, E | - Ein korrekter DIT wurde erstellt und im AD umgesetzt<br>- Die Passwortrichtlinien wurden am richtigen Ort vorgenommen<br>- 3 der 6 GPO-Aufgaben sind umgesetzt | Alle 6 GPO-Aufgaben sind umgesetzt |
| 8 | Suche im Directory | D, C | - Der Bind in `ldp.exe` ist erfolgreich, der Base-DN ist korrekt hergeleitet<br>- Alle 4 Suchen aus Teil B sind über beide Wege gelöst und dokumentiert | - Alle 5 verschachtelten Filter aus Teil C sind korrekt, inkl. Gegentest<br>- Ein Filter ist als gespeicherte Abfrage **und** als GPO-Zielgruppenadressierung wirksam eingepflegt<br>- Der Entscheid zum Dienstkonto ist begründet dokumentiert |
| 9 | Identity Management & PowerShell Debugging | E, H | - Das Skript läuft fehlerfrei<br>- Die drei Benutzer sind in den korrekten OUs angelegt<br>- Ein Fehlerprotokoll der korrigierten Fehler liegt vor | Das Skript prüft vor dem Anlegen auf bereits existierende Konten, überspringt diese mit Warnung und legt neue mit Erfolgsmeldung an; beides ist nachgewiesen |
| 10 | MS Entra ID & MS Entra Connect | G, B | - Entra Connect ist eingerichtet, die lokalen AD-Objekte sind in Entra ID sichtbar<br>- In Entra ID und im lokalen AD ist ein Custom UPN/Domain eingerichtet | Der Client ist hybrid joined; `dsregcmd /status` zeigt bei Device State → AzureAdJoined `YES` |
| 10 B | Variante B: Zweiter DC & Replikation | G, B | - Der zweite DC ist in der bestehenden Domäne promoted und in `dsa.msc` sichtbar<br>- `repadmin /replsummary` zeigt eine fehlerfreie Replikation | - Eine Änderung ist in **beide** Richtungen nachweislich repliziert<br>- Der Ausfalltest von DC1 ist protokolliert, inkl. Anmeldeserver des Clients<br>- Der Entscheid zu FSMO, Global Catalog und DNS ist begründet dokumentiert |
| 11 | Servergespeicherte Benutzerprofile | B, G | Roaming Profiles oder Folder Redirection ist eingerichtet und getestet | FS-Logix ist eingerichtet |
| 12 | Netzlaufwerk to Azure Migration | G, H | - Unter Azure ist ein Storage-Account mit einem Share eingerichtet<br>- Die Verbindung über einen Storage-Account-Key funktioniert | - Die Verbindung funktioniert über SSO ohne zusätzliche Authentifizierung<br>- Die Berechtigungen sind verifiziert: Sekretariat = Read, Buchhaltung = Write |
| 12 B | Variante B: AWS S3 Backup | H | - S3 Bucket und IAM-User sind eingerichtet, die AWS CLI ist konfiguriert<br>- Das Sync-Skript überträgt die Dateien nachweislich | - Der Sync läuft automatisiert über den Task Scheduler<br>- Der Restore einer einzelnen Datei aus dem Bucket ist nachgewiesen |
| 13 | SSO Python App | B, C | Die App ist in Entra ID registriert, der Login-Test in Chrome mit manueller Anmeldung funktioniert | Token-basiertes SSO in Chrome **und** klassisches Kerberos-/WIA-SSO in Edge funktionieren, beides per Video belegt |
| 13 B | Variante B: SSO mit IIS & Kerberos | B, C | - IIS ist installiert, Windows Authentication ist aktiv, Anonymous ist deaktiviert<br>- Die Seite ist vom Client erreichbar | - Der Zugriff erfolgt ohne Anmeldefenster (Intranet-Zone konfiguriert)<br>- Die Autorisierung über eine AD-Gruppe ist umgesetzt und mit einem nicht berechtigten Benutzer geprüft (401) |

---

## Stufen 3 und 4 – Mündlicher Nachweis

Die Stufen 3 und 4 werden **nicht** über die Umgebung erreicht, sondern im Gespräch. Damit
wird geprüft, ob Sie verstanden haben, was Sie gebaut haben.

### Ablauf

Umsetzung und mündlicher Nachweis sind zeitlich getrennt:

- **Tag 3–8 – Umsetzung und Abgabe (Stufe 1–2):** Sie bauen die Umgebung und geben die Aufträge
  ab. Pro Tag sind **maximal drei Abgaben** möglich; alle Abgaben müssen bis Ende von Tag 8
  erfolgen. Bei der Abgabe werden Stufe 1 und 2 abgenommen.
- **Tag 9–10 – Schlussbesprechung (Stufe 3–4):** Der mündliche Nachweis für alle abgegebenen
  Aufträge findet gebündelt in der Schlussbesprechung statt. Die Vorbereitungszeit dazwischen
  ist bewusst gewollt — das erneute Durcharbeiten der Aufträge festigt das Verständnis.

Pro Auftrag läuft der mündliche Nachweis so ab und dauert wenige Minuten:

1. Die Lehrperson zieht **zufällig eine Frage** aus dem Fragenkatalog des Auftrags.
2. Beantworten Sie diese Frage korrekt und ohne Hilfe → **Stufe 3 erreicht**. Es folgt eine
   zweite, andere Frage. Auch diese korrekt beantwortet → **Stufe 4 erreicht**.
3. Können Sie die erste Frage nicht beantworten, gibt die Lehrperson **einen vorformulierten
   Hinweis**. Beantworten Sie die Frage danach korrekt → **Stufe 3 erreicht**, Stufe 4 kann für
   diesen Auftrag nicht mehr versucht werden.
4. Bleibt die Frage auch mit Hinweis unbeantwortet, bleibt es bei **Stufe 2**.

| Situation | Ergebnis |
| --- | --- |
| 1. Frage ohne Hilfe korrekt, 2. Frage ohne Hilfe korrekt | Stufe 4 |
| 1. Frage ohne Hilfe korrekt, 2. Frage nicht korrekt | Stufe 3 |
| 1. Frage erst mit Hinweis korrekt | Stufe 3 (Stufe 4 gesperrt) |
| 1. Frage auch mit Hinweis nicht korrekt | Stufe 2 |

### Fragenkatalog

Zu jedem Auftrag gehören **10 Fragen**. Sie liegen als `fragen.md` im Ordner des jeweiligen
Auftrags — zum Beispiel
[Auftrag 03](../../03-auftraege/03-neue-gesamtstruktur-und-client/fragen.md).

Stand: Aufträge 01–09 vorhanden, Aufträge 10–13 und die Varianten B `tbd`

Die Fragen prüfen das **fachliche Verständnis** hinter dem, was Sie gebaut haben: Wozu dient
ein Trust? Was passiert bei einem Domain-Join im Hintergrund? Warum scheitert Kerberos bei
Zeitversatz? Ein Teil der Fragen verlangt zusätzlich den Bezug auf Ihre eigene Umsetzung —
welche Variante Sie gewählt haben und warum.

Der Katalog ist bewusst offen einsehbar. **Sich die Antworten im Selbststudium zu erarbeiten,
ist ausdrücklich erwünscht** — das ist der eigentliche Lernprozess des Moduls. Entscheidend
ist, dass Sie im Gespräch frei und in eigenen Worten antworten können.

> [!TIP]
>
> Nutzen Sie den Fragenkatalog als Selbsttest, bevor Sie einen Auftrag abgeben. Lassen Sie sich
> die Themen von einer KI erklären und abfragen — der Rahmen dafür steht in
> [ki-nutzung.md](../../ki-nutzung.md).

### Protokollierung

Die Lehrperson hält fest, welche Fragen gezogen wurden und ob ein Hinweis nötig war. Damit ist
die Bewertung nachvollziehbar und es wird keine Frage doppelt gestellt.

---

## Notenberechnung

Pro Auftrag ist die erreichte Stufe die Punktzahl (Stufe 1 = 1 Punkt bis Stufe 4 = 4 Punkte).

( Erreichte Punkte / Maximale Punkte ) x 5 + 1 = Modulnote (0.5 gerundet)

> [!IMPORTANT]
>
> Die Umsetzung allein erreicht höchstens Stufe 2, also die Hälfte der Punkte. Ohne den
> mündlichen Nachweis ist das Modul nicht bestehbar — eine funktionierende Umgebung, die Sie
> nicht erklären können, zählt nur zur Hälfte.

## Kompetenzfelder

Die Spalte «Kompetenzfelder» verweist auf die Kompetenzbänder des Moduls gemäss
[kompetenzmatrix.ch](https://kompetenzmatrix.ch/informatiker/cluster-platform/m159/).

| Feld | Bezeichnung | HZ |
| --- | --- | --- |
| A | Struktur und Objekte | 1 |
| B | Einsatz Directory Service | 1 |
| C | LDAP als Protokoll | 2, 6 |
| D | Suche im Directory | 2 |
| E | Objektklassen und Attribute | 2 |
| F | LDIF | 2, 6 |
| G | Datenaustausch | 3, 4 |
| H | Testen | 5 |
| I | Dokumentation und Übergabe | 7 |

> [!NOTE]
>
> Auftrag 02 (Initial Setup) ist eine Infrastrukturvoraussetzung und deckt kein
> Directory-Service-Kompetenzfeld ab. Das Feld **F** (LDIF) ist aktuell durch keinen Auftrag
> abgedeckt: `tbd`

## Varianten B

Für die cloud-abhängigen Aufträge des Blocks 2 existieren cloud-unabhängige Ausweichvarianten.
Sie werden nach denselben vier Stufen bewertet. Ein Wechsel setzt einen dokumentierten
Fehlschlag beim Cloud-Zugang und die Freigabe der Lehrperson voraus; die Einzelheiten stehen in
der [Auftragsübersicht](../../03-auftraege/readme.md).

**Auftrag 05 (Variante B)** ist anders gelagert: keine Ausweichvariante bei einem Fehlschlag,
sondern eine **frei wählbare Alternative** zum AWS-AD-Trust — etwa um die Kosten des Managed AD
zu sparen oder den Kontrast zwischen klassischem Trust und moderner Föderation zu zeigen. Sie
wird mit der Lehrperson abgesprochen und deckt zusätzlich das Feld **C** (LDAP als Protokoll) ab.

Wo eine Variante andere Kompetenzfelder abdeckt als das Original, ist das in der Tabelle oben
ausgewiesen. Für Auftrag 12 entfällt in Variante B das Feld **G** — es bleibt über Auftrag 05
(AWS Managed Microsoft AD) abgedeckt.
