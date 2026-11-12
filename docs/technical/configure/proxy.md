---
tags:
  - Konfiguration
  - Proxy
  - Netzwerk
---

# Ausgehenden Proxy konfigurieren

<!-- md:version 2.1.5 -->

Wenn euer OSIRIS-Server das Internet nur über einen Proxy erreichen darf, könnt ihr diesen optional in der `CONFIG.php` eintragen. Das betrifft Verbindungen **vom Server zu externen Diensten**. Die Proxy-Einstellungen im Browser der Nutzenden reichen dafür nicht aus.

!!! info "Verfügbarkeit und Geltungsbereich"
    Diese Konfiguration setzt eine OSIRIS-Version mit dem gemeinsamen HTTP-Client (`php/HttpClient.php`) voraus. Bei älteren Installationen ist zunächst ein Update auf einen Stand mit dieser Unterstützung erforderlich.

    Aktuell wird die Einstellung für den **Download der Länderliste** und die **Abfrage von Journal-Kennzahlen beim Anlegen eines Journals** verwendet. Andere externe Abfragen, insbesondere die Journalsuche und separate Kennzahlen-Aktualisierungen, sind damit noch nicht abgedeckt.

## Proxy in der CONFIG.php eintragen

Ergänzt im PHP-Bereich eurer `CONFIG.php` die Adresse und den Port eures Proxys:

```php
define('PROXY_URL', 'http://proxy.example.org:3128');
```

Ersetzt die Beispieladresse durch die Angaben eurer IT. Gebt das Protokoll des **Proxys** an: Ein HTTP-Proxy kann auch Verbindungen zu HTTPS-Zielen weiterleiten. Dafür wird ein CONNECT-Tunnel verwendet; die Zielverbindung bleibt TLS-verschlüsselt, sofern der Proxy keine TLS-Inspektion durchführt.

Falls der Proxy eine Anmeldung verlangt, ergänzt zusätzlich:

```php
define('PROXY_USERNAME', 'benutzername');
define('PROXY_PASSWORD', 'passwort');
```

Die Anmeldemethode wird mit dem Proxy ausgehandelt. Welche Methoden verfügbar sind, hängt von der auf dem Server installierten libcurl ab, beispielsweise Basic, Digest oder NTLM. Die Zugangsdaten werden nur angewendet, wenn auch `PROXY_URL` gesetzt ist. Schützt die `CONFIG.php` entsprechend und nehmt echte Zugangsdaten nicht in die Versionsverwaltung auf.

## Betrieb ohne explizite Proxy-Konfiguration

Alle drei Einstellungen sind optional. Ist `PROXY_URL` nicht gesetzt, `null` oder eine leere Zeichenkette, erzwingt OSIRIS keinen Proxy. Vorhandene Proxy-Umgebungsvariablen von libcurl wie `https_proxy`, `http_proxy`, `ALL_PROXY` und `NO_PROXY` gelten weiterhin. Sie müssen in der Umgebung des PHP-Dienstes verfügbar sein; Einstellungen einer interaktiven Shell werden nicht zwangsläufig übernommen.

Ein nicht leerer Wert für `PROXY_URL` hat bei den beiden unterstützten Vorgängen Vorrang vor den Proxy-Umgebungsvariablen und auch vor `NO_PROXY`. Ein leerer Wert deaktiviert also keinen bereits über die Umgebung gesetzten Proxy.

## Verbindungen und Zertifikate

Für die beiden Vorgänge benötigt der Server über den Proxy Zugriff auf folgende HTTPS-Ziele (Port 443):

| Vorgang | Ziel |
| --- | --- |
| Länderliste herunterladen | `raw.githubusercontent.com` |
| Journal-Kennzahlen beim Anlegen abfragen | `osiris-app.de` |

Die normale Länderauswahl liest aus der lokalen Datenbank. Eine Internetverbindung ist für den Import bzw. die Aktualisierung der Länder erforderlich.

Die PHP-Erweiterung **cURL** muss installiert und für den PHP-Dienst aktiviert sein. Die Zertifikatsprüfung bleibt eingeschaltet. Falls euer Proxy TLS-Inspektion verwendet, muss eure IT das zugehörige CA-Zertifikat im für PHP/cURL verwendeten Vertrauensspeicher hinterlegen. Ein eigener CA-Bundle-Pfad kann über `curl.cainfo` in der PHP-Konfiguration gesetzt werden. Die Zertifikatsprüfung sollte nicht abgeschaltet werden.

## Verhalten bei Fehlern

Der Verbindungsaufbau ist auf fünf Sekunden begrenzt. Die gesamte Anfrage darf höchstens zehn Sekunden dauern, beim Download der Länderliste höchstens 30 Sekunden. Weiterleitungen sind auf drei und auf HTTP(S) beschränkt.

- Scheitert die Kennzahlenabfrage oder liefert sie ungültiges JSON, wird das Journal ohne die externen Kennzahlen gespeichert. Bei einem solchen Fehler werden weitere ISSN-Abfragen für diesen Speichervorgang abgebrochen. Im PHP-Fehlerprotokoll erscheint eine allgemeine Meldung.
- Scheitert der Länderdownload oder liefert er ungültige Daten, wird ein Fehler angezeigt. Die vorhandenen Länder werden in diesem Fall nicht ersetzt.

## Einrichtung prüfen

Lasst eure IT die Zugriffe aus derselben PHP-Umgebung prüfen, in der OSIRIS läuft. Ein erfolgreicher Aufruf im Browser oder über die Server-Konsole allein bestätigt diese Konfiguration noch nicht. Bei einem Fehler helfen die Meldungen im PHP-Fehlerprotokoll bei der Unterscheidung zwischen Verbindungs-, Proxy-Anmelde-, HTTP- und Zertifikatsproblemen.

Testet anschließend die [Aktualisierung der Länderliste](../../admins/custom_data/countries.md) und das Anlegen eines Journals mit einer ISSN. Dass das Journal gespeichert wird, bestätigt allein noch keinen erfolgreichen externen Zugriff: Das Speichern ist auch bei einem Ausfall der Kennzahlenabfrage möglich.
