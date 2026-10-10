# Projects by Funding Number

## `GET /api/projects-by-funding-number`

Finds projects by exact stored `funding_number` values.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `number` | string or array | Required. Comma-separated values or repeated `number[]` parameters. Each value is URL-decoded again in the route. |
| `limit` | integer | Maximum returned rows; subject to the current [pagination behavior](../index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/projects-by-funding-number?apikey=YOUR_API_KEY&number[]=GRANT-001&number[]=GRANT-002
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
      "name": "EXAMPLE",
      "title": "Example research project"
    }
  ]
}
```

Alternatively:

```http
GET /api/projects-by-funding-number?apikey=YOUR_API_KEY&number=GRANT-001,GRANT-002
```

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Project ObjectID as an Extended JSON object, not a string `id`. |
| `name, title` | Stored project identifier and title, where present. |

`count` is the number of matching projects before slicing. Values are not trimmed; spaces in a comma-separated input remain part of the funding number. No additional project-view permission filter or explicit sort is applied in this route.

#### Errors

Omitting `number` returns:

```json
{
  "status": 400,
  "count": 0,
  "error": "WrongCall",
  "msg": "No funding number provided"
}
```

A supplied number with no match returns an empty successful list.
