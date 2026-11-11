# Ansible-Hausaufgabe: Einen Ubuntu-Server konfigurieren

## Ziel

Erstellen Sie ein Ansible-Projekt, das einen Ubuntu-Server reproduzierbar konfiguriert. Die Hausaufgabe baut auf den [Schritt-für-Schritt-Übungen](uebungen.md) auf und erweitert diese um mehrere Rollen, eine Firewall, einen Cron-Job und eine überprüfbare SSH-Konfiguration.

Am Ende sollen Sie zeigen können, dass:

* das Playbook syntaktisch korrekt ist,
* die drei Rollen einzeln ausgeführt werden können,
* ein verschlüsselter Vault-Wert während eines Playbook-Laufs verfügbar ist,
* der gewünschte Systemzustand tatsächlich erreicht wurde und
* ein zweiter vollständiger Lauf keine unnötigen Änderungen vornimmt.

## Sicherheitshinweise – bitte vor Beginn lesen

* Arbeiten Sie ausschließlich in einer wegwerfbaren Ubuntu-VM, nicht auf Ihrem Host-System oder einem produktiven Server.
* Die Firewall und SSH-Konfiguration können den Netzwerkzugriff verändern. Behalten Sie die VirtualBox-Konsole als Rückweg verfügbar.
* Richten Sie einen SSH-Schlüsselzugang ein und testen Sie ihn **vor** dem Deaktivieren der Passwortanmeldung. Testen Sie den Schlüsselzugang nach der Änderung erneut.
* Der Cron-Job startet die VM täglich um 04:00 Uhr neu. Lassen Sie die VM nur laufen, wenn ein solcher Neustart unproblematisch ist.

## Rahmenbedingungen

* Zielsystem: [Ubuntu 20.04 LTS](https://releases.ubuntu.com/20.04/)
* Virtualisierung: [VirtualBox](https://www.virtualbox.org/)
* Ansible wird in der Ubuntu-VM ausgeführt.
* Verwenden Sie `localhost` mit einer lokalen Ansible-Verbindung. Dadurch wird kein zweiter Rechner benötigt.
* Die VM ist sowohl Control Node als auch Managed Node.

***

## Aufgabe 1: Projekt und Inventory anlegen

Erstellen Sie ein Projektverzeichnis mit mindestens folgender Struktur:

```text
ansible-server/
├── inventory.ini
├── requirements.yml
├── site.yml
├── README.txt
├── group_vars/
│   └── server/
│       └── vault.yml
└── roles/
    ├── firewall/
    │   └── tasks/
    │       └── main.yml
    ├── maintenance/
    │   └── tasks/
    │       └── main.yml
    └── sshd/
        ├── handlers/
        │   └── main.yml
        └── tasks/
            └── main.yml
```

Das Inventory soll `localhost` enthalten und eine lokale Verbindung verwenden:

```ini
[server]
localhost ansible_connection=local
```

Installieren Sie Ansible und die erforderlichen Projekt-Abhängigkeiten in der VM. Legen Sie die `requirements.yml` für das benötigte Firewall-Modul an und installieren Sie die Collection mit `ansible-galaxy collection install -r requirements.yml`.

Verwenden Sie für diese Aufgabe das Modul `community.general.ufw`. Der Inhalt von `requirements.yml` lautet:

```yaml
---
collections:
  - name: community.general
```

Installieren Sie die Collection vor der Syntaxprüfung:

```sh
ansible-galaxy collection install -r requirements.yml
```

***

## Aufgabe 2: Geheimnis mit Ansible Vault verwalten

Erstellen Sie eine verschlüsselte Variablendatei für die Gruppe `server`. Verwenden Sie ausschließlich einen unkritischen Dummy-Wert, zum Beispiel einen fiktiven Übungs-Token. Verwenden Sie niemals ein echtes Passwort, einen privaten Schlüssel oder ein echtes API-Token.

Für diese Hausaufgabe verwenden alle dasselbe Vault-Testpasswort:

```text
Ansible-Labor-2026
```

Geben Sie dieses Passwort ein, wenn `ansible-vault create` danach fragt, und bestätigen Sie es. Das Passwort ist absichtlich Teil der Aufgabenstellung und bietet daher **keinen Schutz für echte Geheimnisse**. Verwenden Sie es ausschließlich für den Dummy-Wert dieser Übung.

Erstellen Sie die Datei im Projektverzeichnis:

```sh
mkdir -p group_vars/server
ansible-vault create group_vars/server/vault.yml
```

Legen Sie darin eine Variable an:

```yaml
---
vault_demo_token: "nur-ein-testwert"
```

Da `vault.yml` unter `group_vars/server/` liegt und das Inventory eine Gruppe namens `server` enthält, lädt Ansible die Datei automatisch als Gruppenvariablen.

Fügen Sie den folgenden Task unter `tasks:` in das Playbook ein:

```yaml
- name: Vault-Variable prüfen, ohne sie auszugeben
  ansible.builtin.assert:
    that:
      - vault_demo_token is defined
      - vault_demo_token | length > 0
    success_msg: "Vault-Variable ist geladen."
    fail_msg: "Vault-Variable fehlt oder ist leer."
  tags:
    - vault-check
```

Speichern Sie weder eine unverschlüsselte Kopie noch eine zusätzliche Passwortdatei im Projekt. Die verschlüsselte Datei `vault.yml` darf in der Abgabe enthalten sein. Legen Sie sie vor der ersten Playbook-Ausführung an. Erstellen Sie das Playbook mit diesem Task in der nächsten Aufgabe.

***

## Aufgabe 3: Playbook und Rollen

Erstellen Sie `site.yml`. Es soll die Gruppe `server` verwalten und die drei Rollen in dieser Reihenfolge verwenden:

1. `firewall`
2. `maintenance`
3. `sshd`

Jede Rolle muss über einen eigenen Tag einzeln ausführbar sein: `firewall`, `maintenance` beziehungsweise `sshd`.

Das Playbook soll administrative Aufgaben mit erhöhten Rechten ausführen. Verwenden Sie Ansible-Module und keine einzelnen Shell-Skripte, um die geforderten Zustände herzustellen.

***

## Aufgabe 4: Rolle `firewall`

Erstellen Sie eine Rolle, die UFW installiert, konfiguriert und aktiviert.

Anforderungen:

* Eingehender Netzwerkverkehr ist standardmäßig gesperrt.
* Ausgehender Netzwerkverkehr ist standardmäßig erlaubt.
* Die folgenden eingehenden TCP-Ports sind erlaubt:

| Port | Dienst |
|---:|---|
| 22 | SSH |
| 80 | HTTP |
| 443 | HTTPS |
| 110 | POP3 |
| 587 | SMTP-Submission |

* Aktivieren Sie UFW erst, nachdem die benötigte SSH-Regel gesetzt wurde.
* Andere eingehende Ports dürfen nicht zusätzlich freigegeben werden.

***

## Aufgabe 5: Rolle `maintenance`

Erstellen Sie einen Cron-Job für `root`, der die VM täglich um 04:00 Uhr neu startet.

* Verwenden Sie das Ansible-Cron-Modul.
* Geben Sie dem Eintrag einen eindeutigen Namen, damit Ansible ihn bei weiteren Läufen erkennen und aktualisieren kann.
* Verwenden Sie den Neustart-Befehl des Ubuntu-Systems.

Der Job wird nicht durch Abwarten bis 04:00 Uhr getestet. Prüfen Sie stattdessen den tatsächlich installierten Cron-Eintrag wie im Abschnitt **Prüfung** beschrieben.

***

## Aufgabe 6: Rolle `sshd`

Erstellen Sie eine Rolle, die sicherstellt, dass:

* der SSH-Server installiert ist,
* die Anmeldung mit öffentlichem Schlüssel erlaubt ist,
* die Anmeldung mit Passwort deaktiviert ist und
* der SSH-Dienst nach einer tatsächlichen Konfigurationsänderung neu gestartet wird.

Verwenden Sie einen Handler für den Neustart. Validieren Sie die SSH-Konfiguration, bevor sie übernommen wird, zum Beispiel mit `sshd -t`.

**Wichtig:** Installieren und testen Sie zuerst Ihren öffentlichen Schlüsselzugang. Prüfen Sie nach dem Playbook-Lauf die effektive SSH-Konfiguration und testen Sie eine neue SSH-Verbindung mit dem Schlüssel. Schließen Sie eine bereits bestehende SSH-Sitzung nicht, bevor der neue Zugang funktioniert.

Falls noch kein SSH-Server installiert ist, richten Sie den Schlüsselzugang vor der Ausführung der Rolle ein:

```sh
sudo apt install openssh-server
ssh-keygen -t ed25519 -f ~/.ssh/ansible_lab
mkdir -p ~/.ssh
cat ~/.ssh/ansible_lab.pub >> ~/.ssh/authorized_keys
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
ssh -i ~/.ssh/ansible_lab "$USER"@localhost
```

Bestätigen Sie den Host-Key beim ersten Verbindungsaufbau. Verwenden Sie niemals den privaten Schlüssel in der `authorized_keys`-Datei und geben Sie den privaten Schlüssel nicht mit ab.

***

## Aufgabe 7: Rollen gezielt ausführen

Stellen Sie sicher, dass jede Rolle einzeln ausgeführt werden kann. Verwenden Sie dazu die Rollen-Tags aus `site.yml`.

Führen Sie vor der Änderung an der Firewall mindestens die folgenden Befehle aus:

```sh
ansible-playbook -i inventory.ini site.yml --syntax-check --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --list-hosts --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --list-tasks --tags firewall --ask-vault-pass
```

Führen Sie anschließend jede Rolle einzeln aus:

```sh
ansible-playbook -i inventory.ini site.yml --tags firewall --ask-become-pass --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --tags maintenance --ask-become-pass --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --tags sshd --ask-become-pass --ask-vault-pass
```

Führen Sie außerdem den Vault-Prüftask separat aus:

```sh
ansible-playbook -i inventory.ini site.yml --tags vault-check --ask-vault-pass
```

Der Befehl muss nach Eingabe des Vault-Passworts erfolgreich enden und die allgemeine Erfolgsmeldung des Assert-Tasks anzeigen. Er darf den Inhalt von `vault_demo_token` nicht ausgeben.

Prüfen Sie danach jeweils den Zustand anhand der folgenden Prüfschritte.

***

## Prüfung: erwartete Ergebnisse

Führen Sie die Befehle in der VM aus und dokumentieren Sie die Ergebnisse in `README.txt`. Beim Vergleich der Befehlsausgabe unterscheiden Sie zwischen den Tasks, die geändert wurden, und denen, die bereits korrekt eingerichtet waren.

### 1. Playbook-Syntax und Zielhost

```sh
ansible-playbook -i inventory.ini site.yml --syntax-check --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --list-hosts --ask-vault-pass
```

**Erwartet:** Die Syntaxprüfung endet erfolgreich. Die Hostliste enthält `localhost`.

### 2. Ansible Vault

Prüfen Sie, dass die Datei tatsächlich verschlüsselt ist:

```sh
head -n 1 group_vars/server/vault.yml
```

**Erwartet:** Die erste Zeile beginnt mit `$ANSIBLE_VAULT;`. Führen Sie anschließend den Prüftask aus:

```sh
ansible-playbook -i inventory.ini site.yml --tags vault-check --ask-vault-pass
```

**Erwartet:** Nach Eingabe des Vault-Passworts endet der Task erfolgreich und bestätigt nur, dass die Variable geladen wurde. Der Dummy-Wert selbst wird nicht ausgegeben.

### 3. Firewall

```sh
sudo ufw status verbose
```

**Erwartet:**

* `Status: active`
* Standardrichtlinie für eingehenden Verkehr: `deny`
* Standardrichtlinie für ausgehenden Verkehr: `allow`
* `22/tcp`, `80/tcp`, `443/tcp`, `110/tcp` und `587/tcp` sind eingehend erlaubt.
* Es gibt keine zusätzlichen eingehenden Freigaben.

Kontrollieren Sie die komplette Ausgabe. Eine einzelne passende Zeile reicht nicht als Nachweis, wenn daneben weitere Ports freigegeben sind.

### 4. Cron-Job

```sh
sudo crontab -l
```

**Erwartet:** Die Root-Crontab enthält genau einen von Ansible verwalteten Eintrag für den täglichen Neustart um 04:00 Uhr. Er muss der Form `0 4 * * * <Neustart-Befehl>` entsprechen. Es ist nicht erforderlich, den Neustart abzuwarten.

### 5. Effektive SSH-Einstellungen

```sh
sudo sshd -T | grep -E '^(pubkeyauthentication|passwordauthentication) '
```

**Erwartet:**

```text
pubkeyauthentication yes
passwordauthentication no
```

Prüfen Sie zusätzlich von einem zweiten Terminal aus, dass Sie sich mit dem konfigurierten Schlüssel bei der VM anmelden können. Ein erfolgreicher Konfigurationstest ersetzt diesen Verbindungstest nicht.

### 6. Idempotenz

Führen Sie das vollständige Playbook zweimal aus:

```sh
ansible-playbook -i inventory.ini site.yml --ask-become-pass --ask-vault-pass
ansible-playbook -i inventory.ini site.yml --ask-become-pass --ask-vault-pass
```

**Erwartet:** Der erste Lauf nimmt die notwendigen Änderungen vor. Beim zweiten Lauf ist `changed=0` zu erwarten. Falls ein Task beim zweiten Lauf weiterhin eine Änderung meldet, untersuchen und korrigieren Sie die Ursache.

***

## Abgabe

Geben Sie ein ZIP-Archiv mit dem vollständigen Projektverzeichnis ab. Es muss mindestens enthalten:

* Inventory, Playbook und Rollen mit allen benötigten Dateien,
* die verschlüsselte Datei `group_vars/server/vault.yml` mit einem Dummy-Wert,
* `requirements.yml` für die benötigte `community.general`-Collection,
* `README.txt` mit:
  * Installations- und Ausführungsbefehlen,
  * den Prüfbefehlen aus diesem Aufgabenblatt,
  * einer kurzen Ergebnisdokumentation der Prüfungen und
  * der verwendeten Ansible-Version.

Das gemeinsame Vault-Testpasswort steht oben in dieser Aufgabenstellung und muss nicht separat übermittelt werden. Legen Sie es trotzdem nicht zusätzlich in einer Passwortdatei in der Abgabe ab. Unverschlüsselte Kopien von `vault.yml` dürfen **nicht** enthalten sein.
Laden Sie die ZIP-Datei im Kurs unter der entsprechenden Abgabe hoch.

* Zu spät eingereichte Dateien werden nicht gewertet.
* Andere Abgaben sowie passwortgeschützte oder beschädigte ZIP-Dateien werden nicht gewertet.
* Abgaben per E-Mail werden nicht gewertet.
