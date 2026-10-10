# Organizations

## `GET /api/organizations`

Returns a compact list of organizations, optionally filtered or searched by name, synonym, or ROR identifier.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `filter` | object/array | MongoDB filter using bracket notation. Takes precedence over `json` and `search`. |
| `json` | string | JSON-encoded MongoDB filter. Takes precedence over `search`. |
| `search` | string | Trimmed, case-insensitive regular expression on `name` or `synonyms`. ROR identifiers are checked first. |
| `limit` | integer | Maximum rows under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/organizations?apikey=YOUR_API_KEY&search=Example
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "Example University",
      "ror": "https://ror.org/04z8jg394",
      "location": "Berlin"
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Organization ID as string; `_id` is excluded. |
| `name` | Stored organization name, where present. |
| `ror` | Stored ROR identifier, where present. |
| `location` | Stored location, where present. |

#### ROR Search

A full `https://ror.org/…` URL, or a recognized short ROR ID, first triggers an exact `ror` lookup. Short IDs are prefixed with `https://ror.org/`. If a match is found, those results are returned immediately. Otherwise the original search text is used as a regular expression on names and synonyms.

`count` is the number of matching organizations before slicing. No explicit sort is applied. `columns[]`, `aggregate`, and `full` are not supported.
