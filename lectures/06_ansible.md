<!--

author:   Andreas Heil

email:    andreas.heil@hs-heilbronn.de

version:  0.1

language: de

narrator: DE German Male

tags: devops, lecture, ansible, automatisierung
comment:  

-->


# DevOps - Ansible

<!-- data-type="none" -->
| Parameter | Kursinformationen |
| --- | --- |
| **Veranstaltung:** | `262062 DevOps`|
| **Semester** | `SEB4` |
| **Hochschule:** | `Hochschule Heilbronn` |
| **Inhalte:** | `Container ` |
| Startseite | [https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1](https://liascript.github.io/course/?https://raw.githubusercontent.com/aheil/devops/master/README.md#1) | 
| **Link auf den GitHub:** | [https://github.com/aheil/devops/blob/main/lectures/06_ansible.md](https://github.com/aheil/devops/blob/main/lectures/06_ansible.md) |
| **Autoren** | @author |

## Ziele und Kompetenzen

- Einen Weg der Automatisierung **kennen lernen** 
- Grundlagen von Ansible **kennen lernen**

## Automatisierung 

**Brainstorming**

- Was machen Sie eigentlich alles am Rechner von Hand...?

{{1}}
************************************

**Probleme manuell installierter Server**

* Schwer wartbar 
* Kosten- und zeitintensiv
* Setup des Servers ist nicht einfach (wenn überhaupt) reproduzierbar
* Oft voller Bugs und Fehler 

************************************

{{2}}
************************************

**Eine Alternative: Skripte**

- Alles was langweilig ist, oft wiederholt wird, mühsam zu tippen ist, kein Spass macht, kann man skripten... 

- Shell-Skripte (Unix)
- Batch, PowerShell unter Windows 
- Man kann auch jede beliebige Programmiersprache zur Automatisierung verwenden
- Eigentlich schon immer da...

************************************

{{3}}
************************************

**Vorteile von Skripten**

* Einfach zu verstehen 
* Straight-forward
* Einfach zu ändern
* Werden interpretiert

************************************

{{4}}
************************************

**Nachteile von Skripten**

* Werden interpretiert 
* Kompatibilität (z.B. bei unterschiedlichen Versionen und Shell-Varianten wie *bash* und *sh* 
* Nur lokale Ausführung 
* Keine Parallelität 
* Langsam (pro Kommando ein Prozess vgl. Vorlesung Betriebssysteme)

************************************

## Betrieb 

Das Ops in DevOps

Ist eigentlich allen klar was "Betrieb" oder "Betriebsumfeld" bedeutet?     

{{1}}
************************************

**Betrieb vs. Entwicklung**

* Betrieb 

  * Will eigentlich keine Änderungen
  * Strebt vorzugsweise Stabilität an 
  * Möglichst wenige Bereitschaftseinsätze (Samstags auf Sonntag um 03:00 am Morgen)

* Entwicklung 

  * Will permanent Änderungen (Programmieren macht Spaß)
  * Neue Features um Kunden glücklich zu machen
  * Agilität (das haben wir Ihnen jetzt ja lange genug beigebracht)
  * Andauernd neue Versionen von Framework XYZ, neue Sprachen, neue Tools etc.

************************************

{{2}}
************************************

**Ziele der Automatisierung** 

* Entwicklung 

  * Automatisiertes Testen 
  * Automatisiertes Kompilieren
  * Automatisiertes Paketieren  

* Was wollen wir speziell im Betriebsumfeld automatisiert skripten?
    * Automatisierte Installationen
    * Automatisierte Updates
    * Automatisierte Konfigurationen
    * Automatisierte Installationen 
    * Automatisierte Rückbauten 

************************************

## Konfigurationsmanagement

* Was ist eigentlich ein sehr mächtiges Werkzeug in der Entwicklung? 

  * Versionskontrolle  

* Dann ist das doch für den Betrieb auch keine schlechte Idee...

{{1}}
************************************

**Was wir am besten können**

* Wir schreiben wieder und wieder die gleichen Shell- und Batch-Skripte 
* Wir fassen die Skripte, ergänzen diese mit div. Programmen und bauen ein Framework 
* Wir generalisieren das Framework so weit, dass es andere verwenden können...

************************************

{{2}}
************************************

**Frameworks**

* Salt / SaltStack
* Puppet 
* Chef
* **Ansible** 

************************************

## Ansible

**Was ist Ansible **

* Framework zur Automatisierung administrativer Tätigkeiten
* Leicht verständlich 
* Deklarativ
* Idempotent 
* Modular

{{1}}
************************************

**Eigenschaften von Ansible** 

* Kein Master
* Keine Agenten 
* Konfiguration in YAML
* Einfach zu lernen (Hausaufgabe!)

************************************

{{2}}
************************************

Was wird benötigt 

* Ansible verwendet keinen zentralen Server
* Alle Aufgaben werden in *Playbooks* gespeichert 
* Control Node
    * Python (2.7/3.x) 
    * Ansible
* Nodes 
    * ssh-server
    * Python (2.7/3.x)

************************************

{{3}}
************************************

**Inventory**

* Liste von Servern auf die ein Playbook angewendet werden kann

```shell
[db_master]
193.161.1.14

[db_replica]
163.161.1.21
163.161.1.22
163.161.1.23

[web_01]
163.161.1.2
```

* Einfache Textdateien
* Beschreiben die Server 
* IP-Adressen *oder* DNS-Namen
* Werden mittels Namen gruppiert 


* Variablen und Enumerationen
* Bei *vielen* Systemen sinnvoll

```shell
[db_master]
db-[a:c]-srv

[db_replica]
db[1:3]-srv

[web_01]
{{web_srv}}
```
************************************

{{4}}
************************************

**Playbook**

* Beschreibt den _Zielzustand_ des Nodes 
* Deklarativ (Unterschied zu imperativ klar?)
* Nutzt YAML (YAML bekannt?)

```shell
-hosts: dev_server  # Gruppenname aus Host-Datei
  user: ubuntu      # Authetifizierung
  sudo: true
  roles
    - java          # Rollen, die installiert werden sollen
    - springboot    
```

> **HINWEIS**: Einen großen Teil Ihrer zukünftigen Karriere in der IT werden Sie damit verbringen, nach fehlenden oder überflüssigen Leerzeichen und nach falschen Einrückungen in YAML-Dateien zu suchen. You are welcome.

************************************

{{5}}
************************************

**Variablen**

* Ermöglichen die Wiederverwendung von Playbooks 
* Verschlüsselung (z.B. für Passwörter)
* Können für Tasks, Kommandozeile, Dateien etc. verwendet werden

_provisioning/vars/main.yml_:

```shell
web_srv
```

_provisioning/hosts_:

```shell
[web_01]
{{web_srv}}
```

************************************

{{6}}
************************************

**Rollen-Verzeichnisse** 

```plain
└── provisioning/ 
    ├── .gitignore
    ├── dev-playbook.yml
    ├── prod-playbook.yml
    ├── group_vars/
    │   └── all.yml
    └── roles/
        ├── java/
        │   ├── defaults/ # Variablen
        │   ├── files/   
        │   ├── handlers/
        │   └── tasks/
        ├── ...
        └── springboot/
            ├── ...
            └── ...
```

************************************

{{7}}
************************************

**Module**

 Quasi für "Alles" Module verfügbar 

```shell
- name: Copy the required files
  copy:
    src: files/
    dest: "{{config_dir}}"
    force: yes
  tags: 
    - config
```

************************************

{{8}}
************************************

Shell-Module

Für alles, für das es keine Modul gibt 🤓

```shell
- name: Magic happens hier 
  shell: /opt/hardening.sh
  tags: 
    - shell
```

> **Hinweis**: Datei vorher auf Server kopieren, Executable Flag setzen etc.  

************************************

{{9}}
************************************

**Handlers** 

_provisioning/roles/db/handlers/main.yml:_

```
---
- name: restart db
  service: name=db =state=refresh
```

```
- name: Copy DB config
  copy: src=db.conf dest/etc/db/db.conf
  notify: restart db
```
************************************

{{10}}
************************************

**Spezifische Module ausführen**

Spezifische Module ausführen

```shell
sudo ansible-playbook -i hosts playbook.yml --tags hardening
```

Hinweis: 

* Es werden nur Rollen in Betracht gezogen, die im Playbook aufgeführt sind 
* Mehrere Tags pro Eintrag möglich


************************************