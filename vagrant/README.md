# Vagrant: Virtuelle Umgebungen automatisieren

## Lernziele

Nach dieser Vorlesung können Sie:

* erklären, welches Problem Vagrant löst und wo es eingesetzt wird,
* die Begriffe Provider, Box, Vagrantfile und Provisioner einordnen,
* den Lebenszyklus einer Vagrant-Umgebung mit den wichtigsten Kommandos steuern,
* ein einfaches Vagrantfile lesen und schreiben (Ressourcen, Netzwerk, Synced Folders),
* eine VM per Shell-Skript oder Ansible provisionieren,
* mehrere Maschinen in einem Vagrantfile beschreiben und
* Vagrant gegenüber Docker abgrenzen.

> **Einstieg:** Eine neue Kollegin startet im Team. Bis ihre Entwicklungsumgebung genauso läuft wie Ihre – gleiche Betriebssystemversion, gleiche Pakete, gleiche Ports – vergehen zwei Tage. Und trotzdem gilt am Ende: _„Bei mir funktioniert's.“_ Wie ließe sich das mit einer einzigen Datei im Repository lösen?

<figure><img src="../.gitbook/assets/vagrant.png" alt=""><figcaption></figcaption></figure>

***

## 1. Warum Vagrant?

### Das Problem: unterschiedliche Umgebungen

Entwicklungsrechner unterscheiden sich: Windows, macOS, Linux, verschiedene Paketversionen, lokal installierte Datenbanken. Das Zielsystem (z. B. ein Ubuntu-Server) sieht oft ganz anders aus. Je größer der Unterschied, desto häufiger tauchen Fehler erst beim Deployment auf.

**Gedankenspiel:** Wie groß ist der Unterschied zwischen Ihrer Entwicklungsumgebung und dem System, auf dem Ihre Software später läuft?

### Die Idee

Vagrant ist ein Werkzeug zum **automatisierten Aufsetzen virtueller Entwicklungsumgebungen**. Die gesamte Umgebung wird in einer Textdatei – dem `Vagrantfile` – beschrieben und liegt mit im Repository. Ein einziges Kommando erzeugt daraus eine lauffähige VM:

```sh
vagrant up
```

Vagrant wurde von Mitchell Hashimoto in Ruby entwickelt und wird heute von HashiCorp gepflegt. Seit 2023 steht es unter der Business Source License (BSL); für Lehre und interne Nutzung ist das unproblematisch.

### Anwendungsgebiete

* **Identische Umgebungen** für alle Entwicklerinnen und Entwickler eines Teams
* **Software testen** auf unterschiedlichen Betriebssystemen und Versionen
* **Konfigurationsmanagement testen**, z. B. Ansible-Playbooks, bevor sie auf echte Server treffen
* **Multi-Server-Szenarien** (Webserver + Datenbank) lokal nachstellen

### Zur Erinnerung: Warum automatisieren?

* **Zeitgewinn** – kein manuelles Installieren von Betriebssystem und Paketen
* **Fehlervermeidung** – keine vergessenen Schritte
* **Reproduzierbarkeit** – jede Umgebung entsteht aus derselben Beschreibung

***

## 2. Grundbegriffe

| Begriff         | Bedeutung                                                                                   | Beispiele                               |
| --------------- | ------------------------------------------------------------------------------------------- | --------------------------------------- |
| **Provider**    | Virtualisierungstechnik, auf der die VM läuft                                               | VirtualBox, Hyper-V, VMware, Docker     |
| **Box**         | Vorgefertigtes Basis-Image, aus dem die VM erzeugt wird – das Betriebssystem ist schon installiert | `bento/ubuntu-24.04`, `ubuntu/jammy64`  |
| **Vagrantfile** | Konfigurationsdatei (Ruby-Syntax), beschreibt die Umgebung                                  | liegt im Projektverzeichnis             |
| **Provisioner** | Richtet die VM nach dem Start ein                                                           | Shell, Ansible, Chef, Puppet            |

Das Zusammenspiel:

```
Vagrantfile ──► Vagrant ──► Provider (z. B. VirtualBox) ──► VM aus Box
                                                           │
                                     Provisioner (Shell/Ansible) richtet sie ein
```

Öffentliche Boxes finden Sie im [Vagrant-Box-Katalog von HashiCorp](https://portal.cloud.hashicorp.com/vagrant/discover). Achten Sie auf vertrauenswürdige Herausgeber (z. B. `bento`, `ubuntu`, `generic`) – eine Box ist ein komplettes Betriebssystem, das Sie ausführen.

***

## 3. Lebenszyklus und Kommandos

| Kommando                      | Wirkung                                                         |
| ----------------------------- | --------------------------------------------------------------- |
| `vagrant init <box>`          | Legt ein Vagrantfile im aktuellen Verzeichnis an                |
| `vagrant up`                  | Erstellt und startet die VM (beim ersten Mal inkl. Provisioning) |
| `vagrant ssh`                 | Shell-Zugriff auf die VM                                        |
| `vagrant status`              | Zustand der VMs dieses Projekts                                 |
| `vagrant global-status`       | Zustand aller Vagrant-Umgebungen auf dem Rechner                |
| `vagrant provision`           | Führt den Provisioner erneut aus                                |
| `vagrant reload [--provision]` | Neustart, z. B. nach Änderungen am Vagrantfile                 |
| `vagrant halt`                | Fährt die VM herunter                                           |
| `vagrant destroy`             | Löscht die VM vollständig                                       |
| `vagrant box add\|list\|remove\|prune` | Lokale Boxes verwalten                                 |

Typischer Ablauf:

```
init ──► up ──► ssh / arbeiten ──► provision / reload ──► halt ──► destroy
```

> **Merke:** `vagrant destroy` + `vagrant up` muss immer wieder zur selben Umgebung führen. Wenn nicht, steckt manuelle Arbeit in der VM, die ins Vagrantfile gehört.

***

## 4. Das erste Vagrantfile

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.hostname = "dev"
end
```

* `Vagrant.configure("2")` – Version 2 des Konfigurationsformats
* `config.vm.box` – welche Box verwendet wird
* Beim ersten `vagrant up` wird die Box heruntergeladen und lokal zwischengespeichert.

### Ressourcen des Providers konfigurieren

Provider-spezifische Einstellungen werden in einem eigenen Block gesetzt[^1]:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
end
```

Das ist **Infrastructure as Code**: Hardware-Ausstattung, Betriebssystem und Konfiguration sind versioniert, nachvollziehbar und per Code-Review überprüfbar.

***

## 5. Netzwerk und Synced Folders

### Port Forwarding

Ein Dienst in der VM (z. B. Webserver auf Port 80) wird über einen Port auf dem Host erreichbar:

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080
```

Danach ist der Webserver der VM unter `http://localhost:8080` erreichbar. Alle Teammitglieder nutzen dieselben Ports.

### Private Netzwerk

Für die Kommunikation zwischen mehreren VMs oder direkten Zugriff vom Host:

```ruby
config.vm.network "private_network", ip: "192.168.56.10"
```

### Synced Folders

Das Projektverzeichnis auf dem Host ist in der VM standardmäßig unter `/vagrant` eingebunden. Sie bearbeiten Dateien mit Ihrer IDE auf dem Host – die VM sieht die Änderungen sofort.

```ruby
config.vm.synced_folder "./site", "/var/www/html"
```

***

## 6. Provisionierung

Eine Box enthält nur das Betriebssystem. Software und Konfiguration bringt der **Provisioner** in die VM. Er läuft beim ersten `vagrant up` sowie bei `vagrant provision` bzw. `vagrant reload --provision`.

### Shell-Provisioner

Für einfache Fälle genügt ein Skript:

```ruby
config.vm.provision "shell", path: "bootstrap.sh"
```

`bootstrap.sh` im Verzeichnis des Vagrantfiles:

```sh
#!/usr/bin/env bash
set -e
apt-get update
apt-get install -y nginx
```

Oder direkt inline:

```ruby
config.vm.provision "shell", inline: "apt-get update && apt-get install -y nginx"
```

### Ansible als Provisioner

Hier schließt sich der Kreis zur [Ansible-Vorlesung](../ansible/README.md): Vagrant liefert die Maschine, Ansible die Konfiguration[^2].

| Provisioner     | Wo läuft Ansible?               | Hinweis                                                  |
| --------------- | ------------------------------- | -------------------------------------------------------- |
| `ansible`       | Auf dem Host                    | Ansible muss auf dem Host installiert sein (nicht nativ unter Windows) |
| `ansible_local` | In der VM                       | Vagrant installiert Ansible in der VM – funktioniert auf jedem Host |

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"

  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
end
```

Das `playbook.yml` liegt neben dem Vagrantfile und ist über `/vagrant` in der VM verfügbar. Weil Ansible-Playbooks idempotent sind, kann `vagrant provision` beliebig oft ausgeführt werden.

***

## 7. Mehrere Maschinen

In einem Vagrantfile lassen sich mehrere VMs definieren – z. B. ein Webserver und eine Datenbank:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"

  config.vm.define "web" do |web|
    web.vm.hostname = "web"
    web.vm.network "private_network", ip: "192.168.56.10"
    web.vm.network "forwarded_port", guest: 80, host: 8080
  end

  config.vm.define "db" do |db|
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.56.11"
  end
end
```

Kommandos beziehen sich dann auf alle oder auf einzelne Maschinen:

```sh
vagrant up          # beide VMs
vagrant ssh web     # nur in den Webserver
vagrant halt db     # nur die Datenbank stoppen
```

***

## 8. Eigene Boxes

Eigene Boxes lohnen sich, wenn viel vorinstallierte Software benötigt wird und das Provisioning sonst sehr lange dauert.

* `vagrant package` erzeugt aus einer laufenden Vagrant-VM eine `.box`-Datei.
* Grundsätzlich kann jede VirtualBox-VM als Box dienen, sofern Vagrants Anforderungen erfüllt sind[^3] – u. a. muss der erste Netzwerkadapter ein NAT-Adapter sein und ein `vagrant`-Benutzer mit SSH-Zugang existieren.
* Für automatisierte, reproduzierbare Box-Builds wird in der Praxis [Packer](https://developer.hashicorp.com/packer) eingesetzt.

***

## 9. Vagrant vs. Docker

|                  | Vagrant (VM)                               | Docker (Container)                                    |
| ---------------- | ------------------------------------------ | ----------------------------------------------------- |
| Isolation        | Eigenes Betriebssystem inkl. Kernel        | Teilt den Kernel des Hosts                            |
| Startzeit        | Minuten                                    | Sekunden                                              |
| Ressourcen       | Hoch (vollständige VM)                     | Gering                                                |
| Typischer Zweck  | Komplette Server-/Entwicklungsumgebungen nachbilden | Anwendungen bauen, ausliefern, betreiben      |
| Beschreibung     | `Vagrantfile`                              | `Dockerfile`, `compose.yaml`                          |

Die Werkzeuge schließen sich nicht aus: Docker kann als Provider für Vagrant dienen, und – wie in der Hausaufgabe – kann in einer Vagrant-VM Docker laufen.

***

## 10. Voraussetzungen und typische Stolpersteine

* **Provider installieren**, z. B. [VirtualBox](https://www.virtualbox.org/)
* **Vagrant installieren**: [developer.hashicorp.com/vagrant/install](https://developer.hashicorp.com/vagrant/install)
* **Windows:** Hyper-V/WSL2 und VirtualBox können sich gegenseitig stören. Bei Problemen die VirtualBox-Version aktualisieren oder Hyper-V als Provider nutzen.
* **Apple Silicon (ARM):** Viele Boxes gibt es nur für x86. Boxes mit `arm64`-Variante (z. B. `bento`) und einen passenden Provider (VMware Fusion, Parallels) wählen.
* Fügen Sie das Verzeichnis `.vagrant/` zur `.gitignore` hinzu – es enthält lokale Zustandsdaten.

***

## Zusammenfassung

* Vagrant beschreibt virtuelle Entwicklungsumgebungen als Code im `Vagrantfile`.
* Eine **Box** liefert das Betriebssystem, ein **Provider** die Virtualisierung, ein **Provisioner** die Konfiguration.
* `vagrant up`, `ssh`, `provision`, `halt` und `destroy` bilden den Lebenszyklus.
* Port Forwarding, private Netzwerke und Synced Folders verbinden Host und VM.
* Mit `ansible_local` lassen sich Ansible-Playbooks direkt beim Start der VM anwenden.
* Mehrere Maschinen können in einem Vagrantfile gemeinsam beschrieben werden.
* VMs isolieren stärker als Container, sind aber schwergewichtiger.

### Verständnisfragen

1. Was ist der Unterschied zwischen einer Box und einem Provider?
2. Warum sollte `vagrant destroy && vagrant up` immer zur gleichen Umgebung führen?
3. Wann wählen Sie den Provisioner `ansible_local` statt `ansible`?
4. Wozu dient der Ordner `/vagrant` in der VM?
5. Nennen Sie je ein Szenario, in dem Sie Vagrant bzw. Docker bevorzugen würden.

[^1]: https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/configuration

[^2]: https://developer.hashicorp.com/vagrant/docs/provisioning/ansible_local

[^3]: https://developer.hashicorp.com/vagrant/docs/providers/virtualbox/boxes
