---
layout:
  title:
    visible: true
  description:
    visible: true
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Docker Compose

## Ziele und Kompetenzen

* Den Vorteil der Verwaltung von Container mit Docker Compose **verstehen**
* Die wichtigsten Elemente von Docker Compose **kennen**
* Einfach Beispiele in Docker Compose erfolgreich selbst **erstellen und ausführen können**

## **Wozu Docker Compose?**

In der vorherigen Einheit haben wir einen einzelnen Container auf der Kommandozeile gestartet. Was aber, wenn wir mehrere Container zusammen starten wollen, weil diese alle für eine Anwendung benötigt werden. Z.B. eine Datenbank, ein Backend, ein Webserver und das Frontend? Eventuell bestehen Abhängigkeiten zwischen den Containern (z.B. sollte die Datenbank bereits verfügbar sein, bevor das Backend darauf zugreift, und das Backend soll verfügbar sein, bevor über das Frontend darauf zugegriffen wird. &#x20;

Docker Compose (kurz **Compose**) ist eine einfache und **deklarative** Möglichkeit, mehrere Container zu definieren, zu verwalten und zu orchestrieren, die eine Anwendung als ganzes zu managen. Durch die Verwendung einer einzigen **YAML-Datei** können wir die gesamte Infrastruktur ihrer Anwendung beschreiben, einschließlich der Container-Konfiguration, Netzwerke, Volumes und Umgebungsvariablen.

## Einführung Docker-Compose

```yaml
# Docker-Compose-Version, die verwendet werden soll
version: '3'

# Definition der Dienste (der Container), die in diesem 
# Docker-Compose-Projekt erstellt werden sollen
services:
  # Service 1: Webserver
  web:
    # Verwendetes Docker-Image
    image: nginx:latest
    # Portmapping für den Container (host_port:container_port), 
    # entspricht dem Paramter -p aus der vorherigen Übung 
    ports:
      - "8080:80"
    # Bind mount, um Daten zwischen Host und Container zu teilen (host_dir:container_dir),
    # entspricht dem Paramter -v aus der vorherigen Übung
    volumes:
      - ./html:/usr/share/nginx/html
    # Umgebungsvariablen für den Dienst
    environment:
      - ENV_VARIABLE=value

  # Service 2: Datenbank
  db:
    # Verwendetes Docker-Image
    image: mysql:5.7
    # Umgebungsvariablen für den Dienst
    environment:
      - MYSQL_ROOT_PASSWORD=example
    # Volumes, um Daten zwischen Host und Container zu teilen (volume_name:container_dir)
    volumes:
      - db_data:/var/lib/mysql

# Definition von Volumes, die in diesem Docker-Compose-Projekt
# verwendet werden sollen
volumes:
  # Volume für die Datenbank
  db_data:

```

## Named Volumes

Wo liegen die Daten, wenn ein Volume so angegeben wird?

```yaml
volumes:
  db_data:
```

Hier wird ein sogenanntes **Named Volume** mit dem Namen `db_data` definiert. Diese _Named Volumes_ sind persistente Speicherorte, die von Docker verwaltet werden. Sie werden außerhalb des internen Dateisystems des Containers gespeichert und können zwischen verschiedenen Containern oder beim Neustart des Containers erhalten bleiben.

Named Volumes werden in einem **speziellen Verzeichnis auf dem Hostsystem** gespeichert, das **von Docker verwaltet** wird. Der genaue Speicherort kann je nach dem verwendeten Betriebssystem und der Docker-Konfiguration variieren.

Wenn ein Container dieses _Named Volume_ verwendet, werden die Daten, die in dieses Volume geschrieben werden, auf dem Hostsystem gespeichert, anstatt im Dateisystem des Containers selbst. **Dadurch bleiben die Daten auch dann erhalten, wenn der Container beendet oder gelöscht wird**.\


> **OpSec Tip**
>
> Da _Named Volumes_ persistent sind und Daten zwischen Containern und Neustarts speichern, können Sicherheitsrisiken entstehen, wenn sensible oder vertrauliche Daten in diesen Volumes gespeichert werden, insbesondere wenn der Zugriff nicht angemessen kontrolliert wird.

Zusätzlich zum Named Mount existiert ein sog. **Bind Mount**. Bind Mounts wurden bereits in der vorherigen Übung verwendet. Der Hauptunterschied zwischen ihnen liegt in der Art und Weise, wie sie verwendet werden und wie Docker sie verwaltet:

### Named Volume

* Ein _Named Volume_ ist ein von Docker verwaltetes und benanntes Speichervolumen, das unabhängig vom Dateisystem des Hosts ist.
* Docker erstellt das Volume automatisch, wenn es benötigt wird, und kann es zwischen Containern und Hosts verschieben.
* _Named Volumes_ sind plattformübergreifend und bieten eine einfache Möglichkeit, persistente Daten zwischen Containern zu speichern.
* Sie sind besonders nützlich für Daten, die über den Lebenszyklus eines einzelnen Containers hinaus erhalten bleiben müssen.
* Beispiel: \
  \
  `docker run -v mein_volume:/pfad/im/container ...`

### **Bind Mount**

* Ein Bind Mount bindet ein Verzeichnis oder eine Datei vom Host-Dateisystem direkt in den Container ein.
* Dies ermöglicht es dem Container, auf die Dateien im Host-Dateisystem zuzugreifen, als ob sie Teil des Containers wären.
* Bind Mounts bieten eine einfache und direkte Möglichkeit, Dateien zwischen Host und Container zu teilen.
* Sie sind nicht Docker-spezifisch und können daher auch außerhalb von Docker verwendet werden.
* Beispiel:\
  \
  `docker run -v /pfad/auf/dem/host:/pfad/im/container ...`

Bind Mounts sind gut geeignet, um bestimmte Dateien oder Verzeichnisse aus dem Host-Dateisystem in einen Container einzubinden, Named Volumes hingegen bieten eine robuste Lösung für die Speicherung persistenter Daten bieten, die unabhängig vom Lebenszyklus der Container sind.



> **🗯** Bei der Verwendung von Parametern auf der Konsole wird in beiden Fällen der Paramter -v genutzt. Ob es sich um ein Named Volume oder um ein Bind Mount handelt erkennt man daran, ob lediglich ein Name für das Volume oder ein Pfad auf dem Host-System angegeben wird.

##

## Hausaufgabe

Bearbeiten Sie die Aufgabe [Docker CI Container](https://prof.aheil.de/devops/uebungsaufgaben/docker-ci-container).&#x20;

## Links

* Docker Compose Doku: [https://docs.docker.com/compose/](https://docs.docker.com/compose/)
* The Official YAML Web  Site: [https://yaml.org/](https://yaml.org/)&#x20;

\


