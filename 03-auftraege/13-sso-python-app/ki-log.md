## Auftrag 13 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Schritte für App-Registrierung, App-Start, Chrome- und Edge-Test | Auftrag mit meiner Umgebung in Schritte zerlegen, App lokal vorbereiten | Login in Chrome (manuell und per SSO) und in Edge auf `client1`, Prüfung der `.env` auf das richtige Format | Alle drei Tests funktionieren, Stufe 1 und 2 belegt |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Vorbereiten der App und beim Prüfen der `.env`, ohne dass die Werte angezeigt wurden.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Ich hatte das Client Secret einmal im Klartext auf einem Bild und beim Eintragen die Spalte «Geheime ID» statt «Wert» kopiert. Die Formatprüfung der Datei hat es gezeigt. Das Secret habe ich ersetzt.
