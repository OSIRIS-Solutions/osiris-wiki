# Journal Records

## `GET /api/journal`

Returns full stored journal documents. Despite its singular name, this endpoint returns a list rather than a single journal.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `search` | string | Case-insensitive regular expression on `journal`, or exact stored `issn` match. Only the title expression is trimmed. |
| `limit` | integer | Maximum returned rows; subject to the current [pagination behavior](../index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/journal?apikey=YOUR_API_KEY&search=Example
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "_id": {
        "$oid": "665ef1234567890abcdef1234"
      },
      "journal": "Example Journal",
      "abbr": "Ex. J.",
      "publisher": "Example Press",
      "issn": [
        "1234-5678"
      ],
      "oa": true,
      "impact": [
        {
          "year": 2025,
          "impact": 3.25
        }
      ]
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Stored ObjectID as Extended JSON; no string `id` is automatically added. |
| `journal, abbr, publisher` | Stored title, abbreviation, and publisher, where present. |
| `issn, oa, country` | Stored ISSNs, open-access value, and country, where present. |
| `impact` | Complete stored impact history, where present. |
| `Other fields` | All other stored fields, including configured fields and metadata, are retained. |

`count` is the number of matching journal documents before slicing. No explicit sort is applied. `json`, `filter`, `columns[]`, and `full` are not supported.
