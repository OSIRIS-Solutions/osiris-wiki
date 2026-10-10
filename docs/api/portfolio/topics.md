# Topics

## `GET /portfolio/topics`

Returns configured institutional research topics.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/topics?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "biology",
      "name": "Biology",
      "name_de": "Biologie",
      "subtitle": "Research in biology",
      "subtitle_de": null,
      "description": "Example description",
      "description_de": null,
      "color": "#368c73"
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Stored topic identifier. |
| `name, name_de` | English/German topic name. |
| `subtitle, subtitle_de` | Stored bilingual subtitle. |
| `description, description_de` | Stored bilingual description. |
| `color` | Configured display color. |

#### Response Behavior

Topics with `inactive=true` are excluded. This route does not additionally check `hide`. No explicit sort is applied. These topic IDs differ from OpenAlex spectrum IDs.

#### Related Endpoints

- [Topic Details](topics/profile.md)
- [Topic Activities](topics/activities.md)
- [Topic All Activities](topics/all-activities.md)
- [Topic Collaborators Map](topics/collaborators-map.md)
- [Topic Projects](topics/projects.md)
- [Topic Publications](topics/publications.md)
- [Topic Spectrum](topics/spectrum.md)
- [Topic Staff](topics/staff.md)
- [Topic Units](topics/units.md)
