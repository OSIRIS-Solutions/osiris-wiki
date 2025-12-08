---
tags:
  - Linux
  - server
  - Apache
  - PHP
  - MongoDB
  - installation
---

# Linux

## 1. requirements

OSIRIS is a web application that runs on a Linux server. It is installed via the command line. You have to set up the distribution yourself, but the installation of all dependencies is also described below. OSIRIS has been tested with Debian 11 and CentOS 8, but the installation should also work on other distributions that provide the same packages.
 
* Linux server with root rights (not described further here)
* Web server (Apache2 recommended)
* PHP 8.x
* MongoDB server (tested with versions 5, 6 and 7)
* Composer
* Git


## 2. prepare server

### 2.1 Install Apache and PHP


Install Apache2, PHP 8 and the required PHP modules:

=== "Debian"

    ```bash
    sudo apt-get install -y apache2 php php-cli php-pear php-dev php-ldap libzip-dev gcc
    ```

=== "CentOS"

    ```bash
    sudo yum install -y httpd php php-cli php-pear php-devel php-ldap libzip-devel gcc
    ```

Start and activate Apache:


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

:warning: Please make sure that **AllowOverwrite All** is set in your Apache configuration (/etc/apache2/sites-enabled/000-default.conf).  

You will need to reload Apache for the change to take effect:

```bash
sudo systemctl reload apache2
```

### 2.2 Install MongoDB



 Follow the official instructions:
 [MongoDB Installation](https://www.mongodb.com/docs/manual/administration/install-on-linux/)



=== "Debian"

    ```bash
    sudo apt-get install -y mongodb
    ```

=== "CentOS"

    ```bash
    sudo yum install -y mongodb-org
    ```

Start MongoDB and activate it at system startup:

```bash
sudo systemctl start mongod
sudo systemctl enable mongod
```

Check if MongoDB is running:

```bash
sudo systemctl status mongod
```

### 2.3 Install MongoDB PHP driver

Important: Use the MongoDB driver version **smaller 2.0**!
 

```bash
sudo pecl install mongodb-1.21.0
```

Add the following lines to your `php.ini`:


```ini title="php.ini"
extension=mongodb.so
extension=zip.so
```

## 3. download and install OSIRIS


### 3.1 Install Git

If Git is not yet installed, install it with :


=== "Debian"

    ```bash
    sudo apt-get install git # Debian/Ubuntu
    ```

=== "CentOS"

    ```bash
    sudo yum install git # CentOS/Rocky
    ```

### 3.2 Download OSIRIS

Change to the directory in which you want to install OSIRIS. The `.` at the end of the following command ensures that all files are loaded into the current directory. If you want to install OSIRIS in a subfolder, enter the path instead.

```bash
cd /var/www/html
git clone https://github.com/OSIRIS-Solutions/osiris.git .
```

### 3.3 Install Composer and resolve dependencies

Composer installs all dependencies that OSIRIS requires. This may take a few minutes.

```bash
php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
sudo php composer-setup.php --install-dir=/usr/local/bin --filename=composer
composer update
```

In case of problems:

```bash
composer update --ignore-platform-reqs
```

### 3.4 Customise configuration


The configuration of OSIRIS can be found in the file `CONFIG.php`. This file is not included in the Git repository as it must be adapted to the respective installation. Copy the `CONFIG.default.php` and rename it to `CONFIG.php`.



!!! warning "Important!"

    All server-side settings are made in the `CONFIG.php` file. Here you can configure the authentication method, the database connection and many other settings. This file is not overwritten during an update, so you do not have to make your settings again.

All information on the settings can be found in the [Basic configuration](../configure/index.md) section.
 
## 4 Configure Apache


### 4.1 Activate mod\_rewrite


=== "Debian"

    ```bash
    sudo a2enmod rewrite
    sudo systemctl restart apache2
    ```

=== "CentOS"

    ```bash
    # Make sure that LoadModule rewrite_module is active in httpd.conf
    sudo systemctl restart httpd
    ```

### 4.2 Note .htaccess


From OSIRIS version 1.3.6 the `.htaccess` file is supplied. When installing in a subfolder, please adjust `RewriteBase` and the `ROOTPATH` in `config.php`.

```apache
DirectoryIndex index.php
RewriteEngine on
RewriteBase /
RewriteRule ^(css|img|js|uploads|settings.json|manifest.json)($|/) - [L]
RewriteRule ^(.*)$ index.php [QSA]
```

## 5. test and install


Open your browser and go to:

```url
http://<your-server-ip>/
```

You should see a red box indicating that OSIRIS still needs to be installed. Don't panic, this only refers to the database. Click on "Install" and OSIRIS will create the database and prepare everything.  

:warning: If you get an error message and the installation path is not found, check whether the **.htaccess** file exists and looks as described in [4.2](/technical/install/linux/#42-htaccess-beachten). The path should also be set to "/".
 

Finally, the OSIRIS login page should load and the installation should be complete! 🎉


## Summary: Checklist

- X] MongoDB is running
- X] Apache and PHP correctly installed
- X] MongoDB PHP driver version 1.21.0 active
- X] OSIRIS loaded from Git and Composer dependencies installed
- [X] mod\_rewrite active, AllowOverwrite All set and .htaccess customised
- X] OSIRIS loads in the browser



