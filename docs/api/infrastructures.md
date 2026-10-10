# Infrastructures

## `GET /api/infrastructures`

Returns full stored research infrastructure documents.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `filter` | object/array | MongoDB filter using bracket notation. Takes precedence over `json`. |
| `json` | string | JSON-encoded MongoDB filter, used only when `filter` is absent. |
| `limit` | integer | Maximum rows under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/infrastructures?apikey=YOUR_API_KEY&json=%7B%22public%22%3Atrue%7D
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
      "type": "research_infrastructure",
      "public": true,
      "persons": [],
      "topics": [],
      "start_date": "2025-01-01",
      "end_date": null
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Stored ObjectID as Extended JSON. |
| `id` | Stored infrastructure identifier, if present. This route does not create a string MongoDB ID. |
| `name, name_de, subtitle, subtitle_de, description` | Stored display text, where present. |
| `type, infrastructure_type, access` | Stored type and access classifications, where present. |
| `persons, coordinator, coordinator_organization` | Stored personnel and coordinator references, where present. |
| `collaborative, collaborators` | Stored collaboration flag and partner references, where present. |
| `topics, public, link, contact_email` | Stored topics, visibility flag, external URL, and contact information, where present. |
| `start_date, end_date` | Stored date range, where present. |
| `statistic_frequency, statistic_fields` | Stored reporting configuration, where present. |
| `Other fields` | All additional stored fields and metadata are retained. |

`count` is the number of matching records before slicing. No explicit sort or permission-based visibility filter is applied in this handler. `search` and `columns[]` are not supported.
