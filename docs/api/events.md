# Events

## `GET /api/events`

Returns full conference/event records from the `conferences` collection. `/api/conferences` and `/api/events` return the same data, with separate endpoint documentation.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |

The route does not read `json`, `filter`, or `search`. It passes no count to the shared response helper, so `limit` and `offset` have no effect.

#### Example Request

```http
GET /api/events?apikey=YOUR_API_KEY
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
      "title": "Example conference",
      "location": "Berlin",
      "start": "2026-11-01",
      "end": "2026-11-03"
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
| `start, end` | Stored start/end dates, where present. |
| `location, country` | Stored venue location and country, where present. |
| `participants, activities, topics` | Stored participant, activity, and topic references, where present. |

#### Response Behavior

The query sorts by `start` descending, including for deadlines; it does not sort deadlines by `date`. Fields may be absent depending on the record. `count` is currently always `0`, even for a non-empty `data` array. Use the returned array to determine how many rows were received.

For a compact conference projection with working counts, filters, and aggregation, use [conference search](conferences/search.md).
