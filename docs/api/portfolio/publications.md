# Publications

## `GET /portfolio/publications`

Returns rendered publications for the entire Portfolio.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |
| `affiliated` | string | Only the literal value `true` restricts results to stored `affiliated=true`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/publications?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
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
| `id` | Activity ObjectID as a string (global lists only). |
| `html` | Stored `rendered.portfolio` HTML, potentially containing `**PORTAL**` placeholders. |
| `print` | Stored rendered print citation HTML. |
| `search` | Stored rendered plain citation text. |
| `type, subtype` | Stored activity category/subtype codes, not translated display labels. |
| `year, month, day` | Stored date components, where present. |
| `icon` | Stored rendered icon, potentially HTML. |
| `affiliated` | Stored affiliation flag (global lists and infrastructure activity list only). |

#### Response Behavior

Requires stored `type=publication`. It does not restrict publications to Portfolio-enabled subtypes in this list.

Hidden activities (`hide=true`) are excluded. `affiliated=true` adds an exact stored affiliation filter; other parameter values do not enable it. Rows are sorted by year, month, then day descending.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
