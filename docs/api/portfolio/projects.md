# Projects

## `GET /portfolio/projects`

Returns public projects for Portfolio project lists.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/projects?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "EXAMPLE",
      "title": "Example research project",
      "funder": "Example funder",
      "funding_number": [
        "GRANT-001"
      ],
      "role": "coordinator",
      "type": "third-party",
      "start": {
        "year": 2025,
        "month": 1
      },
      "end": null,
      "teaser_en": "Example project",
      "teaser_de": null
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Project MongoDB ObjectID as string. |
| `name, title` | Stored project identifier and title. |
| `funder, funding_organization, funding_number` | Stored funding values/references, not formatted organization links. |
| `role, type` | Stored project role and type codes. |
| `start, end` | Stored legacy date values; may be date objects. |
| `teaser_en, teaser_de` | Stored bilingual teaser text. |
| `image` | Stored image value/path, where present. |

#### Response Behavior

Requires stored `public=true`. Results are sorted by stored `year` and `month` descending. No entity-view session permission restriction is added by this route. Stored codes/references are not rendered or translated here.

#### Related Endpoints

- [Project Details](projects/profile.md)
- [Project Activities](projects/activities.md)
- [Project All Activities](projects/all-activities.md)
- [Project Collaborators Map](projects/collaborators-map.md)
- [Project Publications](projects/publications.md)
- [Project Staff](projects/staff.md)
