## Auftrag 04 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Befehle für Benutzer, Gruppen, Ordner, Freigaben, NTFS-Rechte und ABE | Auftrag und Berechtigungsmatrix lesen und meine Abteilungen daraus als Befehle umsetzen | `Get-ADGroupMember` für die Gruppen, `icacls` für alle neun Ordner (keine geerbten Rechte, kein `BUILTIN\Users`), `Get-SmbShare`, `FolderEnumerationMode` | Alle Berechtigungen stimmen mit der Matrix überein |
| Matrix aus dem Bild in Rechte übersetzen | Berechtigungsmatrix (Bild) für meine vier Abteilungen als Tabelle aufschreiben. Ich sollte die Tabelle selbst gegen das Original prüfen | Die Tabelle gegen die Matrix kontrolliert und die Rechte in der Praxis getestet | Drei Tests als `jmueller`, `slehmann`, `kpromoter` auf `client1` ergeben genau die erwarteten Ergebnisse (lesen, schreiben, kein Zugriff) |
| AGDLP-Konzept entwerfen | Verbesserung der Rechtestruktur für zwei Abteilungen vorschlagen | `icacls` zeigt auf `Buchhaltung` und `Sekretariat` nur noch die `DL-…`-Gruppen | Konzept als Diagramm und auf beiden Ordnern umgesetzt |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Übertragen der Matrix in Befehle und beim Entwurf des AGDLP-Konzepts mit Diagramm.
2. **Wo lag sie falsch oder musste ich korrigieren, und wie habe ich es gemerkt?** Beim Anlegen der Benutzer lehnte die Domäne mein Passwort ab (Richtlinie). Dabei blieben deaktivierte Konten zurück («account already exists» beim zweiten Versuch). Ich habe das mit `Get-ADUser … Enabled` geprüft und mit gültigem Passwort repariert. `Add-ADGroupMember` wurde zuerst nicht erkannt, weil das AD-Modul in dieser PowerShell nicht verfügbar war. Danach lief es. Beim AGDLP-Konzept hat die KI der Geschäftsleitung auf den umgestellten Ordnern nur noch Lesen gegeben. Das ist ihre Entscheidung, die ich übernommen habe, weil ich sie begründen kann: Die GL braucht Einsicht, aber nicht überall Schreibrechte.
