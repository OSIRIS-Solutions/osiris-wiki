# Infrastructures

## `GET /portfolio/infrastructures`

Returns public research infrastructures.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/infrastructures?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "_id": {
        "$oid": "665ef1234567890abcdef1234"
      },
      "id": "example-lab",
      "name": "Example Laboratory",
      "subtitle": "Research services",
      "start_date": "2025-01-01",
      "end_date": null,
      "type_id": "research_infrastructure",
      "logo": "<div class=\"infrastructure-logo-placeholder\"></div>"
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Retained MongoDB ObjectID in Extended JSON form. |
| `id` | Stored infrastructure identifier, not the MongoDB ID. |
| `name, name_de, subtitle, subtitle_de, description, description_de` | Stored bilingual display text. |
| `start_date, end_date` | Stored date range. |
| `type_id` | Stored `type` code. |
| `image` | Stored image data, if projected and present. |
| `logo` | Rendered logo HTML: an embedded data-URL image or a placeholder `<div>`. |

#### Response Behavior

Requires `public=true` and sorts by `name` ascending. `_id` and any projected image data are retained. `logo` is HTML, not an image URL.

#### Related Endpoints

- [Infrastructure Details](infrastructures/profile.md)
- [Infrastructure Activities](infrastructures/activities.md)
