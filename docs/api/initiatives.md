# Initiatives and Objectives

These read-only endpoints expose initiatives, their objectives, and linked research content. For the separately authenticated public endpoints, see [Initiatives and Objectives in the Portfolio API](portfolio/initiatives.md).

## Authentication and access

The `initiatives` feature must be enabled. API clients need the `api` surface and the **`initiatives.read`** scope, which covers both initiatives and objectives. Enable this scope under **Administration → API clients**.

```http
GET /api/initiatives?status=current&lang=en
X-OSIRIS-Client-ID: osc_YOUR_CLIENT_ID
Authorization: Bearer YOUR_CLIENT_SECRET
```

The configured global OSIRIS API key remains supported through `apikey` or `X-API-Key`. Same-origin authenticated browser requests can use session authentication; these users must also have `initiatives.view` or `initiatives.edit`.

Related-content endpoints additionally require the content scope shown below. For authenticated browser sessions, activities, projects, proposals, and infrastructures retain their normal visibility/ownership restrictions. API clients without a browser session use their scopes.

## Endpoints

All IDs below are 24-character MongoDB ObjectID strings.

| Request | Result |
|---|---|
| `GET /api/initiatives` | List of initiatives |
| `GET /api/initiatives/{id}` | One initiative |
| `GET /api/initiatives/{id}/objectives` | Objectives belonging to this initiative |
| `GET /api/objectives` | List of objectives |
| `GET /api/objectives/{id}` | One objective, including its `initiative` ID |
| `GET /api/initiatives/{id}/{content}` | Content associated with the initiative |
| `GET /api/objectives/{id}/{content}` | Content explicitly associated with this objective |

Supported `{content}` values:

| Value | Additional client scope |
|---|---|
| `activities` | `activities.read` |
| `projects` | `projects.read` |
| `proposals` | `projects.read` |
| `events` | `events.read` |
| `infrastructures` | `infrastructures.read` |
| `deadlines` | `events.read` |

`activities` includes publications. `events` reads the existing conferences collection. Feature-dependent content returns an empty list when its feature is disabled. Objective relationships use only that objective's explicit assignments. Initiative relationships use the saved initiative links, including parent links derived from objectives; they do not add unrelated records or traverse project/activity connections.

## Parameters

| Parameter | Applies to | Description |
|---|---|---|
| `status` | Initiative/objective lists, including an initiative's objectives | `all` (default), `current`, or `past`. Past means archived or an end date before today. Detail URLs remain available for past records. |
| `initiative` | Objective lists | Parent initiative ID. It cannot override the parent of `/api/initiatives/{id}/objectives`. |
| `search` | Initiative/objective lists | Case-insensitive text search in returned data, after language selection. |
| `filter` | Initiative/objective lists | MongoDB filter using bracket notation; takes precedence over `json`. |
| `json` | Initiative/objective lists | JSON-encoded MongoDB filter. String IDs in `_id`, `initiative`, `initiatives`, and `objectives` are converted for equality, inequality and membership operators; logical `$and`, `$or`, and `$nor` groups are supported. |
| `lang` | All endpoints | Requested configured content language; `all` retains language maps. Uses the usual OSIRIS language fallback. Fixed objective texts currently remain ordinary strings. |
| `limit` | All lists and related-content lists | Positive maximum row count; absent, zero or negative means no maximum. |
| `offset` | All lists and related-content lists | Number of rows to skip; negative values become zero. Works independently of `limit`. |

`status`, `search`, `filter`, `json`, and `initiative` do not filter related content. Use the existing content/search APIs for more detailed content queries. No writes, aggregation or column selection are provided by these endpoints.

Lists sort current records before past records, then by name. Their response `count` gives the match count before pagination. Detail responses contain an object and `count: 1`.

## Examples

```http
GET /api/objectives?initiative=6afa00000000000000000001&status=all&lang=all
```

```http
GET /api/initiatives?json={"_id":"6afa00000000000000000001"}
```

URL-encode JSON in actual query strings.

```json
{
  "status": 200,
  "count": 1,
  "data": {
    "id": "6afa10000000000000000001",
    "initiative": "6afa00000000000000000001",
    "code": "26-01",
    "name": "Develop integrated environmental risk indicators",
    "description": "Combine observations and predictive models.",
    "type": "objective",
    "start_date": "2026-01-01",
    "end_date": "2027-12-31",
    "inactive": false
  }
}
```

The example is abbreviated. Internal responses include stored metadata and custom fields. Top-level `_id` is exposed as a string `id`; objective `initiative` references are also strings. Other nested MongoDB references may retain Extended JSON. Content records retain their existing string `id` where present. These endpoints preserve numeric-looking strings such as objective code `"001"`.

## Errors

Both HTTP status and JSON `status` are set: `400` for invalid parameters/JSON, `403` for failed authentication, missing scopes or missing browser-session permission, and `404` for missing/invalid IDs, disabled initiatives or unsupported relationships. The response uses the usual `status`, `count`, `error`, and `msg` error envelope.
