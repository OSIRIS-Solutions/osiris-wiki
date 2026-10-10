# Unit Collaborators Map

## `GET /portfolio/unit/{id}/collaborators-map`

Returns organization markers associated with one unit.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Group ID, or `0` for the institute-wide map. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/collaborators-map?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 2,
  "data": [
    {
      "_id": {
        "$oid": "665ef1234567890abcdef1234"
      },
      "count": 1,
      "data": {
        "name": "Example University",
        "type": "university",
        "location": "Berlin",
        "country": "DE",
        "ror": null,
        "lat": 52.52,
        "lng": 13.405,
        "role": "partner"
      },
      "public_count": 1
    },
    {
      "_id": "https://ror.org/EXAMPLE",
      "count": 1,
      "data": {
        "name": "Example Institute",
        "lat": 50.0,
        "lng": 10.0,
        "role": "coordinator",
        "current": true
      },
      "self": true
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id or _id` | Organization identifier; exact representation varies by endpoint and row type, as described below. |
| `count` | Number of collaborator entries across matched projects; counts entries, not necessarily distinct projects. |
| `public_count` | Count of collaborator entries from projects with `public=true`; present on aggregated project rows, not all rows. |
| `data` | Organization metadata: `name`, `type`, `location`, `country`, `ror`, `lat`, and `lng`; role is present on selected row types. |
| `self` | Only added as `true` for the appended institute marker. |
| `data.current` | Only added to the institute metadata as `true`. |

#### Response Behavior

For `id=0`, aggregates all project collaborators without a unit filter. Other IDs include the selected unit and descendants. Non-public projects remain included. Organizations are grouped by ObjectID, with `count` and `public_count`, sorted by count descending; `_id` remains Extended JSON and `data.role` is set to `partner`.

The institute marker is appended when coordinates exist, with `_id` equal to the institute ROR value, `self: true`, and `data.current=true`. Its role is the project's role for project maps, otherwise `coordinator`. The marker is included in response `count`.
