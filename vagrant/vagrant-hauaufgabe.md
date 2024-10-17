# Vagrant Hauaufgabe

In dieser Aufgabe entwickeln Sie ein Deployment auf Basis einer Vagrantbox, in der Sie das zuvor gelernte anwenden.

Sie deployen&#x20;

* einen Web Server in einem Docker Container,
* der eine statische Webseite ausliefert.&#x20;
* Der Conntainer als auch der statische Content werden mittels&#x20;
* einem Ansible Skript&#x20;
* in einer Vagrantbox ausgerollt.

## Aufgabenstellung

## Teilaufgabe 1

&#x20;Erstellen Sie ein Dockerfile zum Erzeugen eines entsprechenden Images analog zu Aufgabe 2

* Verwenden Sie hierfür ein Alpine (aktuellste Version)
* Installieren Sie Apache2 oder nginx im Image und konfigurieren Sie es so, das es eine statische Seite ausliefert
* Stellen Sie sicher, dass eine statische Webseite (`index.html`) über ein Volume/Mounting ausgeliefert wird.

### Teilaufgabe 2

* Erstellen Sie eine Vagrantbox
* Die Vagrantbox muss Docker ausführen können und über Ansible provisioniert werden
* Erzeugen Sie ein Ansible Skript, das sowohl Docker installiert, als auch das entsprechende Dockerfile in der Vagrantbox deployt.
* Stellen Sie sicher, dass das Volume/Mount für die statische `index.html` Datei aus dem `/vagrant` Share stammt, und die Datei auf dem Host-System aktualisiert werden kann.

## Abgabe

Abgegeben werden muss das Vagrantfile, das Dockerfile, die index.html als auch alle weiteren notwendigen Dateien.&#x20;

Falls eine Ordnerstruktur erfordelrich ist, muss diese so abgegeben werden.

Reichen Sie Abgabe als ZIP-File ein (kein 7z, kein rar, kein tar und kein tar.gz).

Zur Evaluierung der Abgabe wird das Archiv entpackt und `vagrant up` ausgeführt.&#x20;
