---
title: Assign contents
---

# Assign contents to initiatives and objectives

<!-- md:version 3.0.0 -->

This feature is available from **OSIRIS 3.0.0** (beta). Administrators determine which forms contain an initiative or objective field and whether selection is required. The fields can be offered for activities, projects and proposals, events, deadlines and infrastructures.

## Select initiatives directly

Create or edit the content. In **Initiatives**, click the relevant entries. Multiple assignments are possible. Click again to remove a selection, then save the form.

Direct assignments work without objectives and without a linked project.

## Select objectives

1. Open **Select objectives** in the form.
2. Search names, identifiers or descriptions. Results are grouped by initiative.
3. Click an entry or its checkbox. You can select several objectives.
4. Selected entries appear above the selector button. Use the **×** on an entry to remove it.
5. Save the form to persist the assignments.

The selector also supports keyboard use: Tab moves between controls, Enter opens the selector button, Space toggles a focused checkbox and Escape closes the open selector.

### Which initiative is linked?

You do not need to select the parent initiative separately. OSIRIS automatically saves it from the objective assignment. If both fields are present, direct initiatives and those derived from objectives are combined.

Removing an objective also removes its automatically derived initiative unless it remains through another selected objective or a direct selection. Linking an activity to a project does not replace initiative/objective selection on the activity.

## Activity dates and past entries

For **activities**, regular selection follows the activity date rather than the day of data entry. Enter the date first. Changing it automatically updates the selection.

For example, in February you can assign a publication from the previous year to the objective applicable at that time. Both the objective's and its initiative's periods are considered. Archived entries are excluded from regular selection. Previously saved assignments remain when editing, even if they are now outside the selection.

With the additional permission to assign outside the period, **Show all** becomes available. It lets you search and select non-matching or archived entries. They are marked **Outside the selection for this date**. Use **Show matching** to switch back.

This date-aware selection currently applies to activity forms. Selection in the other supported forms is not restricted by date in the same way.

## If something is missing

- **No selector:** It has not been configured for this type or project phase. Ask your administrator.
- **No matching entry:** Check the activity date, search, period and archive status. An appropriately authorised person can help with exceptions.
- **Selection required but no suitable assignment:** Use only an institute-approved fallback such as “Other”. Do not create a meaningless objective just to complete the form.

[Overview](index.md) · [Analysis](statistics.md)
