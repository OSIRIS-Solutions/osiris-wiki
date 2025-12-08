---
tags:
  - workflows
  - automation
  - OpenAlex
  - cron job
---


# Optional workflows

Workflows run regularly and make it possible to automate work steps.


## General setup

Python 3 is required for all current (and presumably future) workflows. The best way to install Python is to look it up [in the official documentation](https://wiki.python.org/moin/BeginnersGuide/Download). You will also need the Python package installation manager pip.

Once Python 3 and pip are installed, you can use the following command line command to install the MongoDB connector, which is also required for all jobs:


```bash
pip install pymongo
```

You must also copy the configuration file `config.default.ini` in the `jobs` subfolder and name the copy `config.ini` (this procedure prevents your configuration from being overwritten by OSIRIS updates). In the config file, all variables must be adjusted accordingly, for example the information for the database connection.


## The queue workflow


The queue workflow retrieves new activities from online sources and stores them in a queue. Users are notified when new activities are waiting in the queue and can simply add them to OSIRIS.


### Preparation

To prepare this workflow, three additional Python packages must first be installed. This is done again with pip:

```bash
pip install diophila
pip install nameparser
pip install levenshtein
```

In addition, you must change the OpenAlex ID of your institute in the `config.ini` file. To find this ID, it is best to search for your institute on the [OpenAlex website](https://explore.openalex.org/). You will find the ID either as the last part of the URL or under Identifiers &#8594
 openalex. It begins with an I, followed by a series of numbers. It must be entered in the `config.ini`:

```ini
[OpenAlex]
Institution = I7935750
```

Also relevant is the `StartYear`, from which activities are imported, as well as the `AdminMail`, with which the queries are made to OpenAlex.


If you want to [Import legacy data](../data.md), you are now ready to do so.

### Create the cron job

Last but not least, we can create the cron job. A cron job is a scheduled, repetitive process that a system executes at a set time. The following settings mean that the workflow is executed once a week, more precisely every Sunday at 2 am.
We use the `nano` editor for this action, but you can of course use any other editor.



```bash
EDITOR=nano crontab -e

# enter this as cronjob:
0 2 * * 0 python3 /var/www/html/jobs/openalex_parser.py

# press Ctrl+O to save and Ctrl+X to exit
```

The output should show you that a new cron job has been installed. In the following, new publications will be added to your waiting list every Sunday.

