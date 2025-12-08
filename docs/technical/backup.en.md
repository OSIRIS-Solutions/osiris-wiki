---
tags:
  - Backup
  - database
  - MongoDB
---


# Backup

To save the database completely, use the backup tool integrated in MongoDB as follows:

```bash
mongodump --db osiris
```

This command creates a backup of the database "osiris" in a directory called "dump" in the current working directory. If your database has a different name, replace "osiris" with the corresponding name.

You can also save the backup in another directory by using the "--out" parameter:

```bash
mongodump --db osiris --out /path/to/backup
```

Since OSIRIS is not really "installed", but only copied to a directory, you can also simply copy the entire OSIRIS directory and migrate it to the new server.


```bash
zip -r osiris_backup.zip /path/to/osiris
```

Alternatively, you can also back up only the important files, i.e. the configuration files and the files uploaded by users. The latter are located in the `/uploads` directory. You can simply copy these files:

```bash
zip -r osiris_partial_backup.zip /path/to/osiris/CONFIG.php /path/to/osiris/uploads
```

If your user images are not saved in the database (a question of setting), then you should also save the directory `/img/users`.

