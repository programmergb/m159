## Auftrag 07 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| DIT entwerfen und als Diagramm zeichnen | DIT nach den Regeln des Auftrags für meine Abteilungen mit Beispiel-DNs | Struktur im AD angelegt und mit `Get-ADOrganizationalUnit` und `Get-ADUser` gegen das Diagramm geprüft | OU-Struktur und Benutzer stimmen mit dem DIT überein |
| GPOs für Netzlaufwerke, Verknüpfung, Dateikopie, Drucker, 7-Zip anlegen | Jeden Teil des Auftrags in Befehle und GUI-Schritte aufteilen und Bilder an den richtigen Stellen anfordern | Pro GPO `gpresult /r` und der sichtbare Effekt auf `client1` als `jmueller` und `kpromoter` | Alle sechs Teile funktionieren und sind mit Bildern belegt |
| WMI-Filter erklären | Filter für Windows 10, Unterschied zu Windows 11 und Servern | Zwei Tests: mit einem Filter für Server 2025 kommt die Datei an, mit dem Windows-10-Filter nicht (`Denied (WMI Filter)`) | Mechanismus belegt, obwohl mein Client ein Server ist |
| Namenskonvention der GPOs | PDF-Kapitel 5.4 lesen und eine Konvention ableiten | Alle GPOs folgen `<Kategorie> <Zweck> <Comp\|User> - GB` | Einheitliche Namen |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Zerlegen des grossen Auftrags in Schritte und beim Finden der Ursachen, wenn etwas nicht wie erwartet kam.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die KI ging davon aus, dass die Datei im Temp-Ordner der RDP-Sitzung landet. Sie lag aber direkt in `...\AppData\Local\Temp`. Ich habe das mit einer Suche im ganzen Temp-Ordner gemerkt. Beim Drucker-Test war `Get-PrintConfiguration` mit einer Liste von Namen ein Fehler der KI (es nimmt nur einen Namen). Bei der 7-Zip-Installation verlangt der Auftrag eine Zielgruppenadressierung, die es für die Softwareinstallation nicht gibt. Ich habe stattdessen die Sicherheitsfilterung verwendet und das in der Doku begründet. Beim Drucker-Eintrag war zuerst das Häkchen «Standarddrucker» gesetzt, was bei zwei Druckern nicht sinnvoll ist, weil sich beide überschreiben würden. Das fiel auf dem Bild des Dialogs auf, und ich habe es korrigiert.
