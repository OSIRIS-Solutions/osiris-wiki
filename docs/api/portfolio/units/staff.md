# Unit Staff

## `GET /portfolio/unit/{id}/staff`

Returns a compact staff list for one unit.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Group ID; `0` returns all active visible staff. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/staff?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "displayname": "Alex Example",
      "academic_title": "Dr.",
      "position": "Researcher",
      "position_de": "Forschende",
      "firstname": "Alex",
      "lastname": "Example"
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Person MongoDB ObjectID as string. |
| `displayname` | First and last name joined with a space for unit/topic staff; stored display name for project staff. |
| `academic_title, position, position_de` | Stored academic title and bilingual position. |
| `firstname, lastname` | Stored name components (unit/topic staff only). |

#### Response Behavior

Active visible persons with memberships in this unit/descendants and no past end date are included. The start date is not checked. `id=0` is special: it returns all active visible persons without restricting unit membership. A literal root group ID instead follows normal membership filtering. Rows are sorted by surname ascending. No contact details or profile images are returned.
