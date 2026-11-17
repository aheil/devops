# Vagrant-Hausaufgabe: Docker-Webserver in einer Vagrant-VM

## Ziel

In dieser Hausaufgabe kombinieren Sie die bisherigen Themen: **Vagrant** erzeugt eine VM, **Ansible** provisioniert sie und **Docker** betreibt darin einen Webserver, der eine statische Webseite ausliefert. Die Hausaufgabe baut auf den [Schritt-für-Schritt-Übungen](uebungen.md) auf.

```
Host                         Vagrant-VM (Ubuntu)                 Docker-Container (Alpine)
─────────────────────        ─────────────────────────────       ─────────────────────────
Browser :8080  ───────────►  Port 8080  ─────────────────────►   Webserver :80
projekt/site/index.html ──►  /vagrant/site/index.html  ──────►   (Bind-Mount) Webroot
```

Am Ende soll gelten:

* `vagrant up` erzeugt die komplette Umgebung ohne manuelle Schritte,
* die Webseite ist vom Host aus im Browser erreichbar,
* eine Änderung an `index.html` auf dem Host ist nach einem Neuladen sofort sichtbar und
* ein erneutes `vagrant provision` ändert nichts mehr (Idempotenz).

## Rahmenbedingungen

* Box: eine aktuelle Ubuntu-LTS-Box (z. B. `bento/ubuntu-24.04`)
* Provider: VirtualBox
* Provisionierung ausschließlich über Ansible – empfohlen ist `ansible_local`, damit die Abgabe auf jedem Host (auch Windows) läuft
* Keine manuellen Schritte in der VM: Alles, was Sie per `vagrant ssh` ausprobieren, muss am Ende im Playbook stehen.

## Vorgegebene Projektstruktur

```
vagrant-homework/
├── Vagrantfile
├── playbook.yml
├── docker/
│   ├── Dockerfile
│   └── nginx.conf        # nur falls benötigt
├── site/
│   └── index.html
└── README.md             # kurze Anleitung + Antworten auf die Reflexionsfragen
```

***

## Aufgabe 1: Dockerfile

Erstellen Sie unter `docker/` ein Dockerfile für einen Webserver (analog zur [Docker-Hausaufgabe](../container/docker-container-hausaufgabe.md)):

* Basis-Image: Alpine mit **fester Versionsangabe** (z. B. `alpine:3.20`, nicht `latest`)
* Installieren Sie Nginx oder Apache2 und konfigurieren Sie den Server so, dass er eine statische Seite ausliefert.
* Der Webserver muss im Vordergrund laufen, sonst beendet sich der Container sofort.
* Die Datei `index.html` wird **nicht** ins Image kopiert, sondern zur Laufzeit per Bind-Mount eingebunden.

> **Tipp:** Testen Sie das Image zuerst lokal oder in der VM mit `docker build` und `docker run -v ...`, bevor Sie es in Ansible gießen.

## Aufgabe 2: Vagrantfile

* Verwenden Sie die oben genannte Box und vergeben Sie einen Hostnamen.
* Begrenzen Sie die Ressourcen (z. B. 2 GB RAM, 2 CPUs).
* Leiten Sie Gast-Port `8080` auf Host-Port `8080` weiter.
* Binden Sie `playbook.yml` als Ansible-Provisioner ein.

## Aufgabe 3: Ansible-Playbook

Erstellen Sie ein Playbook, das

1. Docker in der VM installiert (z. B. Paket `docker.io`) und den Dienst aktiviert und startet,
2. den Benutzer `vagrant` zur Gruppe `docker` hinzufügt,
3. das Image aus `/vagrant/docker` baut und
4. den Container startet – mit
   * Bind-Mount von `/vagrant/site` auf das Webroot im Container (nur lesend, `:ro`),
   * Port-Mapping `8080:80` und
   * Neustart-Richtlinie `unless-stopped`, damit der Container ein `vagrant reload` übersteht.

Verwenden Sie dafür Module statt `shell`/`command`. Die Collection `community.docker` bietet `docker_image` und `docker_container`.

> **Tipp:** Die Collection und das Python-Paket für die Docker-API müssen in der VM vorhanden sein. Das lässt sich ebenfalls im Playbook erledigen (z. B. Paket `python3-docker`).

## Aufgabe 4: README und Reflexion

Beschreiben Sie in der `README.md` in wenigen Sätzen, wie man das Projekt startet und prüft. Beantworten Sie außerdem kurz:

1. Warum wird `index.html` per Bind-Mount eingebunden statt ins Image kopiert? Wann wäre das Kopieren die bessere Wahl?
2. Welche Schicht (Vagrant, Ansible, Docker) ist für was verantwortlich?
3. Was passiert bei `vagrant destroy` mit Ihrer `index.html`, und warum?

***

## Prüfung: erwartete Ergebnisse

Führen Sie diese Schritte vor der Abgabe selbst in einem **frisch entpackten** Verzeichnis aus:

| # | Befehl / Aktion                                  | Erwartung                                     |
| - | ------------------------------------------------ | --------------------------------------------- |
| 1 | `vagrant up`                                     | Läuft fehlerfrei durch                        |
| 2 | `curl http://localhost:8080`                     | Liefert den Inhalt Ihrer `index.html`         |
| 3 | `index.html` auf dem Host ändern, erneut `curl`  | Neuer Inhalt, ohne Neustart von VM/Container  |
| 4 | `vagrant provision`                              | `PLAY RECAP` mit `changed=0`                  |
| 5 | `vagrant reload`, danach `curl`                  | Seite ist weiterhin erreichbar                |
| 6 | `vagrant ssh -c "docker ps"`                     | Container läuft, ohne `sudo`                  |

## Häufige Fehler

* Container startet und beendet sich sofort → Webserver läuft nicht im Vordergrund.
* `403 Forbidden` → Bind-Mount zeigt auf das falsche Verzeichnis oder Webroot passt nicht zur Serverkonfiguration.
* Port bereits belegt → anderen Host-Port wählen und in README dokumentieren.
* Funktioniert nur beim zweiten `vagrant up` → Reihenfolge im Playbook prüfen (Docker vor Image-Build).

## Bewertung

| Kriterium                                              | Anteil |
| ------------------------------------------------------ | ------ |
| `vagrant up` läuft ohne manuelle Schritte durch        | 30 %   |
| Dockerfile (Alpine, feste Version, Bind-Mount)         | 20 %   |
| Playbook (Module statt Shell, idempotent)              | 25 %   |
| Live-Änderung von `index.html` und `reload`-Festigkeit | 10 %   |
| README und Reflexionsfragen                            | 15 %   |

### Zusatzaufgabe (optional)

Ersetzen Sie das Dockerfile + `docker_container` durch eine `compose.yaml`, die über das Modul `community.docker.docker_compose_v2` gestartet wird.

***

## Abgabe

Abzugeben ist das Projektverzeichnis gemäß der vorgegebenen Struktur. Das Verzeichnis `.vagrant/` gehört nicht in die Abgabe.

Reichen Sie die Abgabe als **ZIP-Datei** ein (kein 7z, rar, tar oder tar.gz).

Zur Bewertung wird das Archiv entpackt und `vagrant up` ausgeführt.
