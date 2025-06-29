# Zur Beta wechseln

Im Beta-Branch von OSIRIS befinden sich die neuesten Entwicklungen, die noch nicht in der stabilen Version enthalten sind. Dieser Branch wird regelmäßig aktualisiert und enthält neue Features, Bugfixes und Verbesserungen, sobald sie bereit sind. Die Nutzung des Beta-Branches ist ideal für Nutzer:innen, die die neuesten Funktionen ausprobieren möchten und bereit sind, mögliche Instabilitäten und eventuelle Bugs in Kauf zu nehmen. Sobald eine neue Version stabil ist, wird sie in den Haupt-Branch (main) übernommen. Das kann je nach Funktionsumfang und Testphase unterschiedlich lange dauern.

## 1.	In das Projektverzeichnis wechseln
Öffne ein Terminal (bzw. Bash) und gehe in den Ordner mit dem Repository. In Apache ist dies in der Regel `/var/www/html/`. Falls du das Repository an einem anderen Ort hast, passe den Pfad entsprechend an:

```bash
cd /var/www/html/
```


## 2.	Verfügbare Remote-Branches anzeigen (optional)
Falls du prüfen möchtest, ob beta existiert:

```bash
git fetch
git branch -r
```


## 3.	Zum Branch beta wechseln
Falls du den Branch noch nicht lokal hast:

```bash
git checkout -b beta origin/beta
```

Falls du ihn schon lokal hast:

```bash
git checkout beta
```


## 4.	Neuste Änderungen vom Remote holen
Um den aktuellen Stand von origin/beta in dein lokales beta zu holen:

```bash
git pull
```

## 5. Composer-Dependencies aktualisieren

Falls du Composer benutzt, um die Abhängigkeiten zu verwalten, führe den folgenden Befehl aus:

```bash
composer update
```

## 6. OSIRIS aktualisieren

Um OSIRIS zu aktualisieren, musst du nur die Webseite aufrufen und ggf. die Datenbankmigration durchführen. Dies geschieht in der Regel automatisch, wenn du die Seite im Browser öffnest. Dies geschieht nur, wenn die Beta bereits einen Versionssprung enthält.

