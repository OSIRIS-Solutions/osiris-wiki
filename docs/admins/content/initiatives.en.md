---
title: Configure initiatives
---

# Configure initiatives

<!-- md:version 3.0.0 -->
<!-- md:feature -->

This guide applies from **OSIRIS 3.0.0**, initially in beta. Your institute may call initiatives “missions” or “working groups”. See the [comparison with research areas and organisational units](../../topics/organizational-structure.md) for their purpose and the [user guide](../../users/initiatives/index.md) for everyday use.

## 1. Enable the feature and adapt its names

1. In the admin area, open **System → Features** (`/admin/features`). Enable initiatives and save.
2. Open **System → Languages** (`/admin/languages`). Use the overrides for each language to adapt the terms to your institute.
3. Adjust at least the singular and plural: `common.initiative` and `common.initiatives`. Objectives use `initiatives.objective` and `initiatives.objectives`. Repeat for the interface languages you need.

Names are changed through language management; there is no separate name input beside the feature switch. Renaming does not change the data structure or permission IDs.

## 2. Assign roles and rights

Open [Roles and rights](../user_roles/roles_rights.md). Choose an existing role or create a role for maintaining initiatives. Assign it to the responsible people; see [Distributing roles](../user_roles/distribute_roles.md).

| Permission | Effect |
| --- | --- |
| View initiatives (`initiatives.view`) | Access initiative and objective overviews, detail pages and statistics. |
| Edit initiatives (`initiatives.edit`) | Create, edit, archive and restore initiatives and objectives; also provides access to their pages. |
| Assign outside the period (`initiatives.select-outside-period`) | Use **Show all** in activities to assign entries outside the regular date selection. |

Displayed names follow your translations. These rights cover initiatives and objectives together. Editing is not limited to a person's memberships or to individual initiatives, so grant it selectively to those responsible for maintaining the data.

Configuring the form requires admin access (`admin.see`). Editing linked activities or projects continues to depend on their respective permissions. Membership alone does not grant editing rights.

## 3. Configure the initiative form

Open **Data Model & Content → Data model** (`/admin/data-model`). The initiative section provides a field overview and a link to the form builder. Alternatively, **Configure form** on the initiative overview opens it directly (`/admin/initiatives/fields`).

Add available modules and [custom fields](../custom_data/custom-fields.md), change their order and properties, and use headings for structure. The saved form applies to all initiatives in the instance and also determines the fields displayed on their detail pages.

**Name and period are system fields.** They remain in the form but can be positioned. An initiative's start is required; an empty end means an open period. Localised variants are available for name, title and description. Use either the simple or localised variant of each field. For required localised fields, the base language is sufficient.

### Memberships

If needed, choose one of these modules:

| Module | Purpose |
| --- | --- |
| `persons` | Members without an additional role. |
| `persons-role` | Members with a role from the **Person roles** vocabulary (`persons-role`). |

Both save the same person assignment and are alternatives. Default roles are **Member**, **Lead** and **Co-Lead**; their labels may differ by language. Extend them through [vocabulary management](../custom_data/vocabulary.md). Explicit memberships can appear in the person's profile affiliations. Contributing to a linked publication does not imply membership.

If direct membership is not needed, omit the person module. Add partners, potential funders, regions and strategy information using suitable modules or custom fields. Do not reuse one stored organisation list for two different meanings; use separate custom fields instead.

## 4. Prepare objectives

Objectives do not have their own form builder. Their fixed inputs are identifier, title, description, type, start and end date. The title is the only required text input; identifiers are currently optional and are not checked for uniqueness. Agree on a naming convention such as `26-01`.

Manage types through the **Objective types** vocabulary (`objective-type`). Its default value is `objective`. Add scientific, political or practical objectives as needed. Each objective belongs to exactly one initiative; memberships are maintained only on the initiative.

## 5. Offer selectors in other forms

Open the form builder for the relevant activity type, project phase or other supported content. Add one of the following modules and save:

| Module | Meaning |
| --- | --- |
| `initiatives` | Directly assign one or more initiatives. |
| `objectives` | Assign one or more objectives. Their parent initiatives are saved automatically. |

The modules support activities, projects and proposals, events, deadlines and infrastructures. They appear only in forms where you add them. Configure each required project phase separately. Selection can be required or optional. Usually one module is sufficient; include both only if both levels of assignment are needed.

Linking an activity to a project does not automatically transfer that project's initiatives or objectives. Make this assignment in the activity form itself.

For activities, regular selection follows the activity date. The additional **Show all** permission allows exceptions. This date restriction currently applies to activities; it is not applied in the same way to the other forms.

!!! tip "Required selection with justified exceptions"
    If every relevant activity needs an explicit assignment, create a fallback initiative such as “Other”. With objective-only selection, it also needs a suitable objective. Omit the module from activity types where assignment is irrelevant.

## 6. Try the configuration

Test with an ordinary user role and a maintenance role: create an initiative and two objectives, assign an activity, change its date and enter an earlier result. Then check detail pages, profile memberships, advanced search and [statistics and exports](../../users/initiatives/statistics.md).

See the user guides for [creating and archiving](../../users/initiatives/manage.md) and [linking contents](../../users/initiatives/linking.md). The [OSIRIS API](../../api/initiatives.md) supports external analysis. Public portfolio interfaces are a separate extension.
