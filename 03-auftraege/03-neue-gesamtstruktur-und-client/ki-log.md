## Auftrag 03 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Befehle für AD-DS-Rolle, Promotion, DNS-Weiterleitung, Reverse-Zonen, PTR, Papierkorb, Domänenbeitritt | Auftrag mit meiner Planung (`corp.m159giacomo.dynv6.net`) in Schritte und Befehle zerlegen | Pro Schritt Ausgabe geprüft: `Get-WindowsFeature`, `Get-ADDomainController`, `Get-DnsServerForwarder`, `Get-DnsServerZone`, `nslookup` in beide Richtungen, `Get-ADOptionalFeature` | Alles funktioniert, `client1` ist in der Domäne |
| Port-Test zum Domänencontroller auswerten | Aus dem Auftrag eine `Test-NetConnection`-Schleife für die Ports 88, 135, 139, 389, 445 bauen lassen | Erster Test: Port 139 `False`. Regel in der Sicherheitsgruppe ergänzt, zweiter Test: alle `True` | Fehler gefunden und behoben, Vorher- und Nachher-Bild in der Doku |
| Warum 9.9.9.9 statt 8.8.8.8 | Stichworte zur Begründung | Selbst nachgelesen und in eigenen Worten formuliert | Begründung in der Doku |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufteilen des grossen Auftrags in prüfbare Schritte und bei den PowerShell-Befehlen für DNS und AD.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Keine relevanten Fehler der KI. Der Port-Test hat eine Lücke in der Planung aufgedeckt (Port 139, siehe Tabelle).
