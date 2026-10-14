## Auftrag 08 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Filter, LDAP-Query für die GPO und Konfigurationsblatt | Filter aufstellen, warum `memberOf` bei Nesting nichts findet, Blatt ausfüllen | Filter ausgeführt und Trefferzahlen verglichen, zwei Tests, Desktop von `kpromoter` und `jmueller` | Filter und Zielgruppenadressierung wirken |
| Entscheid Dienstkonto und Port 389/636 | Optionen und Folgen aufzählen | Dienstkonto mit Lese- und Schreibversuch, LDAPS auf dem DC geprüft | LDAPS geht im Labor nicht, im Blatt steht der Soll-Zustand |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Verstehen der Filter-Regeln `…1.4.1941` und `…1.4.803`.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** `Test-NetConnection` auf Port 636 zeigte `True`, obwohl LDAPS nicht geht: Der Port nimmt TCP an, beendet aber die TLS-Verhandlung. Erst der TLS-Test und `ldp.exe` haben das gezeigt.
