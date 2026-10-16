## Auftrag 10 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Schritte für Cloud-Admin, Domain-Verifikation, UPN, Entra Connect und Hybrid Join | Auftrag Schritt für Schritt mit meiner Umgebung durchgehen | Pro Schritt Bilder und Ausgaben geprüft (Domain verifiziert, `ADSync` läuft, Benutzer in Entra, `dsregcmd`) | `client1` ist hybrid joined |
| Fehlersuche beim Hybrid Join | Ausgaben von `dsregcmd` und der geplanten Aufgabe auswerten | Fehlercode `0x801c03f3` eingegrenzt, Zertifikat im Computerobjekt geprüft, Delta-Synchronisierung gestartet, Beitritt wiederholt | Beitritt klappt im zweiten Versuch |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Eingrenzen des Beitrittsfehlers anhand der gefilterten `dsregcmd`-Ausgabe.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die KI hielt den ersten Download-Knopf zuerst für Cloud Sync und war sich beim Titel des Fensters nicht sicher. Erst der Dateiname der heruntergeladenen Datei hat es geklärt. Das deaktivierte Konto beim Anlegen von `entra-admin` stand schon auf dem Bild der Zusammenfassung.
