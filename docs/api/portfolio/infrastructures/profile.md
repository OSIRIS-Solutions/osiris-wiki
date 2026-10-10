# Infrastructure Details

## `GET /portfolio/infrastructure/{id}`

Returns a public infrastructure profile and its resolved personnel.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored infrastructure identifier, not a MongoDB ObjectID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/infrastructure/example-lab?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 16,
  "data": {
    "_id": {
      "$oid": "665ef1234567890abcdef1234"
    },
    "id": "example-lab",
    "name": "Example Laboratory",
    "subtitle": "Research services",
    "start_date": "2025-01-01",
    "end_date": null,
    "type_id": "research_infrastructure",
    "logo": "<div class=\"infrastructure-logo-placeholder\"></div>",
    "type": "Local",
    "category": "Research Infrastructure",
    "access": "User access",
    "persons": [],
    "collaborative": false,
    "n_activities": 3,
    "contact_email": "lab@example.org",
    "link": "https://example.org/lab"
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Retained MongoDB ObjectID in Extended JSON form. |
| `id` | Stored infrastructure identifier, not the MongoDB ID. |
| `name, name_de, subtitle, subtitle_de, description, description_de` | Stored bilingual display text. |
| `start_date, end_date` | Stored date range. |
| `logo` | Rendered logo HTML: an embedded data-URL image or a placeholder `<div>`. |
| `type_id` | Raw stored category code. |
| `category, type, access` | Translated vocabulary labels for category, infrastructure type, and access; unresolved values become empty strings. |
| `infrastructure_type, collaborative, coordinator_institute, coordinator_organization` | Retained stored configuration fields. |
| `contact_email, link` | Stored email and external URL. |
| `persons` | Current non-hidden member persons with `id`, `displayname`, `academic_title`, bilingual positions, `depts`, `since`, and bilingual `role`. |
| `coordinator, collaborators` | For collaborative infrastructures: resolved organization summaries `{name,location,ror,type}`. The institute uses `type=self`. |
| `n_activities` | Count of non-hidden activities linked by stored infrastructure ID; no workflow or subtype restriction is added to this counter. |
| `news` | Optional public linked news dated up to today, newest first. |

#### Response Behavior

Looks up stored infrastructure `id` with `public=true`. `_id` remains in the output. Image data is removed after generating `logo` HTML. Personnel with an end timestamp in the past or a hidden/unresolved person are excluded; active status and membership start date are not checked. `since` is the start year.

When the institute coordinates a collaborative infrastructure it becomes the coordinator; otherwise the external coordinator is resolved and the institute is inserted among collaborators. Additional partner references are resolved from the organizations collection.

#### Errors

Missing or non-public infrastructures return `404 DataNotFound` with `msg: "Infrastructure not found"`.
