# OSIRIS konfigurieren

Damit OSIRIS läuft müssen die wichtigen Einstellungen vorgenommen werden. Dazu muss die Datei [CONFIG.default.php](https://github.com/JKoblitz/osiris/blob/master/CONFIG.default.php) kopiert und in `CONFIG.php` umbenannt werden. In dieser Datei könnt ihr grundlegende Einstellungen anpassen, die nicht durch Updates überschrieben werden.

## Grundlegende Einstellungen


### Administrator:in

Die Variable `ADMIN` definiert den Nutzernamen des Administrators.
Per Default kann dort nur ein Admin angegeben werden, dieser kann aber durch Bearbeitung eines Nutzerprofils anderen Nutzern ebenfalls Admin-Rechte geben.


### Relativer Pfad

Die Variable `ROOTPATH` definiert, in welchem Unterordner euer Projekt wohnt. Sollte sich das Projekt wie oben angewiesen im Stammordner befinden, muss die Variable leer bleiben.


### OSIRIS in einem Unterordner

Wenn euer Projekt in einem Unterordner angelegt werden soll, müsst ihr diese Information an zwei stellen verändern. Nehmen wir im folgenden Beispiel mal an, das System wird im Unterordner `osiris` installiert. Einmal müsst ihr in der `CONFIG.php` die folgende Zeile anpassen:


```php
define('ROOTPATH', '/osiris');
```

Außerdem müsst ihr den Pfad auch in der `.htaccess`-Datei anpassen:


```apache
RewriteBase /osiris
```
