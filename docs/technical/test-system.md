# Testsystem einrichten

Hier beschreibe ich eine Möglichkeit, ein Testsystem für OSIRIS **auf dem gleichen Server** wie das Produktivsystem einzurichten. Das ist besonders nützlich, um neue Funktionen zu testen, ohne das Produktivsystem zu beeinträchtigen.

## Voraussetzungen

- Ein Server mit installiertem OSIRIS
- Zugriff auf die Serverkonsole

## Schritte

Hier sind die Schritte, um ein Testsystem für OSIRIS einzurichten.

### 1. Klonen des Produktivsystems

Zunächst musst du eine Kopie des Produktivsystems erstellen. Dies kann durch einfaches Kopieren der Verzeichnisse und Dateien erfolgen.

```bash
cp -r /path/to/production /path/to/production/test
```

### 2. Datenbank kopieren (optional)

Falls die Datenbank für das Testsystem am Anfang die gleichen Daten wie das Produktivsystem haben soll, kannst du ein Backup der Datenbank erstellen und diese in eine neue Testdatenbank importieren. Dies ist optional, aber empfohlen, um realistische Tests durchzuführen.

Hier muss der Pfad zum Backup und ggf. der Name der Datenbank angepasst werden:

```bash
mongodump --db osiris --out /path/to/backup
mongorestore --db osiris_test /path/to/backup/osiris
```

Das könnte beispielsweise so aussehen, wenn du das Backup in deinem Home-Verzeichnis speichern möchtest:

```bash
mongodump --db osiris --out ~/backup
mongorestore --db osiris_test ~/backup/osiris
```

### 3. .htaccess-Dateien anpassen

Zuerst musst du sicherstellen, dass der neue Testordner beim Rewrite von Apache ausgelassen wird. Ansonsten wird OSIRIS deine Testseite nicht korrekt laden sondern nur einen `404`-Fehler anzeigen.

Öffne die `.htaccess`-Datei im Hauptverzeichnis des **Produktivsystems**:

```bash
nano /path/to/production/.htaccess
```

Hier musst du die `RewriteRule`-Direktive anpassen, um das Testsystem auszuschließen. Füge eine neue Zeile hinzu, die den Testordner ausschließt (neu ist nur `|test`):

```apache
RewriteRule ^(css|img|js|uploads|settings.json|manifest.json|test)($|/) - [L]
```

Als nächstes müssen wir sicherstellen, dass das Testsystem korrekt funktioniert. Dazu müssen wir die `.htaccess`-Datei im Testverzeichnis anpassen. Öffne die `.htaccess`-Datei im Testverzeichnis:

```bash
nano /path/to/production/test/.htaccess
```

Ändere die `RewriteBase`-Direktive, um auf das Testsystem zu verweisen.

```apache
RewriteBase /test
```

### 4. Konfiguration anpassen

Den Pfad zum Testsystem musst du außerdem in der Konfigurationsdatei anpassen. Öffne die Konfigurationsdatei des Testsystems:

```bash
nano /path/to/production/test/CONFIG.php
```

Hier musst du die `ROOTPATH`-Konstante anpassen, damit sie auf das Testsystem verweist:

```php
define('ROOTPATH', '/test');
```

Vergiss nicht, auch die `DB_NAME`-Konstante anzupassen, damit sie auf die Testdatenbank verweist:

```php
define('DB_NAME', 'osiris_test');
```

Ich empfehle außerdem auf dem Testsystem die Konstante `LIVE` auf `false` zu setzen. Dadurch erschreint oben direkt unter dem Logo ein Hinweis, dass es sich um das Testsystem handelt. Das hilft, Verwechslungen zu vermeiden.

```php
define('LIVE', false);
```


### 5. Das Testsystem testen

Jetzt solltest du in der Lage sein, auf das Testsystem zuzugreifen, indem du die URL deines Servers gefolgt von `/test` aufrufst:

```
http://<your-osiris-server>/test
```

Wenn alles korrekt eingerichtet ist, solltest du die Testversion von OSIRIS sehen, die unabhängig von deinem Produktivsystem funktioniert. Dies kannst du testen, indem du eine neue Aktivität hinzufügst und überprüfst, ob sie nur im Testsystem erscheint und nicht im Produktivsystem.

## Sicherheitshinweise

Da das Testsystem auf dem gleichen Server wie das Produktivsystem läuft, teilen sie sich die sogenannte *Session*. Das bedeutet, dass du dich im Produktivsystem anmelden kann und dann auch im Testsystem angemeldet bleibst und umgekehrt. Das kann praktisch sein, vor allem, um den Testprozess zu beschleunigen, aber es kann auch zu Verwirrung führen, wenn du nicht aufpasst. 


Wenn du nicht möchtest, dass alle Nutzenden des Produktivsystems auch Zugriff auf das Testsystem haben, kannst du die `.htaccess`-Datei im Testverzeichnis so anpassen, dass für den Zugriff ein Passwort erforderlich ist. Hier ist ein einfaches Beispiel, wie du das machen kannst:

```apache
AuthType Basic
AuthName "Restricted Area"
AuthUserFile /path/to/.htpasswd
Require valid-user
```

Du musst dann eine `.htpasswd`-Datei erstellen, die die Benutzernamen und Passwörter enthält. Das kannst du mit dem `htpasswd`-Befehl tun:

```bash
htpasswd -c /path/to/.htpasswd username
```
Du wirst dann aufgefordert, ein Passwort für den Benutzer festzulegen. Wenn du mehrere Benutzer hinzufügen möchtest, kannst du den `-c`-Parameter weglassen, um die Datei nicht zu überschreiben:

```bash
htpasswd /path/to/.htpasswd anotheruser
```