---
tags:
  - Upgrade
  - MongoDB
  - backup
  - composer
---


# Upgrade to the latest version

To upgrade to the latest version of OSIRIS, please follow the steps below.

## Step 1: Backup

Before you start the upgrade, you should create a backup of your database and your files. This is important in case something goes wrong and you need to revert to the previous version. The backup should be saved on an external storage device to ensure that it is not lost. To do this, use the backup tool integrated in MongoDB as follows:


```bash
mongodump --db osiris
```
This command creates a backup of the database "osiris" in a directory called "dump" in the current working directory. If your database has a different name, replace "osiris" with the corresponding name.

You can also save the backup in another directory by using the "--out" parameter:

```bash
mongodump --db osiris --out /path/to/backup

```
To back up the files, you can simply copy the entire OSIRIS directory and save it in a safe place. The most important folder is the "uploads" folder, where all files uploaded by users are stored. If user photos are not stored in the database, you should also back up the `img/users` folder.


```bash
zip -r osiris_backup.zip /path/to/osiris

# or individual folders
zip -r uploads_backup.zip /path/to/osiris/uploads
zip -r img_users_backup.zip /path/to/osiris/img/users
```

## Step 2: Download the latest version from GitHub

To download the latest version of OSIRIS, you can use the [GitHub repository](https://github.com/OSIRIS-Solutions/osiris). To do this, navigate to the directory where OSIRIS is installed and execute the following command:

```bash
git pull
```

This command downloads the latest version of OSIRIS and updates your local repository. If there are any conflicts, you will need to resolve them manually before proceeding.

## Step 3: Update dependencies with Composer

After you have downloaded the latest version of OSIRIS, you need to update the dependencies. You can use Composer to do this:

```bash
composer install
```

This command downloads the latest versions of the dependencies and installs them in your project. If there are problems, you can use the ``composer update`` command to update the dependencies.

## Step 4: Updating the database via the web interface

Once you have updated the dependencies, you can migrate the database to the latest version. To do this, you must call up the OSIRIS web interface. You should now see the following message there:


> A new OSIRIS version has been found.
>
> OSIRIS will be automatically updated and set up. Depending on the version, this may take a while, so please make sure you do not reload or close the page during the process.
>
> Installed: 1.X.X | Latest: 1.Y.Y


Click on the "Update OSIRIS" button to start the migration process. This process may take some time depending on the size of the database and the number of data records. Please wait patiently until the process is complete. You should see a success message when the migration has been successfully completed.

## Step 5: Completion

Once the migration is complete, you should have successfully installed the latest version of OSIRIS. You can now go to the OSIRIS web interface and check that everything is working as expected. If there are any problems, you can use the backup to revert to the previous version.

## If something goes wrong

If something goes wrong and you need to revert to the previous version, you can use the backup you created in the first step. You can restore the database by restoring the backup with the following command:


```bash
mongorestore --db osiris /path/to/backup/dump/osiris
```

If you need to restore the files, you can unzip the backup and copy the files to the OSIRIS directory. Make sure you restore the `uploads` folder and the `img/users` folder if you have backed them up.



