---
tags:
  - Linux
  - Server
  - Apache
  - PHP
  - MongoDB
  - Installation
---

# Linux

## 1. Voraussetzungen

OSIRIS ist eine Webanwendung, die auf einem Linux-Server läuft. Die Installation erfolgt über die Kommandozeile. Die Distribution müsst ihr selbst aufsetzen, doch die Installation aller Abhängigkeiten wird auch weiter unten beschrieben. OSIRIS ist getestet mit Debian 11 und CentOS 8. Die Installation sollte aber auch auf anderen Distributionen funktionieren, die die gleichen Pakete bereitstellen.
 
* Linux-Server mit Root-Rechten (wird hier nicht weiter beschrieben)
* Webserver (Apache2 empfohlen)
* PHP 8.x
* MongoDB Server (getestet mit Version 5, 6 und 7)
* Composer
* Git


## 2. Server vorbereiten

### 2.1 Apache und PHP installieren 


Installiere Apache2, PHP 8 und benötigte PHP-Module:

=== "Debian"

    ```bash
    sudo apt-get install -y apache2 php php-cli php-pear php-dev php-ldap libzip-dev gcc
    ```

=== "CentOS"

    ```bash
    sudo yum install -y httpd php php-cli php-pear php-devel php-ldap libzip-devel gcc
    ```

Apache starten und aktivieren:


=== "Debian"

    ```bash
    sudo systemctl start apache2
    sudo systemctl enable apache2
    ```

=== "CentOS"

    ```bash
    sudo systemctl start httpd
    sudo systemctl enable httpd
    ```

:warning: Bitte achtet darauf, dass in eurer Apache-Konfiguration (/etc/apache2/sites-enabled/000-default.conf) **AllowOverwrite All** gesetzt ist.  

Damit die Änderung wirksam wird müsst ihr Apache neu laden:

```bash
sudo systemctl reload apache2
```

### 2.2 MongoDB installieren



 Folgt der offiziellen Anleitung:
 [MongoDB Installation](https://www.mongodb.com/docs/manual/administration/install-on-linux/)



=== "Debian"

    ```bash
    sudo apt-get install -y mongodb
    ```

=== "CentOS"

    ```bash
    sudo yum install -y mongodb-org
    ```

MongoDB starten und beim Systemstart aktivieren:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

Überprüfe, ob MongoDB läuft:

```bash
sudo systemctl status mongod
```

### 2.3 MongoDB PHP-Treiber installieren

Wichtig: Nutze die MongoDB-Treiber-Version **kleiner 2.0**!
 

```bash
sudo pecl install mongodb-1.21.0
```

In deiner `php.ini` folgende Zeilen hinzufügen:


```ini title="php.ini"
extension=mongodb.so
extension=zip.so
```

## 3. OSIRIS herunterladen und installieren


### 3.1 Git installieren

Falls Git noch nicht installiert ist, installiere es mit:


=== "Debian"

    ```bash
    sudo apt-get install git # Debian/Ubuntu
    ```

=== "CentOS"

    ```bash
    sudo yum install git     # CentOS/Rocky
    ```

### 3.2 OSIRIS herunterladen

Wechsle in das Verzeichnis, in dem du OSIRIS installieren möchtest. Der `.` am Ende des folgenden Befehls sorgt dafür, dass alle Dateien in das aktuelle Verzeichnis geladen werden. Wenn du OSIRIS in einem Unterordner installieren möchtest, gib stattdessen den Pfad an.

```bash
cd /var/www/html
git clone https://github.com/OSIRIS-Solutions/osiris.git .
```

### 3.3 Composer installieren und Abhängigkeiten auflösen

Composer installiert alle Abhängigkeiten, die OSIRIS benötigt. Das kann einige Minuten dauern.

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
composer update
```

Bei Problemen:

```bash
composer update --ignore-platform-reqs
```

### 3.4 Uploads-Verzeichnis anlegen
Erstelle das `uploads`-Verzeichnis und setze die richtigen Berechtigungen:

```bash
mkdir uploads
sudo chown -R www-data:www-data uploads
sudo chmod -R 755 uploads
```

Dieses Verzeichnis wird von OSIRIS verwendet, um hochgeladene Dateien zu speichern, es gibt aber immer wieder Probleme mit Git, weshalb dieses leere Verzeichnis nicht im Repository enthalten ist.

### 3.5 Konfiguration anpassen


Die Konfiguration von OSIRIS ist in der Datei `CONFIG.php` zu finden. Diese Datei ist im Git-Repository nicht enthalten, da sie an die jeweilige Installation angepasst werden muss. Kopiere die `CONFIG.default.php` und benenne sie in `CONFIG.php` um.



!!! warning "Wichtig!"

    In der Datei `CONFIG.php` werden alle Server-seitigen Einstellungen vorgenommen. Hier kannst du die Authentifizierungsmethode, die Datenbankverbindung und viele weitere Einstellungen vornehmen. Diese Datei wird bei einem Update nicht überschrieben, sodass du deine Einstellungen nicht erneut vornehmen musst.

Alle Informationen zu den Einstellungen findest du in dem Abschnitt [Basiskonfiguration](../configure/index.md).
 
## 4. Apache konfigurieren


### 4.1 mod\_rewrite aktivieren


=== "Debian"

    ```bash
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    ```

=== "CentOS"

    ```bash
    # Stelle sicher, dass LoadModule rewrite_module aktiv ist in httpd.conf
    sudo systemctl restart httpd
    ```

### 4.2 .htaccess beachten


Ab OSIRIS Version 1.3.6 wird die `.htaccess`-Datei mitgeliefert. Bei Installation in einem Unterordner passe bitte `RewriteBase` an und den `ROOTPATH` in `config.php`.

```apache
DirectoryIndex index.php
RewriteEngine on
RewriteBase /
RewriteRule ^(css|img|js|uploads|settings.json|manifest.json)($|/) - [L]
RewriteRule ^(.*)$ index.php [QSA]
```

## 5. Testen und Installieren


Öffne deinen Browser und gehe zu:

```url
http://<deine-server-ip>/
```

Du solltest eine rote Box sehen mit dem Hinweis, dass OSIRIS noch installiert werden muss. Keine Panik, damit ist nur die Datenbank gemeint. Klicke auf "Installieren" und OSIRIS wird die Datenbank anlegen und alles vorbereiten.  

:warning: Wenn ihr eine Fehlermeldung bekommt und der Installationspfad nicht gefunden wird, überprüft ob die **.htaccess**-Datei vorhanden ist und wie unter [4.2](/technical/install/linux/#42-htaccess-beachten) beschrieben aussieht. Zudem sollte der Pfad auf "/" gesetzt sein. 
 

Zum Schluss sollte die Login-Seite von OSIRIS laden und die Installation somit abgeschlossen sein! 🎉


## Zusammenfassung: Checkliste

- [X] MongoDB läuft
- [X] Apache und PHP korrekt installiert
- [X] MongoDB PHP-Treiber Version 1.21.0 aktiv
- [X] OSIRIS aus Git geladen und Composer-Abhängigkeiten installiert
- [X] mod\_rewrite aktiv, AllowOverwrite All gesetzt und .htaccess angepasst
- [X] OSIRIS lädt im Browser



