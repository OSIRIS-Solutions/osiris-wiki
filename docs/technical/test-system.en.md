# Set up test system

Here I describe a way of setting up a test system for OSIRIS **on the same server** as the production system. This is particularly useful for testing new functions without affecting the production system.

## Prerequisites

- A server with OSIRIS installed
- Access to the server console

## Steps

Here are the steps to set up a test system for OSIRIS.

### 1. cloning the productive system

Firstly, you need to create a copy of the production system. This can be done by simply copying the directories and files.

```bash
cp -r /path/to/production /path/to/production/test
```

### 2. copy database (optional)

If the database for the test system should initially have the same data as the production system, you can create a backup of the database and import it into a new test database. This is optional, but recommended in order to carry out realistic tests.

The path to the backup and, if necessary, the name of the database must be adjusted here:

```bash
mongodump --db osiris --out /path/to/backup
mongorestore --db osiris_test /path/to/backup/osiris
```

This could look like this, for example, if you want to save the backup in your home directory:

```bash
mongodump --db osiris --out ~/backup
mongorestore --db osiris_test ~/backup/osiris
```

### 3. Customise .htaccess files

Firstly, you must ensure that the new test folder is omitted from the Apache rewrite. Otherwise OSIRIS will not load your test page correctly and will only display a ``404`` error.

Open the `.htaccess` file in the root directory of the **productive system**:

```bash
nano /path/to/production/.htaccess
```

Here you must adapt the `RewriteRule` directive to exclude the test system. Add a new line that excludes the test folder (only `|test` is new):

```apache
RewriteRule ^(css|img|js|uploads|settings.json|manifest.json|test)($|/) - [L]
```

Next, we need to make sure that the test system is working correctly. To do this, we need to customise the `.htaccess` file in the test directory. Open the `.htaccess` file in the test directory:

```bash
nano /path/to/production/test/.htaccess
```

Change the `RewriteBase` directive to point to the test system.

```apache
RewriteBase /test
```

### 4. customise configuration

You must also adjust the path to the test system in the configuration file. Open the configuration file of the test system:

```bash
nano /path/to/production/test/CONFIG.php
```

Here you must adjust the `ROOTPATH` constant so that it points to the test system:

```php
define('ROOTPATH', '/test');
```

Don't forget to also adjust the `DB_NAME` constant so that it refers to the test database:

```php
define('DB_NAME', 'osiris_test');
```

I also recommend setting the constant `LIVE` to `false` on the test system. This causes a note to appear directly under the logo at the top indicating that this is the test system. This helps to avoid confusion.

```php
define('LIVE', false);
```


### 5. test the test system

You should now be able to access the test system by calling up the URL of your server followed by `/test`:

```
http://<your-osiris-server>/test
```

If everything is set up correctly, you should see the test version of OSIRIS working independently of your production system. You can test this by adding a new activity and checking that it only appears in the test system and not in the production system.

## Safety instructions

As the test system runs on the same server as the production system, they share the so-called *session*. This means that you can log in to the production system and then remain logged in to the test system and vice versa. This can be practical, especially to speed up the testing process, but it can also lead to confusion if you are not careful.


If you don't want all users of the production system to have access to the test system, you can modify the `.htaccess` file in the test directory to require a password for access. Here is a simple example of how you can do this:

```apache
AuthType Basic
AuthName "Restricted Area"
AuthUserFile /path/to/.htpasswd
Require valid-user
```

You must then create an `.htpasswd` file containing the user names and passwords. You can do this with the `htpasswd` command:

```bash
htpasswd -c /path/to/.htpasswd username
```
You will then be prompted to set a password for the user. If you want to add multiple users, you can omit the `-c` parameter so as not to overwrite the file:

```bash
htpasswd /path/to/.htpasswd anotheruser
```