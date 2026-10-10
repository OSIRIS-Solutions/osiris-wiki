# Unit Research

## `GET /portfolio/unit/{id}/research`

Returns the unit's configured research sections.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored group ID, or `0` for the root group. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/research?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "title": "Biology",
      "title_de": "Biologie",
      "subtitle": "Research focus",
      "subtitle_de": "Forschungsschwerpunkt",
      "info": "Example research text",
      "info_de": "Beispieltext",
      "activities": [
        {
          "id": "665ef1234567890abcdef1234",
          "icon": "Rendered icon",
          "html": "Rendered Portfolio citation"
        }
      ]
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `title, title_de, subtitle, subtitle_de` | Stored bilingual research section headings. |
| `info, info_de` | Stored bilingual research text, returned without Markdown conversion. |
| `activities` | Resolved activities with string `id`, rendered `icon`, and `html`. Unresolved activities are skipped. |

#### Response Behavior

`0` resolves to the root group. Missing research sections return an empty list. This standalone endpoint uses `rendered.portfolio` for linked activity `html`; the embedded unit-profile sections instead use `rendered.print`. No extra activity hide/subtype/workflow checks are applied to these referenced activities. Missing groups are not explicitly rejected.
