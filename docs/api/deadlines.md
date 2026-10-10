# Deadlines

## `GET /api/deadlines`

Returns all stored deadlines and marks their relevance to the current session roles.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |

The route does not read `json`, `filter`, or `search`. It passes no count to the shared response helper, so `limit` and `offset` have no effect.

#### Example Request

```http
GET /api/deadlines?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 0,
  "data": [
    {
      "_id": {
        "$oid": "665ef1234567890abcdef1234"
      },
      "id": "665ef1234567890abcdef1234",
      "title": "Example submission deadline",
      "date": "2026-11-01",
      "roles": [
        "researcher"
      ],
      "relevant": false
    }
  ]
}
```

#### Returned Fields

Full stored documents are retained, with additional fields added by the route. The example is abbreviated; record fields depend on the data stored in OSIRIS.

| Field | Description |
|---|---|
| `_id` | Stored ObjectID as Extended JSON. |
| `id` | String representation of `_id`. |
| `title, description, type, url` | Stored title, description, category, and external URL, where present. |
| `date, year, month, day, quarter` | Stored deadline date and date components, where present. |
| `roles` | Stored roles to which the deadline is relevant. |
| `relevant` | True if the stored `roles` intersect the current session roles. This does not filter the list. |

#### Response Behavior

The query sorts by `start` descending, including for deadlines; it does not sort deadlines by `date`. Fields may be absent depending on the record. `count` is currently always `0`, even for a non-empty `data` array. Use the returned array to determine how many rows were received.
