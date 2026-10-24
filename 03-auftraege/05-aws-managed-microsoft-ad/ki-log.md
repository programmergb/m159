## Auftrag 05 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Schritte für Verzeichnis, Conditional Forwarder und Trust auf beiden Seiten | Auftrag mit meiner Planung in Schritte zerlegen | `nslookup` auf die AWS-Domäne, Status auf AWS «Bestätigt», `Get-ADTrust`, `Get-ADDomain -Server`, `nltest` | Trust funktioniert in beide Richtungen |
| Fehlerbild «Bestätigung fehlgeschlagen» und «Credentials nicht ausreichend» einordnen | Meldungen erklären | AWS-Status nach dem Anlegen der Gegenseite neu geprüft | Beide Meldungen waren ein Zwischenzustand, kein Fehler im Trust |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Einordnen der zwei Fehlermeldungen und beim Zusammenstellen der Sicherheitsüberlegungen.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Keine relevanten Fehler. Ob die Sicherheitsgruppen schon ausreichen, haben wir nicht angenommen, sondern mit dem fertigen Trust und der Abfrage der Gegendomäne bestätigt.
