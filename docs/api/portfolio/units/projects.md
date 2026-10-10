# Unit Projects

## `GET /portfolio/unit/{id}/projects`

Returns public projects associated with one unit.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Group ID; `0` resolves to the root unit. Includes descendant groups. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/projects?apikey=YOUR_PORTFOLIO_API_KEY
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
      "type": "Research project",
      "start": {
        "year": 2025,
        "month": 1
      },
      "end": null,
      "teaser_en": "Example project",
      "teaser_de": null,
      "_id": {
        "$oid": "665ef1234567890abcdef1234"
      },
      "type_details": {
        "id": "third-party",
        "name": "Research project",
        "name_de": "Forschungsprojekt"
      }
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
| `_id` | Retained Extended JSON ObjectID. |
| `acronym, name_de, title_de` | Additional stored acronym and bilingual identifiers/titles. |
| `abstract, abstract_de` | Stored bilingual abstract. |
| `start_date, end_date` | Stored date range alongside legacy `start`/`end`. |
| `type_details` | Configured project type object `{id,name,name_de}`, if the stored type is recognized. |

#### Response Behavior

Matches public projects whose `units` include this unit or a descendant.

Rows are sorted by year then month descending. A recognized `type` code is replaced by its configured English name and the complete type label object is added as `type_details`. Unrecognized types remain unchanged.
