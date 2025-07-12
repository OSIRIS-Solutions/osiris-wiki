---
tags:
  - Nutzerverwaltung
  - LDAP
  - Authentifizierung
  - OAUTH2
---


# Nutzerverwaltung

OSIRIS funktioniert zurzeit mit drei unterschiedlichen Möglichkeiten der Nutzerverwaltung, die über die `CONFIG.php` eingestellt werden können:


`LDAP` 

:   **LDAP**: über ein Active Directory werden die Nutzer zentral verwaltet und mittels [LDAP-Authentifizierung](https://www.redhat.com/de/topics/security/what-is-ldap-authentication) werden die Daten an OSIRIS weitergegeben. Falls es euch möglich ist, solltet ihr diese Einstellung verwenden, da sie einerseits Arbeit spart (Nutzer müssen nicht manuell angelegt werden) und andererseits auch mehr Sicherheit bietet.

`AUTH` 

:   **Auth-Addon**:
OSIRIS hat ein mitgeliefertes Addon zur Nutzer-Authentifizierung. Nutzer können sich dabei selbst einen Account erstellen. Dies kann aber in den Einstellungen (ab Version 1.5.0) deaktiviert werden, sodass Accounts nur noch zentral durch Admins angelegt werden können.

`OAUTH2` 

:   **Microsoft-SSO**:
OSIRIS kann auch mit Microsoft-SSO (Single Sign-On) verwendet werden. Dabei wird die Authentifizierung über Microsoft Azure Active Directory durchgeführt. Diese Option ist besonders nützlich, wenn ihr bereits Microsoft 365 oder andere Microsoft-Dienste nutzt und eine nahtlose Integration wünscht. In Zukunft wird es auch noch weitere OAuth-Provider geben, die OSIRIS unterstützen wird.

## Nutzerverwaltung mit LDAP

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

#### Mögliche Fallstricke


Es kann zu Problemen kommen, wenn das LDAP-Password ein Zeichen enthält, das als Code interpretiert wird. Das ist zum Beispiel der Fall, wenn das Passwort ein Dollar-Zeichen ($) enthält. In diesem Fall muss das Zeichen mit einem Backslash (\) escaped werden oder der String mit einfachen Anführungszeichen gesetzt werden. Das Passwort würde dann so aussehen: `define("LDAP_PASSWORD", "\$uperSecret");` oder `define("LDAP_PASSWORD", '$uperSecret');`.



Bei der LDAP-Domain ist entweder eine Domain mit `@` anzugeben oder eine Base-DN, bei der der Nutzername dann mit `%s` angegeben werden muss. Das sieht dann so aus:
`define("LDAP_DOMAIN", "uid=%s,ou=people,dc=stark,dc=com");`

#### Synchronisation der Nutzerattribute


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


## Nutzerverwaltung mit Auth-Addon


Um die OSIRIS-eigene Nutzerauthentifizierung zu nutzen, muss nur folgende Einstellung in der `CONFIG.php` angepasst werden:



```php
define('USER_MANAGEMENT', 'AUTH');
```

Die Nutzer-Authentifizierung findet jetzt über das OSIRIS-Addon statt. LDAP-Konfiguration muss demnach nicht vorgenommen werden.



#### Migration der Nutzerverwaltung


Sowohl LDAP als auch das Auth-Addon kümmern sich um die **Authentifizierung** der Nutzer. Bei LDAP werden grundlegende Nutzerdaten ebenfalls aus dem Active Directory synchronisiert, beim Auth müssen diese vom Nutzer bei der Registrierung angelegt werden. **Die eigentlichen Nutzerdaten werden aber immer in OSIRIS gespeichert.**
Das führt dazu, dass sich im Nachhinein leicht von Auth auf LDAP migriert werden kann. Dabei muss nur beachtet werden, dass die **Nutzernamen mit denen im LDAP identisch sind**.
Dadurch weiß das System später noch, welcher Nutzer zu welchem Account gehört.

## Nutzerverwaltung mit Microsoft-SSO


OSIRIS kann auch mit Microsoft-SSO (Single Sign-On) verwendet werden. Dabei wird die Authentifizierung über Microsoft Azure Active Directory durchgeführt. Das wird wie folgt konfiguriert:



```php
define('USER_MANAGEMENT', 'OAUTH2');

// OAUTH user management:
define('OAUTH', 'Microsoft');
define('CLIENT_ID', 'DEINE_CLIENT_ID');
define('CLIENT_SECRET', 'DEIN_CLIENT_SECRET');
define('REDIRECT_URI', 'http://localhost/login-callback.php');
define('AUTHORITY', 'https://login.microsoftonline.com/common');
define('SCOPES', 'openid profile email User.Read');
```

