---
tags:
- E-Mail
- Cron-Job
---

# E-Mail-Digest Einrichtung

<!-- md:version 1.6.0 -->

Damit OSIRIS regelmäßig die Mail-Digests (tägliche, wöchentliche oder monatliche Zusammenfassungen) verschickt, muss ein Cron-Job auf dem Server eingerichtet werden.

Zuerst benötigst du den geheimen Schlüssel (CRON_SECRET), den du in der CONFIG.php hinterlegen kannst:

```php
define('CRON_SECRET', 'YOUR_CRON_SECRET');
```

Danach öffne die Crontab deines Servers:
```bash
crontab -e
```

Füge folgenden Eintrag hinzu:

```bash
0 8 * * * curl -s "https://your-osiris.de/cron/digest?key=YOUR_CRON_SECRET" > /var/log/osiris-digest.log 2>&1
```

Ersetze `https://your-osiris.de` durch die URL deiner OSIRIS-Installation und `YOUR_CRON_SECRET` durch den in den Einstellungen hinterlegten geheimen Schlüssel.