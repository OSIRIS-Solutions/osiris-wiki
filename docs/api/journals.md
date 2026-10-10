# Journals

## `GET /api/journals`

Returns journal table rows with publication counts and the latest stored impact entry.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `limit` | integer | Maximum rows under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

`search`, `json`, `filter`, and `columns[]` are not supported. Use [journal records](journals/records.md) or [journal search](journals/search.md) for filtering.

#### Example Request

```http
GET /api/journals?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "Example Journal",
      "abbr": "Ex. J.",
      "publisher": "Example Press",
      "open_access": true,
      "issn": [
        "1234-5678"
      ],
      "country": "DE",
      "if": {
        "year": 2025,
        "impact": 3.25
      },
      "count": 12
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Journal ID as string. |
| `name` | Stored `journal` title; empty-string fallback. |
| `abbr, publisher, country` | Stored abbreviation, publisher, and country; empty-string fallbacks. |
| `open_access` | Stored `oa` value, or `null`. |
| `issn` | Stored ISSN array, or an empty array. |
| `if` | Complete `impact` entry with the greatest numeric year, or `null` if no entries exist. |
| `count` | Number of activities whose string `journal_id` matches this journal ID. This is separate from the response-level count. |
| `Configured fields` | Additional stored fields configured in `journal-data`. Only field names containing letters, digits, underscores, or hyphens are accepted. Missing fields are omitted. |

#### Response Behavior

Rows are sorted by activity count descending, then by journal name using natural case-insensitive comparison. Impact values are taken from stored data, without an external lookup. The response-level `count` is the number of journals before slicing.
