---
title: LDAP
tags:
  - Nutzerverwaltung
  - LDAP
  - Authentifizierung
---

# Nutzerverwaltung mit LDAP

<!-- md:version 1.0.0 --> 
<!-- md:feature -->

OSIRIS unterstützt Microsoft Active Directory und OpenLDAP. Die Anmeldung wird über LDAP geprüft; Profile, Rollen und Rechte werden in OSIRIS verwaltet. Alle folgenden Einstellungen gehören in die `CONFIG.php`. Änderungen an `php/LDAPInterface.php` sind für die hier beschriebenen Konfigurationen nicht nötig.

## Active Directory oder OpenLDAP auswählen

`USER_MANAGEMENT` bleibt in beiden Fällen auf `LDAP`. Mit `OPEN_LDAP` wählt ihr die passenden Nutzerattribute:

| Einstellung | Anmeldung mit | Eindeutige Kennung |
| --- | --- | --- |
| `OPEN_LDAP = false` (Standard) | `samaccountname` | `objectguid` |
| `OPEN_LDAP = true` | `uid` | `entryuuid` |

`OPEN_LDAP` schaltet auch weitere Profilattribute um, beispielsweise von `description` auf `department` für die Organisationseinheit. Es ist daher keine frei konfigurierbare Auswahl eines einzelnen Login-Attributs.

### Beispiel: Active Directory

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'LDAP');
define('OPEN_LDAP', false);
define('ADMIN', 'musterfrau');

define('LDAP_IP', 'ldap.example.org');
define('LDAP_PORT', 389);
define('LDAP_USE_TLS', true);
define('LDAP_USER', 'osiris');
define('LDAP_PASSWORD', 'service_account_password');
define('LDAP_DOMAIN', '@example.org');
define('LDAP_BASEDN', 'DC=example,DC=org');
define('LDAP_BIND_METHOD', 'username');
```

`LDAP_USER` ist ein technischer Account zum Suchen und Lesen des Verzeichnisses. Er muss kein LDAP-Administrator sein. `ADMIN` ist dagegen der Nutzername der Person, die OSIRIS administriert. Diese beiden Accounts erfüllen unterschiedliche Aufgaben.

### Beispiel: OpenLDAP mit Nutzern in unterschiedlichen Teilbäumen

```php title="CONFIG.php"
define('USER_MANAGEMENT', 'LDAP');
define('OPEN_LDAP', true);
define('ADMIN', 'musterfrau'); // uid des OSIRIS-Administrators

define('LDAP_IP', 'ldap.example.org');
define('LDAP_PORT', 389);
define('LDAP_USE_TLS', true);
define('LDAP_USER', 'cn=osiris,ou=services,dc=example,dc=org');
define('LDAP_PASSWORD', 'service_account_password');
define('LDAP_DOMAIN', '');
define('LDAP_BASEDN', 'dc=example,dc=org');
define('LDAP_BIND_METHOD', 'dn');
```

OSIRIS sucht hier unterhalb von `LDAP_BASEDN` einschließlich aller Unterbäume nach `(uid=nutzername)`. Damit können beispielsweise `uid=musterfrau,ou=research,dc=example,dc=org` und `uid=mustermann,ou=administration,dc=example,dc=org` gefunden werden. Wählt eine gemeinsame Suchbasis, unter der alle benötigten Nutzer liegen. Die Nutzernamen müssen innerhalb dieser Suchbasis eindeutig sein.

Die Beispiele verwenden StartTLS auf Port 389. Falls euer Server LDAPS verwendet, setzt stattdessen `LDAP_USE_SSL` auf `true`, `LDAP_USE_TLS` auf `false` und den passenden Port, üblicherweise 636. Hostname und Zertifikatsvertrauen müssen zu eurem Server passen.

## Suchbasis, Bind-Methode und Domain

Die Anmeldung erfolgt in drei Schritten:

1. OSIRIS verbindet sich mit `LDAP_USER` und `LDAP_PASSWORD` für die Suche.
2. Es sucht den eingegebenen Nutzernamen unter `LDAP_BASEDN`, je nach `OPEN_LDAP` über `samaccountname` oder `uid`.
3. Es prüft das eingegebene Passwort durch einen zweiten Bind als gefundener Nutzer.

`LDAP_BIND_METHOD` bestimmt die Kennung für diesen zweiten Bind:

| Wert | Kennung |
| --- | --- |
| `username` (Standard) | Eingegebener Nutzername |
| `dn` | Vollständiger DN aus dem Suchergebnis; geeignet für unterschiedliche Teilbäume |
| `userPrincipalName` | Attribut `userPrincipalName` aus dem Suchergebnis; für Active Directory |

`LDAP_DOMAIN` ist **nicht die Suchbasis**. Die Bind-Funktion verwendet sie für beide Bind-Vorgänge nach folgenden Regeln:

| Konfiguration bzw. Kennung | Verhalten |
| --- | --- |
| Kennung beginnt exakt mit `cn=` | Wird unverändert verwendet |
| `LDAP_DOMAIN` enthält `%s` | Kennung wird in die Vorlage eingesetzt, z. B. `uid=%s,ou=people,dc=example,dc=org` |
| Andernfalls | `LDAP_DOMAIN` wird an die Kennung angehängt, z. B. `musterfrau@example.org` |
| `LDAP_DOMAIN` ist leer | Kennung bleibt unverändert |

!!! warning "Aktuelle Einschränkung bei vollständigen DNs"
    Die Bind-Funktion erkennt derzeit nur das kleingeschriebene Präfix `cn=` automatisch als vollständigen DN. Bei einem gefundenen DN mit `uid=` oder `CN=` kann eine nicht leere `LDAP_DOMAIN` deshalb auch bei `LDAP_BIND_METHOD = 'dn'` angehängt bzw. als Vorlage angewendet werden. Für das OpenLDAP-Beispiel oben muss `LDAP_DOMAIN` daher leer bleiben und `LDAP_USER` als vollständiger Service-DN angegeben werden. Die Konstante muss trotzdem definiert sein.

`OPEN_LDAP = true` stellt also die Attribute um, wählt aber nicht automatisch die DN-basierte Anmeldung. Für einen verzweigten OpenLDAP-Baum sind zusätzlich `LDAP_BIND_METHOD = 'dn'` und die passende Domain-Einstellung erforderlich.

## Admin-Rechte und weitere Rollen

LDAP prüft die Zugangsdaten. **OSIRIS-Rollen werden nicht automatisch aus LDAP-Gruppen oder LDAP-Rollenattributen übernommen.** Neue LDAP-Profile erhalten zunächst keine zusätzlichen Rollen außer der allgemeinen Nutzerrolle.

Den ersten Administrator legt ihr in `CONFIG.php` fest:

```php title="CONFIG.php"
define('ADMIN', 'musterfrau');
```

Wenn `musterfrau` der in OSIRIS geführte Nutzername des LDAP-Accounts ist (`uid` bzw. `samaccountname`), erhält diese Person nach erfolgreicher Anmeldung automatisch die Rolle `admin`. Achtet auf die exakte Schreibweise einschließlich Groß- und Kleinschreibung. `ADMIN` erzeugt keinen lokalen Account und ersetzt keine Passwortprüfung.

Weitere Rollen und Admin-Rechte vergebt ihr in OSIRIS über **Rollen & Rechte** oder im Nutzerprofil unter **Account**. Sie bleiben in OSIRIS gespeichert und gelten auch bei Anmeldung über LDAP. Siehe [Rollen verteilen](../../admins/user_roles/distribute_roles.md).

## Lokale Gast-Accounts zusätzlich zu LDAP

<!-- md:version 1.6.2 -->

Auch mit `USER_MANAGEMENT = 'LDAP'` könnt ihr lokale Accounts ohne LDAP-Eintrag verwenden. Öffnet im Admin-Bereich **Gast-Accounts** (`/admin/guest-account`) und legt über **Gast-Account hinzufügen** einen Account mit Nutzername, lokalem Passwort und Profilangaben an. Optional könnt ihr ein Gültigkeitsende setzen; ohne Datum ist der Account unbefristet.

Die Anmeldung nutzt dasselbe Loginformular:

1. OSIRIS versucht die Anmeldung über LDAP.
2. Liefert dieser Versuch eine fehlgeschlagene Anmeldung zurück, prüft OSIRIS den lokalen Gast-Account und dessen Passwort sowie ein gegebenenfalls gesetztes Gültigkeitsende.

Das ist die vorhandene Login-Kaskade **LDAP → Gast-Account**. Eine zusätzliche Kaskade zum `AUTH`-Addon gibt es nicht; ihr müsst dafür nicht die Nutzerverwaltung umstellen. Verwendet für Gast-Accounts eigene Nutzernamen, die nicht bereits im LDAP vergeben sind.

Auch für diese Accounts werden Rechte in OSIRIS vergeben. Für einen lokalen Admin könnt ihr dem angelegten Gast-Account zusätzlich die Rolle `admin` geben oder seinen Nutzernamen als `ADMIN` konfigurieren. Die Bezeichnung „Gast-Account“ beschränkt die zusätzlich zuweisbaren Rollen nicht. Diese Login-Accounts sind von der [Verwaltung von Gastaufenthalten](../../users/people/guests.md) zu unterscheiden.

!!! note "Kein garantierter Notzugang bei LDAP-Ausfall"
    Die Gastprüfung folgt auf einen erfolglosen LDAP-Login. Wirft bereits der LDAP-Verbindungsaufbau oder StartTLS eine Exception, erreicht der aktuelle Ablauf diese Prüfung nicht. Ein Gast-Account ist deshalb kein garantierter Notzugang bei einem LDAP-Ausfall.

## Filtern von Nutzer:innen


<!-- md:version 1.3.6 -->

Ihr könnt auch festlegen, welche Nutzer:innen aus dem LDAP-Server synchronisiert werden sollen. Dazu könnt ihr den `LDAP_FILTER`-Parameter in der `CONFIG.php` setzen. Zum Beispiel:

```php title="CONFIG.php"
define("LDAP_FILTER", "(objectClass=person)");
```

**Der Filter gilt für das Abrufen und Synchronisieren der Nutzerlisten, nicht für die einzelne Nutzersuche beim Login.** Er ist daher keine Zugriffsbeschränkung für die Anmeldung. Ein LDAP-Attribut wie `role=employee` im Filter weist außerdem keine OSIRIS-Rolle zu.

Dieser Filter sorgt dafür, dass nur Nutzer:innen mit der Objektklasse `person` synchronisiert werden. Ihr könnt den Filter anpassen, um andere Kriterien zu verwenden, die für eure Nutzer:innen zutreffen.

Hier sind ein paar Beispiele für häufig verwendete Filter:

```php title="CONFIG.php"
// Nur Nutzer:innen mit der Rolle "Mitarbeiter"
define("LDAP_FILTER", "(role=employee)");
// Nur Nutzer:innen, die in der Abteilung "Forschung" arbeiten
define("LDAP_FILTER", "(department=Forschung)");
// Nur Nutzer:innen, die in der OU "Mitarbeitende" sind
define("LDAP_FILTER", "(ou=Mitarbeitende)");
// Nur Nutzer:innen, die Mitglied einer bestimmten Gruppe sind
define("LDAP_FILTER", "(memberOf=CN=Forschung,OU=Gruppen,DC=example,DC=com)");
```

Ihr könnt auch mehrere Filter kombinieren, um komplexere Abfragen zu erstellen. Zum Beispiel:

```php title="CONFIG.php"
define("LDAP_FILTER", "(&(objectClass=person)(role=employee)(department=Forschung))");
```

Mehr Informationen zu LDAP-Filtern findet ihr in der [LDAP-Dokumentation](https://ldap.com/ldap-filters/).


## Mögliche Fallstricke

Es kann zu Problemen kommen, wenn das LDAP-Password ein Zeichen enthält, das als Code interpretiert wird. Das ist zum Beispiel der Fall, wenn das Passwort ein Dollar-Zeichen ($) enthält. In diesem Fall muss das Zeichen mit einem Backslash (\) escaped werden oder der String mit einfachen Anführungszeichen gesetzt werden. Das Passwort würde dann so aussehen: `define("LDAP_PASSWORD", "\$uperSecret");` oder `define("LDAP_PASSWORD", '$uperSecret');`.

## Synchronisation der Nutzerattribute

<!-- md:version 1.4.1 -->

Dieses Feature ist nur für die Synchronisation mit LDAP verfügbar.

Im Admin-Bereich kann man in den Generellen Einstellungen unter dem Reiter "Mitarbeitende" Attribute definieren, die über LDAP synchronisiert werden sollen. Ihr gebt hier für jedes Feld den genauen Namen an, der im LDAP-Server hinterlegt ist. Wenn ihr auf "Speichern und Vorschau zeigen" klickt, zeigt OSIRIS euch eine Tabelle mit allen Nutzer:innen an und ihr könnt überprüfen, ob die Attribute korrekt ausgelesen werden.

<!-- md:version 1.5.0 -->

Ab Version 1.5.0 findet ihr die Einstellungen zur Synchronisation im neuen Bereich "Inhalte" &#8594 "Personen" &#8594 "LDAP-Attribute". Die Einstellungen sind die gleichen wie zuvor, nur dass sie jetzt in einem eigenen Bereich zu finden sind.

Wichtig: Diese Attribute können dann nicht mehr in OSIRIS selbst bearbeitet werden. Um die Synchronisierung zu aktivieren, müsst ihr außerdem im Folgenden den CRON-Job aktivieren. Das ist wichtig, damit die Attribute regelmäßig aktualisiert werden.

Damit OSIRIS die LDAP-Attribute regelmäßig aktualisiert, müsst ihr einen CRON-Job einrichten, der das entsprechende Skript täglich ausführt.

Öffnet dazu die Crontab eures Systems:

```bash
crontab -e
```

Fügt dann folgende Zeile hinzu, um die Synchronisation täglich um 2:30 Uhr nachts auszuführen:

```cron
30 2 * * * /usr/bin/php /pfad/zu/osiris/jobs/synchronize_users.php >> /var/log/osiris_sync.log 2>&1
```

Hinweis: Ersetzt `/pfad/zu/osiris/` durch das Verzeichnis, in dem OSIRIS bei euch installiert ist.

Ihr könnt den Befehl auch manuell testen:


```bash
php /pfad/zu/osiris/jobs/synchronize_users.php
```

So könnt ihr prüfen, ob die Synchronisation korrekt läuft, bevor sie automatisch per CRON gestartet wird.

Wenn die Synchronisation erfolgreich war, findet ihr genau dort, wo ihr zuvor die Felder definiert habt einen Zeitstempel mit der letzten erfolgreichen Synchronisation.

