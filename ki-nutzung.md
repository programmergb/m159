# KI-Nutzung im Modul 159

Dieses Dokument regelt verbindlich, wie Künstliche Intelligenz (KI) im Modul 159 eingesetzt
werden darf, welche Rolle sie im Lernprozess einnimmt und wie ihr Einsatz nachgewiesen und
bewertet wird. Es gilt für alle Aufträge der LB2.

Modulverantwortliche Person und Kontakt: siehe GitLab-Projektbeschreibung dieses Repositories.

---

## Grundprinzip: Die Umgebung ist die Prüfinstanz

In M159 bauen Sie eine echte Infrastruktur auf AWS. Das hat eine Konsequenz, die den gesamten
KI-Einsatz in diesem Modul bestimmt:

> [!IMPORTANT]
>
> **KI kann Ihnen den Weg zu einer Lösung abkürzen. Sie kann das Resultat nicht fälschen.**
>
> Ein LDAP-Suchfilter liefert die richtigen Objekte oder nicht. Kerberos funktioniert nach dem
> Zeitabgleich oder nicht. `dsregcmd /status` zeigt `AzureAdJoined : YES` oder eben nicht.

Deshalb gibt es in diesem Modul **kein KI-Verbot**. Stattdessen gilt durchgehend:

**Sie dürfen KI für Hypothese, Erklärung und Formulierung einsetzen. Der Nachweis ist immer ein
Artefakt aus Ihrer eigenen Umgebung.**

---

## 1. KI-Nutzungsrahmen

### Erlaubt

| Einsatz | Beispiel |
| --- | --- |
| Konzepte erklären lassen | «Erkläre mir den Unterschied zwischen Freigabe- und NTFS-Berechtigungen» |
| Fehlermeldungen einordnen | «Was bedeutet dieser Kerberos-Fehler und welche Ursachen kommen infrage?» |
| Hypothesen für die Fehlersuche generieren | «Nenne mir 5 mögliche Ursachen, warum der Domain-Join scheitert» |
| Syntax erklären und prüfen lassen | «Was macht dieser LDAP-Filter genau?» |
| Skripte verstehen und debuggen | PowerShell-Fehler analysieren |
| Dokumentation sprachlich überarbeiten | Ihren eigenen Text kürzen, strukturieren, korrigieren lassen |
| Varianten vergleichen lassen | «Welche Optionen gibt es, um ein Zusatzattribut am User zu speichern?» |

### Nicht erlaubt

| Einsatz | Warum |
| --- | --- |
| KI direkt an Ihre Umgebung anbinden, sodass sie Schritte selbst ausführt (z. B. per MCP an Browser, RDP oder SSH) | Dann setzt die KI den Auftrag um, nicht Sie — die Eigenleistung entfällt (KI als Lösungsautomat, siehe Abschnitt 2) |
| Screenshots oder Videos generieren oder verändern | Fälschung eines Funktionsnachweises |
| Ausgaben von Befehlen erfinden lassen | Fälschung eines Funktionsnachweises |
| Dokumentation zu Schritten schreiben lassen, die Sie nicht ausgeführt haben | Sie dokumentieren eine fremde Umgebung |
| KI-Ausgaben ungeprüft übernehmen | Verstoss gegen die Verifikationspflicht (siehe Abschnitt 6) |
| KI-Einsatz verschweigen | Verstoss gegen die Nachweispflicht (siehe Abschnitt 9) |

> [!WARNING]
>
> Ein gefälschter Funktionsnachweis wird wie eine nicht erbrachte Leistung behandelt. Bereits
> in den bestehenden Abgaberegeln gilt: In Dokumentation und Screencast muss **eindeutig**
> erkennbar sein, dass es sich um Ihre eigene Umgebung handelt.

---

## 2. Rolle der KI im Lernprozess

KI übernimmt in diesem Modul vier klar abgegrenzte Rollen:

| Rolle | Was die KI tut | Was Sie tun |
| --- | --- | --- |
| **Tutor / Erklärer** | Erklärt Konzepte auf Nachfrage, in Ihrem Tempo, so oft Sie wollen | Sie stellen die Frage und prüfen die Antwort gegen die Fachliteratur |
| **Sparringpartner** | Liefert Hypothesen bei der Fehlersuche und Gegenargumente bei Entscheiden | Sie testen die Hypothesen an Ihrem System und entscheiden |
| **Reviewer / Feedbackgeber** | Prüft Ihre Konfiguration, Ihren Filter oder Ihr Skript und benennt Schwachstellen | Sie beurteilen, welcher Einwand zutrifft, und verantworten die Änderung |
| **Lernbegleiter** | Fragt Sie zum Fragenkatalog ab und stellt Rückfragen zu Ihren Antworten | Sie antworten frei und erkennen selbst, wo Ihr Verständnis noch dünn ist |

Nicht vorgesehen ist die Rolle als **Lösungsautomat**. Nicht aus formalen Gründen: Wer sich die
Lösung generieren lässt, steht in der Schlusspräsentation vor einer Umgebung, die er nicht
erklären kann (siehe Abschnitt 6).

---

## 3. KI-Kompetenzaufbau

Der kompetente Umgang mit KI ist in diesem Modul ein eigenes Lernziel, kein Nebeneffekt. Sie
üben dabei drei Fähigkeiten:

1. **Präzise fragen (Prompt Engineering).** Eine KI-Antwort ist nur so gut wie der Kontext, den
   Sie mitliefern. In M159 heisst das konkret: Domainname, Betriebssystemversion, exakte
   Fehlermeldung, bereits ausgeschlossene Ursachen.
2. **Antworten verifizieren und Quellen validieren.** Sie lernen, KI-Ausgaben gegen eine
   unabhängige Quelle zu prüfen: Microsoft Learn, die Fachliteratur im Repository oder — am
   stärksten — Ihre eigene Umgebung.
3. **Halluzinationen erkennen.** KI-Modelle erfinden im Zweifel plausibel klingende Antworten,
   statt Unwissen einzuräumen. Typische Fälle in diesem Modul: Cmdlets oder Parameter, die es
   nicht gibt; erfundene Menüpfade; Attributnamen, die im Schema nicht existieren. Das Muster
   ist immer dasselbe — die Antwort klingt sicher und lässt sich nicht nachvollziehen.
4. **Grenzen erkennen.** Sie lernen einzuschätzen, wo KI in der Systemtechnik zuverlässig ist
   (Syntax, Konzepterklärung, Fehlerklassifikation) und wo sie systematisch scheitert
   (aktuelle Portalpfade, versionsspezifische Menüführung, Ihre konkrete Netzwerktopologie).

> [!TIP]
>
> KI-Modelle haben einen Wissensstand mit Stichdatum und kennen Ihre AWS-Umgebung nicht. Gerade
> bei Azure- und Entra-Portalpfaden sind Anleitungen häufig veraltet — die Oberflächen ändern
> sich schneller als die Trainingsdaten. Wenn eine Klickanleitung nicht passt, ist das meist
> kein Fehler von Ihnen.

---

## 4. KI-gestützter Lernprozess

KI ist an definierten Stellen fest in die Aufträge eingebaut. Der Ablauf ist überall derselbe:

```
Eigener Versuch  →  KI als Tutor/Sparringpartner  →  Verifikation am System  →  Dokumentation
```

Der erste Schritt ist nicht verhandelbar: **Versuchen Sie es zuerst selbst.** Wer bei jeder
Hürde sofort die KI fragt, übt das Fragen — nicht das Fach.

### Laufende Abgaben statt einer Schlussabgabe

Der Lernprozess ist in diesem Modul in Schleifen organisiert. Sie geben **jeden Auftrag
einzeln ab**, sobald er fertig ist (Tag 3–8), und erhalten dabei sofort Rückmeldung, ob die
Umsetzung stimmt (Stufe 1–2). Was Sie dort über Ihre Arbeitsweise lernen, fliesst in den
nächsten Auftrag ein — dreizehnmal hintereinander.

Diese Rückkopplung ist ein Grund, warum sich ein KI-Einsatz ohne eigenes Verständnis in M159
nicht auszahlt: Eine Umsetzung, die Sie nicht durchdrungen haben, fällt schon bei der Abnahme
auf. Den zweiten Teil — den mündlichen Nachweis Ihres Verständnisses (Stufe 3–4) — erbringen
Sie gebündelt in der Schlussbesprechung. Die zugehörigen Fragen liegen pro Auftrag offen vor;
Sie dürfen sich darauf vorbereiten, beantworten müssen Sie sie in eigenen Worten.

### Aufträge mit ausformuliertem KI-Anteil

| Auftrag | Wofür |
| --- | --- |
| [08 – Suche im Directory](03-auftraege/08-suche-im-directory/readme.md) | LDAP-Filter erklären und prüfen lassen, ohne die Lösung zu erfragen |
| [10 B – Zweiter DC & Replikation](03-auftraege/10-ms-entra-id-ms-entra-connect/variante-b-zweiter-dc-replikation.md) | Replikationsfehler eingrenzen, Hypothesen am System prüfen |

In allen übrigen Aufträgen gelten dieselben Regeln, auch ohne eigenen Abschnitt.

---

## 5. Reflexion der KI-Nutzung

Zu jedem Auftrag mit KI-Anteil beantworten Sie **zwei kurze Fragen** in Ihrem Repository. Kein
Aufsatz — zwei bis drei Sätze pro Frage genügen:

1. **Wo hat die KI mir geholfen?** Was ging dadurch schneller oder wurde verständlicher?
2. **Wo lag die KI falsch oder war unbrauchbar?** Wie haben Sie es gemerkt?

Frage 2 ist die wichtigere. Wer nie eine falsche KI-Antwort bemerkt hat, hat entweder nicht
verifiziert oder die KI nicht ernsthaft eingesetzt.

---

## 6. Eigenleistung trotz KI

Die Eigenleistung wird in M159 nicht über Textanalyse oder KI-Detektoren geprüft, sondern über
Mechanismen, die strukturell nicht delegierbar sind:

| Mechanismus | Warum KI hier nicht hilft |
| --- | --- |
| **Mündlicher Nachweis in der Schlussbesprechung** | Eine zufällig gezogene Frage beantworten Sie im Gespräch in eigenen Worten — vorbereiten dürfen Sie sich, im Moment selbst antworten müssen Sie ohne Hilfsmittel |
| **Live-Demonstration der Umgebung** | Die Umgebung muss vor Ort funktionieren, nicht auf einem Screenshot |
| **Rückfragen zu eigenen Entscheiden** | «Warum haben Sie diese OU-Struktur gewählt?» lässt sich nicht vorgenerieren |
| **Artefakte aus der eigenen Umgebung** | Ihre Domainnamen, IP-Adressen und Hostnamen sind eindeutig |

Der mündliche Nachweis ist der wichtigste dieser Mechanismen, weil er **die Hälfte der Punkte
trägt** (Stufen 3 und 4). Die Fragen sind offen einsehbar und Sie dürfen sich mit KI darauf
vorbereiten — beantworten müssen Sie sie selbst.

> [!NOTE]
>
> Daraus folgt die praktische Regel für Sie: **Sie müssen jeden Schritt Ihrer Dokumentation
> erklären können.** Wenn Sie eine KI-generierte Konfiguration übernehmen, ohne sie zu
> verstehen, fällt das schon bei der Abnahme auf — und spätestens im mündlichen Nachweis, der
> die Hälfte der Punkte trägt.

---

## 7. Bewertung des KI-Einsatzes

Der KI-Einsatz wird **nicht separat benotet**. Er ist in der
[Kompetenzmatrix LB2](08-kompetenznachweise/lb2/kompetenzmatrix-lb2.md) an folgenden Stellen
verankert:

| Stufe | Bezug zur KI-Nutzung |
| --- | --- |
| **1 und 2** (Umsetzung) | `ki-log.md` und Reflexion gehören zu den Abgabekriterien der Aufträge mit KI-Anteil |
| **3 und 4** (Fachgespräch) | Prüfen das Verständnis unabhängig davon, wie die Lösung entstanden ist |

Bewertet wird also **nicht, ob** Sie KI eingesetzt haben, sondern ob Ihre Lösung funktioniert,
ob Sie sie erklären können und ob Ihr Vorgehen nachvollziehbar dokumentiert ist.

> [!TIP]
>
> Rechnen Sie es sich durch: Wer alles baut, aber nichts erklären kann, erreicht pro Auftrag
> zwei von vier Punkten. Wer versteht, was er baut, erreicht vier. Die Zeit, die Sie beim
> Abkürzen sparen, holen Sie im Gespräch nicht wieder herein.

---

## 8. KI-Tutor- und Prompt-Unterstützung

Die folgenden Prompt-Muster sind auf M159 zugeschnitten. Ersetzen Sie die Platzhalter in eckigen
Klammern durch Ihre eigenen Werte.

### Konzept verstehen

```text
Erkläre mir [Konzept] im Kontext von Active Directory Domain Services.
Ich bin Lernende:r im Bereich Systemtechnik und kenne bereits [Vorwissen].
Nenne mir zum Schluss zwei typische Missverständnisse zu diesem Thema.
```

### Fehler eingrenzen (statt Lösung erfragen)

```text
Ich erhalte folgende Fehlermeldung: [exakte Meldung].
Umgebung: [OS-Version], Domain [FQDN], Rolle [DC/Client].
Ich habe bereits geprüft: [was Sie ausgeschlossen haben].
Nenne mir die 5 wahrscheinlichsten Ursachen, sortiert nach Wahrscheinlichkeit,
und für jede: mit welchem Befehl kann ich sie überprüfen?
Gib mir noch keine Lösung.
```

Der letzte Satz ist entscheidend. Er verwandelt die KI von einem Lösungsautomaten in einen
Sparringpartner und lässt die eigentliche Diagnosearbeit bei Ihnen.

### Eigene Lösung prüfen lassen

```text
Ich habe folgenden LDAP-Suchfilter geschrieben: [Ihr Filter].
Ziel: [was er finden soll].
Erkläre mir Schritt für Schritt, welche Objekte dieser Filter zurückgibt.
Sage mir nicht, ob er richtig ist — ich will es selbst beurteilen.
```

### Entscheid abwägen

```text
Ich muss [Entscheidung, z. B. ein Zusatzattribut am User-Objekt speichern].
Nenne mir die gängigen Optionen mit je zwei Vor- und Nachteilen.
Empfehle mir nichts — ich entscheide selbst und begründe es.
```

### Konfiguration prüfen lassen (Review)

```text
Hier ist meine Konfiguration für [Thema]: [Ihre Einstellungen].
Prüfe sie wie ein Reviewer: Nenne mir Schwachstellen und Risiken,
sortiert nach Schweregrad.
Behebe nichts — ich entscheide, was ich ändere.
```

### Sich auf den mündlichen Nachweis vorbereiten

```text
Du bist mein Lernbegleiter. Stelle mir nacheinander Fragen zu [Thema aus dem Auftrag].
Nach jeder meiner Antworten: sage mir nicht sofort die Lösung, sondern stelle eine
Rückfrage, die mich auf die Lücke stösst.
Erst wenn ich zweimal danebenliege, erkläre es mir.
```

Dieses Muster ist der Kern des Selbsttests: Sie lassen sich zum
[Fragenkatalog](03-auftraege/readme.md) Ihres Auftrags abfragen und merken selbst, wo Ihr
Verständnis noch dünn ist — bevor es die Lehrperson merkt.

### Dokumentation überarbeiten

```text
Hier ist meine Dokumentation zu [Auftrag]: [Ihr Text].
Kürze sie, ohne fachliche Aussagen zu verändern oder zu ergänzen.
Markiere Stellen, die fachlich unklar oder unbelegt sind.
```

> [!NOTE]
>
> Allen Mustern ist eines gemeinsam: **Hinweise statt Lösungen.** Formulierungen wie «Gib mir
> noch keine Lösung», «Sage mir nicht, ob es richtig ist» oder «Behebe nichts» halten die
> Denkarbeit bei Ihnen. Ohne sie liefert die KI sofort ein Ergebnis — und Sie haben nichts
> geübt.

> [!WARNING]
>
> Fügen Sie **niemals** Client Secrets, Passwörter, Access Keys oder Tenant-IDs in einen
> KI-Prompt ein. In Auftrag 13 arbeiten Sie mit einem Entra-ID Client Secret — dieses gehört
> weder in einen Prompt noch in Ihr Git-Repository. Ersetzen Sie solche Werte durch Platzhalter.

---

## 9. Nachweis der KI-Nutzung

Wo Sie KI eingesetzt haben, halten Sie das in Ihrem Repository fest. Der Aufwand ist bewusst
klein gehalten — legen Sie pro Auftrag mit KI-Anteil eine Datei `ki-log.md` an:

```markdown
## Auftrag [Nr.] – KI-Einsatz

| Wofür eingesetzt | Prompt (sinngemäss) | Wie verifiziert | Ergebnis |
| --- | --- | --- | --- |
| Kerberos-Fehler eingrenzen | Fehlermeldung + Umgebung, 5 Ursachen erfragt | w32tm /query /status auf Client und DC | Zeitversatz 11 Min, korrigiert – Login funktioniert |

### Reflexion
1. Wo hat die KI geholfen?
2. Wo lag sie falsch, und wie habe ich es gemerkt?
```

Die Spalte **«Wie verifiziert»** ist der Kern des Nachweises. Sie zeigt, dass Sie die KI-Antwort
geprüft und nicht übernommen haben.

> [!TIP]
>
> Führen Sie das Log **während** der Arbeit, nicht danach. Der Eintrag entsteht in dem Moment,
> in dem Sie ohnehin gerade verifizieren — nachträglich ist es Rekonstruktionsarbeit.

### Entscheidungsprotokoll

Wo ein Auftrag einen Entscheid verlangt — Dienstkonto in Auftrag 08, FSMO-Verteilung in
Variante B von Auftrag 10, RDP-Konzept in Auftrag 03 — halten Sie zusätzlich fest:

- **Welche Optionen** standen zur Wahl?
- **Wofür** haben Sie sich entschieden?
- **Warum**, und was sprach dagegen?

Zwei bis drei Sätze genügen. Dieses Protokoll ist die Vorlage für Ihre Antwort im
mündlichen Nachweis — und es zeigt, dass der Entscheid Ihrer war, nicht der einer KI.

### Ihre Versionshistorie

Sie arbeiten im eigenen Git-Repository. Regelmässige Commits mit aussagekräftigen Meldungen
machen Ihren Lernprozess ohne Zusatzaufwand sichtbar: Man sieht, wie eine Lösung entstanden
ist, nicht nur, dass sie am Schluss da war.

---

## Verwandte Dokumente

- [Instruktionen zum Modul](01-instruktionen/readme.md) – Arbeitsweise, Abgabe, Präsentation
- [Kompetenznachweise](08-kompetenznachweise/readme.md) – Bewertung und Notengebung
- [Kompetenzmatrix LB2](08-kompetenznachweise/lb2/kompetenzmatrix-lb2.md) – Bewertungskriterien
  je Auftrag
- [Aufträge](03-auftraege/readme.md) – Übersicht aller Aufträge
