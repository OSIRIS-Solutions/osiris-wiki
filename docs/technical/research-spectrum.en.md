---
tags:
  - Research spectrum
  - OpenAlex
  - Citations
  - Cron job
---

# Update the research spectrum

The script `jobs/openalex_topics_citations.py` adds OpenAlex topics and citation counts to existing OSIRIS publications. Topics provide the basis for the research spectrum. The data is stored in each publication's `openalex` field in the MongoDB collection `activities`.

It processes activities with `type = publication` and a non-empty DOI in the `doi` field. It does not import new publications. Importing publications into the queue is handled by the separate [queue workflow](configure/workflows.md#the-queue-workflow).

## Requirements

These examples assume a Linux server with OSIRIS installed at `/var/www/html`. Adjust this path to your installation. Prepare and run the script as the same user whose crontab will run it later.

You need:

- Python 3 with virtual environment support and pip;
- the Python packages `requests` and `pymongo`;
- access to the OSIRIS database and outbound HTTPS access to `api.openalex.org`;
- an up-to-date script that uses bearer authentication and preserves existing metadata on temporary fetch failures.

For Docker installations, Python and the packages must be available wherever you run the job. The supplied production image for the PHP application does not include Python. A job running on the host needs a MongoDB address reachable from the host; the Compose hostname `mongo` normally cannot be resolved there.

## Prepare Python

Create a virtual environment and install the packages:

```bash
cd /var/www/html
python3 -m venv .venv
/var/www/html/.venv/bin/python -m pip install requests pymongo
```

You can use an existing suitable Python environment instead. Use its absolute interpreter path for both the initial run and the cron job. The commands shown here do not require activating the environment.

## Configure the connection and API key

The script reads `config.ini` from the `jobs` directory, independently of the current working directory. If the file does not exist, copy the template:

```bash
cd /var/www/html/jobs
cp -n config.default.ini config.ini
```

Edit the following values in that file, preserving existing settings for other jobs:

```ini
[Database]
Connection = mongodb://localhost:27017
Database = osiris

[OpenAlex]
ApiKey = YOUR_OPENALEX_API_KEY
```

Use the actual database address and name for your OSIRIS installation. If MongoDB requires authentication, include the required credentials and options in the connection URI. The job user needs read/write access to `activities` and read access to `config.ini`.

Get an API key from your [OpenAlex settings](https://openalex.org/settings/api). It is optional for this script but recommended for regular requests. Leave `ApiKey` empty to run without a key. The script sends the configured key as `Authorization: Bearer …`; see [OpenAlex authentication](https://help.openalex.org/api/authentication/). It reads the key from `config.ini`, not an environment variable. This script does not use `Institution`, `StartYear` or `AdminMail`.

## Run the script for the first time

Run the job manually before scheduling it:

```bash
/var/www/html/.venv/bin/python -u /var/www/html/jobs/openalex_topics_citations.py
```

The job writes directly to the database and has no dry-run mode. It fetches missing OpenAlex data and refreshes existing data only when the last fetch is more than 30 days old. Existing error blocks with `status = error` are retried regardless of their age.

The first run may query many publications. The script waits 0.2 seconds between regular requests, in addition to API response time. For 10,000 requests, these waits alone take at least about 33 minutes. Wait for the final summary:

```text
Done. processed=1000 updated=850 skipped=150 not_found=20 errors=0 rate_limited=0
```

These are example values. The counters mean:

| Counter | Meaning |
| --- | --- |
| `processed` | Publications examined from the database query. |
| `updated` | Changed OpenAlex blocks, including stored “not found” results. |
| `skipped` | Publications skipped, for example because their data is still current. |
| `not_found` | DOIs for which OpenAlex returned HTTP 404. |
| `errors` | Failed requests, including HTTP, network and response format errors. |
| `rate_limited` | Requests rejected with HTTP 429 due to an API limit. |

Then check the research spectrum in OSIRIS at `/spectrum`. It uses publications with assigned OpenAlex topics; publications without a DOI or topics do not contribute. No separate recalculation or import script is required.

## Schedule the daily cron job

A daily run is recommended even though existing data is refreshed only about once a month. This adds new publications promptly and retries failed requests on the next run. Running only on the first of each month can skip a refresh after a short February because 30 days have not yet elapsed.

Open the crontab of the user who successfully ran the script manually:

```bash
EDITOR=nano crontab -e
```

Add this entry:

```cron
# Daily at 03:15: update OpenAlex topics and citation counts
15 3 * * * /var/www/html/.venv/bin/python -u /var/www/html/jobs/openalex_topics_citations.py >> /var/www/html/jobs/openalex_topics_citations.log 2>&1
```

The schedule uses the server cron service's time zone. Adjust the paths and ensure the cron user can create or write to the log file. `-u` provides timely output; `>>` appends output and `2>&1` includes error messages in the log. Configure log rotation for ongoing operation.

Check the entry with `crontab -l`. After the next scheduled run, inspect the output:

```bash
tail -n 50 /var/www/html/jobs/openalex_topics_citations.log
```

## Failures and retries

HTTP or network errors and invalid responses leave existing OpenAlex metadata and its fetch date unchanged. If there is no existing data, the script does not create an error block with a new 30-day cooldown. These publications are retried on the next daily run.

On HTTP 429, the script waits five seconds and moves on to the next publication. The rejected request is retried on the next run. HTTP 404 is stored as `status = not_found` and checked again after 30 days; this replaces the previous OpenAlex block.

Check `errors` and `rate_limited` in the summary, as well as whether the job started. Individual fetch failures currently do not produce a non-zero exit code. For recurring HTTP 401/403, check the API key; for HTTP 429, check the API budget; for connection errors, check network access. Missing Python modules indicate an incorrect interpreter or missing packages.
