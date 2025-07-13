---
tags:
  - Nutzerverwaltung
  - LDAP
  - Authentifizierung
---

# Nutzerverwaltung mit LDAP

OSIRIS funktioniert zurzeit bevorzugt mit [LDAP-Authentifizierung](https://www.redhat.com/de/topics/security/what-is-ldap-authentication), die von den meisten Instituten zur Nutzerverwaltung verwendet wird.

Die LDAP-Einstellungen müssen ebenfalls in der Configuration angegeben werden. Falls ihr selbst nicht wisst, welche Einstellungen ihr vornehmen müsst, fragt am besten bei eurer IT nach.

```
define('USER_MANAGEMENT', 'LDAP');

define("LDAP_IP", "100.10.100.0");
define("LDAP_PORT", 389);
define("LDAP_USER", "osiris");
define("LDAP_DOMAIN", "@domain.local");
define("LDAP_PASSWORD", "ldap_password");
define("LDAP_BASEDN", "OU=Users,OU=DSMZ,DC=dsmz,DC=local");
```

## Filtern von Nutzer:innen

Ihr könnt auch festlegen, welche Nutzer:innen aus dem LDAP-Server synchronisiert werden sollen. Dazu könnt ihr den `LDAP_FILTER`-Parameter in der `CONFIG.php` setzen. Zum Beispiel:

```php
define("LDAP_FILTER", "(objectClass=person)");
```

Dieser Filter sorgt dafür, dass nur Nutzer:innen mit dem Objektklasse `person` synchronisiert werden. Ihr könnt den Filter anpassen, um andere Kriterien zu verwenden, die für eure Nutzer:innen zutreffen.

Hier sind ein paar Beispiele für häufig verwendete Filter:

```php
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

```php
define("LDAP_FILTER", "(&(objectClass=person)(role=employee)(department=Forschung))");
```

Mehr Informationen zu LDAP-Filtern findet ihr in der [LDAP-Dokumentation](https://ldap.com/ldap-filters/).

## Mögliche Fallstricke

Es kann zu Problemen kommen, wenn das LDAP-Password ein Zeichen enthält, das als Code interpretiert wird. Das ist zum Beispiel der Fall, wenn das Passwort ein Dollar-Zeichen ($) enthält. In diesem Fall muss das Zeichen mit einem Backslash (\) escaped werden oder der String mit einfachen Anführungszeichen gesetzt werden. Das Passwort würde dann so aussehen: `define("LDAP_PASSWORD", "\$uperSecret");` oder `define("LDAP_PASSWORD", '$uperSecret');`.

Bei der LDAP-Domain ist entweder eine Domain mit `@` anzugeben oder eine Base-DN, bei der der Nutzername dann mit `%s` angegeben werden muss. Das sieht dann so aus:
`define("LDAP_DOMAIN", "uid=%s,ou=people,dc=stark,dc=com");`

## Synchronisation der Nutzerattribute


Dieses Feature ist nur für die Synchronisation mit LDAP verfügbar.

Im Admin-Bereich kann man in den Generellen Einstellungen unter dem Reiter "Mitarbeitende" Attribute definieren, die über LDAP synchronisiert werden sollen. Ihr gebt hier für jedes Feld den genauen Namen an, der im LDAP-Server hinterlegt ist. Wenn ihr auf "Speichern und Vorschau zeigen" klickt, zeigt OSIRIS euch eine Tabelle mit allen Nutzer:innen an und ihr könnt überprüfen, ob die Attribute korrekt ausgelesen werden.

Ab Version 1.5.0 findet ihr die Einstellungen zur Synchronisation im neuen Bereich "Inhalte" > "Personen" > "LDAP-Attribute". Die Einstellungen sind die gleichen wie zuvor, nur dass sie jetzt in einem eigenen Bereich zu finden sind.

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

