# Datenbank-Verbindung

Die Datenbank-Verbindung muss ebenfalls in der CONFIG-Datei angegeben werden. Um gegebenenfalls Servereinstellungen und Nutzernamen angeben zu können, wird direkt der Verbindungsstring übergeben. Wenn ihr MongoDB mit den Standardeinstellungen in der gleichen VM installiert habt, sollten die Voreinstellungen funktionieren:

```php
define("DB_NAME", "osiris");
define("DB_STRING", "mongodb://localhost:27017/" . DB_NAME . "?retryWrites=true&w=majority");
```

Mehr über die Verbindung mit dem Server könnt ihr direkt in der [MongoDB-Dokumentation](https://www.mongodb.com/docs/php-library/current/tutorial/connecting/) nachlesen.
