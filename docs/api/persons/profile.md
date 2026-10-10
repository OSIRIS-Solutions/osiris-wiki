# Person Details

## `GET /api/users/{id}`

Returns one stored person record. A valid MongoDB ObjectID is looked up as `_id`; other values are looked up as usernames.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Required person ObjectID or username. |
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `columns[]` | array | Optional stored fields to include. Adds `id` as a string; `_id` is retained. |

#### Example Request

```http
GET /api/users/alex.example?apikey=YOUR_API_KEY&columns[]=first&columns[]=last
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": {
    "_id": {
      "$oid": "665ef1234567890abcdef1234"
    },
    "id": "665ef1234567890abcdef1234",
    "first": "Alex",
    "last": "Example"
  }
}
```

#### Response Behavior

`data` is a single object, not an array. Without `columns[]`, it contains the complete stored person document, including `_id` in Extended JSON form; a separate string `id` is not automatically added. With `columns[]`, only the projection's requested fields plus `_id` and `id` are returned. Missing stored fields may be omitted.

`count` is `1`. `limit` and `offset` do not slice this object. `full`, `json`, and `filter` are not supported.

#### Errors

Unknown ID or username:

```json
{
  "status": 404,
  "count": 0,
  "error": "DataNotFound",
  "msg": "User not found"
}
```
