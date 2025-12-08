---
tags:
  - Reporting
  - Scopus
  - SJR
  - Web of Science
---

# Impact factors

Impact factors (IF) are still important today for many institutions and researchers to assess the quality and influence of publications. In OSIRIS, the impact factors are automatically added to the journals if they are available. However, it should be noted that this is not the widely used Journal Impact Factor from Web of Science (Clarivate), but the 2-Year Citedness Index from Skopus, which is made available via the [Scimago Journal Rank](https://www.scimagojr.com/journalrank.php) (SJR). This index is an alternative metric based on the number of citations in the past two years and is considered useful by many researchers.

## Why not use the Journal Impact Factor?

OSIRIS is open source and focusses on transparency and openness. The Journal Impact Factor from Web of Science is a proprietary index that is chargeable and not available for all journals. Even if your institute has a licence for Web of Science, automatic access (via an API) to the Journal Impact Factor is often not possible, as this requires an additional paid licence. For an open source solution such as OSIRIS, which is intended especially for smaller institutes and research-related organisations, this is not practicable and does not comply with our principles of openness and transparency.

In contrast, the Scimago Journal Rank is freely accessible and offers a transparent calculation method based on publicly available data. We have therefore decided to use the Scimago Journal Rank as the standard impact factor in OSIRIS. However, there is the limitation that Scimago does not offer an API to retrieve the data automatically. However, it is possible to export the data as a CSV file, which is why we download the data from Scimago once a year and import it into the public OSIRIS API. This means that the impact factors for all journals listed in Scimago are also available free of charge for your OSIRIS instance. This is a service provided by OSIRIS Solutions GmbH and is accessible to all OSIRIS users.

