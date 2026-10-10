# Collaborators Map

## `GET /portfolio/collaborators-map`

Returns organization markers for the institute-wide collaboration map.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/collaborators-map?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 2,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "count": 3,
      "public_count": 2,
      "data": {
        "name": "Example University",
        "type": "university",
        "location": "Berlin",
        "country": "DE",
        "ror": null,
        "lat": 52.52,
        "lng": 13.405
      }
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

Aggregates collaborator entries from all projects with a `collaborators` field, including non-public projects. Organizations are resolved by ObjectID and project-derived rows are sorted by descending count. Their identifier is string `id`.

Organizations marked `is_collaborator=true` but not already represented are appended with `id`, `count: 0`, and `data.role=partner`; they have no `public_count`. If institute latitude and longitude are set, the institute is appended with `_id` equal to its ROR value (possibly empty), `self: true`, and role `coordinator`. This mixed identifier schema is intentional in the current output. The institute marker is included in response `count`.
