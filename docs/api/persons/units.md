# Person Unit Memberships

## `GET /api/user-units/{id}`

Returns a person's unit memberships with resolved unit names and a past-membership flag.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Required person ObjectID or username. |
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |

#### Example Request

```http
GET /api/user-units/alex.example?apikey=YOUR_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": {
    "name": "Alex Example",
    "units": [
      {
        "unit": "BIO",
        "start": "2024-01-01",
        "end": null,
        "name": "Biology",
        "in_past": false
      }
    ]
  }
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `name` | Person's first and last name. |
| `units` | Array of membership objects; additional stored membership fields are retained. |
| `units[].unit` | Unit identifier. |
| `units[].start, units[].end` | Stored membership dates, where present. |
| `units[].name` | Translated unit name, or `Unit not found`. |
| `units[].in_past` | True if an end date is set and earlier than today. |

#### Response Behavior

`data` is an object and `count` is `1`, also for an empty membership list. The stored list is reversed before deduplicating by unit ID; the last stored membership for each unit wins. The route does not sort by date or exclude future memberships. `in_past` only checks the end date.

Unknown persons return `status: 404`, `error: DataNotFound`, and a translated `User not found` message. `limit`, `offset`, and `columns[]` are not supported.
