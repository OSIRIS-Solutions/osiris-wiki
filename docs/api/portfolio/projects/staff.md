# Project Staff

## `GET /portfolio/project/{id}/staff`

Returns persons linked to a project, ordered by project role.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Project MongoDB ObjectID or exact stored project name. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/project/665ef1234567890abcdef1234/staff?apikey=YOUR_PORTFOLIO_API_KEY
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
      "role": "",
      "depts": []
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
| `role` | Rendered project role. Currently read from an unset output field rather than the source membership; it may be empty or incorrect. |
| `depts` | Legacy level-1 department map keyed by group ID, with `{en,de}` labels; empty result is `[]`. |

#### Response Behavior

The project is looked up by ObjectID or exact `name`. Hidden or unresolved persons are skipped, but inactive persons are included. The handler does not require project `public=true`. Memberships are sorted by `applicant`, `PI`, `Co-PI`, `worker`, `associate`, then `student`. A project without persons returns an empty list.

#### Errors

Missing projects return `404 DataNotFound` with `msg: "Project not found"`.
