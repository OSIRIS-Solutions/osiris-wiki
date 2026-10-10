# Teaching

## `GET /api/teaching`

Returns teaching modules from the teaching catalogue.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `search` | string | Case-insensitive regular expression on `title`, or exact `module` match. Only the title expression is trimmed. |
| `limit` | integer | Maximum rows under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/teaching?apikey=YOUR_API_KEY&search=biology
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "title": "Introduction to Biology",
      "module": "BIO-101",
      "semester": "Winter",
      "affiliation": "Example University"
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Teaching record ID as string. |
| `title, module, semester` | Stored course title, module identifier, and semester; empty-string fallbacks. |
| `affiliation` | Organization name if the stored value is a valid ObjectID; otherwise the stored value. Missing or unresolved affiliations become an empty string. |

`count` is the number of matching modules before slicing. No explicit sort is applied. `json`, `filter`, and `columns[]` are not supported.
