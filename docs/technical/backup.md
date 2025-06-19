# Backup

Um die Datenbank komplett zu speichern, benutzt du am das in MongoDB integrierte Backup-Tool wie folgt:

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

Alternativ kannst du auch nur die wichtigen Dateien sichern, das sind die Konfigurationsdateien und die von Nutzenden hochgeladenen Dateien. Letzte befinden sich im Verzeichnis `/uploads`. Du kannst diese Dateien einfach kopieren:

```bash
zip -r osiris_partial_backup.zip /path/to/osiris/CONFIG.php /path/to/osiris/uploads
```

Falls eure Nutzerbilder nicht in der Datenbank gespeichert werden (eine Frage der Einstellung), dann solltest du auch das Verzeichnis `/img/users` sichern.

