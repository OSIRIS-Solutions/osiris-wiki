---
status: false
tags:
    - projects
---
<!-- md:version 1.2.2 -->
<!-- md:feature -->

# Manage projects

With the **Project** function, OSIRIS offers the possibility to keep track of and manage every step of a project applied for at the institute. This page explains the settings you can make for the project function in the admin area. You can find explanations on how to use it [here](/users/projects/).

Projects are an optional function and must be switched on in the admin area under **Settings &#8594 Functions**. The *Projects* button then appears in the menu on the left under *Data*.

Various categories of projects can be created and existing ones edited under **Content &#8594 Projects**.

## Creating a new project category

In the admin area, you can create different categories of projects. This can be particularly useful if there are other types of projects at your institute in addition to third-party funded projects that you would like to record with OSIRIS.  

![New project](screenshots/project_new.png)
///caption
Button to create a new project category
///

As with [Activity categories](/admins/content/activities/#create-new-category), you must also enter the following information here:

- **Unique ID:** Choose an ID for your project category that is unique
- Icon:** The link will take you to a page where you can choose a suitable icon for your category. All you have to do is copy the code into the field
- **Colour:** Choose a colour for your category
- **Name:** Give your project category a unique and short name

In addition to the mandatory information, you can make many different settings for your new category.

![Create project](screenshots/projects_ertsellen.png)
///caption
Here you have the option of making various settings for your new project category
///

- Deactivate:** You have the option to deactivate this project category. Deactivated project categories are retained for past activities, but no new ones can be added.
- Subprojects:** Here you can determine whether users can add subprojects to this category of projects. Sub-projects are projects that are linked to a main project and are displayed in the project overview.
- Applications:** A project can either be created directly or goes through an application phase first. Depending on the type of project, it makes sense to only include new projects as applications or to omit the applications completely. The *Applications* button appears directly above the *Project* button when selected in the menu on the left.
- Notifications:** Here you can set which role of users or which individual users must be informed when a new project or application is created or when changes are made. To do this via e-mail, the [e-mail settings](/admins/email/) must be correctly set up and functional.

Once you have saved your settings, you will be redirected to the phases of your project. The number of different phases depends on whether you have activated the requests function. If this is the case, your project will be divided into four phases. If you deactivate the function, you will only be shown one project phase.

![Project phases](screenshots/project_phases.png)
///caption
The four phases of your project with application function
///

OSIRIS has defined mandatory fields for each phase that cannot be deactivated. Depending on the phase, you have additional fields to choose from, which you can add to the form to be completed. You can also make these mandatory fields by double-clicking on them - they will then be marked with a red star.

![Example Projects Approved](screenshots/projects_approved.png)
///caption
Selection of fields that you can add to the form, here for example the **Short title** field in yellow (optional) and **Full title** in red (mandatory field)
///

As you can see, the [custom fields](/admins/content/custom-fields/) are also displayed for selection in the *Applied for, Approved and Project* phases. You can therefore also define your own fields for projects.  

## Vocabulary

You will find many of the predefined fields in the forms to be filled in as drop-down selections. To edit the selection, you can adjust the [Vocabulary](/admins/content/vocabulary/) for the respective field. The default values often correspond to the KDSF vocabulary and should not be changed.

:exclamation: For technical reasons, it is not possible to delete values. However, you can deactivate them so that they are no longer displayed in the dropdown.

## User rights

There are a variety of rights for **projects and applications** that you can assign individually to user roles. A distinction is made between rights for applications and projects, as well as for specially created entries or all existing entries.

![Projects rights](screenshots/projects_rights.png)
///caption
Overview of the rights for projects and applications that you can assign to the individual user roles in OSIRIS
///

In particular, you should ensure that the **delete** rights are only assigned to selected roles. You should also decide whether all users should have an overview of all current applications and projects or whether the view of entries should be restricted.