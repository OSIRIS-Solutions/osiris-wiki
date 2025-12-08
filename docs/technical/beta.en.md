# Switch to beta

The beta branch of OSIRIS contains the latest developments that are not yet included in the stable version. This branch is updated regularly and contains new features, bug fixes and improvements as soon as they are ready. Using the beta branch is ideal for users who want to try out the latest functions and are prepared to accept possible instabilities and bugs. As soon as a new version is stable, it is transferred to the main branch. This may take a different amount of time depending on the range of functions and test phase.

## 1. change to the project directory
Open a terminal (or Bash) and go to the folder with the repository. In Apache this is usually `/var/www/html/`. If you have the repository in a different location, adjust the path accordingly:

```bash
cd /var/www/html/
```


## 2. show available remote branches (optional)
If you want to check whether beta exists:

```bash
git fetch
git branch -r
```


## 3. switch to branch beta
If you do not yet have the branch locally:

```bash
git checkout -b beta origin/beta
```

If you already have it locally:

```bash
git checkout beta
```


## 4. get latest changes from remote
To get the current status of origin/beta in your local beta:

```bash
git pull
```

## 5. update composer dependencies

If you are using Composer to manage the dependencies, run the following command:

```bash
composer update
```

## 6. update OSIRIS

To update OSIRIS, you only need to open the website and perform the database migration if necessary. This usually happens automatically when you open the page in the browser. This only happens if the beta already contains a version jump.

