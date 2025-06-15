# Altdaten importieren

## Variante 1: Altdaten automatisch über OpenAlex importieren


Es ist möglich, ohne großen Aufwand Altdaten in OSIRIS zu importieren. Um OSIRIS auf den Import vorzubereiten, müssen jedoch erst ein paar Installationsschritte und Konfigurationen durchgeführt werden. Am besten haltet ihr euch dafür an die Anleitung zur  [Einrichtung der Workflows](#workflows). Ihr müsst dabei das generelle Setup sowie die Vorbereitung des Queue-Workflows abarbeiten (inklusive der Konfiguration), bis zu dem Punkt, an dem steht, dass ihr bereit für den Altdaten-Import seid. Dann kehrt hierher zurück, um mit dem Import der Altdaten fortzufahren.

Mit dem folgenden Script werden alle Altdaten aus OpenAlex direkt in eure Datenbank importiert. Warum das eine tolle Idee ist, lest ihr am besten [hier](https://openalex.org/about).
Das Script zum Import der Altdaten wird mit folgendem Aufruf gestartet:

```bash
python jobs/import_data.py
```

Wenn das Skript erfolgreich ausgeführt wurde, dann müssten jetzt DOIs erscheinen, die nacheinander in die Datenbank eingepflegt werden.
Je nachdem, wie viele Altdaten importiert werden, kann dieser Prozess eine ganze Weile dauern.


## Variante 2: Altdaten über eine Liste von DOIs importieren
Ihr könnt auch eine Liste von DOIs nutzen, um schnell viele Daten in OSIRIS zu importieren.
Dazu habe ich euch ein [Python-Skript](https://github.com/JKoblitz/osiris/blob/master/jobs/import_doi.py) vorbereitet, dass ihr ebenfalls im Ordner Jobs findet (erst ab dem 5. März 2024).

Ihr führt dazu folgende Schritte durch:


1. Ihr tragt alle DOIs, die ihr gern importieren wollt, in eine CSV-Datei ein, eine DOI pro Zeile. Am besten ohne URL, geht aber wohl auch mit. Ich habe euch auch schon eine [Beispieldatei](https://github.com/JKoblitz/osiris/blob/master/jobs/doi.csv) mit in den Ordner gepackt.
2. Falls eure Datei anders heißt oder woanders gespeichert ist, müsst ihr den Pfad in der `import_doi.py` anpassen.
3. Das wars eigentlich auch schon. Jetzt nur noch folgenden Befehl in der Kommandozeile ausführen und ab geht die Post:

```bash
python jobs/import_doi.py
```



!!! info "Anmerkung"

    Vorhandene Einträge, deren DOI bereits in der Datenbank hinterlegt ist, werden nicht noch einmal hinzugefügt. Dies kann auch nicht umgangen werden, denn es führt dazu, dass ein Skript, sollte es abgebrochen sein, ohne Änderungen noch mal ausgeführt werden kann.
    
    Standardmäßig wird auch mittels Levenshtein-Ähnlichkeit nach übereinstimmenden Titeln (mehr als 90% Übereinstimmung) gefiltert. Falls das nicht gewünscht ist, müsst ihr in der `import_doi.py` den Wert `ignoreDupl` auf `False` setzen.
