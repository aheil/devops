---
layout: slide
title: DevOps Einführung
---


## DevOps
### Einheit 01: Einführung

<small>Prof. Dr.-Ing. Andreas Heil</small>

<small>Dieses Werk ist lizensiert unter einer<br />[Creative Commons Namensnennung 4.0 International Lizenz](http://creativecommons.org/licenses/by/4.0/).</small>

<small>v.1.0.0<small>

---

## Ziele und Kompetenzen

Grundlegende Probleme der herkömmlichen Entwicklungs- und
Betriebsorganisation **kennen lernen**, 

**verstehen** was mit DevOps erreicht werden soll und

einige Möglichkeiten **kennen lernen** wie diese Ziele erreicht werden
können.

---

<!-- .slide: class="two-floating-elements" -->

## Project Phoenix


* Kim, Behr & Spafford
* Deutsche Ausgabe
* O’Reilly, 2015
* ISBN 978-3958751750

![Project Phoenix (dt. Ausgabe)](../img/project_phoenix_cover.png)

---

## „Projekt Phoenix“ - Ausgangslage

> Bill Palmer ist IT-Manager bei Parts Unlimited. An einem Dienstag morgen erhält er auf der Fahrt zur Arbeit einen Anruf seines CEO. Die neue IT-Initiative der Firma mit dem Codenamen Projekt Phoenix ist **entscheidend für die Zukunft** von Parts Unlimited, aber das **Projekt hat Budget und Zeitplan massiv überzogen**. Der CEO will, dass Bill direkt an ihn berichtet und das ganze **Chaos in neunzig Tagen aufräumt**, denn sonst wird Bills gesamte Abteilung outgesourct.“

<small>Quelle: [1]</small>

---

## „Projekt Phoenix“ – Die ersten Fails

### Fail 1

Die Gehaltssoftware spuckt bei einem signifikanten Teil der Mitarbeiter
nur Hieroglyphen aus, anstatt das tatsächliche Gehalt zu berechnen.

Gehälter drohen, nicht ausgezahlt zu werden. Der Betriebsrat würde
Alarm schlagen und die Presse negative Schlagzeilen zu Parts Unlimited
schreiben.

---

## „Projekt Phoenix“ – Die ersten Fails

### Fail #2

Weil sich unterschiedliche Teams nicht abgestimmt haben, dauert der
Rollout einer Kassensoftware viel zu lange. Die Datenbank-Migration
dauert nicht nur Stunden, sondern Tage.

Doof, dass die Kassen am nächsten Tag wieder öffnen und ohne
Software nicht arbeiten können. Filialen nehmen die Kreditkartendaten
der Kunden entgegen und schreiben sie auf, was aber ein
Sicherheitsverstoß ist...

---

## Zitate aus „Projekt Phoenix“

### Über die Zusammenarbeit von Development und Operations

> Es ist erstaunlich, wie die Übergaben zwischen Entwicklung und IT Operations jedes Mal in die Hose gehen. Aber angesichts des dauerhaften Kleinkriegs zwischen den beiden Gruppen sollte ich nicht überrascht sein

<small>Quelle: [1]</small>

---

## Zitate aus „Projekt Phoenix“

### Über Brent, dem Alleskönner, von dem alles abhängt

> Ich sage ja nicht, dass Brent das absichtlich macht, aber ich frage mich, ob Brent sein Wissen als eine Art Superheldenkraft sieht. Vielleicht will ein Teil von ihm das gar nicht aufgeben. Er hat damit eine Stellung inne, von der ihn niemand vertreiben kann.“ „Vielleicht, vielleicht auch nicht“, sage ich. „Aber ich werde euch erzählen, was ich sicher weiß: Jedes Mal, wenn wir Brent etwas fixen lassen, das keiner von uns wiederholen kann, wird Brent ein bisschen klüger und das gesamte System ein bisschen dümmer. Wir müssen dem ein Ende bereiten.“

<small>Quelle: [1]</small>

---

## Zitate aus „Projekt Phoenix“

### Über erfolgreiche Teams

> Ein großartiges Team bedeutet nicht, dass es nur aus den klügsten Köpfen besteht. Ein Team wird dann gut sein, wenn sich alle vertrauen können. Es entsteht viel Power, wenn diese Magie zwischen den Leuten existiert.

<small>Quelle: [1]</small>

---

## Zitate aus „Projekt Phoenix“

### Über die Zeit, in der ein neues Produkt auf dem Markt sein muss (Time-to-Market)

> Produkte müssen in sechs Monaten auf dem Markt sein. Höchstens in neun. Ansonsten schnappt sich eine chinesische Firma unsere Idee, bringt sie in die Läden unserer Konkurrenz und bekommt damit einen großen Marktanteil. In diesen Zeiten starker Konkurrenz muss man schnell im Markt sein und auch die Fehler schnell machen. Wir können uns keine mehrjährigen Entwicklungszyklen mehr leisten und erst am Ende erkennen, ob das Produkt gut ist oder beim Kunden durchfällt. Es müssen kurze und schnelle Zyklen sein, in denen das Feedback aus dem Markt fortlaufend einfließt.“

<small>Quelle: [1]</small>

---

## DevOps: Begriff

* Kunstwort aus engl. »Development« = Entwicklung und »Operations«
= Betrieb
* »DevOps« klingt wesentlich cooler als »EntBet«
* DevOps beschreibt eine Form der Zusammenarbeit, bei der
**Entwicklung und Betrieb** einer Software als **untrennbare Einheit**
angesehen werden
* Mittlerweile auch Abwandlungen davon, z.B. DevSecOps, GitOps etc.

---

## Was gehört alles dazu?

![Was gehört alles dazu?](../img/was_gehoert_dazu.png)

---

## Wie wurde früher Software entwickelt?

![Wie wurde früher Software entwickelt?](../img/frueher.png)

<small>Häufig ist dies auch heute noch der Fall.</small>

---

## Der ewige Interessenskonflikt

* Es werden dringliche Zuarbeiten von anderen Teams oder Abteilungen benötigt
* Es werden Tickets erzeugt
* Wartezeiten entstehen

<small>Siehe Screencast zum Thema Verschwendung</small>

---

## Wie könnte dieser Interessenskonflikt gelöst werden?

Tipp aus der Vorlesung Software Engineering komplexer Systeme: 

**Warum war Canon in den 1970ern mit der Canon AE-1 gleich nochmal so erfolgreich?**

<small>Sie haben sicherlich gedacht, dass Sie das Zeug aus der Vorlesung nie mehr benötigen…  </small>

---

## Lösung: Cross-funktionale Teams

### Ein **gemeinsames** Produktteam

![Cross-funktionale Teams](../img/cross_functional_team.png)

---

### Und was ist mit dem DevOps Engineer?

![DevOps Engineer](../img/devops_engineer.png)

---

### Und was ist mit dem DevOps Engineer?

![DevOps Engineer](../img/monster.png)

<small>Quelle: [2]</small>
---
### Was soll mit DevOps erreicht werden?

* Team ist eigenständig lieferfähig
* Alle benötigten Skills sind im Team vorhanden 
* Team hat alle erforderlichen Befugnisse 
* Umdenken in der Organisation: »«Betroffene zu Beteiligten machen«
* Schnelle Lieferung von Software-Änderungen optimieren
* Lead Time soll möglichst somit gehalten werden, daraus resultierende kürzere Time-to-Market soll Marktvorteile sichern

---

### Wie erreichen wir die vorherigen Ziele?

* Automatisieren von Prozessen
* Minimieren von Übergaben
* Cross-funktionale Teams 
* Agile Vorgehensmodelle (Scrum oder Kanban)
* Gute Feedback- und Fehler-Kultur (vgl. Screencasts)
* Fachlicher Schnitt anstelle eines techn. Schnitts (vgl. SEKS) als Kernelement

---

### Zielbild: T-Shaped Skill

![T-Shaped SKill](../img/t-shaped_skill.png)

<small>CEO of IDEO Design Consultancy erstmals 1991 von David Guest eingeführt, von Tim Brown CEO von IDEO Design Consultancy für Bewerberprofile eingesetzt.  </small>

---

### Beispiel: T-Shaped Skills

![T-Shaped Profiles](../img/t-shaped_profiles.png)

---

### Was soll mit DevOps erreicht werden?

* Team ist **eigenständig lieferfähig**
* Alle **benötigten Skills** sind im Team **vorhanden** 
* Team hat alle **erforderlichen Befugnisse **
* Umdenken in der Organisation: **»Betroffene zu Beteiligten machen«**
* **Schnelle Lieferung** von Software-Änderungen optimieren
* **»Lead Time«** soll möglichst *gering* gehalten werden, daraus resultierende **kürzere Time-to-Market** soll Marktvorteile sichern


---

### Wie erreichen wir die vorherigen Ziele?

* Automatisieren von Prozessen
* Minimieren von Übergaben
* Cross-funktionale Teams 
* Agile Vorgehensmodelle (Scrum oder Kanban)
* Gute Feedback- und Fehler-Kultur (vgl. Screencasts)
* Fachlicher Schnitt anstelle eines techn. Schnitts (vgl. SEKS) als Kernelement

---

### Infrastructure as Code (IaC)

* Gesamte Infrastruktur wird in Konfigurationsdateien beschrieben
  * Build-Umgebungen 
  * Laufzeitumgebungen 
  * Firewall-Regeln
  * Skalierungsparameter
  * Konfigurationen für Anwendungen 
* Spezielle Auszeichnungssprache (JSON, YAML) in Form einer Domain Specific Language (DSL)
* Beschreibungsdateien werden versioniert und liegen wie Code in einem Repository (z.B. Git)

---

### Metriken - Begriffserklärung

* **Metriken**: Helfen Sachverhalte zu verstehen und zu bewerten. (vgl. SEKS: Software-Metriken)

* **Monitoring**: Visualisierung von Metriken

* **Alarmierung**: Aktiver Hinweis beim Unter- oder Überschreiten von Schwellwerten (E-Mail, SMS, Push Notification, Ampelsystem)

---

### Matriken 

Metriken geben ein Einblick wie es unserer Applikation geht, indem kontinuierlich Messwerte geliefert werden, die visualisiert werden können. Außerdem werden wir alarmiert, wenn Messwerte bestimmte Schwellwerte unter- oder überschreiten.

---

### Wo werden Metriken eingesetzt?

* Alle Cloud-Anbieter unterstützen unterschiedlichste Messtechnologien- und Plattformen 
* Jedes Rechenzentrum nutzt Metriken zur Überwachung
* Auch ein NAS im privaten Einsatz nutzt Metriken (Durchsatz, verfügbarer Speicherplatz etc.)
* Es gibt technische als auch fachliche Metriken (vgl. SEKS Business- und Projektmetriken)
* Große Plattformen (Amazon, etsy etc.) werten Metriken automatisch aus und spielen Vorgängerversionen automatisiert ein, sollte in Update der Shop-Software Probleme verursachen
Metriken können z.B. für A/B-Testing genutzt werden

---

### Beispiel: Technische Metriken

* Durchschnittlich verarbeitet HTTP-Requests 
* Durchschnittliche Verarbeitungsdauer eines HTTP-Requests 
* Anzahl von HTTP-Requests, die mit einem Fehler beantwortet werden
* CPU-Auslastung (über die Zeit gemessen)
* Arbeitsspeicherauslastung (steigend, greift der Garbage Collector etc.)
* Netzwerkauslastung (Peaks)
* Latenzen 
* …

---

### Beispiel: Fachliche Metriken eines Web Shops

* Anzahl Neuregistrierung innerhalb der letzten Stunde 
* Heutige Kündigungsquote im Vergleich zum selben Tag der Vorwoche 
* Anzahl an- bzw. abgemeldeter Kunden am Newsletter
* Durchschnittlicher Einkaufs-/Warenkorbwert am heutigen Tag
* Umsatz in € über den Web Shop an diesem Tag
* Anzahl der abgeschickten Bestellungen in der letzten Stunde und deren Bezahlungsart 
* … 

---

### Continuous Integration

![CI](../img/ci.png)

---

### Continuous Deployment

![CI](../img/cd.png)

---

### Feedback und verbessertes Lernen

* Produktideen sind nichts anderes als Hypothesen.
* Je schneller eine Organisation Hypothesen einzeln oder parallel in Produktion bringt und aussagekräftige Metriken über den Erfolg oder Misserfolg sammeln kann, desto früher können aus diesen Informationen neue Hypothesen gebildet werden, und der Zyklus beginnt erneut. 
* Das Ergebnis ist eine Organisation, die ihre Kunden nach und nach immer besser versteht.
* Dasselbe gilt auch für externe Impulse durch Wettbewerber oder neue Technologien: Je kürzer der Feedbackzyklus ist, desto schneller kann die Organisation reagieren.

---

### Zusammenfassung

* Sie haben einige Probleme kennen gelernt, denen mit DevOps begegnet werden kann
* Sie haben gelernt, was mit DevOps erreicht werden kann
* Sie haben einige organisatorische als auch technische Aspekte kennen gelernt, die im DevOp-Ansatz angewandt werden
* Cross Functional Teams
* T-Shaped Skills
* Selbstständiges Arbeiten
* Metriken
* CI/CD

---

Referenzen 

[1] Kim G., Behr  K., Spafford G., Projekt Phoenix, O’Reilly, 2015, ISBN 978-3958751750
[2] https://www.monster.de
[3] Kasteleiner, B., Schwartz, A. DevOps. Informatik Spektrum 42, 211–214 (2019). https://doi.org/10.1007/s00287-019-01173-2


---