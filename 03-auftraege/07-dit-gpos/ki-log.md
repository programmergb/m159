## Auftrag 07 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| DIT, GPOs für Laufwerke, Verknüpfung, Dateikopie, Drucker und 7-Zip | Jeden Teil des Auftrags in Befehle und GUI-Schritte zerlegen | `gpresult /r` und sichtbarer Effekt auf `client1` mit `jmueller` und `kpromoter` | Alle Teile funktionieren, mit Bildern belegt |
| WMI-Filter | Filter für Windows 10 und Unterschied zu Servern erklären | Zwei Tests: Server-2025-Filter kopiert die Datei, Windows-10-Filter nicht | Mechanismus belegt, obwohl `client1` ein Server ist |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Zerlegen des Auftrags und bei der Ursachensuche.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die Datei lag nicht im Temp-Ordner der RDP-Sitzung, sondern direkt in `...\AppData\Local\Temp`. Das zeigte eine Suche im ganzen Temp-Ordner. Für 7-Zip verlangt der Auftrag Zielgruppenadressierung, die es für Softwareinstallation nicht gibt. Ich habe die Sicherheitsfilterung verwendet und das in der Doku begründet.
