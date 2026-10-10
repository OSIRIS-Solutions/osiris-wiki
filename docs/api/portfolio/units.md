# Units

## `GET /portfolio/units`

Returns organization units for Portfolio navigation.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/units?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "BIO",
      "name": "Biology Unit",
      "name_de": "Biologie",
      "parent": "EXAMPLE",
      "unit": "department",
      "level": 1,
      "hide": false,
      "order": 1
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Stored group ID, not a MongoDB ObjectID. |
| `name, name_de` | Stored bilingual unit name. |
| `parent` | Parent group ID. |
| `unit` | Stored organizational unit-type code. |
| `level` | Hierarchy level; root is level 0. |
| `hide, order` | Stored visibility flag and ordering value, where present. |

#### Response Behavior

All groups are included: the route intentionally does not filter `hide`, `inactive`, or level. No explicit sort is applied. Consumers building public navigation should consider the returned `hide` flag.

#### Related Endpoints

- [Unit Details](units/profile.md)
- [Unit Activities](units/activities.md)
- [Unit All Activities](units/all-activities.md)
- [Unit Collaborators Map](units/collaborators-map.md)
- [Unit Collaborators by Country](units/collaborators-by-country.md)
- [Unit Cooperation](units/cooperation.md)
- [Unit Numbers](units/numbers.md)
- [Unit Projects](units/projects.md)
- [Unit Publications](units/publications.md)
- [Unit Research](units/research.md)
- [Unit Spectrum](units/spectrum.md)
- [Unit Staff](units/staff.md)
- [Unit Teaching](units/teaching.md)
