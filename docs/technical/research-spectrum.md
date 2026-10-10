---
tags:
  - Forschungsspektrum
  - OpenAlex
  - Zitationen
  - Cron-Job
---

# Forschungsspektrum aktualisieren

Das Skript `jobs/openalex_topics_citations.py` ergänzt bestehende Publikationen in OSIRIS um OpenAlex-Themen (Topics) und Zitationszahlen. Die Themen bilden die Grundlage für das Forschungsspektrum. Die Daten werden im Feld `openalex` der jeweiligen Publikation in der MongoDB-Sammlung `activities` gespeichert.

Berücksichtigt werden Aktivitäten mit `type = publication` und einem nicht leeren DOI im Feld `doi`. Das Skript importiert keine neuen Publikationen. Für den Import in die Warteschlange gibt es den separaten [Queue-Workflow](configure/workflows.md#der-queue-workflow).

## Voraussetzungen

Die folgenden Beispiele gelten für einen Linux-Server mit OSIRIS unter `/var/www/html`. Passe diesen Pfad an deine Installation an. Führe die Vorbereitung und den ersten Lauf unter demselben Benutzer aus, dessen Crontab später den Job startet.

Du benötigst:

- Python 3 mit Unterstützung für virtuelle Umgebungen und pip;
- die Python-Pakete `requests` und `pymongo`;
- Zugriff auf die OSIRIS-Datenbank und ausgehende HTTPS-Verbindungen zu `api.openalex.org`;
- eine aktuelle Fassung des Skripts, die Bearer-Authentifizierung verwendet und vorhandene Daten bei vorübergehenden Abruffehlern erhält.

Bei einer Docker-Installation müssen Python und die Pakete in der Umgebung vorhanden sein, in der du den Job ausführst. Das mitgelieferte Produktionsimage für die PHP-Anwendung enthält kein Python. Ein Job auf dem Host braucht eine vom Host erreichbare MongoDB-Adresse; der Compose-Hostname `mongo` ist dort normalerweise nicht auflösbar.

## Python-Umgebung vorbereiten

Lege eine virtuelle Umgebung an und installiere die benötigten Pakete:

```bash
cd /var/www/html
python3 -m venv .venv
/var/www/html/.venv/bin/python -m pip install requests pymongo
```

Falls bereits eine geeignete Python-Umgebung vorhanden ist, kannst du diese verwenden. Nutze dann ihren absoluten Interpreterpfad auch für den ersten Lauf und den Cron-Job. Eine Aktivierung der virtuellen Umgebung ist bei den hier gezeigten Befehlen nicht nötig.

## Verbindung und API-Key konfigurieren

Das Skript liest `config.ini` direkt aus dem Verzeichnis `jobs`, unabhängig vom aktuellen Arbeitsverzeichnis. Falls die Datei noch nicht existiert, kopiere die Vorlage:

```bash
cd /var/www/html/jobs
cp -n config.default.ini config.ini
```

Bearbeite die vorhandene Datei und passe die folgenden Werte an. Bestehende Einstellungen für andere Jobs bleiben erhalten:

```ini
[Database]
Connection = mongodb://localhost:27017
Database = osiris

[OpenAlex]
ApiKey = DEIN_OPENALEX_API_KEY
```

Verwende die tatsächliche Datenbankadresse und den Datenbanknamen deiner OSIRIS-Installation. Falls MongoDB eine Anmeldung benötigt, muss die Verbindungsadresse auch die erforderlichen Zugangsdaten und Optionen enthalten. Der ausführende Benutzer braucht Lese- und Schreibzugriff auf `activities` sowie Leserechte für `config.ini`.

Einen API-Key kannst du in den [OpenAlex-Einstellungen](https://openalex.org/settings/api) erhalten. Er ist für dieses Skript optional, für regelmäßige Abrufe aber empfehlenswert. Ohne Key lässt du `ApiKey` leer. Das Skript sendet einen eingetragenen Key als `Authorization: Bearer …`; siehe [OpenAlex-Authentifizierung](https://help.openalex.org/api/authentication/). Es liest den Key aus `config.ini`, nicht aus einer Umgebungsvariable. `Institution`, `StartYear` und `AdminMail` werden von diesem Skript nicht benötigt.

## Ersten Lauf ausführen

Starte den Job zunächst manuell, bevor du ihn automatisierst:

```bash
/var/www/html/.venv/bin/python -u /var/www/html/jobs/openalex_topics_citations.py
```

Der Lauf schreibt direkt in die Datenbank; eine Vorschau ohne Änderungen gibt es nicht. Fehlende OpenAlex-Daten werden ergänzt, vorhandene Daten erst dann erneuert, wenn ihr Abruf mehr als 30 Tage zurückliegt. Bereits gespeicherte Fehlerblöcke mit `status = error` werden unabhängig von ihrem Alter erneut abgefragt.

Beim ersten Lauf können viele Publikationen abgefragt werden. Zwischen regulären Abrufen wartet das Skript 0,2 Sekunden, dazu kommt die Antwortzeit der API. Bei 10.000 Abrufen entstehen allein dadurch mindestens etwa 33 Minuten Wartezeit. Warte auf die abschließende Zusammenfassung:

```text
Done. processed=1000 updated=850 skipped=150 not_found=20 errors=0 rate_limited=0
```

Die Zahlen sind ein Beispiel. Die Zähler bedeuten:

| Zähler | Bedeutung |
| --- | --- |
| `processed` | Geprüfte Publikationen aus der Datenbankabfrage. |
| `updated` | Geänderte OpenAlex-Blöcke, einschließlich gespeicherter „nicht gefunden“-Ergebnisse. |
| `skipped` | Übersprungene Publikationen, etwa wegen noch aktueller Daten. |
| `not_found` | DOIs, die OpenAlex mit HTTP 404 beantwortet hat. |
| `errors` | Fehlgeschlagene Abrufe, etwa HTTP-, Netzwerk- oder Antwortformatfehler. |
| `rate_limited` | Abrufe, die mit HTTP 429 wegen eines API-Limits abgewiesen wurden. |

Prüfe anschließend das Forschungsspektrum in OSIRIS unter `/spectrum`. Es basiert auf Publikationen mit zugeordneten OpenAlex-Themen; Publikationen ohne DOI oder ohne Themen tragen nicht dazu bei. Ein separates Neuberechnungs- oder Importskript ist dafür nicht erforderlich.

## Täglichen Cron-Job einrichten

Ein täglicher Start ist empfohlen, obwohl vorhandene Daten nur etwa monatlich erneuert werden. Neue Publikationen werden so zeitnah ergänzt und fehlgeschlagene Abrufe beim nächsten Lauf erneut versucht. Ein Start nur am Monatsersten kann nach einem kurzen Februar die Aktualisierung auslassen, weil noch keine 30 Tage vergangen sind.

Öffne die Crontab des Benutzers, der den manuellen Lauf erfolgreich ausgeführt hat:

```bash
EDITOR=nano crontab -e
```

Füge diese Zeile ein:

```cron
# Täglich um 03:15 Uhr: OpenAlex-Themen und Zitationszahlen aktualisieren
15 3 * * * /var/www/html/.venv/bin/python -u /var/www/html/jobs/openalex_topics_citations.py >> /var/www/html/jobs/openalex_topics_citations.log 2>&1
```

Die Zeit richtet sich nach der Zeitzone des Cron-Dienstes auf dem Server. Passe die Pfade an und stelle sicher, dass der Cron-Benutzer die Logdatei anlegen bzw. beschreiben darf. `-u` sorgt für zeitnahe Ausgaben; `>>` hängt Ausgaben an, `2>&1` nimmt auch Fehlermeldungen in das Log auf. Richte für den dauerhaften Betrieb eine Logrotation ein.

Mit `crontab -l` kontrollierst du den Eintrag. Nach dem nächsten geplanten Lauf kannst du die Ausgabe prüfen:

```bash
tail -n 50 /var/www/html/jobs/openalex_topics_citations.log
```

## Fehler und erneute Versuche

Bei HTTP- oder Netzwerkfehlern sowie ungültigen Antworten bleiben vorhandene OpenAlex-Daten und ihr Abrufdatum erhalten. Ohne vorhandene Daten wird kein Fehlerblock mit neuer 30-Tage-Frist angelegt. Diese Publikationen werden beim nächsten täglichen Lauf erneut abgefragt.

Bei HTTP 429 wartet das Skript fünf Sekunden und fährt mit der nächsten Publikation fort. Der abgewiesene Abruf wird beim nächsten Lauf erneut versucht. Ein HTTP 404 wird dagegen als `status = not_found` gespeichert und erst nach 30 Tagen erneut geprüft; dabei wird der bisherige OpenAlex-Block ersetzt.

Kontrolliere deshalb nicht nur, ob der Job gestartet wurde, sondern auch `errors` und `rate_limited` in der Zusammenfassung. Einzelne Abruffehler führen derzeit nicht zu einem von null verschiedenen Exit-Code. Bei wiederkehrenden HTTP 401/403 prüfst du den API-Key, bei HTTP 429 das API-Budget und bei Verbindungsfehlern den Netzwerkzugang. Eine Meldung zu fehlenden Python-Modulen deutet auf einen falschen Interpreter oder fehlende Pakete hin.
