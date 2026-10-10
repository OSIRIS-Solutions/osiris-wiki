# API Overview

OSIRIS provides two separate APIs. Both remain under the same API documentation section, with independent endpoint references and authentication settings.

| API | Purpose | Endpoint prefix | API-key setting |
|---|---|---|---|
| [OSIRIS API](index.md) | Reporting, exports, integrations, and access to research records. | `/api/` | `apikey` |
| [Portfolio API](portfolio/index.md) | Public research profiles, institutional websites, and Portfolio content with rendered representations and linked entities. | `/portfolio/` | `portfolio_apikey` |

Start with the relevant API's basics for authentication, identifiers, response formats, pagination, and visibility rules. Similar entities do not imply identical response schemas or access rules: for example, the OSIRIS person list includes contact/display fields, while Portfolio person lists and profiles follow separate public display settings.

## OSIRIS API

[API Basics](index.md) · [Activities](activities.md) · [Persons](persons.md) · [Projects](projects.md) · [Search and Aggregation](search/index.md)

## Portfolio API

!!! info "A hosted portfolio for your institution"
    [OSIRIS Portfolio](https://osiris-portfolio.de) is our SaaS solution for presenting institutional research. The service also supports the continued development of open-source OSIRIS.

[Portfolio Basics](portfolio/index.md) · [Settings](portfolio/settings.md) · [Publications](portfolio/publications.md) · [Persons](portfolio/persons.md) · [Units](portfolio/units.md) · [Projects](portfolio/projects.md)
