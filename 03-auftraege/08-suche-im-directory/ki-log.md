## Auftrag 08 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Suchfilter für Teil 2 und 3 und Erklärung der Vergleichsregeln | Filter zu den Suchen aufstellen und erklären, warum `memberOf` bei verschachtelten Gruppen nichts findet | Jeden Filter ausgeführt und die Trefferzahl mit meiner Struktur verglichen: der naive Filter liefert 0, der korrigierte 1. Dazu zwei Tests (Deaktivieren, neue Gruppe) | Filter und Erklärung stimmen |
| LDAP-Query für die Zielgruppenadressierung und Konfigurationsblatt | Filter aus 3.3 mit `%LogonUser%` ergänzen und die beiden Formulare ausfüllen | Auf `client1`: `kpromoter` bekommt die Verknüpfung, `jmueller` nicht. Dienstkonto mit Lese- und Schreibversuch getestet | Wirksam und belegt |
| Entscheid zu Dienstkonto und Port 389/636 | Optionen und Folgen aufzählen | LDAPS auf dem DC selbst geprüft (Port-Test, TLS-Verbindung, `ldp.exe`) | LDAPS geht im Labor nicht, im Blatt steht der Soll-Zustand mit Begründung |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Verstehen der Filter-Regeln `1.2.840.113556.1.4.1941` und `1.2.840.113556.1.4.803` und beim Aufbau der Tests.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** `Get-Credential` lieferte keine Anmeldedaten (kein Anmeldefenster oder abgebrochen), die KI hat es durch `Read-Host` und `PSCredential` ersetzt. Der Test `Test-NetConnection` auf Port 636 zeigte `True`, obwohl LDAPS nicht funktioniert: Der Port nimmt die TCP-Verbindung an, die TLS-Verhandlung wird aber sofort beendet. Erst der TLS-Test und `ldp.exe` haben das offengelegt. Der erste Pfad zum Zertifikatsspeicher (`Cert:\LocalMachine\NTDS\My`) existierte nicht. Beim Testbenutzer war mein Passwort zu kurz, deshalb blieb das Konto deaktiviert. Auch das habe ich an der Ausgabe gemerkt.
