---
tags:
- data
- legacy data
- import
- OpenAlex
- DOIs
- Cron-Job
---

# Import legacy data

## Variant 1: Import legacy data automatically via OpenAlex


It is possible to import legacy data into OSIRIS without much effort. However, to prepare OSIRIS for the import, a few installation steps and configurations must first be carried out. The best way to do this is to follow the instructions for [setting up workflows](configure/workflows.md). You have to work through the general setup and the preparation of the queue workflow (including the configuration) until you reach the point where it says that you are ready for the legacy data import. Then return here to continue with the import of the legacy data.

The following script will import all legacy data from OpenAlex directly into your database. You can read why this is a great idea [here](https://openalex.org/about).
The script for importing the legacy data is started with the following call:

```bash
python jobs/import_data.py
```

If the script was executed successfully, DOIs should now appear and be entered into the database one after the other.
Depending on how much legacy data is imported, this process can take quite a while.


## Variant 2: Importing legacy data via a list of DOIs
You can also use a list of DOIs to quickly import a lot of data into OSIRIS.
I have prepared a [Python script](https://github.com/JKoblitz/osiris/blob/master/jobs/import_doi.py) for this purpose, which you can also find in the Jobs folder (only from 5 March 2024).

You carry out the following steps:


1. you enter all the DOIs you would like to import into a CSV file, one DOI per line. Preferably without a URL, but you can also use one. I have already included a [sample file](https://github.com/JKoblitz/osiris/blob/master/jobs/doi.csv) in the folder.
2. if your file has a different name or is stored somewhere else, you have to adjust the path in `import_doi.py`.
3. that's actually it. Now just execute the following command in the command line and off you go:

```bash
python jobs/import_doi.py
```



!!! info "Note"

    Existing entries whose DOI is already stored in the database are not added again. This cannot be bypassed, as it means that if a script is cancelled, it can be executed again without changes.
    
    By default, Levenshtein similarity is also used to filter for matching titles (more than 90% match). If this is not desired, you must set the value `ignoreDupl` in `import_doi.py` to `False`.



## Rendering the imported data

<!-- md:version 1.8.1 -->

No matter whether you have imported legacy data or are continuously adding new data from other sources (e.g., an institutional repository), the data must be rendered so that they can be displayed correctly in OSIRIS. To do this, you need to call a URL in your OSIRIS that triggers the rendering process. If you regularly import data, it is advisable to automate this process via a cron job.

First, you need the secret key (CRON_SECRET), which you can store in CONFIG.php:

```php
define('CRON_SECRET', 'YOUR_CRON_SECRET');
```

To start the rendering process, you can execute the following command in the command line. This will call a URL in your OSIRIS that triggers the rendering process. Only data that has not yet been rendered will be rendered.

```bash
curl -s "https://your-osiris.de/smart-render?key=YOUR_CRON_SECRET"
```

Replace `https://your-osiris.de` with the URL of your OSIRIS installation and `YOUR_CRON_SECRET` with the secret key stored in the settings.


!!! info "What does rendering mean?"
    Rendering generates various representations from the different data fields of an entry (e.g., title, authors, abstract, etc.) that are needed for display in the OSIRIS user interface. These include, for example, the HTML representation, the print representation for export, the icons, etc. Without rendering, the entries in OSIRIS would not be displayed correctly, and when calling various tables, you will see a **"DataTables warning"**.
