---
description: >-
  Erstellen Sie ein Docker-Image, das ein einfaches Python-Skript ausführt, das
  “Hallo, Docker!” ausgibt.
---

# Docker Container Übungsaufgabe

1. **Installieren Sie Docker:**
   * Stellen Sie sicher, dass Docker auf Ihrem System installiert ist. Anleitungen finden Sie auf der Docker-Website.
   * Unter Windows benötigen Sie Windows Subsystem für Linux. Sollten Sie noch keine Distribution installiert haben und eine entsprechende Fehlermeldung beim Start von Docker erhalten, führen SIe folgenden Befehl aus:\
     \
     `wsl.exe --install --no-distribution`
2. **Erstellen Sie ein Projektverzeichnis:**
   *   Erstellen Sie ein neues Verzeichnis für Ihr Projekt:\


       ```bash
       mkdir my-python-docker-project
       cd my-python-docker-project
       ```
3. **Erstellen Sie ein Python-Skript:**
   *   Erstellen Sie eine Datei namens `app.py` mit folgendem Inhalt:

       ```python
       print("Hallo, Docker!")
       ```
4. **Erstellen Sie eine Dockerfile:**
   *   Erstellen Sie eine Datei namens `Dockerfile` im selben Verzeichnis mit folgendem Inhalt:

       ```docker
       # Verwenden Sie ein Basis-Image mit Python
       FROM python:3.9-slim

       # Kopieren Sie das Python-Skript in das Arbeitsverzeichnis des Containers
       COPY app.py /app/app.py

       # Setzen Sie das Arbeitsverzeichnis
       WORKDIR /app

       # Führen Sie das Python-Skript aus
       CMD ["python", "app.py"]
       ```
5. **Bauen Sie das Docker-Image:**
   *   Führen Sie den folgenden Befehl aus, um das Docker-Image zu erstellen:

       ```bash
       docker build -t my-python-app .
       ```
6. **Starten Sie einen Container aus dem Image:**
   *   Führen Sie den folgenden Befehl aus, um einen Container zu starten:

       ```bash
       docker run my-python-app
       ```
7. **Überprüfen Sie das Ergebnis:**
   * Sie sollten die Ausgabe “Hallo, Docker!” im Terminal sehen.

**Bonus:**

* Ändern Sie das Python-Skript, um eine andere Nachricht auszugeben, und bauen Sie das Image erneut.
* Experimentieren Sie mit zusätzlichen Python-Bibliotheken und erweitern Sie die Dockerfile entsprechend.
