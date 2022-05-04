---
title: Hausaufgabe Docker Image
---

## Aufgabe: Docker CI/CD Image

### Zusammenfassung 
In dieser Aufgabe erstellen Sie ein Docker Image, das einem CI/CD Prozess zur statischen Code-Analyse verwendet werden kann. Für die AUfgabe wird das Image mittels Docker Compose gebaut gestartet.

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
    - Beide Paramter (Git Repository und der Pfad zum Code innerhalb des Repositories) können als Parameter in der `docker-compose.yml` Datei geändert werden. 
        - Änderungen an den Parametern treten immer erst nach dem Neustart des Containers (via `docker-compose up`) in Kraft.
    - Alle Fehler, die auftreten sollen im Container Log via (`docker logs`)[https://docs.docker.com/engine/reference/commandline/logs/] ausgegeben werden.
    - Werden bei der Ausführung von Checkstyle Abweichungen von den Google Java Style Guides festgestellt, soll diese Ausgabe ebenfalls im container log ausgegeben werden. 

Hinweis: Zusätzliche Informationen zu Parametern für Images und Containers finden Sie folgendem Artikel: [https://vsupalov.com/docker-arg-env-variable-guide/](https://vsupalov.com/docker-arg-env-variable-guide/).


### Abgabe 
- Stellen Sie das `Dockerfile` als auch die zusätzliche geforderten Dateien (Gradle Skript, Checkstyle Datei und docker-compose.yml) als ZIP-Datei bereit und stellen Sie diese in [ILIAS](https://ilias.hs-heilbronn.de/goto.php?target=crs_262954&client_id=iliashhn) unter der entsprechenden Abgabe bereit. 
- Zu spät eingereichte Dateien werden nicht gewertet. 
- Andere Abgaben (oder korrupte ZIP-Dateien) werden nicht gewertet.
- Abgaben via E-Mail werden nicht gewertet.