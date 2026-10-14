## Auftrag 04 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Befehle für Benutzer, Gruppen, Freigaben, NTFS-Rechte und ABE aus der Berechtigungsmatrix | Matrix für meine Abteilungen in Befehle übersetzen, ich prüfe die Tabelle selbst | `icacls` aller neun Ordner und drei Tests als `jmueller`, `slehmann`, `kpromoter` | Rechte stimmen mit der Matrix überein |
| AGDLP-Konzept | Verbesserung der Rechtestruktur für zwei Abteilungen vorschlagen | `icacls` zeigt nur noch die `DL-…`-Gruppen | Als Diagramm und auf zwei Ordnern umgesetzt |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Übertragen der Matrix in Befehle und beim AGDLP-Entwurf.
2. **Wo lag sie falsch oder musste ich korrigieren?** Mein erstes Benutzerpasswort verletzte die Richtlinie, dabei blieben deaktivierte Konten zurück. Ich habe das mit `Get-ADUser … Enabled` geprüft und repariert. Beim AGDLP-Konzept bekam die GL nur noch Lesen. Das habe ich übernommen, weil sie Einsicht, aber nicht überall Schreibrechte braucht.
