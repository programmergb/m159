## Auftrag 03 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Befehle für AD, DNS, Reverse-Zonen, PTR, Papierkorb und Domänenbeitritt | Auftrag mit meiner Planung in Schritte und Befehle zerlegen | Ausgabe pro Schritt geprüft, `nslookup` in beide Richtungen | Alles funktioniert, `client1` ist in der Domäne |
| Port-Test zum DC | `Test-NetConnection` für die Ports aus dem Auftrag | Port 139 `False`, Regel ergänzt, danach alle `True` | Lücke gefunden und behoben |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufteilen des Auftrags in prüfbare Schritte und bei den Befehlen.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Keine relevanten Fehler. Der Port-Test hat nur die Lücke in der Planung gezeigt (Port 139).
