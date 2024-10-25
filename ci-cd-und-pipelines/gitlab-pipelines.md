# GitLab Pipelines

#### Ziele der Übungen

Nehmen wir an, dass Sie an einer Spring Boot-Anwendung arbeiten, die auf GitLab gehostet wird. Die Anwendung wird mit Gradle erstellt und als Docker-Image verpackt. Derzeit werden alle Schritte vom Build über den Test bis zum Deployment manuell durchgeführt. Sie möchten den Prozess jedoch mithilfe der CI/CD-Pipelines von GitLab automatisieren. Folgen Sie den nachstehenden Übungen, um Automatisierungen zu implementieren.

### Übung 1

Implementieren Sie grundlegende Regeln und CI-Funktionalitäten.

1. Sperren Sie den Hauptbranch (Main) in GitLab, um direkte Pushes zu verhindern.
2. Erstellen Sie eine GitLab-CI-Pipeline, die bei einem Push auf einen Feature-Branch folgende Stages ausführt:
   * Build\
     Bauen Sie die Spring Boot Anwendung mit Gradle. Erstellen Sie eine JAR-Datei in den Artefakten.
   * Test\
     Führen Sie die Unit-Tests mit Gradle aus. Erstellen Sie einen JUnit Report in den Artefakten.

### Übung 2

Erstellen Sie eine Merge-Request-Pipeline, die bei Merge-Requests auf den Hauptbranch läuft.

1. Erstellen Sie eine Merge-Request-Pipeline, die folgende stages ausführt:
   * Build (zusätzlich zu Übung 1)
     * Holen Sie die Version aus der build.gradle-Datei und schreiben Sie sie in eine Umgebungsdatei, die in der QA-Stage verwendet wird.
   * Test
   * QA\

     * Schreiben Sie ein Bash-Skript, das überprüft, ob der Changelog im Merge-Request aktualisiert wurde. Wenn der Changelog nicht aktualisiert wurde, sollte die Pipeline fehlschlagen.
     * Schreiben Sie ein Bash-Skript, das überprüft, ob die neueste Version im CHANGELOG.md dieselbe ist wie in der build.gradle-Datei. Wenn die Versionen nicht übereinstimmen, sollte die Pipeline fehlschlagen.

### Übung 3

Erstellen Sie eine Deployment stage, die beim Push auf den Hauptzweig ausgeführt wird.

1. Erstellen Sie eine Dockerdatei, die ein Docker-Image für die Spring Boot-Anwendung erzeugt. Verwenden Sie eclipse-temurin:21 als Base Image. (Tipp: Verwenden Sie das JAR-Artefakt aus der Build-Stage mit einer ENV Variable für den JAR-Namen.)
2. Erstellen Sie eine Deployment stage, die beim Push auf den Hauptzweig ausgeführt wird und das Docker-Image deployed.

* Verwenden Sie Kaniko, um das Docker-Image zu erstellen.
* Verwenden Sie die GitLab Container Registry, um das Docker-Image zu speichern.
* Taggen Sie das Docker-Image mit latest.

### Übung 4

Erstellen Sie eine Release-Pipeline, die bei der Erstellung eines Tags läuft.

1. Erstellen Sie eine Release-Pipeline, die bei der Erstellung eines Tags folgende stages ausführt:
   * Build
   * Test
   * Deploy (mit Tag anstelle von latest)
   * Release\
     Schreiben Sie ein Bash-Skript, das ein neues GitLab-Release erstellt und Folgendes einschließt (Tipp: Sie können auch mehrere Jobs in einer Stage verwenden):
     * Den Tag-Namen
     * Den Changelog der Version als Beschreibung
     * Reference auf den Commit, der getaggt wurde
     * Den Link zum Docker-Image in der GitLab Container Registry

### Übung 5

Schreiben Sie ein README.md in dem Sie die Funktionalitäten und Nutzung Ihrer CI/CD-Pipeline für neue Entwickler dokumentieren.
