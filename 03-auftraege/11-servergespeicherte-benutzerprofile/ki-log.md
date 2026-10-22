## Auftrag 11 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Schritte und Befehle für Freigaben, Ordnerumleitung, FSLogix und GPO | Auftrag in prüfbare Schritte zerlegen | Pro Schritt Ausgaben und Bilder, Test über zwei Clients mit `jmueller` | Beide Verfahren funktionieren |
| Fehlersuche, als FSLogix nicht griff | Ausgabe des Servers und der Clients auswerten | Registry-Werte der Clients und neuer Test nach Neustart | Ursache war der fehlende Neustart der Clients |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufbau der Freigaben mit `CREATOR OWNER` und bei der Auswertung der drei Bilder des ersten Versuchs.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die Aufgabe nennt die Einstellungen «Swap directory name components» und «Virtual disk type». In meiner FSLogix-Version heissen sie anders (*Flip Flop Profile Directory Name*, *Volume Type*) und liegen in einem Unterordner. Das habe ich am Bild der Einstellungsliste gesehen.
