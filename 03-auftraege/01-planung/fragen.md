# Auftrag 01 – Fragenkatalog

Diese Fragen dienen dem mündlichen Nachweis der Stufen 3 und 4 gemäss
[Kompetenzmatrix](../../08-kompetenznachweise/lb2/kompetenzmatrix-lb2.md).

**Für Lernende:** Der Katalog ist offen einsehbar. Arbeiten Sie die Antworten vor der Abgabe
durch — im Selbststudium, mit der Fachliteratur oder mit KI-Unterstützung gemäss
[ki-nutzung.md](../../ki-nutzung.md). Entscheidend ist, dass Sie im Gespräch frei und in
eigenen Worten antworten können.

**Für die Lehrperson:** Eine Frage zufällig ziehen. Der Erwartungshorizont nennt, was fallen
muss. Der Hinweis wird nur gegeben, wenn ohne ihn keine Antwort kommt — dann ist Stufe 3
erreichbar, Stufe 4 nicht mehr.

---

### 1

**Frage:** Warum sollte der interne AD-Domainname nicht identisch mit der öffentlichen
Website-Domain der Firma sein?

**Erwartungshorizont:** Namenskonflikt zwischen interner und öffentlicher Auflösung; intern
müssten sonst alle öffentlichen Einträge nachgepflegt werden; erhöhter Wartungsaufwand und
Fehleranfälligkeit.

**Hinweis:** Stellen Sie sich vor, ein Mitarbeiter im Büro ruft die Firmen-Website auf. Welchen
Server fragt sein Rechner, und was findet der?

---

### 2

**Frage:** Welchen internen Domainnamen haben Sie gewählt, und was spricht gegen die früher
übliche Endung `.local`?

**Erwartungshorizont:** Eigene Wahl korrekt genannt und begründet; `.local` kollidiert mit
mDNS/Bonjour und ist für Zertifikate nicht mehr verwendbar; heute üblich ist eine Subdomain
einer selbst besessenen Domain.

**Hinweis:** Es geht um eine Endung, die von einem anderen Dienst zur Gerätesuche im lokalen
Netz belegt ist.

---

### 3

**Frage:** Was ist ein FQDN? Nennen Sie den FQDN Ihres geplanten Domain Controllers.

**Erwartungshorizont:** Vollständiger Name aus Hostname plus Domainname; eindeutig im
Namensraum; eigener DC-FQDN korrekt genannt.

**Hinweis:** Der Hostname allein reicht nicht. Was muss noch dazu, damit der Name weltweit
eindeutig wäre?

---

### 4

**Frage:** Warum plant man IP-Adressen und Hostnamen vorab, statt sie beim Aufsetzen einfach zu
vergeben?

**Erwartungshorizont:** Nachträgliche Änderungen sind aufwendig, weil Dienste, DNS-Einträge
und Konfigurationen daran hängen; Doppelvergaben werden vermieden; die Umgebung bleibt
nachvollziehbar.

**Hinweis:** Denken Sie an den Moment, in dem Sie eine IP-Adresse ändern müssen, nachdem
bereits fünf Systeme darauf verweisen.

---

### 5

**Frage:** Ihre Domäne hat einen DNS-Namen und einen NetBIOS-Namen. Wo liegt der Unterschied?

**Erwartungshorizont:** DNS-Name als vollständiger hierarchischer Name; NetBIOS-Name als kurze
Form, maximal 15 Zeichen, aus Kompatibilitätsgründen weiterhin vorhanden.

**Hinweis:** Bei der Promotion wurden Ihnen zwei Namen vorgeschlagen. Wie sahen sie aus, und
welcher war kürzer?

---

### 6

**Frage:** Warum braucht ein Domain Controller zwingend eine statische IP-Adresse?

**Erwartungshorizont:** Clients finden den DC über DNS-Einträge, die auf seine Adresse
verweisen; bei wechselnder Adresse zeigen die Einträge ins Leere; der DC ist zugleich
DNS-Server, auf den die Clients fest konfiguriert sind.

**Hinweis:** Der DC ist der Server, den alle anderen finden müssen. Was passiert, wenn er
seine Adresse wechselt?

---

### 7

**Frage:** Ihre Planung enthält Client Secrets und Passwörter. Wo gehören solche Werte hin und
wo ausdrücklich nicht?

**Erwartungshorizont:** Nicht ins Git-Repository und nicht in KI-Prompts; getrennte, geschützte
Ablage; in der Dokumentation nur Platzhalter.

**Hinweis:** Ihr Repository ist möglicherweise für andere sichtbar, und Git vergisst nichts.

---

### 8

**Frage:** Was passiert, wenn Sie den AD-Domainnamen nachträglich ändern wollen?

**Erwartungshorizont:** Grundsätzlich möglich, aber aufwendig und riskant; betrifft
Zertifikate, angebundene Dienste, Clients; in der Praxis wird meist neu aufgebaut und
migriert.

**Hinweis:** Es gibt ein Werkzeug dafür. Fragen Sie sich, warum es trotzdem kaum jemand
einsetzt.

---

### 9

**Frage:** Was haben Sie bei der Prüfung der Cloud-Bereitschaft konkret getestet, und was wäre
Ihr Plan B gewesen?

**Erwartungshorizont:** Aktivierung von Azure for Students und Zugriff auf den Entra-ID-Tenant;
Kenntnis der Varianten B und der Voraussetzungen für einen Wechsel.

**Hinweis:** Es ging um zwei Nachweise. Und um die Frage, was passiert, wenn einer davon
scheitert.

---

### 10

**Frage:** Warum ist eine Planung, die zu jedem Wert eine Begründung liefert, mehr wert als
eine, die nur Werte auflistet?

**Erwartungshorizont:** Nachvollziehbarkeit für Dritte und für die spätere Übergabe;
Entscheide bleiben überprüfbar; Fehler fallen bereits beim Begründen auf.

**Hinweis:** Stellen Sie sich vor, jemand anderes übernimmt in einem Jahr Ihre Umgebung. Was
braucht diese Person von Ihnen?
