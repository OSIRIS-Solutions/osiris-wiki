# Activity Table

## `GET /api/all-activities`

Returns streamed activity rows for OSIRIS tables and external portals. Before querying, OSIRIS generates and stores rendered representations for activities that have no `rendered` field.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `json` | string | JSON-encoded MongoDB filter. Overrides `filter`. |
| `filter` | object/array | Alternative MongoDB filter via bracket notation. |
| `type` | string | Activity type; added to the filter if non-empty. |
| `page` | string | Display context. Default: `all-activities`. `my-activities` filters by user; `portal` rewrites activity and person links. |
| `user` | string | Username for `page=my-activities`. Defaults to the session username. |
| `path` | string | Base path for rewritten activity and person links. Takes precedence over `page=portal`. |

`limit` and `offset` are ignored. `columns[]`, `aggregate`, and `full` are not supported.

#### Example Request

```http
GET /api/all-activities?apikey=YOUR_API_KEY&type=publication&page=portal
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "quarter": "2025Q1",
      "icon": "<i class=\"ph ph-file-text\"></i>",
      "activity": "<a href=\"/activity/665ef1234567890abcdef1234\">Example publication</a>",
      "links": "",
      "search-text": "Example citation",
      "start": "2025-01-01",
      "end": "",
      "departments": [
        "BIO"
      ],
      "epub": false,
      "type": "Publications",
      "subtype": "Journal Article",
      "year": 2025,
      "authors": "Example A.",
      "editors": "",
      "title": "Example publication",
      "topics": [],
      "raw_type": "publication",
      "raw_subtype": "article",
      "affiliated": true,
      "workflow": "verif",
      "tags": []
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Activity ID as string. |
| `activity` | Rendered HTML. Uses the session user's `display_activities` setting (`web` by default; otherwise `print`). |
| `icon` | Rendered activity icon, potentially HTML. |
| `quarter` | Rendered quarter, or an empty string. |
| `links` | Always an empty string. |
| `search-text` | Rendered print citation; may contain HTML. |
| `start, end` | Stored `start_date` and `end_date`, or empty strings when absent/null. |
| `departments` | Stored `units` array. |
| `epub` | Stored electronic-publication value, if present. |
| `type, subtype` | Rendered type and subtype labels. |
| `raw_type, raw_subtype` | Stored type and subtype identifiers. |
| `year` | Stored year, or `0`. |
| `authors, editors, title` | Rendered values, or empty strings. |
| `topics` | Topic IDs, or an empty array. |
| `affiliated` | Stored affiliation flag, or `false`. |
| `workflow` | Stored `workflow.status`, or `verif`. |
| `tags` | Stored tags, if present. |

#### Filtering and Links

A top-level `projects` filter value is converted to a MongoDB ObjectID. `page=my-activities` combines the user's visibility filter with `rendered.users=user`. For other pages, session visibility restrictions are added when the request has a session username and no `apikey` query parameter.

With `path=/portal`, links to `/activities/view` become `/portal/activity`, and links to `/profile` become `/portal/person`. With `page=portal` and no `path`, they become `/activity` and `/person`. Only the `activity` HTML field is rewritten.

`count` is the number of streamed rows. No explicit result sort is applied; fields without a stored value may be omitted.
