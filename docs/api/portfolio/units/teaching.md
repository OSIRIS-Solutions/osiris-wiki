# Unit Teaching

## `GET /portfolio/unit/{id}/teaching`

Returns teaching modules represented by matching activity records.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Group ID or `0` for the root. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/teaching?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "BIO-101",
      "title": "Introduction to Biology",
      "affiliation": "Example University",
      "count": 2
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Module MongoDB ObjectID as string. |
| `name` | Stored teaching `module` identifier. |
| `title, affiliation` | Stored module title and affiliation, without organization-name resolution. |
| `count` | Number of matching source activities for the module; not the response-level count. |

#### Response Behavior

The group and its descendants are included; `0` resolves to the root group.

Requires activity `type=teaching`, non-null `module_id`, and `hide != true`. Activities are grouped by `module_id` and sorted by descending activity count; unresolved modules are skipped. Response `count` counts resolved modules. The local examples had no matching modules; the illustrated non-empty row follows the code.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
