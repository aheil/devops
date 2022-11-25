<!--

author:   Andreas Heil

email:    andreas.heil@hs-heilbronn.de

version:  0.1

language: de

narrator: DE German Male

tags: devops, excercises, docker, ansible, vagrant 

comment:  

-->

# DevOps - Übungen und Abgaben 

<!-- data-type="none" -->
| Parameter | Kursinformationen |
| --- | --- |
| **Veranstaltung:** | `262062 DevOps`|
| **Semester** | `SEB4` |
| **Hochschule:** | `Hochschule Heilbronn` |
| **Inhalte:** | `Übungen und Abgaben` |
| Startseite | [https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1](https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1) | 
| **Link auf GitHub:** | [https://github.com/aheil/devops/blob/main/lectures/99_exercises.md](https://github.com/aheil/devops/blob/main/lectures/99_exercises.md) |
| **Darstellung als interaktiver LiaScript Kurs:** | ![https://LiaScript.github.io/course/?https://github.com/aheil/devops/lectures/99_exercises.md](https://LiaScript.github.io/course/?https://github.com/aheil/devops/lectures/99_exercises.md)  |
| **Autoren** | @author |

## Aufgabe 1 - Verständnisfragen

## Aufgabe 2 - Docker Webserver

In dieser Aufgabe erstellen Sie ein erstes Docker Image, das einen einfachen Webserver enthält.

### Aufgabestellung
- Erstellen Sie ein [`Dockerfile`](https://docs.docker.com/engine/reference/builder/) auf Basis dessen Sie ein Image erstellen werden
- Installieren Sie in Ihrem Image [nginx](https://www.nginx.com/) als Webserver. Hinweis: Nutzen Sie hierfür **nicht** das *nginx* Image.
- Erstellen Sie eine Default-Seite (`index.html`) die von Ihrem Webserver standardmäßig angezeigt.
- Die `index.html` Datei soll auf Ihrem Host System vorliegen und via [`Volume`](http://nginx.org/en/docs/beginners_guide.html) oder [`Bind Mount`](https://docs.docker.com/storage/bind-mounts/) innerhalb des Containers bereitgestellt wird. 
- Stellen Sie sicher, dass *nginx* mit dem Starten des Containers startet. 

### Bewertungskriterien 
- Das Image wird auf Basis des bereitgestellten `Dockerfile` erstellt und als Container gestartet.
- Die bereitgestellte `index.html` wird via `http://localhost:80` abgefragt.
- Im Anschluss wird die `index.html` verändert und neu im Browser geladen, die Änderungen sollen sich hierbei widerspiegeln.

### Hinweise
- Weiter Tipps finden Sie unter [Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/)

### Abgabe 
- Erstellen Sie eine `README.txt` Datei in der Sie die Kommandozeile angeben, die für den Start Ihres Containers erforderlich ist (z.B. mit Port- und Volume-Angaben)
- Stellen Sie das `Dockerfile` als auch die `README.txt` als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs_262954&client_id=iliashhn) unter der entsprechenden Abgabe bereit. 
- Zu spät eingereichte Dateien werden nicht gewertet. 
- Andere Abgaben (oder korrupte bzw. passwortgeschützte ZIP-Dateien) werden nicht gewertet.
- Abgaben via E-Mail werden nicht gewertet.

## Aufgabe 3 - Docker CI-Container

In dieser Aufgabe erstellen Sie ein Docker Image, das einem CI/CD Prozess zur statischen Code-Analyse verwendet werden kann. Für die Aufgabe wird das Image mittels Docker Compose gebaut gestartet.

### Aufgabestellung

- Erstellen Sie ein [`Dockerfile`](https://docs.docker.com/engine/reference/builder/) auf Basis dessen Sie ein Image erstellen können
- Erstellen Sie eine `docker-compose.yml` Datei mittels derer das Image gebaut und gestartet werden kann.
- Das Image muss folgende Anforderungen erfüllen:

  - Das Image wird automatisch gebaut, sobald via [Docker Compose](https://docs.docker.com/compose/) `docker-compose up` aufgerufen wird.

  - Nachdem der Container gestartet wurde, wird automatisch ein Git Repository gecloned.
  - Die URL auf das Git Repository kann in der `docker-compose.yml` Datei spezifiziert werden.
  - Hinweis: Die Repository URL darf nicht im Image hart hinterlegt werden. 
  - Nach dem Clonen des Repositories startet Ihr Container [Checkstyle](https://checkstyle.sourceforge.io/) und führt eine Analyse des Codes im Repository auf Basis der [Google Java Style Guide](https://google.github.io/styleguide/javaguide.html) aus. 
  - Die Datei mit der Google Java Style Guide wird von Ihnen bereitgestellt und befindet sich im gleichen Verzeichnis wie das Dockerfile.  
  - Der Pfad des Quell-Codes im Repository (z.B. `/src/foo/bar/`) kann in der `docker-compose.yml` Datei spezifiziert werden. 
  - Hinweis: Der Pfad darf nicht hart im Image hinterlegt sein.  
  - Die Ausführung von Checkstyle erfolgt über ein Gradle Skript.
  - Das Skript befindet sich im selben Verzeichnis wie Ihr Dockerfile und kann baum bauen des Images in dieses kopiert werden. 
  - Beide Parameter (Git Repository und der Pfad zum Code innerhalb des Repositories) können als Parameter in der `docker-compose.yml` Datei geändert werden. 
  - Änderungen an den Parametern treten immer erst nach dem Neustart des Containers (via `docker-compose up`) in Kraft.
  - Alle Fehler, die auftreten sollen im Container Log via [`docker logs`](https://docs.docker.com/engine/reference/commandline/logs/) ausgegeben werden.
  - Werden bei der Ausführung von Checkstyle Abweichungen von den Google Java Style Guides festgestellt, soll diese Ausgabe ebenfalls im container log ausgegeben werden. 

> **Hinweis**: Zusätzliche Informationen zu Parametern für Images und Containers finden Sie folgendem Artikel: [https://vsupalov.com/docker-arg-env-variable-guide/](https://vsupalov.com/docker-arg-env-variable-guide/).

### Abgabe 
- Stellen Sie das `Dockerfile` als auch die zusätzliche geforderten Dateien (Gradle Skript, Checkstyle Datei und _docker-compose.yml_) als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs_262954&client_id=iliashhn) unter der entsprechenden Abgabe bereit. 
- Zu spät eingereichte Dateien werden nicht gewertet. 
- Andere Abgaben (oder passwortgeschützte bzw. korrupte ZIP-Dateien) werden nicht gewertet.
- Abgaben via E-Mail werden nicht gewertet.

## Aufgabe 4 - Ansible Playbook

In dieser Übung erstellen Sie ein Ansible Playbook. 

### Aufgabenstellung 

1. Erstellen Sie ein Ansible Playbook mit mindestens folgenden Bestandteilen: 

  * [Playbook-Datei](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) 
  * Hosts-Datei 
  * Den erforderlichen [Rollen](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html) 

2. Host-Datei 

  * Als Zielsystem nutzen Sie `localhost` oder `172.0.0.1`

3. Erstellen Sie ein Playbook, das folgende Rollen beinhaltet

  * [UFW](https://help.ubuntu.com/community/UFW) 
  
    * Sämtliche Ports sind geschlossen
    * Geöffnet sind die Ports 

      * 22
      * 80
      * 443
      * 110
      * 587
    
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
- Stellen Sie das Playbook als auch die zusätzlich erforderlichen Datei als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs_262954&client_id=iliashhn) unter der entsprechenden Abgabe bereit. 
- Zu spät eingereichte Dateien werden nicht gewertet. 
- Andere Abgaben (oder passwortgeschützte bzw. korrupte ZIP-Dateien) werden nicht gewertet.
- Abgaben via E-Mail werden nicht gewertet.

## Aufgabe 5 - Vagrant Box




