# Ansible Schritt für Schritt: Übungsaufgaben

In diesen Übungen bauen Sie die Inhalte der [Ansible-Vorlesung](README.md) praktisch auf. Die Aufgaben werden schrittweise anspruchsvoller: Sie beginnen mit einer lokalen Verbindung und erstellen am Ende eine kleine, wiederverwendbare Ansible-Rolle.

## Voraussetzungen

* Eine Ubuntu-VM, auf der Sie Änderungen gefahrlos testen können
* Python 3 und Ansible auf der VM
* Ein Terminal mit Bash

Die Übungen verwenden `localhost` als Managed Node. So können Sie die Ansible-Grundlagen zunächst ohne SSH-Verbindung zu einem zweiten Rechner lernen. Führen Sie die Aufgaben nicht auf einem produktiven Server aus.

Ansible installieren (sofern noch nicht vorhanden):

```sh
sudo apt update
sudo apt install ansible curl
```

Prüfen Sie die Installation:

```sh
ansible --version
```

Legen Sie ein Arbeitsverzeichnis an:

```sh
mkdir -p ansible-lab
cd ansible-lab
```

***

## Aufgabe 1: Verbindung zu localhost testen

### Schritt 1: Inventory anlegen

Erstellen Sie die Datei `inventory.ini`:

```ini
[local]
localhost ansible_connection=local
```

Die Gruppe `local` enthält einen Host. `ansible_connection=local` weist Ansible an, die Aufgaben direkt auf diesem Rechner und nicht über SSH auszuführen.

### Schritt 2: Verbindung prüfen

Führen Sie das eingebaute Ping-Modul aus:

```sh
ansible -i inventory.ini local -m ansible.builtin.ping
```

### Prüfen

Die Antwort sollte für `localhost` `SUCCESS` und `"ping": "pong"` enthalten.

**Fragen:** Was bezeichnet `-i`? Was bezeichnet `local` im Befehl?

***

## Aufgabe 2: Erstes Playbook schreiben

Erstellen Sie die Datei `site.yml`:

```yaml
---
- name: Lokalen Host prüfen
  hosts: local
  tasks:
    - name: Betriebssysteminformationen anzeigen
      ansible.builtin.debug:
        msg: "Ansible verwaltet diesen Host."
```

### Schritt 1: Syntax prüfen

```sh
ansible-playbook -i inventory.ini site.yml --syntax-check
```

### Schritt 2: Playbook ausführen

```sh
ansible-playbook -i inventory.ini site.yml
```

### Prüfen

Die Ausgabe enthält die Nachricht `Ansible verwaltet diesen Host.`. Achten Sie auf die Struktur: Ein Play wählt Hosts aus und enthält eine Liste von Tasks; jeder Task ruft ein Modul auf.

**Änderung:** Passen Sie die Nachricht an und führen Sie das Playbook erneut aus. Welcher Teil der Ausgabe zeigt, dass sich dabei kein Systemzustand geändert hat?

***

## Aufgabe 3: Nginx installieren und Idempotenz beobachten

Erweitern Sie `site.yml` um zwei Tasks. Das Paket wird mit erhöhten Rechten installiert und der Dienst wird gestartet und für den Systemstart aktiviert:

```yaml
---
- name: Webserver auf localhost einrichten
  hosts: local
  become: true
  tasks:
    - name: Nginx installieren
      ansible.builtin.apt:
        name: nginx
        state: present

    - name: Nginx starten und aktivieren
      ansible.builtin.service:
        name: nginx
        state: started
        enabled: true
```

### Schritt 1: Playbook ausführen

```sh
ansible-playbook -i inventory.ini site.yml
```

Beantworten Sie bei der Passwortabfrage den Prompt für `sudo`/`become`, falls er erscheint.

### Schritt 2: Ergebnis prüfen

Rufen Sie auf der VM `http://localhost` im Browser auf oder führen Sie Folgendes aus:

```sh
curl http://localhost
```

### Schritt 3: Playbook erneut ausführen

Führen Sie denselben `ansible-playbook`-Befehl erneut aus und vergleichen Sie die Task-Ergebnisse beider Läufe.

### Prüfen

Beim ersten Lauf wird Nginx installiert und gestartet. Beim zweiten Lauf sollte Ansible melden, dass die Tasks bereits den gewünschten Zustand haben und keine weitere Änderung erforderlich ist.

**Fragen:** Woran erkennen Sie Idempotenz in der Ausgabe? Warum ist `state: present` eine Beschreibung des Zielzustands?

***

## Aufgabe 4: Eine Variable verwenden

Verschieben Sie den Paketnamen in eine Variable. Ergänzen Sie in `site.yml` vor `tasks`:

```yaml
  vars:
    webserver_package: nginx
```

Ersetzen Sie im Paket-Task `name: nginx` durch:

```yaml
        name: "{{ webserver_package }}"
```

Führen Sie das Playbook zunächst mit `nginx` aus. Ändern Sie danach den Variablenwert testweise auf `apache2-utils`, führen Sie es erneut aus und prüfen Sie, was Ansible meldet. Stellen Sie den Wert anschließend wieder auf `nginx`. `apache2-utils` enthält Hilfsprogramme, startet aber keinen Apache-Webserver, der mit Nginx um Port 80 konkurrieren würde.

### Prüfen

Sie haben den Paketnamen an einer Stelle änderbar gemacht, ohne die Task-Struktur umzuschreiben. Überlegen Sie, welche Werte in einem größeren Playbook noch geeignete Variablen wären.

***

## Aufgabe 5: Eine eigene Startseite bereitstellen

Ansible soll eine kleine HTML-Datei auf dem Managed Node ablegen.

### Schritt 1: Quelldatei anlegen

Erstellen Sie im Arbeitsverzeichnis die Datei `index.html`:

```html
<!doctype html>
<html lang="de">
  <meta charset="utf-8">
  <title>Ansible-Labor</title>
  <h1>Hallo aus Ansible</h1>
</html>
```

### Schritt 2: Copy-Task ergänzen

Fügen Sie in `site.yml` einen Task hinzu:

```yaml
    - name: Startseite bereitstellen
      ansible.builtin.copy:
        src: index.html
        dest: /var/www/html/index.html
        owner: root
        group: root
        mode: "0644"
```

Da das Play `become: true` verwendet, darf Ansible die Datei im Webserver-Verzeichnis anlegen.

### Schritt 3: Ausführen und prüfen

```sh
ansible-playbook -i inventory.ini site.yml
curl http://localhost
```

Ändern Sie den Text in `index.html`, führen Sie das Playbook erneut aus und prüfen Sie die Antwort von `curl` noch einmal.

### Prüfen

Ansible kopiert die Datei nur dann erneut, wenn sich der Inhalt oder die gewünschten Dateieigenschaften unterscheiden. Erklären Sie, warum das für wiederholte Ausführungen hilfreich ist.

***

## Aufgabe 6: Aufgaben in eine Rolle verschieben

Eine Rolle bündelt zusammengehörige Tasks und lässt sich in mehreren Plays wiederverwenden.

### Schritt 1: Rollenstruktur anlegen

Führen Sie im Projektverzeichnis aus:

```sh
mkdir -p roles/webserver/tasks
mkdir -p roles/webserver/files
```

Verschieben Sie `index.html` nach `roles/webserver/files/index.html`.

### Schritt 2: Tasks in die Rolle verschieben

Erstellen Sie `roles/webserver/tasks/main.yml` und verschieben Sie dort hinein die drei Tasks aus dem bisherigen Playbook:

* Nginx installieren
* Nginx starten und aktivieren
* Startseite bereitstellen

Die Datei sollte anschließend ungefähr so aussehen:

```yaml
---
- name: Nginx installieren
  ansible.builtin.apt:
    name: "{{ webserver_package }}"
    state: present

- name: Nginx starten und aktivieren
  ansible.builtin.service:
    name: nginx
    state: started
    enabled: true

- name: Startseite bereitstellen
  ansible.builtin.copy:
    src: index.html
    dest: /var/www/html/index.html
    owner: root
    group: root
    mode: "0644"
```

Bei einer Rolle sucht das `copy`-Modul `src` standardmäßig im `files`-Verzeichnis der Rolle. Der Paketname wird weiterhin als Variable aus dem Playbook übernommen.

### Schritt 3: Playbook auf die Rolle umstellen

Ersetzen Sie in `site.yml` die `tasks` durch:

```yaml
  roles:
    - webserver
```

Die Variablen können zunächst im Play unter `vars` bleiben. Prüfen Sie die Syntax und führen Sie das Playbook aus:

```sh
ansible-playbook -i inventory.ini site.yml --syntax-check
ansible-playbook -i inventory.ini site.yml
```

### Zielstruktur

```text
ansible-lab/
├── inventory.ini
├── site.yml
└── roles/
    └── webserver/
        ├── files/
        │   └── index.html
        └── tasks/
            └── main.yml
```

### Prüfen

Ändern Sie die Startseite erneut und führen Sie die Rolle aus. Prüfen Sie mit `curl`, ob die Änderung auf dem lokalen Webserver ankommt.

***

## Aufgabe 7: Einen Handler und Tags ergänzen

### Teil A: Handler

Ein Handler eignet sich für eine Aktion, die nur nach einer tatsächlichen Änderung ausgeführt werden soll. Die Startseite benötigt keinen Neustart von Nginx. Erstellen Sie deshalb für diese Übung eine einfache zusätzliche Nginx-Konfiguration.

Legen Sie das Template-Verzeichnis an:

```sh
mkdir -p roles/webserver/templates
```

Erstellen Sie `roles/webserver/templates/lab.conf.j2` mit folgendem Inhalt:

```nginx
server {
    listen 8080;
    server_name _;
    root /var/www/html;
    index index.html;
}
```

Ergänzen Sie in `roles/webserver/tasks/main.yml` einen Task:

```yaml
- name: Zusätzliche Nginx-Konfiguration bereitstellen
  ansible.builtin.template:
    src: lab.conf.j2
    dest: /etc/nginx/conf.d/ansible-lab.conf
    owner: root
    group: root
    mode: "0644"
  notify: Nginx neu laden
```

Ergänzen Sie in `roles/webserver/handlers/main.yml`:

```yaml
---
- name: Nginx neu laden
  ansible.builtin.service:
    name: nginx
    state: reloaded
```

Legen Sie das benötigte Verzeichnis an:

```sh
mkdir -p roles/webserver/handlers
```

Führen Sie das Playbook aus und prüfen Sie die neue Adresse:

```sh
ansible-playbook -i inventory.ini site.yml
curl http://localhost:8080
```

Führen Sie das Playbook ein zweites Mal aus. Der Handler sollte nur beim ersten Lauf aufgerufen werden, wenn die Konfigurationsdatei neu angelegt wird.

### Teil B: Tags

Fügen Sie dem Konfigurations-Task den Tag `config` hinzu:

```yaml
  tags:
    - config
```

Führen Sie nur die markierten Tasks aus:

```sh
ansible-playbook -i inventory.ini site.yml --tags config
```

Mit `--list-tasks` können Sie vorher anzeigen lassen, welche Tasks ausgewählt würden:

```sh
ansible-playbook -i inventory.ini site.yml --tags config --list-tasks
```

**Fragen:** Warum wird der Handler beim unveränderten zweiten Lauf nicht erneut ausgelöst? Was ändert sich, wenn ein Task kein passendes Tag hat?

***

## Zusatzaufgabe: Von der Übung zur Hausaufgabe

Die [Ansible-Hausaufgabe](ansible.md) verwendet Rollen für Firewall, Cron und SSH. Übertragen Sie die Struktur aus den Übungen darauf, aber beachten Sie die Risiken:

* Arbeiten Sie ausschließlich in Ihrer Test-VM und behalten Sie Zugriff über die VirtualBox-Konsole.
* Aktivieren Sie eine Firewall erst, nachdem die benötigten Zugriffsregeln gesetzt und geprüft wurden.
* Deaktivieren Sie Passwort-Logins nicht, bevor ein SSH-Schlüssel-Login erfolgreich getestet wurde.
* Planen Sie in der Übung keinen automatischen Neustart ein. Testen Sie Cron zunächst mit einer ungefährlichen Aktion, die keine Systemänderung mit großen Folgen auslöst.

Erstellen Sie für jede Funktion eine eigene Rolle und prüfen Sie, dass Sie die Rollen einzeln auswählen und ausführen können. Halten Sie in einer README-Datei die Befehle fest, mit denen sich Syntax und Playbook prüfen lassen.

***

## Aufräumen

Wenn Sie die VM nur für diese Übungen verwenden und Nginx danach nicht mehr benötigen, können Sie den Dienst stoppen und das Paket entfernen:

```sh
sudo systemctl disable --now nginx
sudo apt remove nginx
```

Entfernen Sie die VM oder andere Dateien nur, wenn Sie sicher sind, dass Sie diese nicht mehr benötigen.
