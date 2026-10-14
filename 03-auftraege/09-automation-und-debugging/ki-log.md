## Auftrag 09 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Fehler im Skript finden und erklären | Das Skript analysieren, die fünf Fehler benennen und für meine Domäne reparieren, mit einer Existenzprüfung ergänzt | Skript zweimal ausgeführt: beim ersten Lauf werden die Benutzer angelegt (grün), beim zweiten übersprungen (gelb). Dazu `Get-ADUser` mit den DNs der drei Benutzer | Skript läuft fehlerfrei, die drei Benutzer liegen in den richtigen OUs |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Erkennen der fünf Fehler (Trennzeichen, `$( )` in Strings, falsche Pfade, `ConvertFrom-` statt `ConvertTo-SecureString`, OU-Logik) und beim Erklären, warum sie nicht funktionieren.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Hier habe ich keinen Fehler der KI festgestellt. Ich habe die Antwort nicht einfach übernommen, sondern durch den doppelten Lauf geprüft, dass beide Zweige (anlegen und überspringen) wirklich funktionieren.
