# DevOps - Übungen und Abgaben

| Parameter                                        | Kursinformationen                                                                                                                                                                                              |
| ------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Veranstaltung:**                               | `262062 DevOps`                                                                                                                                                                                                |
| **Semester**                                     | `SEB4`                                                                                                                                                                                                         |
| **Hochschule:**                                  | `Hochschule Heilbronn`                                                                                                                                                                                         |
| **Inhalte:**                                     | `Übungen und Abgaben`                                                                                                                                                                                          |
| Startseite                                       | [https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1](https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1) |
| **Link auf GitHub:**                             | [https://github.com/aheil/devops/blob/master/lectures/99\_exercises.md](https://github.com/aheil/devops/blob/master/lectures/99\_exercises.md)                                                                 |
| **Darstellung als interaktiver LiaScript Kurs:** | [https://LiaScript.github.io/course/?https://github.com/aheil/devops/master/lectures/99\_exercises.md](https://liascript.github.io/course/?https://github.com/aheil/devops/master/lectures/99\_exercises.md)   |
| **Autoren**                                      | @author                                                                                                                                                                                                        |

## Aufgabe 1 - Verständnisfragen

In dieser Aufgabe setzen Sie sich mit einem Artikel über DevOps auseinander.

### Aufgabenstellung

* Lesen Sie den Artikel von Kasteleiner und Schwartz **DevOps - Schnell, zuverlässig und sicher von der Idee zur Realisierung**, Informatik Spektrum 42, S.211-214 (2019): https://doi.org/10.1007/s00287-019-01173-2
* Link zum Artikel: \[https://rdcu.be/b709M]
* Welche Punkte sind Ihnen in dem Artikel nicht klar?
  * Notieren Sie drei Fragen zum Artikel in einer Testdatei (\*.txt).
  * Laden Sie die Datei in der zugehörigen Aufgabe in ILIAS hoch.

##

###

### Hinweise

* Weiter Tipps finden Sie unter [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile\_best-practices/)

###

## Aufgabe 3 - Docker CI-Container

In dieser Aufgabe erstellen Sie ein Docker Image, das einem CI/CD Prozess zur statischen Code-Analyse verwendet werden kann. Für die Aufgabe wird das Image mittels Docker Compose gebaut gestartet.

### Aufgabestellung

* Erstellen Sie ein [`Dockerfile`](https://docs.docker.com/engine/reference/builder/) auf Basis dessen Sie ein Image erstellen können
* Erstellen Sie eine `docker-compose.yml` Datei mittels derer das Image gebaut und gestartet werden kann.
* Das Image muss folgende Anforderungen erfüllen:
  * Das Image wird automatisch gebaut, sobald via [Docker Compose](https://docs.docker.com/compose/) `docker-compose up` aufgerufen wird.
  * Nachdem der Container gestartet wurde, wird automatisch ein Git Repository gecloned.
  * Die URL auf das Git Repository kann in der `docker-compose.yml` Datei spezifiziert werden.
  * Hinweis: Die Repository URL darf nicht im Image hart hinterlegt werden.
  * Nach dem Clonen des Repositories startet Ihr Container [Checkstyle](https://checkstyle.sourceforge.io/) und führt eine Analyse des Codes im Repository auf Basis der [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) aus.
  * Die Datei mit der Google Java Style Guide wird von Ihnen bereitgestellt und befindet sich im gleichen Verzeichnis wie das Dockerfile.
  * Der Pfad des Quell-Codes im Repository (z.B. `/src/foo/bar/`) kann in der `docker-compose.yml` Datei spezifiziert werden.
  * Hinweis: Der Pfad darf nicht hart im Image hinterlegt sein.
  * Die Ausführung von Checkstyle erfolgt über ein Gradle Skript.
  * Das Skript befindet sich im selben Verzeichnis wie Ihr Dockerfile und kann baum bauen des Images in dieses kopiert werden.
  * Beide Parameter (Git Repository und der Pfad zum Code innerhalb des Repositories) können als Parameter in der `docker-compose.yml` Datei geändert werden.
  * Änderungen an den Parametern treten immer erst nach dem Neustart des Containers (via `docker-compose up`) in Kraft.
  * Alle Fehler, die auftreten sollen im Container Log via [`docker logs`](https://docs.docker.com/engine/reference/commandline/logs/) ausgegeben werden.
  * Werden bei der Ausführung von Checkstyle Abweichungen von den Google Java Style Guides festgestellt, soll diese Ausgabe ebenfalls im container log ausgegeben werden.

> **Hinweis**: Zusätzliche Informationen zu Parametern für Images und Containers finden Sie folgendem Artikel: [https://vsupalov.com/docker-arg-env-variable-guide/](https://vsupalov.com/docker-arg-env-variable-guide/).

### Abgabe

* Stellen Sie das `Dockerfile` als auch die zusätzliche geforderten Dateien (Gradle Skript, Checkstyle Datei und _docker-compose.yml_) als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs\_262954\&client\_id=iliashhn) unter der entsprechenden Abgabe bereit.
* Zu spät eingereichte Dateien werden nicht gewertet.
* Andere Abgaben (oder passwortgeschützte bzw. korrupte ZIP-Dateien) werden nicht gewertet.
* Abgaben via E-Mail werden nicht gewertet.

## Aufgabe 4 - Ansible Playbook

In dieser Übung erstellen Sie ein Ansible Playbook.

### Aufgabenstellung

1. Erstellen Sie ein Ansible Playbook mit mindestens folgenden Bestandteilen:

* [Playbook-Datei](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html)
* Hosts-Datei
* Den erforderlichen [Rollen](https://docs.ansible.com/ansible/latest/playbook\_guide/playbooks\_reuse\_roles.html)

2. Host-Datei

* Als Zielsystem nutzen Sie `localhost` oder `172.0.0.1`

3. Erstellen Sie ein Playbook, das folgende Rollen beinhaltet

* [UFW](https://help.ubuntu.com/community/UFW)
  * Sämtliche Ports sind geschlossen
  * Geöffnet sind die Ports
    * 22 (SSH), 80 (HTTP), 443 (HTTPS), 110 (POP3), 587 (SMTP)
* [Cron](https://help.ubuntu.com/community/CronHowto)
  * Erstellen Sie einen Cron-Job, der jede Nacht Punkt 04:00 den Server rebootet
* SSH Login
  * Stellen Sie sicher, dass es nicht mehr möglich ist sich mit einem Passwort anzumelden.
  * Stichwort `sshd_config`
  * Hinweis: Bei einem solchen Vorgehen ist es erforderlich, dass Sie vorher den public Key Ihres SSH-Schlüssels auf die Maschine laden (dies ist kein Bestandteil dieser Übung)

4. Stellen Sie sicher, dass jede Rolle einzeln ausgeführt werden kann
5. Erstellen Sie eine Readme-Datei (README.txt), in der der erforderliche Kommando steht, die zum Ausführen des Playbooks erforderlich ist.

* Ihr Zielsystem ist ein [Ubuntu 20.04 LTS](https://releases.ubuntu.com/20.04/)
* Nutzen Sie eine [VirtualBox](https://www.virtualbox.org/) in der Sie Ihr Ubuntu installieren
* Das System ist Host als auch Node gleichzeitig, d.h. Sie führen das Playbook in Ihrer VirtualBox aus.
* Geben Sie die gesamte Verzeichnis-Struktur mit allen erforderlichen Dateien als ZIP-Datei ab.

### Abgabe

* Stellen Sie das Playbook als auch die zusätzlich erforderlichen Datei als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs\_262954\&client\_id=iliashhn) unter der entsprechenden Abgabe bereit.
* Zu spät eingereichte Dateien werden nicht gewertet.
* Andere Abgaben (oder passwortgeschützte bzw. korrupte ZIP-Dateien) werden nicht gewertet.
* Abgaben via E-Mail werden nicht gewertet.

## Aufgabe 5 - Vagrant Box

In dieser Aufgabe entwickeln Sie ein Deployment auf Basis einer Vagrantbox, in der Sie das zuvor gelernte anwenden.

Sie deployen einen Web Server in einem Docker Container, der eine statische Webseite ausliefert. Der Conntainer als auch der statische Content werden mittels einem Ansible Skript in einer Vagrantbox ausgerollt.

### Aufgabenstellung

Teilaufgabe 1: Erstellen Sie ein Dockerfile zum Erzeugen eines entsprechenden Images analog zu Aufgabe 2

* Verwenden Sie hierfür ein Alpine (aktuellste Version)
* Installieren Sie Apache2 oder nginx im Image und konfigurieren Sie es so, das es eine statische Seite ausliefert
* Stellen Sie sicher, dass eine statische Webseite (`index.html`) über ein Volume/Mounting ausgeliefert wird.

Teilaufgabe 2:

* Erstellen Sie eine Vagrantbox
* Die Vagrantbox muss Docker ausführen können und über Ansible provisioniert werden
* Erzeugen Sie ein Ansible Skript, das sowohl Docker installiert, als auch das entsprechende Dockerfile in der Vagrantbox deployt.
* Stellen Sie sicher, dass das Volume/Mount für die statische `index.html` Datei aus dem `/vagrant` Share stammt, und die Datei auf dem Host-System aktualisiert werden kann.
