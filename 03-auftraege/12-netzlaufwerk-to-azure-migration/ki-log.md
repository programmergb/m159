## Auftrag 12 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Schritte für Storage Account, Share, Anbindung per Schlüssel, AD-DS-Join, GPO, Rollen und Tests | Auftrag mit meiner Umgebung in Schritte zerlegen | Pro Schritt Ausgabe und Bild geprüft, Test mit `jmueller` und `tbucher` ohne Schlüssel | Stufe 1 und 2 funktionieren |
| Fehlersuche bei `AzFilesHybrid`, Azure-Anmeldung und fehlenden Rollen | Fehlermeldungen auswerten, Microsoft-Dokumentation zu den Rollennamen lesen | Modul nach dem Nachinstallieren importiert, Anmeldung mit Verzeichnis, Rollen per PowerShell zugewiesen und mit `Get-AzRoleAssignment` geprüft | Alle drei Probleme behoben |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufbau der AD-DS-Anbindung, die viele Stolperstellen hat, und beim Auswerten der Fehler.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** In der ersten Modulliste fehlte `Az.Network`, `Import-Module` meldete es. Die Rollen «SMB Share Reader» und «Contributor» fehlten in der Portalliste. Ich habe in der Microsoft-Dokumentation geprüft, dass es sie gibt, und sie per PowerShell zugewiesen. Dass die Code-Anmeldung scheiterte, kann ich nicht sicher erklären, die Anmeldung mit Angabe des Verzeichnisses hat geklappt.
