---
tags:
  - Settings
  - configuration
  - PHP
---

# Configure OSIRIS

The important settings must be made for OSIRIS to run. To do this, the file [CONFIG.default.php](https://github.com/JKoblitz/osiris/blob/master/CONFIG.default.php) must be copied and renamed to `CONFIG.php`. In this file you can adjust basic settings that will not be overwritten by updates.

## Basic settings


### Administrator:in

The variable `ADMIN` defines the user name of the administrator.
By default, only one admin can be specified here, but this admin can also grant admin rights to other users by editing a user profile.


### Relative path

The variable `ROOTPATH` defines in which subfolder your project resides. If the project is located in the root folder as instructed above, the variable must remain empty.


### OSIRIS in a subfolder

If your project is to be created in a subfolder, you must change this information in two places. Let's assume in the following example that the system is installed in the `osiris` subfolder. Firstly, you need to change the following line in `CONFIG.php`:


```php
define('ROOTPATH', '/osiris');
```

You must also adjust the path in the `.htaccess` file:


```apache
RewriteBase /osiris
```
