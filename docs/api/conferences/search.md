# Conference Search

## `GET /api/search/conferences`

Queries the `conferences` collection with selectable columns and count/numeric aggregation. See [shared search behavior](../search/index.md) for filtering precedence, array unwinding, aggregation output, and validation errors.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `json` | string | JSON-encoded MongoDB filter. Overrides `filter`. |
| `filter` | object/array | MongoDB filter using bracket notation. |
| `search` | string | Trimmed, case-insensitive regular expression on stored `title`; replaces the supplied filter. |
| `columns[]` | array | Selected fields plus string `id`. Array paths may unwind records; see [shared search behavior](../search/index.md#selecting-columns). |
| `aggregate` | string | Non-empty stored field path to group by. |
| `aggregate_function` | string | `count` (default), `sum`, `mean`, or `median`. |
| `aggregate_value` | string | Numeric stored field path, required for `sum`, `mean`, and `median`. |
| `raw` | flag | No effect for this entity; results are not passed through the project renderer. |
| `limit` | integer | Maximum returned rows; subject to the current [pagination behavior](../index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/search/conferences?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "location": "Berlin",
      "start": "2026-11-01",
      "end": "2026-11-03"
    }
  ]
}
```

#### Default Fields

| Field | Description |
|---|---|
| `id` | Conference ID as string. |
| `name` | Stored `name`, if present; not mapped from `title`. |
| `location` | Stored location, if present. |
| `start, end` | Stored start and end dates, if present. |

Missing stored or rendered fields may be omitted. `count` is the number of projected rows before shared pagination; aggregation instead reports the number of groups. `full` and `formatted` are not supported.

#### Title Search

The default `name` column is not derived from `title`. Local conference records contained `title` but no `name`, so the default response omitted `name`. Select `columns[]=title` to retrieve the stored title, or use `search` to filter it.

```http
GET /api/search/conferences?apikey=YOUR_API_KEY&search=workshop&columns[]=title&columns[]=start
```
