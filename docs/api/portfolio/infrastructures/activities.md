# Infrastructure Activities

## `GET /portfolio/infrastructure/{id}/activities`

Returns activities associated with one infrastructure identifier.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored infrastructure identifier. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/infrastructure/example-lab/activities?apikey=YOUR_PORTFOLIO_API_KEY
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
      "html": "<a href=\"**PORTAL**/activity/665ef1234567890abcdef1234\">Example publication</a>",
      "print": "Example A. (2026) Example publication.",
      "search": "Example A. 2026 Example publication",
      "type": "publication",
      "subtype": "article",
      "year": 2026,
      "month": 3,
      "day": 1,
      "icon": "<i class=\"ph ph-file-text\"></i>",
      "affiliated": true
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | Extended JSON ObjectID in context/infrastructure lists; not included in global lists. |
| `html` | Stored `rendered.portfolio` HTML, potentially containing `**PORTAL**` placeholders. |
| `print` | Stored rendered print citation HTML. |
| `search` | Stored rendered plain citation text. |
| `type, subtype` | Stored activity category/subtype codes, not translated display labels. |
| `year, month, day` | Stored date components, where present. |
| `icon` | Stored rendered icon, potentially HTML. |
| `affiliated` | Stored affiliation flag (global lists and infrastructure activity list only). |

#### Response Behavior

Matches the exact stored ID in activity `infrastructures` and excludes hidden activities. There is no infrastructure lookup/public check and no restriction to Portfolio-enabled subtypes or affiliated activities. Rows are sorted by year then month descending; `_id` is retained and no string `id` is added. The local sample returned an empty list.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
