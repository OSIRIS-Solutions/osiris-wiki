# Topic Details

## `GET /portfolio/topic/{id}`

Returns an institutional topic profile, related counters, and navigation.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored institutional research topic ID, not an OpenAlex topic ID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/topic/biology?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 11,
  "data": {
    "id": "biology",
    "name": "Biology",
    "name_de": "Biologie",
    "subtitle": "Research in biology",
    "subtitle_de": null,
    "description": "Example description",
    "description_de": null,
    "color": "#368c73",
    "numbers": {
      "publications": 12,
      "activities": 3,
      "persons": 4,
      "units": 1,
      "projects": 2,
      "collaborators": 2
    },
    "nav_topics": [],
    "nav_units": []
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Stored topic identifier. |
| `name, name_de` | English/German topic name. |
| `subtitle, subtitle_de` | Stored bilingual subtitle. |
| `description, description_de` | Stored bilingual description. |
| `color` | Configured display color. |
| `image` | Stored topic image, if present. |
| `numbers` | Counts named `publications`, `activities`, `persons`, `units`, `projects`, and `collaborators`; see below. |
| `spectrum` | Optional aggregated spectrum when `portfolio-spectrum` is enabled. |
| `news` | Optional public topic news up to today, projected as `{id,title,title_de,teaser,teaser_de,date}`, sorted newest first. |
| `nav_topics, nav_units` | Topic navigation and flattened unit hierarchy. |

#### Response Behavior

Looks up exact topic `id` with `inactive != true`. The route does not explicitly emit a not-found error: a missing/inactive topic can still receive counters and navigation.

Publication and activity counts exclude hidden records; activity counts use Portfolio-enabled non-publication subtypes. Person counts require active visible persons; unit counts exclude hidden groups; project counts require `public=true`. These counters do not apply workflow visibility. Distinct collaborator organizations are counted across topic projects without a public-project filter, but only if the public-project count is nonzero.

`nav_topics` supplies the Portfolio topic list. `nav_units` is a flattened navigation hierarchy with `id`, `parent`, `name`, `name_de`, `order`, `level`, `active`, `open`, `openable`, and `hide` per row. Navigation is built from Portfolio's cached/fetched topic and unit data.
