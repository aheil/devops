<!--

author:   Andreas Heil

email:    andreas.heil@hs-heilbronn.de

version:  0.1

language: de

narrator: DE German Male

tags: devops, lecture, value stream maps

comment:  

-->

# DevOps - Container

<!-- data-type="none" -->
| Parameter | Kursinformationen |
| --- | --- |
| **Veranstaltung:** | `262062 DevOps`|
| **Semester** | `SEB4` |
| **Hochschule:** | `Hochschule Heilbronn` |
| **Inhalte:** | `Container ` |
| Startseite | [https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1](https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1) | 
| **Link auf den GitHub:** | [https://github.com/aheil/devops/blob/main/lectures/04_kanban.md](https://github.com/aheil/devops/blob/main/lectures/05_vsm.md) |
| **Autoren** | @author |

## Ziele und Kompetenzen

- Eine Methode **kennen lernen**, um in komplexen Produktionsabläufen lange Laufzeiten zu erkennen und zu reduzieren

## Motivation 

Angenommen Sie arbeiten in einem agilen Team, Entwicklung und Betrieb gemeinsam in einem DevOps Team. Ihr Team ist für die Weiterentwicklung und den Betrieb eines zentralen Bestandteiles der Unternehmensinfrastruktur verantwortlich. Ihr Team arbeitet seit Monaten »am Limit«. 

Eines Morgens kommen Sie ins Büro und werden kurzfristig zu einem Eskalationstermin gerufen. Die internen Kunden beschweren sich, dass Ihre Entwicklung so »teuer sind«. Eine Anforderung kosten über 20- bis 30-Tausend Euro und Sie sollen Ihren Prozess optimieren.  

> Die Zukunft Ihres gesamten Teams steht auf dem Spiel.

{{1}}
************************************

**Eine erste Analyse**

* Tatsächlich, in der Regel dauert es 3-4 Monate, bis eine Anforderung vom internen Kunden in Betrieb geht
* Ein Blick in die Zeiterfassung zeigt jedoch, dass die Aufwände für Entwicklung neuer Anforderungen im Median weniger als 1 Personentag in Anspruch nimmt. zeigt jedoch, dass für jede Anforderung nur wenige Stunden bis Tage Aufwand benötigt.

************************************

{{2}}
************************************

**Eine Idee**

* Es scheint einen Unterschied zu geben, wie lange die **Laufzeit** ist, um eine Anforderung umzusetzen und der tatsächlichen **Bearbeitungszeit**. 
* Allerdings scheinen die Kosten nicht der Bearbeitungszeit zu entsprechend. 
* Der Gesamtprozess sollte daher genauer betrachtet werden.

************************************

## Value Stream Maps (VSM)

* Aus der industriellen Fertigung 
* Beschreiben den Materialfluss vom Lieferanten bis zum Kunden durch die eigene Organisation
* Lassen Wartezeiten, zu geringe oder zu große Materialbestände erkennen

{{1}}
************************************

**Praxistipps** 

* VSMs am besten zunächst auf Papier zeichnen 
* Ein möglichst großes Papier verwenden
* Möglichst viele Beteiligte entlang der Wertschöpfungskette involvieren 
* VSMs sind keine Einzeltätigkeiten oder Aufgaben, die an einen Consultant »outgesourced« werden sollten

************************************

{{2}}
************************************

**Erforderlichen Schritte**

* Wertstromanalyse 

  * Wahl des Produktes/Produktfamilie  
  * Erstellung der Ist-Map

* Wertstromdesign

  * Erstellung der Soll-Map
  * Erstellung des Umsetzungsplans

Fokus hier: Ist-Map

************************************

{{3}}
************************************

**Prozessübergreifende Zusammenhänge**

* Transparente Darstellung von Informationsfluss (und Materialfluss) und Darstellung von Zusammenhängen
* Betrachtung des Gesamtprozesses anstelle von Einzelpersonen
* Verbesserungspotentiale werden leicht erkennbar
* Auswirkungen geplanter Maßnahmen in Bezug auf den Prozess werden besser abschätzbar
************************************

{{4}}
************************************

**Auswahl des Produktes/Produktfamilie** 

* In der industriellen Fertigung wird das Produktfamilie ausgewählt, das bzw. die am umsatzstärksten sind
* Ökonomisch betrachtet, ist der größte Effekt zu erwarten
* In unserem Fall betrachten wir natürlich »unser« Team bzw. Produkt
************************************

{{5}}
************************************

**VSM Beispiel**
![](../img/devops.05.vsm_part1.de.png)

************************************

{{6}}
************************************

**VSM Beispiel (Forts.)**
![](../img/devops.05.vsm_part2.de.png)

************************************

{{7}}
************************************

**VSM Beispiel (Forts.)**
![](../img/devops.05.vsm_part3.de.png)

************************************

{{8}}
************************************

**Verschiedene Zeitbegriffe** 

* **Cycle Time**: Durchschnittliche Dauer um einen Prozessschritt abzuschließen
* **Total Value Added Time**: Summe aller der Cycle Time aller Prozessschritte 
* **Lead Time**: Durchschnittliche Zeit bis zur Fertigstellung 

************************************