---
tags:
- e-mail
---

# E-mail digest facility

<!-- md:version 1.6.0 -->

A cron job must be set up on the server so that OSIRIS regularly sends the mail digests (daily, weekly or monthly summaries).

First you need the secret key (CRON_SECRET), which you can store in CONFIG.php:

```php
define('CRON_SECRET', 'YOUR_CRON_SECRET');
```

Then open the crontab of your server:
```bash
crontab -e
```

Add the following entry:

```bash
0 8 * * * curl -s "https://your-osiris.de/cron/digest?key=YOUR_CRON_SECRET" > /var/log/osiris-digest.log 2>&1
```

Replace `https://your-osiris.de` with the URL of your OSIRIS installation and `YOUR_CRON_SECRET` with the secret key stored in the settings.