# Vagrant Schritt für Schritt: Übungsaufgaben

In diesen Übungen setzen Sie die Inhalte der [Vagrant-Vorlesung](README.md) praktisch um. Sie beginnen mit einer einfachen VM und provisionieren am Ende einen Webserver mit Ansible – die Grundlage für die [Hausaufgabe](vagrant.md).

## Voraussetzungen

* [VirtualBox](https://www.virtualbox.org/) (oder ein anderer unterstützter Provider)
* [Vagrant](https://developer.hashicorp.com/vagrant/install)
* Ein Terminal und ca. 5 GB freier Speicherplatz

Prüfen Sie die Installation:

```sh
vagrant --version
VBoxManage --version
```

Legen Sie ein Arbeitsverzeichnis an:

```sh
mkdir vagrant-lab
cd vagrant-lab
```

***

## Aufgabe 1: Die erste VM

### Schritt 1: Vagrantfile anlegen

```sh
vagrant init bento/ubuntu-24.04
```

Öffnen Sie die erzeugte Datei `Vagrantfile`. Die meisten Zeilen sind Kommentare – überfliegen Sie sie, sie dokumentieren die wichtigsten Optionen.

### Schritt 2: VM starten und verbinden

```sh
vagrant up
vagrant ssh
```

In der VM:

```sh
lsb_release -a
exit
```

### Prüfen

* `vagrant status` zeigt `running`.
* In der VM wird Ubuntu 24.04 angezeigt.

***

## Aufgabe 2: Ressourcen und Hostname

Ersetzen Sie den Inhalt des Vagrantfiles durch:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-24.04"
  config.vm.hostname = "lab"

  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048
    vb.cpus = 2
  end
end
```

Übernehmen Sie die Änderungen:

```sh
vagrant reload
```

### Prüfen

```sh
vagrant ssh -c "hostname && nproc && free -m"
```

Erwartet: Hostname `lab`, 2 CPUs, ca. 2 GB RAM.

***

## Aufgabe 3: Synced Folder erkunden

Legen Sie auf dem Host eine Datei an:

```sh
echo "Hallo vom Host" > hallo.txt
```

### Prüfen

```sh
vagrant ssh -c "cat /vagrant/hallo.txt"
```

Ändern Sie die Datei auf dem Host und führen Sie den Befehl erneut aus. Die Änderung ist sofort sichtbar.

***

## Aufgabe 4: Shell-Provisioning und Port Forwarding

### Schritt 1: Skript anlegen

Erstellen Sie `bootstrap.sh`:

```sh
#!/usr/bin/env bash
set -e
apt-get update
apt-get install -y nginx
```

### Schritt 2: Vagrantfile erweitern

Ergänzen Sie im `Vagrant.configure`-Block:

```ruby
  config.vm.network "forwarded_port", guest: 80, host: 8080
  config.vm.provision "shell", path: "bootstrap.sh"
```

### Schritt 3: Anwenden

Port-Änderungen erfordern einen Neustart, das Provisioning muss explizit angestoßen werden:

```sh
vagrant reload --provision
```

### Prüfen

Öffnen Sie `http://localhost:8080` im Browser auf dem Host oder:

```sh
curl http://localhost:8080
```

Erwartet: die Nginx-Standardseite.

***

## Aufgabe 5: Von der Shell zu Ansible

Ersetzen Sie das Shell-Skript durch ein Ansible-Playbook.

### Schritt 1: Playbook anlegen

Erstellen Sie `playbook.yml`:

```yaml
- name: Webserver einrichten
  hosts: all
  become: true

  tasks:
    - name: Nginx installieren
      ansible.builtin.apt:
        name: nginx
        state: present
        update_cache: true

    - name: Startseite bereitstellen
      ansible.builtin.copy:
        content: "<h1>Provisioniert mit Vagrant und Ansible</h1>\n"
        dest: /var/www/html/index.html
        mode: "0644"
```

### Schritt 2: Provisioner tauschen

Ersetzen Sie die Shell-Zeile im Vagrantfile durch:

```ruby
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "playbook.yml"
  end
```

### Schritt 3: Anwenden

```sh
vagrant provision
```

Vagrant installiert beim ersten Lauf Ansible in der VM und führt dann das Playbook aus.

### Prüfen

```sh
curl http://localhost:8080
```

Führen Sie `vagrant provision` ein zweites Mal aus. Im `PLAY RECAP` sollte `changed=0` stehen – das Playbook ist idempotent.

***

## Aufgabe 6: Reproduzierbarkeit testen

Der wichtigste Test: Lässt sich die Umgebung vollständig neu erzeugen?

```sh
vagrant destroy -f
vagrant up
curl http://localhost:8080
```

### Prüfen

Ohne manuelle Schritte wird wieder die eigene Startseite ausgeliefert.

***

## Zusatzaufgabe: Zwei Maschinen

Erweitern Sie das Vagrantfile um eine zweite VM `db` mit eigener IP im privaten Netzwerk (siehe Vorlesung, Abschnitt 7). Prüfen Sie aus der VM `web` heraus, ob `db` erreichbar ist:

```sh
vagrant ssh web -c "ping -c 3 192.168.56.11"
```

***

## Aufräumen

```sh
vagrant destroy -f
vagrant box list
vagrant box remove bento/ubuntu-24.04   # optional, gibt Speicherplatz frei
```
