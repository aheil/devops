---
description: >-
  Erstellen Sie ein Docker Image für dein Einsatz in einer CI/CD Pipeline, das
  mittels Docker Compose gebaut und gestartet wird.
---

# Docker Container Hausaufgabe

## Aufgabe

1. Sofern noch nicht installiert - Docker installieren
   1. Docker Desktop (Windows, Mac, Linux): [https://www.docker.com/products/docker-desktop/](https://www.docker.com/products/docker-desktop/)
   2. Install Docker Engines: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
2. Erstellen Sie ein [`Dockerfile`](https://docs.docker.com/engine/reference/builder/) auf Basis dessen Sie ein Image erstellen werden.
3. Installieren Sie in Ihrem Image [nginx](https://www.nginx.com/) als Webserver. Hinweis: Nutzen Sie hierfür **nicht** das _nginx_ Image.
4. Erstellen Sie eine Default-Seite (`index.html`), die von Ihrem Webserver standardmäßig angezeigt wird.
5. Die `index.html` Datei soll auf Ihrem Host System vorliegen und via [`Volume`](http://nginx.org/en/docs/beginners\_guide.html) oder [`Bind Mount`](https://docs.docker.com/storage/bind-mounts/) innerhalb des Containers bereitgestellt werden.
6. Stellen Sie sicher, dass _nginx_ mit dem Starten des Containers startet.
7. Routen Sie den Port 8080 auf den Port 80 Ihres Containers und öffnen Sie die Datei vom Browser Ihres Hostsystems via (`http://localhost:8080/index.html`)
8. Ändern Sie die Datei auf dem Host-System und laden Sie die Datei neu im Browser.
9. Erstellen Sie eine README.TXT und notieren Sie die Zeile wie der Container über die Kommandozeile gebaut und gestartet werden kann.

### Bewertungskriterien

* Das Image wird auf Basis des bereitgestellten `Dockerfile` und der README.TXT erstellt und der Container gestartet.
* Die bereitgestellte `index.html` wird via `http://localhost:8080` abgefragt.
* Im Anschluss wird die `index.html` auf dem Host-System verändert und neu im Browser geladen, die Änderungen sollen sich hierbei widerspiegeln.
