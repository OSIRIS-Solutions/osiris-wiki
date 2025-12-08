---
tags:
  - Interfaces
  - activities
  - import
---

# Interfaces in OSIRIS

There are various interfaces in OSIRIS that make it possible to import and export data. These interfaces are important for integrating data from other systems or for providing data for external applications. Here is an overview of the most important standard interfaces:

![Interfaces overview](/assets/images/interfaces.png "Interfaces overview")


## Import an activity using PID

Two PIDs (Persistent Identifier) are currently supported: DOI (Digital Object Identifier) and Pubmed ID. These PIDs can be used to import publications and other research activities into OSIRIS. The import is done on the "Add activity" page by entering the PID in the search bar. OSIRIS then searches for the corresponding activity and imports the data automatically. The following happens in the background:

![Importing an activity using PID](/assets/images/import_pid.png "Importing an activity using PID")

### Import via DOI

1. the DOI is checked for possible duplicates in OSIRIS.
2. if no duplicate is found, the DOI is resolved and the publication data is retrieved from the DOI resolver. CrossRef is queried first. If CrossRef does not provide any data, the DOI is retrieved from DataCite.
3. if it is an article in a journal, the following steps are also carried out:
   - The publication is retrieved from OpenAlex using the DOI to determine the OpenAccess status.
   - A check is made to see whether the journal is already available in OSIRIS. The ISSN is used for this purpose.
   - If the journal is not available, it is imported from OpenAlex. The user receives a suggested list of journals found in OpenAlex and can select the appropriate journal, which is then created automatically.
   - If necessary, the journal is also supplemented with the [impact factor](impact.md) via the OSIRIS API, if this is available.
4. the data is entered into the form and the user can still adjust the data before the activity is saved.


## Presentation via interfaces

On 11 June 2025, Julia gave a presentation on the interfaces and standards in OSIRIS. This presentation gives an overview of the different interfaces and how they work and also shows standards available at that time, such as the (KDSF 2.0)[kdsf.md].

[Click here for the presentation!](/assets/documents/OSIRIS Interfaces.pdf){.md-button}