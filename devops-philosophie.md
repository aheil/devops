# DevOps Philosophie

## Lernziele

* **Verstehen** worin die Probleme in der klassischen Trennung zwischen Entwicklung und Betrieb liegen.
* Den zentralen chronischen Konflikt als auch die damit in Zusammenhang stehende Abwärtsspirale hinsichtlich von Firmenzielen **verstehen**.

## Warum wird DevOps benötigt?

Die Frage lässt sich mit einem Blick auf die Evolution von Entwicklung und Deployment von Features in IT-Produkten und Dienstleistungen in den vergangenen 40 Jahren erklären:

| Was              | früher                | heute               |
| ---------------- | --------------------- | ------------------- |
| Kosten           | 2.000.000-3.000.000 € | 1.000-100.000 €     |
| Entwicklungszeit | 1-5 Jahre             | ca. 2 Wochen        |
| Deployment-Zeit  | Wochen - 2-3 Monate   | Wochen - 2-3 Monate |

**Beschleunigter Trend**

|                             | 1970er bis 1980er                     | 1990er                                 | 2000er bis heute                            |
| --------------------------- | ------------------------------------- | -------------------------------------- | ------------------------------------------- |
| Ära                         | Mainframes                            | Client/Server                          | Commodities u. Cloud                        |
| Representative Technologien | COBOL, DB2                            | C++, Oracle, Solaris                   | Java, MySQL, RedHat, Ruby on Rails, PHP, JS |
| Zyklusdauer                 | 1-5 Jahre                             | 3-12 Monate                            | 2-12 Wochen                                 |
| Kosten                      | 1.000.000-100.000.000.000 US$         | 100.000-10.000.000 US$                 | 10.000-1.000.000 US$                        |
| Riskant für                 | Gesamtes Unternehmen                  | Produktlinie-/bereich                  | Produkt-Feature                             |
| Folgen bei Fehlern          | Bankrott, Firmenverkauf, Entlassungen | Umsatzziele verfehlt, C-Level gefeuert | vernachlässigbar                            |

Quelle 1: Adrian Cockcroft, Velocity and Volume (or Speed Wins), Flowcon, November 2013, 2013

Quelle 2: KEYNOTE: Velocity and Volume (or Speed Wins) by Adrian Cockcroft, [https://www.youtube.com/watch?v=wyWI3gLpB8o](https://www.youtube.com/watch?v=wyWI3gLpB8o)

## Konfliktpotential

**Der zentrale chronische Konflikt und die Abwärtsspirale**

* Instandhalten zentraler, kritischer und meist sehr fragiler Systembestandteile – „… wir räumen das später auf…“
* Die Systeme, die an den ehesten Problemen bereiten...
  * ...sind unserer wichtigsten Systeme
  * ...sind Ziel der dringlichsten Änderungen
  * ...schlagen die Änderungen fehl, hat dies massive Auswirkungen auf Kunden, Umsatz, Sicherheit, Finanzen etc.
* Als Konsequenz aus den zuvor genannten Problemen:
  * Es werden neue Features versprochen
  * Der Technologiebereich einer Unternehmung muss diese realisieren
  * Das Versprechen findet statt ohne zu wissen, ob dies technologisch überhaupt möglich ist
* Jetzt gibt es also noch ein neues dringenderes Projekt
  * Neue Herausforderungen
  * Neue Technologien
  * Mehr technische Schulden – „… wir räumen das später auf…“
* Nun sind alle beschäftigt
  * Alles muss am Laufen gehalten werden
  * Alle haben weniger Zeit und alles dauert etwas länger
  * Kommunikation dauert etwas länger
  * Kleine Aktionen haben zu großen Auswirkungen
  * Liste der Aufgaben wird länger
  * Alles wird etwas aufwendiger
  * **Der Betrieb hat es immer schwerer stabile IT Services zu liefern**
  * Änderungen sind nicht willkommen
  *   **Die Entwicklung kann nicht mehr auf Änderungen reagieren**

      <figure><img src=".gitbook/assets/devops.03.chronischer_konflikt.png" alt=""><figcaption></figcaption></figure>

Exkurs: Dringlich vs. Wichtig\



m Sprachgebrauch werden _dringlich_ und _wichtig_ oft und gerne vermischt…

Ist das geplante Release _wichtiger_ als die Fehlkonfiguration der Firewall? Was ist _dringlicher_?

<figure><img src=".gitbook/assets/devops.03.dringlich_wichtig.png" alt=""><figcaption></figcaption></figure>

## **Nicht-technische Konsequenzen**

* Überstunden
* Arbeiten am Wochenende
* Probleme bei Deployments bis hin zu Ausfällen
* Bereitschaften
* Persönliche Heldentaten einzelner Mitarbeiter (Feuerwehreinsätze) Burn-Out
* Kündigung (der besten Mitarbeiter)
* ...
