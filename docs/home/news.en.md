# News

<time datetime="2026-02-10">10.02.2026</time>
<a class="anchor" href="#version-1.8.1" id="version-1.8.1"></a>

## Version 1.8.1

This update brings important security improvements, many small bug fixes, and noticeable enhancements in settings, organizations, reports, and the admin area.

### Security & Stability

A critical issue has been fixed that allowed users without edit permissions for user profiles to edit, deactivate, or even delete other users’ profiles. We sincerely apologize for this issue and thank OSIRIS user Jochen for bringing it to our attention [#408](https://github.com/OSIRIS-Solutions/osiris/issues/408).

- Consistent escaping of user input on portal pages (People, Infrastructures, Units, Topics) to better protect against malformed or manipulated content (fixes #404)   
- Improved error handling for file uploads, including maximum size validation (#394)
- Upload errors and success messages are now rendered correctly
- Password reset for guest accounts implemented with token validation

### Design, Settings & UI

- New font options, including a header font, in the design settings (#395)
- Long links are now automatically shortened in some cases
- Example button (non-functional) removed

### Organizations & Collaborations

- Frequently used organizations are now also suggested in activities
- Organizations can now be saved with a URL
- “Create Organization” button removed (was not functional) (#393)
- All project collaborations can now be removed (#401)
- Scope display in the project view reactivated (#407)

### Reports & Editor

- Report Builder:
    - Blocks can now be duplicated again (#391)
    - Table aggregations are more robust (strings are handled correctly) (#392)
    - More styling options (bold, links, images)
    - Default filter is now empty (improved initial behavior in overviews)

- Quill Editor:
    - Improved handling of empty lines and multi-line content (#387)

- Validation:
    - Empty strings and line breaks are now handled correctly (#403)

### API, Rendering & Backend

- New “Smart Render” endpoint to re-render content that has not yet been rendered (e.g., externally added activities)
- Improved getAuthors logic for dynamic fields and API search (including editors & supervisors) (#399)

### Minor Fixes

- Confirmation dialog when deleting custom fields (#389) and improved related user experience
- More robust text handling in forms
- Various small UI and display issues fixed

<time datetime="2026-02-01">01.02.2026</time>
<a class="anchor" href="#version-1.8.0" id="version-1.8.0"></a>

## Version 1.8.0

There are many new features and improvements, especially in the areas of customization, portfolio, and organizations. Here are the highlights:

### Design & Customization

Until now, it was only possible to adjust the OSIRIS logo and colors. With this update, the customization capabilities have been significantly expanded:

* **Custom fonts:** You can now use your own fonts in OSIRIS to better align the appearance with your corporate identity.

* **Icons:** Choose from three different icon styles (Standard, Filled, or Two-Tone).

* **Many elements adjustable:** For example, the thickness and color of borders, the rounding of corners, shadows, icons, and much more.

* **Header:** You can now customize the header, e.g., adjust its height, move the logos completely to the footer, or color the OSIRIS logo.

### Portfolio

The OSIRIS Portfolio has been expanded with several useful features:

* It is now possible to activate a public portfolio directly within OSIRIS. This feature can be enabled in the admin area under "General Settings" > "Features." The public portfolio is accessible from the homepage without a login.

* Research areas have been added and can now be displayed in the portfolio and via the API. They have a quickly accessible overview page and are also shown in all linked entities (activities, projects, people, groups).

* Infrastructures have been added and can now be displayed in the portfolio and via the API. They have their own page in the portfolio and are also shown in linked activities and on the personal page of the operational staff.

* A new tab for partners has been added, showing a world map with the locations of partners. This map has been improved to make better use of the available space and to look more appealing on large screens. The map now also supports aggregation of partners by zoom level to improve clarity.

* Additional settings for personal profiles have been added, such as the option to disable the display of additional activities or teaching activities. More preview options have also been added.

It is now also possible to **customize the URL** for the external portfolio. For example, you can use your own domain or more easily integrate the portfolio into an existing website. In the admin area under "General Settings" > "Portfolio," there is a new field called "Portfolio Base URL." If this is filled in, this URL will be used for all links generated in the portfolio; otherwise, OSIRIS will attempt to use a relative URL.

### Organizations and Project Collaborations

* It is now possible to upload logos for organizations, which are then displayed in various areas of OSIRIS (e.g., in projects, on the organization page, in the advanced search).

* For a better user experience when manually creating organizations, coordinates can now be automatically retrieved from the specified location (optionally including the country). The current UI language is used to achieve the best possible results.

* On an organization’s overview page, a revised layout now also includes a map showing the organization’s location.

* Synonyms have been added for organizations to improve findability. For example, the Deutsche Forschungsgemeinschaft can now also be found using “DFG.” These synonyms are also retrieved via ROR.

* The collaboration area in projects has been fundamentally redesigned to seamlessly integrate with the new organization management. Existing organizations can be searched for and added as collaboration partners, or new organizations can be created directly within the project.

### Courses

Courses have been completely redesigned and now offer more features:

* The overview page now displays all courses in a table that is much clearer and provides more information.

* Each course has its own dedicated page showing all details, including all linked activities. Activities using the supervisors module also display their semester hours.

* There is a new page to edit existing courses. A new permission (**Edit Courses**) has been added, which is assigned to all users by default, as before.

* Linking with organizations:

  * Instead of entering organizations manually, they can now be selected from the organization management. Alternatively, there is a link to create new organizations.

  * A suggestion list shows the 5 organizations most frequently added to courses to simplify editing.

  * On the organization’s page, all linked courses are displayed.

  * The organization is also shown on the course statistics page.

> Please note that existing courses cannot automatically be linked to organizations. These must be edited manually to establish the connections. Existing organizations, however, will remain and continue to be displayed.

### Projects and Proposals

**Finances** (Due to the sensitivity of this data, it is only visible in the proposal, not in the project itself)

* Third-party funding has been migrated to a new format that now records not only the planned funds but also the actual expenditures per year.

* There is now an overview of a project’s finances showing planned vs. spent funds per year, including analysis and visualization.

* In a new finance dashboard, admins and project leads can generate various financial reports, e.g., planned vs. spent funds. A new permission (View Financial Information) has been added for this, which is not granted by default.

**Forms & Inputs**

* The maximum length for project short titles has been increased from 30 to 100 characters.

* A new acronym field has been added, which is combined with the short title if present.

* Bug fix: Project type is now correctly displayed in the project form.

* A new option allows marking projects and proposals as collaborative projects, following the KDSF data model. For collaborative projects, the identifier and title of the consortium, as well as the lead role, are now requested. To use the Consortium Project data module, it must first be activated in the configuration under Content > Projects. On the second page, the module can be enabled for all phases where it should apply (e.g., Proposed, Approved, Project Phase).

* A new funding program field is now available, which uses a predefined list instead of free text. The funding programs are managed via a vocabulary list in the admin area.

### Documents from Activities

The document management for activities has been migrated to the new system and significantly improved:

* The Documents section in activities has been completely redesigned, providing a much clearer overview and easier handling.

* It is now possible to specify the type of document and add a description.

* Document types are now managed via a vocabulary list in the admin area.

* A new Documents area provides a centralized view of all documents from activities and projects. This list can also be filtered by document type and file type. A new permission (View Documents) has been added under Views, which is not granted by default.

### Linking Activities

* Activities can now be linked to each other to represent relationships such as “is a translation of”, “is an extended version of”, or “is a preprint of”. Any activity types can be linked.

* Links can be displayed in both directions and appear in a dedicated section within the activity.

* The types of links are currently not configurable, but several common relationships are predefined. Further flexibility may be added based on user feedback.

### Organizational Units

* The KDSF research field classification has now also been added to organizational units in OSIRIS. This allows units to be assigned to specific research fields, further improving KDSF compliance.

* A bug was fixed that prevented proper display of the organizational chart when unit names contained parentheses. Additionally, the organizational chart now also supports the German names of units.

### Journals

* It is now possible to delete journals. A new permission (Delete Journals) has been added for this purpose, which is not granted by default. Only journals that are not linked to any activities can be deleted.

### Visualizations

The visualization formerly known as “**Department Network**” has been revised and renamed to “**Activities Network**” to better reflect its new flexibility.

* Grouping can now be done by different entities. Currently supported: organizations (levels 1 and 2) and research areas; more may be added in future versions. Suggestions are welcome!

* Activities from different categories (e.g., publications, lectures, media contributions) can now be visualized.

* The start year can now be freely chosen (default: 5 years back).

* The finished network can be downloaded as PNG or SVG.

### Other Improvements & Bug Fixes

* On a custom field page, it is now shown which activity and project types use this field and whether it is linked to infrastructures or people.

* The query for first and last authors has been removed from the normal “Authors” module.

* If the edition (a data field in activities) is empty or not a number, no suffix is displayed anymore (e.g., 8th, 2nd, 1st).

* Adding empty authors or editors is no longer possible. Error messages are now also translated into German.

* The layout of the news page has been improved.



<time datetime="2025-12-17">17.12.2025</time>
<a class="anchor" href="#version-1.7.1" id="version-1.7.1"></a>

## Version 1.7.1

BIn this version, bug fixes & Minor Improvements were done:

* The modules `supervisors` and `supervisor-thesis` no longer store entries in the authors list. This allows activities to record supervisors and authors independently. The code has been optimized throughout to ensure this separation. You can now also filter by supervisors in the advanced search.

* Fixed an issue where activity types that should not be publicly visible could still be accessed via their exact ID.

* Activity types that are not displayed in the portfolio no longer have the option to be hidden, preventing confusion since they are already invisible.

* Template Improvements:

  * Conditional templates now support multiple conditions using `&` (AND) and `|` (OR).

  * You can now specify a fallback field if a field is empty, e.g., `{field1|field2}`. If `fied1` is empty, `field2` is used. If `field2` is not a field name, the text is output as-is. You can also force text output using quotes: `{field1| "Default Text"}`.


* The person overview can now also be filtered by roles.

* Activity history has been improved: two differently empty values (e.g., "" and []) are no longer considered different. Certain values, like Open Access, are now displayed more clearly (icons are no longer used in lists). Boolean values can now also be displayed in the history.

<time datetime="2025-12-08">08.12.2025</time>
<a class="anchor" href="#version-1.7.0" id="version-1.7.0"></a>

## Version 1.7.0

### New Module: Nagoya / ABS Compliance

OSIRIS now fully supports the processes related to the Nagoya Protocol and national ABS regulations. The new module helps researchers and ABS officers to document all relevant steps in a traceable, complete, and audit-proof manner.

Highlights:

* **Country Review:** Evaluation of all involved countries (Nagoya parties, internal ABS measures, comments).

* **Scope Tracking for Researchers:** Geographical, temporal, material, and use-related scope — including support for multiple sample collections per country.

* **ABS Evaluation (A/B/C):** Classification per country and automatic project classification.

* **Permits & Documents:** Tracking of PIC, MAT, community consent, and other permits. Upload and manage documents directly in OSIRIS.

* **Shared Notes:** For communication between the project team and the ABS compliance team.

* **Dashboard for ABS Teams:** Overview of open evaluations, missing scopes, pending permits, and submitted documents.

More information about the Nagoya module can be found in our [Wiki](https://wiki.osiris-app.de/en/topics/nagoya/).

### Advanced Search for Almost Everything

The **advanced search** has been expanded and improved:

* **Sharing saved searches:** Saved queries can now be shared with individual roles or globally (note: only with the new queries.global permission). Shared queries are marked with a small flag, sorted to the bottom, and cannot be edited. To keep track of many queries, a search function has been added.

* **Nested queries:** Several issues with nested queries have been fixed, and they now display correctly in the preview.

* **Column selection:** The column selection area is now searchable, making it faster to find the desired columns.

* **Stability improvements:** Various minor improvements make the advanced search more stable overall.

#### Expanded Coverage

Because it works so well, the advanced search has now been extended to the following areas:

* Projects & proposals
* Events
* Journals
* Users

You can access the redesigned search pages by clicking the magnifying glass with a plus in the respective overview.

### DOI Mappings for Publication Types

* Admins can now define custom DOI mappings for publication types from CrossRef and DataCite under Admin > Content > Activities > DOI Mapping.

* These mappings are used when importing an activity via DOI to determine the corresponding activity type in OSIRIS.

* This allows more precise mapping of specific publication types, e.g., "`preprint`" or "`conference-paper`".

> Note: This is a beta feature. Please test it extensively and provide feedback to help us improve it!

<time datetime="2025-11-16">16.11.2025</time>
<a class="anchor" href="#version-1.6.2" id="version-1.6.2"></a>

## Version 1.6.2

This update includes numerous enhancements in performance, form logic, LDAP synchronization, journals, events, reports, and UI design. Here are the most important changes:

### Improved Infrastructure Statistics

In this update, we have overhauled the handling of infrastructure statistics and added a migration script to convert existing statistics into a new format.

Greater flexibility: Previously, different statistic fields could be defined, but they were always applied to all infrastructures. Now, statistics can be defined and managed individually for each infrastructure.

Customizable collection periods: It is now possible to set the period for collecting statistics, e.g., annually, quarterly, monthly, or irregularly. This allows for more precise and context-sensitive data collection.

For irregular collection, the date of data entry is stored, allowing historical tracking.

If the same date is reused, existing data is overwritten.

This increased flexibility makes it possible to tailor statistics more closely to the specific requirements and circumstances of each infrastructure. Visualizations have been updated to support these new capabilities.

Integration with notifications: These new statistics are reflected in notifications sent to reporters when they need to update their data. Irregular statistics are not displayed in notifications, but a shortcut has been added to the “Add” menu to simplify data entry.

### Tables & Downloads

Improved table functionality:

* Better stability

* More download options (e.g., PDF); Excel exports are more widely available (e.g., for journals, events, etc.)

* Partial print functionality added

* Page length can now be adjusted

* Statistic tables now also include download and print buttons

* Many visualizations can now also be downloaded as PNG or SVG (e.g., confetti timelines, co-author networks, word clouds). More formats will be supported in future versions.

### LDAP, Users & Roles
#### Guest Accounts

* New: Full management of guest accounts

  * Create, edit, and delete guest accounts

  * New UI pages for management

  * Guests can log in if LDAP fails

  * Guest accounts can have an expiration date

  * Guest accounts are automatically assigned the “guest” role, which exists by default but has no permissions

#### LDAP Synchronization

* Synchronization UI improved

* LDAP timeout now aborts the sync if LDAP is unreachable

* Unit synchronization improvements:

  * Synonyms for organizational units added and recognized

  * Reactivated units are no longer deleted

  * For new units, the current day is no longer assumed as the start of affiliation

  + The sync preview now shows a warning if units cannot be found

  * New synchronization timestamps displayed in the UI

### Events

* Events can now be **centrally disabled** (Settings > Features).

* **Countries** have been added to events.

* **Event handling in “Add Activity”** has been improved:

  * Duplicates are now detected and prevented based on title and start date.

  * Error messages are now displayed correctly (#253).

  * Event type selection and countries have been added.

  * The event selector has been significantly improved, now showing all available events in a searchable dropdown.

  * When selecting an event, title, date, location, and country are automatically pre-filled. This is now more clearly communicated in the UI with a hint text and a brief highlighting of the fields.

### Forms & Input

* **Checkboxes** in person fields now work correctly (#240).

* **Help texts** are now displayed on hover and appear to the left (#288).

* **Research interests** now show German suggestions correctly (#286).

* Users can no longer highlight activities if they are **not authors** (#290).

* If a highlighted activity is hidden, it is no longer delivered via the Portfolio API.

* The label for **research trips** now appears even if research areas are disabled (#282).

* You can now add **journals without an ISSN**.

* **Magazines** (non-standard journals) now have a suggestion list.

* Journals can now be **renamed centrally** (Settings > Features).

* For **project proposals**, the form fields from “Requested” are now pre-filled into “Approved.”

* If an activity is loaded via DOI but its type does not exist, a **warning** is now displayed prompting the user to select a suitable type.

* **Affiliation check** now also considers affiliated editors (#292).

* The **default icon** for activities has been updated.

* The **full link** is now the default in templates; the shortened link can be used as `link-short` and is the default in table views.

### Queue

* **Issue fixed:** personal queue was not visible to all users.

* The queue is no longer hidden if users have a queue warning in their notifications.

* The **entire queue** is now only visible to users with editor permissions.

* Editors can still see their **own personal queue**.

### Reports & Report Builder

* New default variables for time filters in the Report Builder.

* Tables in the Report Builder can now be sorted.

* Time limit in the Report Builder improved to capture long-term activities.

### Settings, UI & Design

* Rendering language added as a global setting (#267).

* Layout of the feature settings has been revised and improved.

* Icons updated to duotone.

* Typography standardized; headers now include icons.

### Other Improvements

* Research areas can now be deactivated.

* Backend admin users are guaranteed admin rights.

* Rendering no longer breaks if journal names contain parentheses.

* Icons in warnings and error messages now display correctly in all browsers.

* Portfolio preview now works correctly with HTTPS.

<time datetime="2025-11-02">02.11.2025</time>
<a class="anchor" href="#version-1.6.1" id="version-1.6.1"></a>

## Version 1.6.1

We did some minor Improvements:

* The feature management UI has been improved by grouping features into categories and adding descriptions. This makes it easier for administrators to understand and manage the available functions.

* A way to centrally disable events has been added. This can be configured in the admin area under General Settings > Features. When disabled, events are no longer shown in the navigation or on the homepage.

* In the News section of a user’s profile page, new users can now also be displayed. This can be configured in the admin area under General Settings > Features.

* Notifications are now highlighted more clearly: the button in the header turns red when new notifications are present, and a message is displayed on the profile homepage.

### Bug Fixes
#### Forms & Input

* Authors and editors are now only added if the form is empty; all can be deleted if not needed (#254).

* Mandatory fields in project forms are now correctly marked (#276).

* The form builder now allows saving fields that contain only editors.

* Float fields in custom fields now accept decimal numbers, not just integers.

* Some form labels were not correctly overridden by user settings – this has been fixed. Author and editor headings are now displayed correctly.

* It is no longer possible to name a custom field `language` (caused issues with MongoDB).

* Fixed an issue where some pages would not load under certain PHP settings.

* Typos (“Ablehnung”) and minor spelling errors corrected (#275, #277).

#### Activities & Display

* Rendering errors with journal names containing parentheses fixed.

* The “no persons” message no longer appears when editors are present.

* Links to organizational units in activities corrected.

* The link on the homepage for locking activities now works again.

* Countries of collaborators are now displayed in the UI language (#273).

* Associated partners are now visible on the collaboration map and highlighted with colors (#274).

* Raw data is no longer interpreted as HTML (#259).

* Column names in statistics and in the supervisor-thesis field corrected (#270).

* Semester hours (SWS) can now be edited again in the author editor.

#### Users & Organizational Units

* Improved UX when adding organizational units.

* Units now appear only once in the author editor (#268).

* Different employee counts on group pages fixed (#258).

* Values for expertise, CV, and research interests can now be set empty in the person editor.

#### Projects & Proposals

* Project types are now sorted by last updated date.

* “Add project proposal” is now the default option when available.

* Confetti timeline in projects can now be filtered correctly.

#### APIs & Documentation

* English API documentation for the Portfolio added (#266).


<time datetime="2025-10-12">12.10.2025</time>
<a class="anchor" href="#version-1.6.0" id="version-1.6.0"></a>

## Version 1.6.0

### New Quality Workflows for Activities

OSIRIS 1.6.0 introduces a completely new system for quality assurance and review workflows – flexible, open, and directly integrated into activities:

* Admins can define custom workflows and assign them to any activity categories.

* Each workflow consists of freely configurable steps, to which roles and organizational units can be assigned.

* Steps can run in parallel or sequentially – ideal for approvals by department heads and libraries.

* Workflow steps can optionally be restricted to the user’s own organizational unit.

* Completed workflows can be retrospectively assigned (“migrated”) to existing activities.

* Once all steps are completed, the activity is automatically set to the status “verified”, which can be centrally filtered.

Activities can optionally be locked after specific workflow steps.

👉 This makes it possible to map internal review and approval processes directly within OSIRIS – from data entry to publication.


### New Workflow UI in Activities

A new workflow widget has been added to activities that are part of a workflow:

* Compact step bar with visual progress indicators, icons, and tooltips.

* Currently active steps are highlighted; completed steps are sorted to the front.

* A slim, fixed progress bar at the top of the interface continuously displays the workflow status.

* When a workflow is completed, the bar turns green; in case of rejection, it turns red.

* Clicking the bar opens the full workflow widget with all details.

* The widget displays all steps, responsible persons, statuses, and any comments.

* Individual workflow steps are logged in the activity’s history.

### New Reviewer Interface

For roles with review responsibilities (e.g., department heads, library staff), a clear and structured new reviewer dashboard has been introduced:

* Displays all activities currently pending review at a glance.

* Filters by category, role, organizational unit, or status.

* Quick actions to approve or reject directly from the list.

* Notifications automatically appear in the sidebar and in the main navigation.

### Option to Reject Activities

Reviewers can now reject activities if, for example, information is missing or unclear. When an activity is rejected, the following happens:

* The workflow status changes to “rejected.”

* Reviewers can leave a comment and return the activity for revision.

* Authors receive a notification including the reviewer’s comment.

* After making revisions, authors can resubmit the activity for review with a single click and may also include a comment.

* All previously completed approvals remain intact – no data loss and no need to restart the workflow.
 

### Workflow Status Filters

* To ensure that filters and overviews function correctly, a new permission for viewing workflows has been introduced. This permission is not assigned by default. Only users with this permission can see and use workflows.

* A new filter for **workflow status** (pending, in-progress, verified, rejected) is now available in the activity overview. Additionally, the quarter field now displays an indicator when an activity is part of a workflow, showing its status.

* The **Advanced Search** now also includes a workflow status filter.

* The **Download section** includes a new filter for workflow status.

* In the **General Settings**, there is now a new Portfolio tab (if enabled), where admins can define which activities are displayed in the portfolio. There are three options:

  * **All activities** (default)

  * **Only approved activities** (activities with the status verified)

  * **Approved activities and activities without a workflow** (activities with the status verified and those not assigned to any workflow)

### Migration & Data Model

* Legacy workflows can be removed or reset via migration (through the workflow interface).

* Automatic initialization for newly created activities.

### New Mail Digest (Notification Summary)

OSIRIS can now send automatic digest emails on request - clearly structured, bilingual, and in the OSIRIS design:

* Provides an overview of pending activities, messages, and tasks (everything visible in the notifications).

* Frequency can be configured per user: none, daily, weekly, monthly.

* The default behavior can be defined globally by admins.

* Emails are sent automatically via the cron job /`cron/digest`.

* Emails are bilingual (DE/EN), fully UTF-8 encoded, and use the OSIRIS design.

### Activity Drafts

* Users can save activities as drafts and complete them later.

* Drafts are only visible to the user who created them.

* Drafts can be loaded, edited, and saved at any time.

* When a draft is finalized and saved, a new activity is automatically created and the draft is deleted.

* This feature can be enabled or disabled in the admin area under Features.

### Tags for Activities, Projects, and Events

* Activities can now be assigned tags to improve categorization and searchability.

* All activities can be filtered by tags.

* Tags can be added in the activity detail view, as well as during the creation or editing of activities. A new data field called `tags` has been introduced for this purpose.

* A new permission `activities.tags` has been added to the rights management system and is not assigned by default. Only users with this permission can add or remove tags from activities. This also requires that the user has permission to edit the respective activity.

* Tags can be centrally managed in the admin area under Content > Tags. Admins can add new tags, edit existing ones, or delete them there.

* This feature can be enabled or disabled in the admin area under Features.

* Tags can be assigned to activities, projects, and events.

### Improved Report Builder

The Report Builder has been revised and now offers an improved user interface as well as additional features for customizing reports.

* **New variables for reports:**
New variables have been introduced that can be used within report templates. These variables can be defined in the Report Builder and allow dynamic content to be inserted into reports. Both the preview and the generated report support these variables.

* **New activity table block:**
A new building block allows you to create a table of activities together with any additional data field. This makes it possible to generate reports containing specific activity-related information relevant to the report, such as impact factors, publication types, or other custom fields.

* **Improved text module support:**
Text modules can now include formatted text, which is correctly rendered in the report. This allows for more visually appealing and better-structured report content.

* **Sorting options:**
Both the activity block and the impact factor block now support sorting (requested by IfL). Sorting can be configured as ascending or descending.

* **Time limitation enabled by default:**
The time filter is now selected by default to avoid confusion.

* **Enhanced user interface:**
The UI has been improved with collapsible sections and copy functions. Dropdown menus for aggregations have been added, along with clearer descriptions of the building blocks.

* **Improved aggregation tables:**
Aggregation tables now use correct field names and values, and empty fields are displayed more clearly. Additionally, “unwinding” array fields ensures that they are counted individually. This means you can now aggregate by units or research areas even if these are defined as multi-select fields.

### New Data Fields

New data fields have been added for activities:

* **Funding Type:**
A dropdown field that allows selecting the type of funding. The available values can be defined in the vocabulary under funding-type and are the same as those used for projects.

* **Tags:**
A multi-select field that allows adding tags to activities. The available values can be managed in the admin area under Content > Tags.

* **Projects:**
A multi-select field that allows linking projects to activities. Multiple projects can be selected, and funding numbers provided via DOI are automatically matched and linked with the corresponding projects.
A new permission has been introduced that allows linking projects to activities without granting permission to edit the projects.

### Auth Token for User Registration

* An **auth token** has been introduced for Auth-based user registration. This token can be used to validate new user registrations and ensure that only authorized requests are processed.

* The token can be passed via the URL to the registration page and is validated there. Upon successful validation, the user can proceed with registration.

* This enhances the security of the registration process and prevents misuse through unauthorized access.

### Extension for Conditional Templates

The template logic has been extended to support conditional rendering based on multiple fields:

* Use `&` to require that all fields are present.

* Use `|` to render content if any of the specified fields is present.

**Examples**:

* `%title Title: % `→ rendered only if `title` is set.

* `%title&authors by %` → rendered only if both `title` and `authors` are filled in.

* `%journal|conference In: %` → rendered if either `journal` or `conference` is present.

This enables more flexible and context-sensitive templates without requiring additional code logic.

### Bug Fixes and Improvements

* Fixed the visualization of organizational unit networks.

* Authors with multiple first names are now abbreviated correctly.

* Fixed an issue where a deactivated category could still be selected when copying an activity.

* The documentation has been replaced with a new wiki at https://wiki.osiris-app.de/. Old help page links have been updated accordingly.

* Improved the display of the world map of cooperation partners by adjusting layout styling and automatically adapting the map size to the available space. The map now looks better on large screens and makes optimal use of available space.

* Fixed an issue where custom fields without a value always displayed “-”.

* The new logic ensures that a custom field explicitly left empty is rendered as empty. However, if the field does not exist at all (e.g., in older activities or if it was added later), the defined default value will be used.


<time datetime="2025-09-30">30.09.2025</time>
<a class="anchor" href="#version-1.5.2" id="version-1.5.2"></a>

## Version 1.5.2

* **New Conditional Templates:** Using %field text%, the text is now only displayed if the corresponding field is not empty. Example: %details Details:% renders “Details:” only if the Details field is not empty.

* **Enhancements for list fields:** Custom fields of type “List” can now be configured to allow selecting “Other” and specifying it via a free text field. This does not yet work in combination with multiple selection.

* **Distribute Roles:** Assigning roles has become much easier.

* **Password Reset:** In the OSIRIS Auth user management, admins with the appropriate permissions can now centrally reset a password and send the user a generated link.

* **Improved ID Sanitization:** OSIRIS now automatically replaces forbidden characters in IDs (e.g., spaces and periods) during input. This now also applies to custom fields, as IDs containing spaces or periods previously caused issues—especially periods, which led to problems in the MongoDB database used.

### Further Improvements to the Form Builder

* Saving is no longer required to preview changes.

* Overwritten labels are now also used as headings in the activity detail table.

* New data field “Projects”: Projects can now be directly linked within the activity form.

* Multiple projects can be selected.

* Funding numbers provided via DOI are automatically matched and linked with projects.

* There is a new permission that allows linking projects to activities without granting permission to edit the projects.

* The new data field can be added to your activity types via the form builder.

### Activities

* Empty fields are no longer displayed within the main table but instead listed at the bottom as a separate list for improved clarity.

* The maintenance message has been improved.

* The “Add Activity” button is now also available under My Activities.

* The print view is now delivered via the Portfolio API.

* There is a new permission to link projects to activities without being able to edit them. This permission is automatically included when the right to edit projects is granted.

### Bug Fixes

* Fixed an issue that prevented activities from being confirmed.

* Fixed a bug where LDAP attribute synchronization assigned attributes to a random user.

* When adding an activity, selecting a category now correctly selects the first type.

* Emails can now be sent without SMTP authentication.

* Fixed the link to activities within events.

* The SWS calculator has been removed from the `supervisor-thesis` module.

* Advanced Search has been improved and should now be more stable.

* Several issues with the new author templates have been resolved.

* Some achievement texts have been improved or corrected in English.

* Various additional translation and text errors have been corrected.

* NPM JSCDN has been removed to improve security.

* Full admin rights are no longer additionally required to synchronize users.

* The cooperation partner map previously did not load if the institute’s latitude and longitude were not set. A default position is now used to prevent this issue.

<time datetime="2025-08-24">24.08.2025</time>
<a class="anchor" href="#version-1.5.1" id="version-1.5.1"></a>

## Version 1.5.1

### New Form Builder

With the new form builder, you can now design activity forms even more flexibly:

* Free layout design with headings, text paragraphs, and separators

* Custom field labels, help texts, and individual field widths

* Live preview of individual fields and the entire form while editing

* Option to copy an existing form and use it as a template

* In the activity types section, data fields are now displayed as badges with icons and include direct links to the new form builder

👉 This makes configuring activity forms easier than ever!

Many thanks to Jochen Knaus for thoroughly testing this feature and providing valuable feedback! 🙏

### Improved Selection Fields (Multi-Select)

* Multi-select fields are now much more intuitive: instead of holding down the Ctrl key, you can simply use checkboxes.

* The field now takes up significantly less space.

* A bug was fixed where interface language settings led to inconsistent values being stored in the database. The English value is now always stored correctly. Additionally, multi-select fields are now properly translated and displayed in the German interface.

### Better Support for Theses

+ A new module called `supervisor-thesis` has been introduced. It functions as an author field (stored in authors). It complements the existing `supervisor` field, which requires semester credit hours. The new field instead stores the supervisor’s role.

* Supervisors are now displayed correctly in the author/editor interface.

### Editor Issues Fixed

* The author editor (Activity > Edit Authors) previously did not work correctly for editors and overwrote authors instead. This has now been fixed.

* Editors were previously unable to confirm their activities. This issue has been resolved—they now correctly see and confirm their activities via notifications.

* Editors are now correctly counted in activities and units.

* Semester credit hours (SWS) and supervisor roles can now also be adjusted in the author editor interface.

### UI/UX Improvements

* Improved display of grouped lists with updated CSS

* New tooltip help texts in activity forms

<time datetime="2025-07-31">31.07.2025</time>
<a class="anchor" href="#version-1.5.0" id="version-1.5.0"></a>

## Version 1.5.0

In this update, projects have been completely revised and made significantly more flexible. Data fields can now be fully defined for both projects and persons. Custom fields are now also available for projects, persons, and infrastructures.

### Improvements in the Admin Area

There is a new page in the admin area called **“Content”**, where all editable OSIRIS content is listed. You will now find the categories for persons, activities, projects, and infrastructures there, as well as custom fields and vocabularies. There are also links to helper tools, such as the list of data fields and templates. Please note that some features previously located under General Settings and Features have been moved here.

The **Features** section has been moved to the general settings. It now offers significantly more configuration options. For example, quarterly controlling can be disabled, the automatic retrieval of journal metrics can be prevented, the calendar and courses can be removed from the navigation, and the import from OpenAlex or Google Scholar can be disabled.

A new option has been added to customize the website **footer**. The content of the legal notice and privacy policy can (and should!) be edited, and links to external resources (e.g., company agreements) can be added.

### Notifications and Messages

* The navigation has been revised: notifications are now located in the header instead of on the dashboard.

* In addition to system notifications, targeted messages can now be sent to specific users or roles.

* Received messages can be marked as read or deleted.

* The message display has been improved and the styling modernized.

* OSIRIS can now optionally send email notifications (e.g., when projects are created or edited). This can be configured in the admin interface.

### Person Settings

* Person settings are now located under **Content**.

* Data fields for persons can be customized and custom fields can be added.

* Positions are now also managed in this new section (previously under General Settings), as are the settings for coins and achievements (previously under Features).

* A new data field called “Keywords” has been added. It can be used similarly to expertise but works with a defined vocabulary. The name of this field can also be defined in the admin area.

* It can be specified whether users are allowed to register themselves or whether only admins can create accounts.

* The selection of organizational units when assigning persons has been improved.

* When creating a person, the system no longer asks for a password if the AUTH add-on is not selected as the user management method (e.g., when LDAP or OAuth2 is used).

### Research Infrastructures

* Data fields for infrastructures are now configurable (under Content).

* Custom fields are also supported.

* Overview tables for infrastructures now include filter and search functions.

* The display of joint infrastructures and cooperation partners has been improved.

* The term “Infrastructures” can now be customized in the admin area.

* Research areas can now also be added to infrastructures and used for filtering.

* Annual statistics for infrastructures are now configurable and can be adjusted via the vocabulary.

* Infrastructure statistics have been improved, can now be filtered by year, and use December 31 of the previous year as the start date. Custom-defined statistical fields are also supported via the vocabulary.

### Projects and Project Applications

* Projects have been completely revised and now cover the entire project lifecycle:

* Project types are configurable (Content > Projects), including their associated data fields.

* Vocabularies such as funding categories can be freely defined.

* Funders, universities, and cooperation partners can be linked via ROR IDs.

* Custom fields are supported for projects.

* Improved support for multilingual input fields (e.g., project titles, abstracts).

* Subprojects have been improved and now display the cooperation partners of the parent project. Automatic inheritance has been removed to increase flexibility.

### Project Applications

* New entity “Project Application” introduced, including status management (Applied, Approved, Rejected).

* Forms now display only the relevant fields depending on the application status.

* New application overview with filtering options.

* Approved applications can be converted into projects.

* New fields have been added to track submission and approval dates.

* Funding information (e.g., third-party funding income) can be maintained for applications.

* The permission system for editing and viewing applications has been refined.

* Documents can be uploaded and managed for applications. New permissions have been introduced for uploading and globally editing documents.

### Additional Improvements

* Improved display and management of project participants, including extended role vocabularies.

* Optimized layout and clearer structure on edit pages.

* Expanded filter and statistics functions (e.g., search by project type or funding status).

* It is now possible to notify specific users or roles when creating or editing a project—either within OSIRIS via the new notification system or via email.

### Display and Visibility of Activities
#### New Templates

* New templates have been added for formatting activities, especially for authors and editors. This makes it easier to display authorships in APA format and other styles. Instructions can be found under Content > Template Builder.

* Additional options have been added for displaying DOIs (e.g., as links or plain text), also configurable in the Template Builder.

* It is now possible to adjust the formatting of affiliated authors. In the general settings, a dropdown menu allows you to choose whether affiliated authors are displayed in bold, italics, underlined, or a combination.

* The visibility of individual activity categories can now be configured. Go to Content > Activities, select a category (e.g., Publications), and click “Edit.” You can define which role is allowed to see activities of this category. By default, this is set to “All.” Users can always see their own activities regardless of the visibility setting. This setting is available at category level only, not at type level.

* Document uploads can now be disabled for individual activity categories. This setting is also available under Content > Activities. By default, uploads are enabled. If disabled, the upload button will no longer appear in the activity detail view, and no documents can be uploaded. Existing documents will not be deleted but will no longer be displayed.

* New activity analytics widget: Confetti Timeline. Currently available for research areas and organizational units, with support for additional entities planned. It displays activities on a timeline and allows filtering by category. The filter also affects the table displayed below.

* A new button has been added to copy the formatted activity entry to the clipboard—useful for quickly sharing information.

* Additional data fields for activities:
  * Organization: Select an organization involved in the activity via ROR. The organization’s location can also be displayed in templates.
  * Organizations: Same as above, but allows selecting multiple organizations.

### Improved Event Management

* Events can now be edited.

* The event display interface has been improved.

* It is now possible to create a new activity directly from the event page.

* A formatted description field and an event type have been added. Event types can be defined via the vocabulary and used as filters.

* Research areas can now be assigned to events, with a corresponding filter.

* A new filter for the year in which the event takes place has been added.

* A new interactive timeline visualization is displayed above the events list and can be filtered by year.

### Research Areas

* Research areas can now be assigned to organizational units.

* The web interface for research areas has been improved and now follows the tab-based approach used elsewhere.

* New visualizations have been added: a network view of linked authors, a word map, and a confetti timeline.

* If no research areas exist, the research area filter widget will no longer be displayed.

### Research Trips

* A new add-on has been introduced for analyzing research trips.

- To activate it:
  1. In the admin area under **Content**, create an activity type with the ID travel. The name can be freely chosen and determines the label in the navigation.
  2. Assign the following fields to this type:
    - `date-range`
    - `countries or country`
    - `authors or scientist`
  3. Activate the **Research Trips** add-on in the general settings under Features.

### Further Improvements and Bug Fixes

* New settings page for customizing the footer (legal notice, privacy policy, external links).

* Improved activity locking (easier to find, individual locking/unlocking possible).

* Portfolio preview revised and now based on actual API data.

* DOI data is now correctly applied when changing activity type.

* Controlling page removed (contained outdated information).

* Email settings can now be saved correctly; full SMTP support.

* Improved visual style of notifications.

* Global control of quarterly reporting (enabled by default).

* Numerous layout and display optimizations.

* Improved filtering and sorting of projects, applications, activities, and infrastructures.

* Filter badges now show the number of entries per category.

* Fixed visibility issues when publishing activities.

* If only an English feature label is set, it is now also shown in German.

* Fixed incorrect display of organizations and partners in lists.

* Improved infrastructure editing permissions (including “edit-own”).

* Improved organization management.

* Inactive users are now sorted to the bottom in selection lists.

* Additional fields added for advanced search.

* Improved admin sidebar navigation.

* Fixed duplicated metric years on refresh.

* Latest JCR impact factor now shown in journal list.

* Correct language for CV headings; replaced hard-coded categories.

* Fixed sorting and display issues in “My Year.”

* Types can now be sorted.

* Fixed journal listing issue without impact factors.

* Fixed incorrect activity display for groups.

* Confirmed quarters now shown in profile table.

* No password prompt on shared profile pages.

* Default colors used if no color assigned to organizational units.*

* Fixed deletion bug for organization unit leadership.

* Fixed table search bug with spaces after reload.

* Organizational units now consistently sorted.

* Fixed issue with certain DataCite DOIs not being processed correctly.