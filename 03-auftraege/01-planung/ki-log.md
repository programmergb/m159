## Auftrag 01 – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Entwurf der Planung (Namen, IPs, Subnetze, Sicherheitsgruppen, Begründungen) | Planung mit meinen Angaben und Begründungen entwerfen, keine Passwörter eintragen | Werte in den Aufträgen 02 bis 04 verwendet: VPC, IPs und `nslookup` stimmen | Brauchbar, zwei Abweichungen nachgeführt (keine Elastic IPs, Port 139) |
| Anmeldefehler bei Azure und Entra ID eingrenzen | Fehlermeldungen (`AADSTS…`) erklären | Browser-Einstellungen angepasst, Zugang erneut versucht | Zugang funktioniert |

### Reflexion
1. **Wo hat die KI geholfen?** Beim Aufbau der Planung und bei den Azure-Fehlermeldungen.
2. **Wo lag sie falsch, und wie habe ich es gemerkt?** Die Sicherheitsgruppe für den DC hatte Port 139 nicht. Das zeigte erst der Port-Test in Auftrag 03.
