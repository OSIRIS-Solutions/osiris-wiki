# Search Index

## `GET /portfolio/search-index`

Returns a complete grouped index for client-side Portfolio search.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/search-index?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 7,
  "data": {
    "publications": [
      {
        "id": "665ef1234567890abcdef1234",
        "search": "example publication",
        "type": "publication",
        "subtype": "article",
        "year": 2026,
        "month": 3,
        "day": 1
      }
    ],
    "projects": [
      {
        "id": "665ef1234567890abcdef1235",
        "name": "EXAMPLE",
        "title": "Example project",
        "search": "example project"
      }
    ],
    "persons": [
      {
        "id": "665ef1234567890abcdef1234",
        "displayname": "Alex Example",
        "position": "Researcher",
        "search": "alex example researcher"
      }
    ],
    "units": [
      {
        "id": "BIO",
        "name": "Biology",
        "unit": "department",
        "level": 1,
        "search": "bio biology"
      }
    ],
    "infrastructures": [
      {
        "id": "example-lab",
        "name": "Example Laboratory",
        "search": "example laboratory"
      }
    ],
    "topics": [
      {
        "id": "665ef1234567890abcdef1235",
        "name": "Biology",
        "search": "biology"
      }
    ],
    "spectrum": []
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `publications` | Non-hidden affiliated publications subject to workflow restrictions: `id`, `search` from `rendered.plain`, `type`, `subtype`, `year`, `month`, and `day`. |
| `projects` | Public projects when enabled: `id`, `acronym`, bilingual name/title fields, and normalized `search`. |
| `persons` | Active visible persons: `id`, `displayname`, `academic_title`, bilingual positions, and normalized `search`. |
| `units` | Visible groups: stored `id`, bilingual names, unit-type code, `level`, and normalized `search`. |
| `infrastructures` | Public infrastructures when enabled: stored `id`, bilingual name/subtitle fields, and normalized `search`. |
| `topics` | Topics with `hide != true`: MongoDB string `id`, bilingual name/subtitle fields, and normalized `search`. Unlike `/portfolio/topics`, this does not filter `inactive` and does not return the stored topic ID. |
| `spectrum` | When enabled: topic summaries `{id,count,name,path,search}` from affiliated activities with non-empty OpenAlex assignments. No hide/type/workflow restriction applies to this group. |

#### Response Behavior

This endpoint returns all seven groups, including empty arrays for disabled/unavailable groups. It does not accept a search query; filtering is performed by the client. Response `count` is 7 for the seven top-level groups, not the total number of searchable records. Do not use shared pagination on this grouped object.

Normalized search strings combine supported display text, remove HTML, decode HTML entities, collapse whitespace, and lowercase the result. Publication `search` is taken directly from stored `rendered.plain`. No explicit entity sorting is applied except to spectrum groups (count descending, then topic ID).

Topic IDs in this index differ from the institutional topic IDs used in topic detail URLs; consumers must account for this current discrepancy.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
