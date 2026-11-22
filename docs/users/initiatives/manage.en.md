---
title: Create and maintain
---

# Create and maintain initiatives and objectives

<!-- md:version 3.0.0 -->

This guide applies from **OSIRIS 3.0.0** (beta). You need **Edit initiatives** permission. Administrators can grant it to your role; membership alone is insufficient. Names and form fields may differ at your institute.

## Create an initiative

1. Open the initiative overview and click **New**.
2. Enter its name and start. Leave the end empty for an open period.
3. Complete the other configured fields, such as a longer title, description or members with roles. Required fields marked in red must be completed.
4. Save. You are taken to the initiative's detail page.

Your administrator configures this form. For localised fields, use the language switch to add translations; required fields need the base language.

## Add an objective

1. Open the parent initiative's detail page.
2. Click **Add objective** in the **Objectives** section.
3. Enter a title and optionally an identifier, description and period. Select a type from the available vocabulary.
4. Save. The objective now has its own detail page and also appears under its initiative.

The parent initiative is set when creating the objective. Start and end are initially filled from the initiative where available; adjust them if the objective covers a different period. An empty end date means an open period.

An identifier such as `26-01` helps with search and selection. It is currently optional and is not checked for uniqueness. Follow your institute's convention. Your administrator can extend the type vocabulary.

Objectives have no members of their own. For a new annual goal, create a new objective instead of overwriting the previous year's goal. Earlier assignments then remain understandable.

## Edit

Open the initiative or objective's detail page and select **Edit**. Each has its own editing page. Saving returns you to the respective entry.

Changing an initiative does not automatically change the stored periods of its objectives. Date-aware activity selection considers the periods of both levels.

## Complete, archive and restore

Reaching the end date marks an entry as expired. **Archive** is an additional explicit status change.

1. Open **Edit** on the entry.
2. In **Status**, open **Archive** and confirm.
3. To bring it back, open the same entry, choose **Restore** and confirm.

Archiving does not remove existing relationships. Past entries remain accessible through the overview. **Restore** does not change the end date: an expired entry remains outside regular selection for current activities. Archiving an initiative does not individually change its objectives' stored statuses; however, activity selection also considers the parent initiative's status.

[Overview](index.md) · [Assign contents](linking.md)
