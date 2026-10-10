# Unit Cooperation

## `GET /portfolio/unit/{id}/cooperation`

Returns a publication-based cooperation matrix between organizational units.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Exact group ID stored in activity `units`; no root alias or descendant expansion. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/cooperation?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 2,
  "data": {
    "matrix": [
      [
        2,
        1
      ],
      [
        1,
        0
      ]
    ],
    "labels": {
      "BIO": {
        "id": "BIO",
        "name": "Biology",
        "name_de": "Biologie",
        "color": "#368c73",
        "count": 3,
        "selected": true
      },
      "CHEM": {
        "id": "CHEM",
        "name": "Chemistry",
        "name_de": "Chemie",
        "color": "#457abc",
        "count": 1,
        "selected": false
      }
    }
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `matrix` | Symmetric numeric matrix of shared-publication counts. A publication belonging to exactly one unit increments that unit's diagonal. |
| `labels` | Map keyed by unit ID. Each entry contains `id`, `name`, `name_de`, `color`, publication `count`, and `selected`. |
| `count (response)` | Number of label entries supplied by the route, rather than the number of matrix rows or top-level keys. |

#### Response Behavior

Selects non-hidden publications since `CURRENTYEAR - 4` whose `units` directly contain the requested ID. This does not expand to descendants and does not resolve `0` to the root. Hidden/unresolved unit labels are skipped.

Matrix row/column order is the insertion order of the internal pair-count map; label order is separately accumulated during traversal and should not be assumed to be a guaranteed sorted matrix index. Duplicate unit entries can also affect counts.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
