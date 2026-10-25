## Auftrag 06 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Beitritt zur AWS-Domäne, RSAT, Windows Admin Center, WinRM auf `dc1` | Auftrag mit meiner Umgebung in Schritte zerlegen, Fehlermeldungen deuten | Ausgaben von `Get-CimInstance`, `Get-Service`, `Get-NetTCPConnection`, `Test-NetConnection`, Verbindung von dc1 in Windows Admin Center und `whoami` auf dc1 | Alle Etappen laufen, `dc1` wird über den Browser verwaltet |
| Fehlerdiagnose bei Windows Admin Center und Port 5985 | Ereignisprotokoll und Portliste auswerten | Dienst läuft nach dem Beenden der alten Prozesse, `TcpTestSucceeded: True` nach der Regel in der Sicherheitsgruppe | beide Fehler behoben |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Eingrenzen der zwei Fehler: Beim Absturz von Windows Admin Center zeigte der Stacktrace im Ereignisprotokoll, dass der Dienst beim Start Prozesse beenden wollte, und beim Port 5985 trennte der Vergleich von Ping und Port die AWS-Seite von der Windows-Seite.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die KI nannte zuerst den Dienst `ServerManagementGateway` (Name der alten Version), der in v2 `WindowsAdminCenter` heisst. Die Abfrage fand nichts. Dass der Absturz von einem übrig gebliebenen Prozess kam, ist eine Vermutung aus dem Stacktrace, belegt habe ich es nur durch das Ergebnis (Dienst läuft danach).
