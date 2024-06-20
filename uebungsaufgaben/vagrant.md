# Vagrant

## Aufgabenstellung

Teilaufgabe 1:&#x20;

Erstellen Sie ein Dockerfile zum Erzeugen eines entsprechenden Images analog zu Aufgabe 2

* Verwenden Sie hierfür ein Ubuntu
* Installieren Sie Apache2 oder nginx im Image und konfigurieren Sie es so, das es eine statische Seite ausliefert
* Stellen Sie sicher, dass eine statische Webseite (`index.html`) über ein Volume/Mounting ausgeliefert wird.

Teilaufgabe 2:

* Erstellen Sie eine Vagrantbox
* Die Vagrantbox muss Docker ausführen können und über Ansible provisioniert werden
* Erzeugen Sie ein Ansible Skript, das sowohl Docker installiert, als auch das entsprechende Dockerfile in der Vagrantbox deployt.
* Stellen Sie sicher, dass das Volume/Mount für die statische `index.html` Datei aus dem `/vagrant` Share stammt, und die Datei auf dem Host-System aktualisiert werden kann.
