# Person Infrastructures

## `GET /portfolio/person/{id}/infrastructures`

Returns public infrastructures linked to one person.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Person MongoDB ObjectID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/person/665ef1234567890abcdef1234/infrastructures?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "example-lab",
      "name": "Example Laboratory",
      "role": "Scientist",
      "start": "2025-01-01",
      "end": null
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Stored infrastructure identifier. |
| `name` | Stored infrastructure name. |
| `role` | Translated infrastructure role label. |
| `start, end` | Dates from the first matching person membership; empty-string fallbacks. |

#### Response Behavior

Resolves the person ObjectID to a username, matches `persons.user`, and requires infrastructure `public=true`. Sorts by infrastructure name. The person is not filtered by `hide`/active status in this route, and past infrastructure memberships are not removed.

#### Errors

Missing persons return `404 DataNotFound` with `msg: "Person not found"`.
