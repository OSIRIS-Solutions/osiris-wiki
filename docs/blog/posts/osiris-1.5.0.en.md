---
draft: false
date: 2025-07-31
category: "Release"
authors:
    - jkoblitz
links:
  - Changelog: "../../home/news/#version-150"
tags: [Release]
---

# OSIRIS version 1.5.0 is now available!

Finally. It took `267` commits and more than `32,000` lines of code, but OSIRIS version 1.5.0 is now officially released! 🎉 Thank you for your patience and support along the way.

## :octicons-rocket-16: What's new in version 1.5.0?

In this version we have once again outdone ourselves and implemented a lot of new features, improvements and bug fixes. We are particularly proud of the changes to the admin interface, which make OSIRIS even more flexible and user-friendly.

You can find the complete list of changes in the [changelog](/home/news.md). Here are the highlights:

- Completely revised project and application management: projects now map the entire life cycle, including configurable types, custom fields, application workflow with status, filter and statistics functions as well as improved presentation. There are also applications as a new entity: management of requested, approved and rejected projects including uploads, rights and status changes.
- Custom fields everywhere: Custom fields can now be defined for projects, persons and infrastructures - via a central configuration in the admin area.
- New admin area "Contents": Overview of all categories, fields, templates & vocabularies. Many settings have been moved here from other areas.
- Advanced notifications & messaging: Messages to roles or individual users, improved display and optional email notifications for certain actions.
- Improved people and infrastructure display: Configurable fields, visibility options, new vocabularies and better filters.
- Events & research trips: New visualisations (timelines), filter options, formatting and activity linking.
- Fine-tuning of the presentation: New templates for activities, controllable visibility of categories, APA styles, new widgets (e.g. confetti timeline).
- Impact factors can be updated: New page for journal metrics, graphs for quartile progressions, manual customisations possible.
- Countless bug fixes and minor improvements: Over 100 minor and major bug fixes and a total of 20 GitHub issues closed.
- and much more!

## :octicons-alert-16: What should be considered?

Please note that the upgrade to version 1.5.0 requires some changes to the database structure. Make sure you create a full backup of your database and files before upgrading. Projects and applications in particular may be affected by the changes as they now have a new structure.

Please also note that some new features may not be activated on release. You can activate them in the admin area under "Settings" > "Functions". Some features also require additional rights or configurations, which you can also make in the admin area.


## :octicons-code-review-16: What's next?

Even though an incredible number of features are already included in this version, some exciting things have not made it into the release. For example, we are already working on the simplified creation of follow-up projects, relationships between activities and a quality management pipeline that will allow you to guide activities through various review stages. There will also be an ORCID integration later this year, which will allow you to access your ORCID data directly in OSIRIS.