---
tags:
  - Troubleshooting
  - MongoDB
---


# Troubleshooting


In the event that something goes wrong, I have collected a few tips here that have already helped me or other users.

### Apparently OSIRIS cannot establish a connection to MongoDB. I get the error `No suitable servers found`.


In my case this was due to the communication of MongoDB via http. In detail, the SELinux policy seems to have blocked MongoDB. The following command solved the problem for me:


```bash
setsebool -P httpd_can_network_connect on
```
