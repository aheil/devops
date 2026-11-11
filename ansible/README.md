# Ansible: Infrastruktur automatisieren

## Lernziele

Nach dieser Vorlesung können Sie:

* erklären, warum Teams Serverkonfiguration automatisieren,
* Control Node, Managed Nodes und Inventory einordnen,
* ein einfaches Ansible-Playbook lesen und schreiben,
* Idempotenz an einem Beispiel erklären und
* den Zweck von Variablen, Rollen, Modulen, Handlern und Tags beschreiben.

> **Einstieg:** Stellen Sie sich vor, Sie richten für ein Team drei Linux-Server ein. Auf jedem sollen dieselben Pakete installiert und dieselben Einstellungen gesetzt werden. Was würden Sie von Hand tun – und was könnte dabei schiefgehen?

***

## 1. Warum automatisieren?

### Manuelle Serverkonfiguration

Bei einer manuellen Installation werden Befehle und Einstellungen direkt auf einem Server vorgenommen. Das kann für einen einzelnen Testserver funktionieren. Mit jedem weiteren Server steigt jedoch das Risiko, dass:

* ein Schritt vergessen oder anders ausgeführt wird,
* Server mit der Zeit voneinander abweichen,
* eine Änderung später nicht mehr nachvollziehbar ist und
* die Einrichtung unnötig viel Zeit kostet.

**Kleines Beispiel:** Auf Server A wird Nginx installiert und Port 80 freigegeben. Auf Server B wird die Firewall-Regel vergessen. Die Server sehen zunächst ähnlich aus, verhalten sich aber unterschiedlich.

### Vom manuellen Schritt zum Skript

Wiederholbare Befehle lassen sich in Skripten zusammenfassen:

```sh
sudo apt-get update
sudo apt-get install -y nginx
sudo systemctl enable --now nginx
```

Das ist ein guter Anfang: Die Schritte sind dokumentiert und erneut ausführbar. Ein Shell-Skript beschreibt aber oft **wie** die Änderung vorgenommen werden soll. Bei komplexeren Abläufen muss es selbst erkennen, ob ein Schritt bereits erledigt ist, Fehler behandeln und auf mehreren Rechnern ausgeführt werden.

Skripte sind weiterhin nützlich. Sie sind besonders passend für kurze, klar abgegrenzte Abläufe. Für wiederholbare Konfiguration über mehrere Systeme helfen Werkzeuge für Konfigurationsmanagement.

### Entwicklung und Betrieb

Entwicklung und Betrieb haben unterschiedliche Schwerpunkte, aber ein gemeinsames Ziel: zuverlässige Software für ihre Nutzerinnen und Nutzer.

* Entwicklung möchte Änderungen und neue Funktionen zeitnah bereitstellen.
* Betrieb möchte, dass Systeme stabil, sicher und nachvollziehbar laufen.

Automatisierung verbindet diese Anliegen: Änderungen können häufiger erfolgen, wenn sie reproduzierbar und überprüfbar sind.

Typische Aufgaben sind:

* Software installieren und aktualisieren,
* Konfigurationsdateien verwalten,
* Benutzer und Dienste einrichten,
* Umgebungen bereitstellen und
* Änderungen wiederholbar ausrollen.

***

## 2. Konfigurationsmanagement und Ansible

Konfigurationsmanagement beschreibt den gewünschten Zustand von Systemen und hilft, diesen Zustand herzustellen und wiederholt zu überprüfen. Die Konfiguration kann – genau wie Programmcode – versioniert und gemeinsam überprüft werden.

Beispiele für Werkzeuge in diesem Bereich sind Salt, Puppet, Chef und Ansible.

**Ansible** automatisiert administrative Aufgaben. Die Konfiguration wird üblicherweise in YAML-Dateien beschrieben. Ansible verbindet sich meist per SSH mit Linux-Systemen; ein dauerhaft installierter Agent auf jedem verwalteten Host ist dafür nicht erforderlich.

### Ansible-Grundmodell

* **Control Node:** Rechner, auf dem Ansible installiert ist und von dem aus die Automatisierung gestartet wird.
* **Managed Node:** Rechner, dessen Zustand Ansible verwaltet.
* **Inventory:** Liste der Managed Nodes und ihrer Gruppen.
* **Playbook:** YAML-Datei, die beschreibt, für welche Hosts welche Aufgaben gelten.
* **Modul:** Ansible-Baustein für eine konkrete Aufgabe, zum Beispiel Pakete installieren oder Dateien kopieren.

Für viele Module auf Linux benötigen Managed Nodes Python. Die genaue Unterstützung hängt von der verwendeten Ansible-Version und dem Zielsystem ab. Für Linux wird meist SSH verwendet; für Windows kommen andere Verbindungsarten zum Einsatz.

***

## 3. Inventory: Welche Systeme werden verwaltet?

Ein Inventory ordnet Hosts Gruppen zu. So kann ein Playbook beispielsweise nur auf Webserver oder nur auf Datenbankserver angewendet werden.

`inventory.ini`:

```ini
[web]
web01.example.test
web02.example.test

[database]
db01.example.test
```

Für eine lokale Demo kann das Inventory so aussehen:

```ini
[local]
localhost ansible_connection=local
```

Ein Inventory kann auch Variablen enthalten, zum Beispiel einen abweichenden SSH-Benutzernamen:

```ini
[web]
web01.example.test ansible_user=ubuntu
```

**Frage an die Runde:** Warum ist eine Gruppe wie `[web]` hilfreicher als eine Liste von IP-Adressen direkt im Playbook?

***

## 4. Erstes Playbook

Ein Playbook beschreibt, **welcher Zustand** erreicht werden soll. Es besteht aus einem oder mehreren Plays. Ein Play verbindet eine Hostgruppe mit Aufgaben, den **Tasks**.

`web.yml`:

```yaml
---
- name: Webserver vorbereiten
  hosts: web
  become: true
  vars:
    web_package: nginx
  tasks:
    - name: Webserver-Paket installieren
      ansible.builtin.package:
        name: "{{ web_package }}"
        state: present
```

Was bedeutet das?

* `hosts: web` wählt die Hosts aus der Inventory-Gruppe `web`.
* `become: true` lässt die nötigen Aufgaben mit erhöhten Rechten laufen. Der ausführende Benutzer muss dafür passend eingerichtet sein.
* `vars` definiert eine wiederverwendbare Variable.
* `ansible.builtin.package` ist ein eingebautes Modul zur Paketverwaltung.
* `state: present` beschreibt den gewünschten Zustand: Das Paket soll installiert sein.

Ausführen:

```sh
ansible-playbook -i inventory.ini web.yml
```

Vor der Ausführung können Sie die Syntax prüfen:

```sh
ansible-playbook -i inventory.ini web.yml --syntax-check
```

### Deklarativ und imperativ

Ein imperativer Befehl beschreibt konkrete Schritte:

> Führe den Paketmanager aus und installiere Nginx.

Eine deklarative Beschreibung sagt:

> Nginx soll installiert sein.

Ansible prüft den aktuellen Zustand und führt die nötige Änderung aus, um den beschriebenen Zustand herzustellen.

### Idempotenz

Ein idempotenter Vorgang kann mehrfach ausgeführt werden, ohne den bereits erreichten Zielzustand immer weiter zu verändern. Im Beispiel wird Nginx beim ersten Lauf installiert. Bei einem weiteren Lauf ist es bereits vorhanden, daher muss Ansible es nicht erneut installieren.

Das macht wiederholte Ausführungen sicherer und erleichtert die Verwaltung eines einheitlichen Zustands. Nicht jeder beliebige Shell-Befehl ist automatisch idempotent – deshalb sollte man, wenn möglich, passende Ansible-Module verwenden.

> **Merksatz:** Beschreiben Sie den gewünschten Zustand möglichst mit einem Modul, statt nur einen Befehl auszuführen.

***

## 5. Variablen und Wiederverwendung

Variablen vermeiden, dass Werte an mehreren Stellen fest eingetragen werden müssen. Im Playbook oben könnte `web_package` zum Beispiel je nach Umgebung unterschiedlich gesetzt werden.

`group_vars/web.yml`:

```yaml
---
web_package: nginx
```

Inventory- und Variablendateien sind keine Vorlagen für beliebigen Text: Das Inventory verwendet seine eigene INI- oder YAML-Syntax. Jinja-Ausdrücke wie `{{ web_package }}` werden in passenden Ansible-Feldern verwendet, nicht zum Ersetzen von Hostnamen in einer INI-Datei.

Passwörter und andere Geheimnisse sollten nicht ungeschützt in ein Repository eingecheckt werden. Ansible Vault kann dabei helfen, vertrauliche Variablen verschlüsselt abzulegen. Zugangsdaten sollten außerdem möglichst über geeignete Schlüssel- oder Secret-Verwaltung bereitgestellt werden.

### Ansible Vault: Geheimnisse verschlüsselt ablegen

Eine normale Variablendatei ist lesbar. Das ist für Einstellungen wie einen Paketnamen in Ordnung, aber nicht für Passwörter, API-Tokens oder private Schlüssel. **Ansible Vault** verschlüsselt Dateien oder einzelne Variablen, sodass verschlüsselte Geheimnisse versioniert werden können, ohne im Repository im Klartext zu stehen.

Erstellen Sie eine verschlüsselte Datei:

```sh
mkdir -p group_vars/web
ansible-vault create group_vars/web/vault.yml
```

Ansible fragt nach einem Vault-Passwort. In der Datei können Sie zum Beispiel eine Übungsvariable im YAML-Format ablegen:

```yaml
---
vault_demo_token: "nur-ein-testwert"
```

Die Datei enthält nach dem Speichern verschlüsselten Text. Ansible lädt Variablen aus `group_vars/web/` automatisch für Hosts der Inventory-Gruppe `web`. Liegt die Vault-Datei an einem anderen Ort, kann sie im Playbook über `vars_files` eingebunden werden.

Führen Sie das Playbook aus und geben Sie das Vault-Passwort auf Nachfrage ein:

```sh
ansible-playbook -i inventory.ini web.yml --ask-vault-pass
```

Mit `ansible-vault edit group_vars/web/vault.yml` bearbeiten Sie den Inhalt, ohne die Datei dauerhaft im Klartext abzulegen. Ansible entschlüsselt die Datei während der Ausführung. Das Vault-Passwort selbst muss vertraulich bleiben und darf nicht im Repository oder in der Abgabe gespeichert werden. Für diese Vorlesungsdemo verwenden Sie ausschließlich einen Dummy-Wert, niemals ein echtes Passwort oder Token.

> **Wichtig:** Vault schützt Geheimnisse im Ruhezustand, nicht vor Personen, die sowohl die verschlüsselte Datei als auch das Vault-Passwort besitzen. Geben Sie Geheimnisse außerdem nicht mit `debug` aus.

***

## 6. Rollen: Aufgaben sinnvoll strukturieren

Wenn Playbooks und Aufgaben wachsen, hilft eine Aufteilung in **Rollen**. Eine Rolle bündelt zusammengehörige Tasks, Variablen, Dateien und Handler, zum Beispiel die Einrichtung eines Webservers.

Eine vereinfachte Struktur:

```text
provisioning/
├── inventory.ini
├── site.yml
└── roles/
    └── webserver/
        ├── defaults/
        │   └── main.yml
        ├── handlers/
        │   └── main.yml
        ├── tasks/
        │   └── main.yml
        └── templates/
```

`site.yml` verwendet die Rolle:

```yaml
---
- name: Webserver konfigurieren
  hosts: web
  become: true
  roles:
    - webserver
```

Die Konvention `tasks/main.yml` ist der Einstiegspunkt für die Tasks der Rolle. Rollen machen Konfigurationen übersichtlicher und können in mehreren Playbooks wiederverwendet werden.

***

## 7. Module statt beliebiger Shell-Befehle

Module sind Ansible-Bausteine für häufige Verwaltungsaufgaben. Sie können den Zustand eines Systems meist besser prüfen und herstellen als ein selbst formulierter Shell-Befehl.

**Datei mit einem Modul kopieren:**

```yaml
- name: Nginx-Konfiguration bereitstellen
  ansible.builtin.copy:
    src: files/site.conf
    dest: /etc/nginx/sites-available/site.conf
    owner: root
    group: root
    mode: "0644"
```

Die Quelle `src` liegt dabei im Kontext des Ansible-Projekts, die Zieldatei `dest` auf dem Managed Node.

**Wann ist ein Shell-Befehl sinnvoll?** Wenn kein passendes Modul existiert oder ein spezieller Befehl nötig ist. Shell-Befehle sollten gezielt eingesetzt werden: Ansible kann deren gewünschten Zustand nicht immer zuverlässig erkennen und sie sind leichter nicht-idempotent.

```yaml
- name: Anwendungsstatus anzeigen
  ansible.builtin.command: /usr/local/bin/my-app --version
  changed_when: false
```

Dieses Beispiel liest nur die Version aus und markiert den Task daher ausdrücklich nicht als zustandsändernd. Für Änderungen sollten Sie ein passendes Modul bevorzugen.

***

## 8. Handler: nur bei einer Änderung reagieren

Ein Handler wird nur ausgeführt, wenn ein Task ihn mit `notify` benachrichtigt. Das ist zum Beispiel sinnvoll, wenn ein Dienst nach einer geänderten Konfigurationsdatei neu geladen werden muss.

```yaml
---
- name: Webserver konfigurieren
  hosts: web
  become: true
  tasks:
    - name: Nginx-Konfiguration bereitstellen
      ansible.builtin.copy:
        src: files/site.conf
        dest: /etc/nginx/sites-available/site.conf
        owner: root
        group: root
        mode: "0644"
      notify: Nginx neu laden
  handlers:
    - name: Nginx neu laden
      ansible.builtin.service:
        name: nginx
        state: reloaded
```

Bleibt die Konfigurationsdatei unverändert, wird der Handler nicht benachrichtigt. So wird der Dienst nicht bei jedem Playbook-Lauf unnötig neu geladen.

***

## 9. Tags: gezielt Aufgaben auswählen

Tags können einzelnen Tasks zugewiesen werden. Damit lassen sich bestimmte Aufgaben gezielt ausführen, zum Beispiel nur die Konfiguration:

```yaml
- name: Nginx-Konfiguration prüfen
  ansible.builtin.command: nginx -t
  changed_when: false
  tags:
    - config
```

Ausführung:

```sh
ansible-playbook -i inventory.ini web.yml --tags config
```

Tags sind ein Auswahlmechanismus, ersetzen aber keine sinnvolle Playbook- und Rollenstruktur. Prüfen Sie beim Ausführen, welche Tasks tatsächlich ausgewählt werden.

***

## 10. Sicher und vorsichtig arbeiten

Ein Playbook kann reale Systeme verändern. Vor allem bei Firewall-, Benutzer- oder SSH-Konfigurationen können Fehler den Zugriff auf einen Server verhindern.

* Testen Sie Änderungen zunächst in einer VM oder einer anderen isolierten Umgebung.
* Stellen Sie sicher, dass ein funktionierender Administrationszugang vorhanden ist, bevor Sie SSH-Einstellungen ändern.
* Verwenden Sie nur die Rechte, die für die Aufgabe erforderlich sind.
* Prüfen Sie Syntax und Zielhosts vor der Ausführung.
* `--check` kann bei unterstützten Modulen einen Probelauf versuchen; nicht alle Aktionen und Module lassen sich dabei vollständig vorhersagen.

Beispiel:

```sh
ansible-playbook -i inventory.ini web.yml --check --diff
```

Ein Probelauf ist hilfreich, aber keine Garantie, dass die echte Ausführung genau gleich verläuft. Prüfen Sie daher auch die Ausgabe und arbeiten Sie für Experimente mit Testsystemen.

***

## Zusammenfassung

* Ansible verwaltet Systeme von einem Control Node aus, meist ohne dauerhaften Agenten auf Linux-Zielsystemen.
* Das Inventory benennt Hosts und Gruppen; Playbooks ordnen ihnen Aufgaben zu.
* Module beschreiben gewünschte Zustände und sind Shell-Befehlen für viele Verwaltungsaufgaben vorzuziehen.
* Idempotenz macht wiederholte Ausführungen berechenbarer.
* Variablen und Rollen helfen bei Wiederverwendung und Struktur.
* Handler reagieren auf Änderungen; Tags wählen bestimmte Tasks aus.
* Ansible-Automatisierung muss vor dem Einsatz auf wichtigen Systemen geprüft werden.

### Verständnisfragen

1. Was ist der Unterschied zwischen einem Inventory und einem Playbook?
2. Warum ist `state: present` in einem Paket-Task ein Beispiel für eine deklarative Beschreibung?
3. Was bedeutet Idempotenz, und warum ist sie beim wiederholten Ausführen eines Playbooks nützlich?
4. Wann würden Sie einen Handler statt eines normalen Tasks verwenden?
5. Warum sollte ein Playbook nicht unkritisch mit `shell`-Befehlen gefüllt werden?
