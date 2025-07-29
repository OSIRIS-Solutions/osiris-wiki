---
tags:
  - Migration
  - MongoDB
  - Backup
---


# OSIRIS auf einen anderen Server migrieren

OSIRIS lässt sich sehr einfach auf einen anderen Server migrieren. Dazu musst du lediglich die Datenbank und die Dateien sichern und auf dem neuen Server wiederherstellen. Hier sind die Schritte, die du befolgen musst:

## Schritt 1: Backup

Dazu benutzt du am das in MongoDB integrierte Backup-Tool wie folgt:

```bash
mongodump --db osiris
```

Dieser Befehl erstellt ein Backup der Datenbank "osiris" in einem Verzeichnis namens "dump" im aktuellen Arbeitsverzeichnis. Falls deine Datenbank anders heißt, ersetze "osiris" durch den entsprechenden Namen.

Du kannst das Backup auch in einem anderen Verzeichnis speichern, indem du den Parameter "--out" verwendest:

```bash
mongodump --db osiris --out /path/to/backup
```

Da OSIRIS nicht wirklich "installiert" wird, sondern nur in ein Verzeichnis kopiert wird, kannst du auch einfach das gesamte OSIRIS-Verzeichnis kopieren und auf den neuen Server migrieren.


```bash
zip -r osiris_backup.zip /path/to/osiris
```


## Schritt 2: OSIRIS auf dem neuen Server installieren

Folge dazu der Anleitung zur Installation von OSIRIS. Kopiere danach alle Backup-Dateien in das entsprechende Verzeichnis auf dem neuen Server.

## Schritt 3: Datenbank und Dateien wiederherstellen
Zu guter Letzt kannst du die Datenbank mit folgendem Befehl wiederherstellen:

```bash
mongorestore --db osiris /path/to/backup/dump/osiris
```

Danach unbedingt neu rendern, damit die Links zu den Aktivitäten auch funktionieren. Dazu navigiere in den Admin-Bereich und klicke auf "Neu rendern" oder navigiere auf die URL `/rerender`.


