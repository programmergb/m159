## Auftrag 01 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Entwurf der Planung: Namenskonzept, IP- und Subnetzplan, Sicherheitsgruppen, Begründungen | Aufgabe und Fragenkatalog gelesen, mit meinen Angaben (Name, Klasse, Domänenname `m159giacomo`) eine begründete Planung entwerfen. Keine echten Passwörter eintragen | Werte in den Aufträgen 02 bis 04 verwendet: VPC und Subnetze stimmen mit der Planung überein, `dc1` und `client1` haben die geplanten IPs, `nslookup` löst die Namen in beide Richtungen auf | Planung war brauchbar. Zwei Abweichungen habe ich in der Planung nachgeführt: keine Elastic IPs, und Port 139 musste für den DC zusätzlich geöffnet werden |
| Prüfen, ob die Cloud-Anmeldung klappt (Azure for Students, Entra ID) | Fehlermeldungen (`AADSTS16000`, `AADSTS50058`, 401) erklären und Schritte zur Behebung nennen | Browser-Einstellungen angepasst und den Zugang erneut versucht | Azure for Students funktioniert, der Zugang zu Entra ID noch nicht. Der Nachweis ist offen, die KI kann die Konten nicht für mich aktivieren |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufbau der Planung (Gliederung, Begründungen zu Namen, IPs und Ports) und beim Eingrenzen der Azure-Fehlermeldungen.
2. **Wo lag sie falsch oder unvollständig, und wie habe ich es gemerkt?** Die Sicherheitsgruppe für den DC hatte Port 139 nicht. Das fiel erst in Auftrag 03 auf, als `Test-NetConnection` auf diesen Port `False` zeigte. Daraus habe ich gelernt, dass eine Planung erst durch den Test in der Umgebung bestätigt ist.
