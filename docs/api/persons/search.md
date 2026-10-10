# Person Search

## `GET /api/search/persons`

Queries the `persons` collection with selectable columns and count/numeric aggregation. See [shared search behavior](../search/index.md) for filtering precedence, array unwinding, aggregation output, and validation errors.

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
GET /api/search/persons?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "first": "Alex",
      "last": "Example",
      "username": "alex.example"
    }
  ]
}
```

#### Default Fields

| Field | Description |
|---|---|
| `id` | Person ID as string. |
| `first, last` | Stored name components. |
| `username` | Stored OSIRIS username. |

Missing stored or rendered fields may be omitted. `count` is the number of projected rows before shared pagination; aggregation instead reports the number of groups. `full` and `formatted` are not supported.

#### Searching Persons

`search` targets `title`, not `first`, `last`, or `username`. Use `json` for person-name filters:

```http
GET /api/search/persons?apikey=YOUR_API_KEY&json=%7B%22last%22%3A%7B%22%24regex%22%3A%22Example%22%2C%22%24options%22%3A%22i%22%7D%7D
```

Unlike [the person list](../persons.md), this route does not add a default `username != null` filter or special handling for current unit memberships and `is_active`.
