---
tags:
  - Workflows
  - Automatisierung
  - OpenAlex
  - Cron-Job
---


# Optionale Workflows

Workflows laufen regelmäßig und ermöglichen es, Arbeitsschritte zu automatisieren.


## Generelles Setup

Für alle aktuellen (und vermutlich auch zukünftigen) Workflows wird Python 3 benötigt. Wie du Python am besten installierst, schaust du am besten [in der offiziellen Dokumentation](https://wiki.python.org/moin/BeginnersGuide/Download) nach. Du wirst ebenfalls den Python-Paketinstallationsmanager pip benötigen.

Sobald Python 3 und pip installiert sind, kannst du mit folgendem Kommandozeilenbefehl den MongoDB-Connector installieren, der ebenfalls für alle Jobs benötigt wird:


```bash
pip install pymongo
```

Außerdem musst du die Konfigurationsdatei `config.default.ini` im `jobs`-Unterordner kopieren und die Kopie `config.ini` nennen (Dieses Vorgehen verhindert, dass deine Konfiguration durch OSIRIS-Updates überschrieben wird). In der Config-Datei müssen alle Variablen entsprechend angepasst werden, zum Beispiel die Informationen für die Datenbankverbindung.


## Der Queue-Workflow


Der Queue-Workflow holt neue Aktivitäten aus Online-Quellen und speichert sie in einer Warteschlange. Die Benutzer werden benachrichtigt, wenn neue Aktivitäten in der Warteschlange warten, und können sie einfach zu OSIRIS hinzufügen.


### Vorbereitung

Zur Vorbereitung dieses Workflows müssen zunächst drei weitere Python-Pakete installiert werden. Dies geschieht erneut mit pip:

```bash
pip install diophila
pip install nameparser
pip install levenshtein
```

Zusätzlich musst du die OpenAlex-ID Ihres Instituts in der Datei `config.ini` ändern. Um diese ID zu finden, suchst du dein Institut am besten auf der [Webseite von OpenAlex](https://explore.openalex.org/). Die ID findest du entweder als letztem Teil der URL oder unter Identifiers &#8594
 openalex. Sie beginnt mit einem I, gefolgt von einer Reihe von Zahlen. Sie muss in der `config.ini` eingetragen werden:

```ini
[OpenAlex]
Institution = I7935750
```

Außerdem von Relevanz ist das `StartYear`, von dem an Aktivitäten importiert werden, sowie die `AdminMail`, mit derer die Abfragen bei OpenAlex getätigt werden.


Falls du [Altdaten importieren](../data.md) willst, bist du jetzt bereit dafür.

### Den Cron-Job anlegen

Zu guter Letzt können wir den Cron-Job anlegen. Ein Cron-Job ist ein geplanter, sich wiederholender Vorgang, den ein System zu einer festgelegten Zeit durchführt. Die folgenden Einstellungen bedeuten, dass der Workflow einmal pro Woche durchgeführt wird, genauer gesagt jeden Sonntag um 2 Uhr morgens.
Wir benutzen den Editor `nano` für diese Aktion, ihr könnt aber natürlich auch jeden anderen Editor verwenden.



```bash
EDITOR=nano crontab -e 

# enter this as cronjob:
0 2 * * 0 python3 /var/www/html/jobs/openalex_parser.py

# press Ctrl+O to save and Ctrl+X to exit
```

Die Ausgabe sollte euch zeigen, dass ein neuer Cron-Job installiert wurde. Im folgenden werden jeden Sonntag neue Publikationen zu euer Warteliste hinzugefügt.

