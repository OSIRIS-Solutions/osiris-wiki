# Proposal Search

## `GET /api/search/proposals`

Queries the `proposals` collection with selectable columns and count/numeric aggregation. See [shared search behavior](../search/index.md) for filtering precedence, array unwinding, aggregation output, and validation errors.

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
| `raw` | flag | Skips project/proposal display formatting when present. |
| `limit` | integer | Maximum returned rows; subject to the current [pagination behavior](../index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/search/proposals?apikey=YOUR_API_KEY&raw=1
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "EXAMPLE",
      "title": "Example proposal",
      "type": "third-party"
    }
  ]
}
```

#### Default Fields

| Field | Description |
|---|---|
| `id` | Proposal ID as string. |
| `name, title` | Proposal identifier and title. |
| `type` | Stored type code with `raw`; display label otherwise. |

Missing stored or rendered fields may be omitted. `count` is the number of projected rows before shared pagination; aggregation instead reports the number of groups. `full` and `formatted` are not supported.

#### Display Mode and Permissions

Without `raw`, all projected fields are passed through the project renderer. Empty values become `"-"`, type codes become display labels, and other fields may become HTML, joined text, or JSON-encoded strings. With `raw=1`, stored values and types are retained.

The `proposals.view` permission is checked in addition to the API key. Without it, only records linked to or created by the session username are included. The local API-key-only checks returned empty lists; the non-empty example is based on the code's projection. Unlike `GET /api/proposals`, this route supports numeric aggregate functions and builds no empty `$match` for unfiltered aggregation.
