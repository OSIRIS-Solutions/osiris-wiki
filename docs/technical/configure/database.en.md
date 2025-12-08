---
tags:
  - MongoDB
  - database
  - configuration
---

# Database connection

The database connection must also be specified in the CONFIG file. To be able to specify server settings and user names if necessary, the connection string is transferred directly. If you have installed MongoDB with the default settings in the same VM, the default settings should work:

```php
define("DB_NAME", "osiris");
define("DB_STRING", "mongodb://localhost:27017/" . DB_NAME . "?retryWrites=true&w=majority");
```

You can read more about the connection to the server directly in the [MongoDB documentation](https://www.mongodb.com/docs/php-library/current/tutorial/connecting/).
