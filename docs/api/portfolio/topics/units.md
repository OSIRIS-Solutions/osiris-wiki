# Topic Units

## `GET /portfolio/topic/{id}/units`

Returns active visible units associated with an institutional research topic.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored institutional topic ID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/topic/biology/units?apikey=YOUR_PORTFOLIO_API_KEY
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
      "unit": {
        "name": "Department",
        "name_de": "Abteilung",
        "head": "Head of Department",
        "head_de": "Abteilungsleitung"
      },
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
| `level` | Hierarchy level; root is level 0. |
| `hide, order` | Stored visibility flag and ordering value, where present. |
| `unit` | Resolved unit-type object with `name`, `name_de`, `head`, and `head_de`, rather than the stored code. |
| `heads` | The code attempts to enrich heads, but `head` is excluded by the projection, so this enrichment normally produces no field. |

#### Response Behavior

Requires `topics` containing the ID, `is_active != false`, and `hide != true`. No explicit sort is applied. This handler uses `is_active`; it does not check the separate stored `inactive` flag.
