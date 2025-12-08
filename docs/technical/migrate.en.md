---
tags:
  - Migration
  - MongoDB
  - backup
---


# Migrate OSIRIS to another server

OSIRIS can be migrated to another server very easily. All you have to do is back up the database and files and restore them on the new server. Here are the steps you need to follow:

## Step 1: Backup

To do this, use the backup tool integrated in MongoDB as follows:

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


## Step 2: Install OSIRIS on the new server

Follow the instructions for installing OSIRIS. Then copy all backup files to the corresponding directory on the new server.

## Step 3: Restore database and files
Finally, you can restore the database with the following command:

```bash
mongorestore --db osiris /path/to/backup/dump/osiris
```

Afterwards, be sure to re-render so that the links to the activities also work. To do this, navigate to the admin area and click on "Rerender" or navigate to the URL `/rerender`.


