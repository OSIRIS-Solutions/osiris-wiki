# Unit Collaborators by Country

## `GET /portfolio/unit/{id}/collaborators-by-country`

Groups unit-associated project collaborator entries by their stored country.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored group ID. Use the actual root ID rather than `0`. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/collaborators-by-country?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 2,
  "data": {
    "countries": [
      {
        "iso": "DE",
        "iso3": "DEU",
        "count": 3,
        "label": "<b>Germany",
        "0": "</b><br/>Projects: 3"
      }
    ],
    "institute": {
      "name": "Example Institute",
      "country": "DE"
    }
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `countries` | Country summaries, one per grouped collaborator country. |
| `countries[].iso, countries[].iso3` | Stored country code and resolved ISO3 value; unresolved/missing countries can be null. |
| `countries[].count` | Number of collaborator entries, not a count of distinct partner organizations. |
| `countries[].label` | Current output contains only the opening `<b>` plus the translated country name. |
| `countries[].0` | Due to an unkeyed string in the route, the closing markup and project-count text are returned under JSON key `"0"`. |
| `institute` | Configured affiliation details for the home institute. |

#### Response Behavior

Includes this unit and descendants; no public-project filter is applied. Groups by `collaborators.country` rather than reading the resolved organization's country. Countries have no explicit sort. Unlike several unit endpoints, this handler does not resolve `0` to the root group first; use the actual root group ID for root statistics.

Response `count` is always 2 for the two top-level keys (`countries`, `institute`), not the number of countries. The malformed `label`/`"0"` split shown in the example matches the current route.
