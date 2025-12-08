---
draft: false
date: 2025-04-04
category: "Community"
authors:
    - jkoblitz
tags: [Release]
---

# Community Meeting on 04.04.2025

Many thanks to everyone who attended the community meeting today!
As promised, here is the summary with the most important information, announcements and discussion points.

## New release: OSIRIS Version 1.2.1

📢 The new version **1.2.1** was released on 03.04.2025 (yesterday). More on 👉🏻 [Github](https://github.com/OSIRIS-Solutions/osiris/releases/tag/v1.4.1).

!!! warning "Important notes"

    There are changes to the **LDAP interface**, therefore the **login should be tested** before going live. In addition, a **maintenance mode** has been introduced to facilitate maintenance.


You can find an overview of the most important new functions here:

<!-- more -->

### 🏢 Organisations

External organisations have been introduced as a separate entity. They can be created automatically via ROR IDs and can be linked to projects and network infrastructures. A new overview page shows all relevant information. ⚠️ Attention: New rights must be assigned for editing, OSIRIS cannot do this automatically.

There was also a question as to whether there is a data field that can be used to add organisations to activities. This is not yet available, but is also planned and will be added in the near future.

### 🧪 Research infrastructures

Research infrastructures can now be created and managed directly in OSIRIS. The data fields are based on the new **KDSF 2.0**. Infrastructures can be linked to persons and activities, including time periods, roles and FTEs. Persons with the "Reporter" role are requested to update their data annually. A statistics page allows analyses as required by the KDSF. Network infrastructures with co-operating institutions can also be mapped. The data is provided via the API. ⚠️ Please remember that infrastructures must be switched on centrally (in the Functions area) and new rights must also be assigned here.

There was also the question of whether usage figures can be transferred automatically from outside. However, this is currently only possible manually, as the external systems often do not offer a suitable interface. In addition, it is perhaps a bit of an effort for a maximum of 5 figures per infrastructure per year.

### ⚙️ Minor improvements

The status field has been extended (e.g. with "in preparation") and an extended logic makes it usable for more use cases. There are also many other small improvements that I could not show in detail, e.g. custom fields now also support clickable URLs.

### 🔐 LDAP Sync

A new LDAP interface allows the read synchronisation of users. Certain attributes can be defined for synchronisation and are then read-only in the OSIRIS UI. There is a PHP script for automatic synchronisation that can be executed with CRON (`php jobs/synchronize_users.php`). A timestamp indicates the last execution. The unique user ID is now saved to avoid problems with name changes - a permanent solution is in the works.

### 🐳 Docker Support

A big thank you to **Paul Gaida**, who packed OSIRIS into Docker! You can find the setup instructions here: 👉 [OSIRIS Docker Setup](https://github.com/OSIRIS-Solutions/osiris/blob/master/docker.md)

An official Docker image on Docker Hub is still pending, but will be delivered later.

Severin (LIB) reported on his container setup with SSL and the ability to perform updates without downtime.

A new test data set was also teased, which can be used for testing purposes and as a development environment in the future. It will then also be made publicly available.

## Discussion & Questions

### 🔓 Open Access Status

It was pointed out once again that the data field "openaccess", which was set by default in the past, does not record the status (e.g. gold or bronze) and is therefore not suitable for the Leibniz query on open access publications. It would be better to replace the data field with openaccess-status. There was further discussion on this point and the wish was expressed to add "diamond" as a status. It was also noted that this field is also supplied by OpenAlex.

### 💡 Idea: Credit Giving in KDSF

Jochen spoke in favour of a more precise mapping of creation performance for activities. Julia explained that this would represent a major expansion, but is possible in principle.

### 📊 Brief outlook: further statistics pages in development

There was also a brief outlook on the current development, which will add various statistics. These are based on the statistics of the infrastructures. There is already a comprehensive statistics page for teaching and work is currently underway on activities and projects.

> 📬 There was also the question of how best to communicate **change requests and problems**. The best way is of course [via ticket (issue) in Github](https://github.com/OSIRIS-Solutions/osiris/issues), as it is open and transparent for all users and code changes and releases can be linked to it. This makes it possible to track later on which problems were fixed with which updates. If no Github account can/will be used, you are of course also welcome to communicate by email. However, please make sure that the problem is described in the best possible way (especially with the question in mind: how can the problem be reproduced?).

If you have any additions or corrections, please let us know. Thank you for your participation - we look forward to the next time! 🙌

## 📅 Save the Date

You can already make a note of the next date in your diary: **18 June 2025 at 13:00** (this time on a Wednesday, as Thursday is a public holiday in some federal states ☺️).