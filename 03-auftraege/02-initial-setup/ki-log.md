## Auftrag 02 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Befehle für die Grundkonfiguration von `dc1`, `client1`, `adminctr1` (Hostname, Ping, Tastatur, IPv6, Verknüpfungen) | In einem anderen KI-Chat (ChatGPT) den Auftrag Schritt für Schritt abgearbeitet und die Befehle bekommen | Auf `client1` mit Abfragen geprüft: `hostname`, `(Get-WinUserLanguageList).LanguageTag`, `Get-NetAdapterBinding -ComponentID ms_tcpip6`, IP, Firewallregel, Ping auf `dc1`. Auf `dc1` IE Enhanced Security im Server Manager kontrolliert | Alle Einstellungen stimmen (`client1`, `de-CH`, IPv6 `False`, `10.0.0.20`, Ping `3/3`) |
| Fehler beim Zugriff auf die AWS-Konsole | Fehlermeldung mit `explicit deny` und Rolle `voclabs` erklären | Lab auf der Vocareum-Seite neu gestartet und die Konsole neu geöffnet | Das Lab war abgelaufen, danach ging alles wieder, die Instanzen waren noch da |
| Dokumentation zusammenstellen und gegen den Auftrag abgleichen | Mit Claude Code meine Screenshots und Schritte in eine Doku bringen und mit den Vorgaben des Auftrags vergleichen | Abgleich Punkt für Punkt gegen den Auftrag, Lücken (VPC-Bilder, IE-Sicherheit) nachgeliefert | Die Doku deckt alle Punkte ab, ein Teil der Einstellungen ist nur als Text belegt |

### Reflexion
1. **Wo hat die KI geholfen?** Beim schnellen Durchlaufen der Standardeinstellungen mit fertigen Befehlen und beim Verstehen der Lab-Fehlermeldung.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Ich hatte die Arbeit zuerst als «Auftrag 3» bezeichnet, es ist aber Auftrag 02 (Initial Setup). Das fiel beim Abgleich mit der Nummerierung im Repository auf. Ausserdem hatte ich in diesem Chat ein Administrator-Passwort genannt. Der Chat wies selbst darauf hin, dass ich es ändern soll, und ich habe es geändert. Solche Werte gehören nicht in einen Prompt.
