# News

## `GET /portfolio/news`

Returns public news items released up to today.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/news?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "title": "Example research news",
      "title_de": null,
      "teaser": "Example summary",
      "teaser_de": null,
      "date": "2026-09-01"
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | News MongoDB ObjectID as string. |
| `title, title_de, teaser, teaser_de` | Stored bilingual title and teaser. |
| `date` | Stored news date. |

#### Response Behavior

Requires `visibility=public` and `date <= today`, and sorts by date descending. When the `news` feature is disabled, returns a successful empty list. Full article content and related entities are available from [news details](news/profile.md).
