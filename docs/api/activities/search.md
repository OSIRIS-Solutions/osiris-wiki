# Activity Search

## `GET /api/search/activities`

Queries the `activities` collection with selectable columns and count/numeric aggregation. See [shared search behavior](../search/index.md) for filtering precedence, array unwinding, aggregation output, and validation errors.

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
GET /api/search/activities?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "activity": "Rendered activity entry",
      "icon": "<i class=\"ph ph-file-text\"></i>",
      "type": "Publications",
      "subtype": "Journal Article",
      "year": 2025
    }
  ]
}
```

#### Default Fields

| Field | Description |
|---|---|
| `id` | Activity ID as string. |
| `activity, icon` | Rendered web representation and icon (potentially HTML). |
| `type, subtype` | Rendered type and subtype labels. |
| `year` | Stored activity year. |

Missing stored or rendered fields may be omitted. `count` is the number of projected rows before shared pagination; aggregation instead reports the number of groups. `full` and `formatted` are not supported.

#### Activity Columns and Permissions

These selected columns are read from `rendered`, rather than from stored raw fields:

```text
web, print, icon, type, subtype, authors, editors, supervisors,
title, departments
```

Other columns use stored values. For example, `authors` returns rendered author text, while `authors.user` unwinds the stored author array and returns nested author data. Aggregation always uses the raw stored field, so `aggregate=type` groups by codes rather than rendered labels.

```http
GET /api/search/activities?apikey=YOUR_API_KEY&columns[]=title&columns[]=authors.user
```

```json
{
  "status": 200,
  "count": 1,
  "data": [{"id": "665ef1234567890abcdef1234", "title": "Example publication", "authors": {"user": "alex.example"}}]
}
```

Session activity visibility restrictions apply when there is no `apikey` query parameter. This endpoint's aggregation supports an empty filter, unlike the unfiltered aggregation currently implemented by `/api/activities`.
