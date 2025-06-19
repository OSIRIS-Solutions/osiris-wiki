# Upgrade auf die neueste Version

Um auf die neueste Version von OSIRIS zu aktualisieren, folge bitte den folgenden Schritten.

## Schritt 1: Backup

Bevor du mit dem Upgrade beginnst, solltest du ein Backup deiner Datenbank und deiner Dateien erstellen. Dies ist wichtig, falls etwas schief geht und du auf die vorherige Version zurückgreifen musst. Das Backup sollte auf einem externen Speichergerät gespeichert werden, um sicherzustellen, dass es nicht verloren geht. Dazu benutzt du am das in MongoDB integrierte Backup-Tool wie folgt:


```bash
mongodump --db osiris
```
Dieser Befehl erstellt ein Backup der Datenbank "osiris" in einem Verzeichnis namens "dump" im aktuellen Arbeitsverzeichnis. Falls deine Datenbank anders heißt, ersetze "osiris" durch den entsprechenden Namen.

Du kannst das Backup auch in einem anderen Verzeichnis speichern, indem du den Parameter "--out" verwendest:

```bash
mongodump --db osiris --out /path/to/backup

```
Für ein Backup der Dateien kannst du einfach das gesamte OSIRIS-Verzeichnis kopieren und an einem sicheren Ort speichern. Der wichtigste Ordner ist der "uploads"-Ordner, in dem alle Dateien, die von Nutzenden hochgeladen wurden, gespeichert sind. Falls Nutzerfotos nicht in der Datenbank gespeichert werden, solltest du auch den `img/users`-Ordner sichern.


```bash
zip -r osiris_backup.zip /path/to/osiris

# oder einzelne Ordner
zip -r uploads_backup.zip /path/to/osiris/uploads
zip -r img_users_backup.zip /path/to/osiris/img/users
```

## Schritt 2: Herunterladen der neuesten Version von GitHub

Um die neueste Version von OSIRIS herunterzuladen, kannst du das [GitHub-Repository](https://github.com/OSIRIS-Solutions/osiris) verwenden. Navigiere dazu in das Verzeichnis, in dem OSIRIS installiert ist, und führe den folgenden Befehl aus:

```bash
git pull
```

Dieser Befehl lädt die neueste Version von OSIRIS herunter und aktualisiert dein lokales Repository. Falls es Konflikte gibt, musst du diese manuell lösen, bevor du fortfährst.

## Schritt 3: Aktualisieren der Abhängigkeiten mit Composer

Nachdem du die neueste Version von OSIRIS heruntergeladen hast, musst du die Abhängigkeiten aktualisieren. Dazu kannst du Composer verwenden:

```bash
composer install
```

Dieser Befehl lädt die neuesten Versionen der Abhängigkeiten herunter und installiert sie in deinem Projekt. Falls es Probleme gibt, kannst du den Befehl `composer update` verwenden, um die Abhängigkeiten zu aktualisieren.

## Schritt 4: Aktualisieren der Datenbank über die Web-Oberfläche

Nachdem du die Abhängigkeiten aktualisiert hast, kannst du die Datenbank auf die neueste Version migrieren. Dazu musst du die Web-Oberfläche von OSIRIS aufrufen. Dort solttest du jetzt folgende Nachricht sehen:


> Eine neue OSIRIS-Version wurde gefunden.
>
> OSIRIS wird automatisch aktualisiert und eingerichtet. Abhängig von der Version kann dies eine ganze Weile dauern, stelle also bitte sicher, dass du die Seite während des Prozesses nicht neu lädst oder schließt.
>
> Installed: 1.X.X | Latest: 1.Y.Y


Klicke auf den Button "OSIRIS aktualisieren", um den Migrationsprozess zu starten. Dieser Prozess kann je nach Größe der Datenbank und Anzahl der Datensätze einige Zeit in Anspruch nehmen. Bitte warte geduldig, bis der Prozess abgeschlossen ist. Du solltest eine Erfolgsmeldung sehen, wenn die Migration erfolgreich abgeschlossen wurde.

## Schritt 5: Abschluss

Nachdem die Migration abgeschlossen ist, solltest du die neueste Version von OSIRIS erfolgreich installiert haben. Du kannst jetzt die Web-Oberfläche von OSIRIS aufrufen und überprüfen, ob alles wie erwartet funktioniert. Falls es Probleme gibt, kannst du das Backup verwenden, um auf die vorherige Version zurückzugreifen.

## Falls etwas schief geht

Falls etwas schief geht und du auf die vorherige Version zurückgreifen musst, kannst du das Backup verwenden, das du im ersten Schritt erstellt hast. Du kannst die Datenbank wiederherstellen, indem du das Backup mit dem folgenden Befehl wiederherstellst:


```bash
mongorestore --db osiris /path/to/backup/dump/osiris
```

Falls du die Dateien wiederherstellen musst, kannst du das Backup entpacken und die Dateien in das OSIRIS-Verzeichnis kopieren. Stelle sicher, dass du den `uploads`-Ordner und den `img/users`-Ordner wiederherstellst, falls du sie gesichert hast.



