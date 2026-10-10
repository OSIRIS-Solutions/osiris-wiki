# Activity Details

## `GET /portfolio/activity/{id}`

Returns a Portfolio-visible activity with citation exports and resolved links.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Activity MongoDB ObjectID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/activity/665ef1234567890abcdef1234?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 20,
  "data": {
    "id": "665ef1234567890abcdef1234",
    "type": "publication",
    "subtype": "article",
    "year": 2026,
    "month": 3,
    "abstract": null,
    "doi": "10.1234/example",
    "pubmed": null,
    "title": "Example publication",
    "authors": [
      {
        "id": "665ef1234567890abcdef1235",
        "name": "Alex Example",
        "orcid": null
      }
    ],
    "depts": [],
    "projects": [],
    "affiliated": true,
    "typeStr": "Publications",
    "subtypeStr": "Journal Article",
    "fields": [
      {
        "id": "journal",
        "key_en": "Journal",
        "key_de": "Zeitschrift",
        "value": "Example Journal"
      }
    ],
    "print": "Example citation",
    "bibtex": "@article{example, title={Example publication}}",
    "ris": "TY  - JOUR\nTI  - Example publication\nER  -",
    "connected_activities": []
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `id, type, subtype, year, month` | String activity ID, stored category/subtype, and nullable date components. |
| `abstract, doi, pubmed` | Stored abstract and publication identifiers, or null. |
| `title, typeStr, subtypeStr` | Rendered title and display labels, with stored-code fallback for labels. |
| `authors` | Author objects `{id,name,orcid}`. Person ID is null if unresolved or hidden; ORCID can fall back to the person record. |
| `affiliated` | Computed from author/editor `aoi` flags, rather than copied from the stored activity flag. |
| `depts` | Level-1 department map `{group_id:{en,de}}`; empty map is serialized as `[]`. |
| `projects` | Resolved linked project summaries: `id`, `name`, `title`, `funder`, `funding_organization`, `funding_number`, `role`, and legacy `start`/`end`. |
| `fields` | Configured public module fields: `{id,key_en,key_de,value}`. Values may contain formatted text or HTML. |
| `journal` | Optional resolved journal `{id,name,issn}`. |
| `organizations` | Optional resolved organizations `{id,name,country,location,synonyms,ror}`. |
| `event` | Optional event `{id,name,title,location,country,start,end,link}`. |
| `print, bibtex, ris` | Rendered print HTML and generated BibTeX/RIS export strings. |
| `infrastructures` | Optional linked infrastructure summaries `{id,name,subtitle,start,end}` when the infrastructure feature is enabled. |
| `topics` | Optional active institutional topic objects `{id,name,name_de,color}`. |
| `connected_activities` | Visible Portfolio-enabled connected activities: `id`, `icon`, `html`, `print`, `title`, `subtitle`, and translated `relationship`. |
| `spectrum` | Optional stored `openalex.topics` array, not the aggregated spectrum schema. |
| `news` | Optional public linked news dated up to today, newest first. |

#### Response Behavior

Requires an ObjectID, `hide != true`, and a subtype enabled for Portfolio. Public module fields depend on the configured subtype and portfolio field selection; unresolved/empty formatted values are skipped. No query parameter chooses those fields.

Related project/infrastructure lookups and connected activities do not necessarily apply the same public/workflow checks as the primary record. Connected activities are checked for hide/subtype, but not separately filtered by workflow status.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.

#### Errors

Missing, hidden, disabled-subtype, or workflow-excluded activities return `404 DataNotFound` with `msg: "Activity not found"`.
