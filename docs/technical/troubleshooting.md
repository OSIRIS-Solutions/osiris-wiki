---
tags:
  - Troubleshooting
  - MongoDB
---


# Troubleshooting


Für den Fall, dass etwas schief laufen sollte, sammle ich hier ein paar Tipps, die mir selbst oder anderen Nutzern schon weitergeholfen haben.

### Anscheinend kann OSIRIS keine Verbindung zu MongoDB aufbauen. Ich bekomme den Fehler `No suitable servers found`.


Bei mir lag das an der Kommunikation von MongoDB über http. Im Detail scheint die SELinux-Policy MongoDB blockiert zu haben. Folgender Befehl hat das Problem bei mir gelöst:


```bash
setsebool -P httpd_can_network_connect on
```
