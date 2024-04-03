# Kennzahlen in der Wertekette

## Lernziele

* Die drei grundlegenden Metriken bzw. Kennzahlen Durchlaufzeit, Verarbeitungszeit als auch %C/A kennen lernen.
* Probleme langer Deployment Durchläufe **verstehen und erkennen** können.
* Den Unterschied zwischen Durchlaufzeiten und Verarbeitungszeit verstehen.

## Technologie-Wertekette

Grundlage stellt das sog. **Lean Manufacturing** dar.

* Prinzipien und Theorien
* Wertekette als „Folge von Aktivitäten, die eine Firma vornimmt, um eine Kundenanforderung zu erfüllen“ \[7] Voraussetzungen für schnelle Durchlaufzeiten: reibungsloser und gleichmäßiger Arbeitsfluss

\[7] K. Martin, M. Osterling, Value Stream Mapping: How to Visualize Work and Align Leadership for Organizational Transformation, McGraw Hill, 2013

**Technologie-Wertekette:** Prozess der ein Geschäftsziel in einen durch Technologie unterstützten Dienst wandelt.

* Begin: Arbeit in der Entwicklung / Backlog aufnehmen
* Entwicklung: Implementierung von Code, Commit in eine Versionsverwaltung, von wo aus, jede Änderung gebaut und mit dem Rest der Anwendung integriert und getestet wird
* Wertschöpfung: Entsteht erst, wenn die Dienste produktiv laufen

Für eine zügige Wertschöpfung...

* muss die Entwicklung in einem schnellen Flow liefern darf Deployment weder Chaos, Ausfälle noch Probleme verursachen

## **Durchlaufzeiten**

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.durchlaufzeiten.png" alt=""><figcaption></figcaption></figure>

* Da für den Kunden relevant, sind Durchlaufzeiten meist im Fokus von Prozessoptimierungen
* Verhältnis von Verarbeitungszeit zu Durchlaufzeit ist wichtige Kennzahl für Effizienz
* Kurze Verarbeitungszeiten bedeuten kürzere Wartezeiten

## **Klassische Deployment Durchläufe**

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.klassisches_deployment.png" alt=""><figcaption></figcaption></figure>

* Wochen bis Monate
* Lange Laufzeiten erfordern Heldentaten
* Probleme werden erst spät erkannt
* Oftmals mit manuellem Testen und Bestätigungsprozessen verbunden

## **Deployment-Durchlaufzeiten**

[![](https://github.com/aheil/devops/raw/main/img/devops.03.deplyment\_durchlaufzeiten.png)](img/devops.03.deplyment\_durchlaufzeiten.png)

* Deployment Durchlaufzeiten beginnen mit dem Check-In, enden mit dem Deployment und
* sind Teil der Verarbeitungszeit.

## **Kurze Deployment-Durchlaufzeiten**

* Entwickler erhalten schnell und fortwährend Feedback zur Arbeit
* Konsequenz: Schnell und unabhängig
  * Implementieren
  * Integrieren
  * Validieren
  * Deployen
* Notwendig
  * Modulare Architektur
  * Saubere Kapselung
  * Loose gekoppelte Komponenten

> **Konsequenz**: Fehler bleiben überschaubar, können schnell eingegrenzt werden und verursachen keinen globalen Schaden.

***

\{{6\}}

***

## Qualitätsmetrik: Complete and Accurate

* Complete and Accurate (%C/A) \[8]
* % the customer can perform a task without having to CAC
  * _**C**orrect information that was supplied_
  * _**A**dd missing information that should have been supplied_
  * _**C**larify information provided that should have been clear_
* D.h. wieviel % der Arbeit kann entlang der Wertekette genutzt werden ohne die erhalten Daten korrigieren, ergänzen oder nachfragen zu müssen
* Bei uns: Wieviel der Arbeit kann im nächsten Prozessschritt genutzt werden, ohne diese zu überarbeiten.

\[8] M. Osterling, K. Martin, Lean Mindset & Behaviours, AMEChicago2012, 2012, [https://www.slideshare.net/KarenMartinGroup/lean-mindsets-behaviors-workshop-ame-chicago-2012](https://www.slideshare.net/KarenMartinGroup/lean-mindsets-behaviors-workshop-ame-chicago-2012)

\*\*%C/A am Beispiel \*\*

[![](https://github.com/aheil/devops/raw/main/img/devops.03.ac.png)](img/devops.03.ac.png)

* Um möglichst schnelle Deployment Durchläufe zu erreichen ist eine hohe %C/A erforderlich
  * Wieviel kann ohne CAC getestet werden
  * Wieviel kann ohne CAC explorativ getestet werden
  * Wie gut kann ohne CAC deployed werden

## 3 Wege

#### Lernziele

* Die drei Wege in DevOps **kennen lernen**.
* Den Nutzen von WIP-Limits **verstehen**.
* Single-Piece Flow kennen lernen und die daraus resultierende Konsequenz **verstehen**.
* **Verstehen** warum viele Übergaben zu langen Deployment-Durchlaufzeiten führen.
* **Verstehen** wie man Flaschenhälse identifiziert und Ansätze kennen lernen, diese zu beseitigen.
* Das Konzept der Verschwendung **kennen lernen** und **verstehen** wie man Verschwendung vermeidet.

Drei Wege als grundlegende Prinzipien für Verhaltensweisen und Muster in DevOps \[9].

* Flow
* Schnelles und kontinuierliches Feedback
* Kultur des firmenweiten kontinuierlichen Lernens

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.drei_wege.png" alt=""><figcaption></figcaption></figure>

\[9] G. Kim, K. Behr, G. Spafford , Projekt Phoenix: Der Roman über IT und DevOps , O\`Reilly, 2015

**Arbeit sichtbar machen**

In Technologiewerteketten ist nicht ersichtlich

* Wo die Arbeit stockt
* Wo sich Arbeit stapelt
* Wo Arbeit hin- und her geschoben wird
* Wo Arbeit begonnen aber nicht beendet wird

**Lösung**: Arbeit via sog. Arbeitsboards sichtbar machen

* Sprint-Planungs-Boards
* Kanban Boards

[![](https://github.com/aheil/devops/raw/main/img/devops.03.board.png)](img/devops.03.board.png)

> Mehr dazu später im Abschnitt Kanban.

## **Durchlaufzeit**

* Wie bestimmt sich die Durchlaufzeit?
* Wann ist die Arbeit fertig

***

## **Work in Progress (WiP)**

Im Industriellen sind die Arbeiten durch Fertigungsprozesse und Kundenbestellungen geprägt, in der IT oftmals durch dringlichere Aufgaben, der „Priorität des Tages“.

Konsequenz:

* Unterbrechungen
* Multitasking
* Damit verbundene Mehraufwände \[10]

\[10] J.S. Rubinstein, D.E. Meyer, J.E. Evans, Executive control of cognitive processes in task switching. Journal of Experimental Psychology: Human Perception and Performance, 27(4), 763–797, 2001, [https://doi.org/10.1037/0096-1523.27.4.763](https://doi.org/10.1037/0096-1523.27.4.763)

## **WiP-Limit**

Work in Progress limitieren:

* Es darf an nichts gearbeitet werden, was nicht zuvor auf einer Karte festgehalten wurde
* Es darf nur an etwas gearbeitet werden, das sich in der Spalte „in Progress“ oder „in Arbeit“ hängt
* Es darf nur eine bestimmte Anzahl Karten in der Sparte „in Progress“ bzw. „in Arbeit hängen“

### **WIP-Limit Konsequenzen**

* WIP-Limits sind wichtige Indikatoren für Durchlaufzeiten \[W1, W2].
* Nichts zu tun trotz voller Spalte „in Bearbeiten“!?
* Was bedeutet das?
* Verlockend, jetzt etwas anderes anzufangen!
* Besser: Herausfinden, warum wir warten und die Ursache beseitigen!

> Stop starting, start finishing.

## **Single-Piece Flow**

* Kleine Batch-Größen
* Lean: Batch-Größen kontinuierlich zu verringern
* Theoretische Untergrenze: Single-Piece Flow

### **Große Batch-Größen**

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.grosse_batch_groessen.png" alt=""><figcaption></figcaption></figure>

### **Kleine Batch-Größen**

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.kleine_batch_groessen.png" alt=""><figcaption></figcaption></figure>

### **Arbeitsschritte**

Wie viele Schritte halten Sie in einem Prozess für realistisch, um Code aus der Versionsverwaltung in die Produktivumgebung zu bringen?

* &#x20;1-10?
* 10-100?
* 100-1.000?

Z.B. Umgebungen erstellen, Server einrichten, Auschecken, Bauen, Testen, Administration, Storage verwalten, Netzwerk konfigurieren, Load Balancing, Sicherheitsmaßnahmen, noch mehr Testen, Freigaben…

## Übergaben

Bei jeder Übergabe

* Ist Kommunikation erforderlich
* Existiert die Gefahr von Wartezeiten
* Geht Information verloren

<figure><img src="https://github.com/aheil/devops/raw/main/img/devops.03.informationsverlust.png" alt=""><figcaption></figcaption></figure>

> **Problem**: Ressourcen werden in unterschiedlichen Werteketten genutzt

### **Vermeiden von Übergaben**

* Maßnahmen
  * Anzahl von Übergaben verringern
  * Automatisierung
  * Reorganisation der Teams
* Resultate
  * Erhöhter Flow durch reduzierte Wartezeiten
  * Zeit reduziert, in der kein Wert generiert wird

### **Flaschenhälse identifizieren**

* Es gibt in Werteketten **genau eine** Fließrichtung
* Es gibt **genau einen** Flaschenhals
* Optimierungen vor dem Flaschenhals verschlimmern den Rückstau
* Optimierungen nach dem Flaschenhals verschlimmern die Idle-Zeit

\-[![](https://github.com/aheil/devops/raw/main/img/devops.03.flaschenhals.png)](img/devops.03.flaschenhals.png)

\*\*Fünf fokussierende Schritte

1. Den Flaschenhals finden
2. Herausfinden, wie der Flaschenhals beseitigt werden kann
3. Alles andere diesen Maßnahmen unterordnen
4. Den Flaschenhals beheben
5. Zurück zu Schritt 1 – jetzt wird es einen neuen Flaschenhals geben

## **DevOps Muster**

Lange Deployment-Laufzeiten folgen oft ähnlichen Mustern:

* Umgebungen
* Deployment
* Testen
* Architektur

### Verschwendung

Verschwendung (engl. waste, jap. Muda 無駄)

Klassisch: Alles wofür ein Kunde nicht bereit ist zu zahlen, oder was er nicht benötigt \[11] Bei uns: Alles was Verzögerung verursachen kann \[12]

\[11] S. Shingo, A Study of the Toyota Productive System: From Industrial Engineering Viewpoint, Productivity Press, 1989 \[12] M. Poppendieck, T. Poppendieck, Implementing Lean Software: From Concept to Cash, Addison-Wesley, 2007

### **Arten der Verschwendung**

* Teilweise erledigte Arbeit
* Zusätzliche Prozesse
* Zusätzliche Features
* Aufgabenwechsel
* Warten
* Bewegung
* Fehler
* Nicht standardisierte oder manuelle Arbeit
* Heldentaten

### Zusammenfassung

* 3 Wege
* Flow erzeugen durch
  * Sichtbar machen der Arbeit
  * WIP-Limits einführen
  * Kleine Batch-Größen nutzen
* Übergaben minimieren bzw. vermeiden
* Flaschenhälse identifizieren und beseitigen
* Verschwendung minimieren
