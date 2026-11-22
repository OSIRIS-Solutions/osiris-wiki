# Initiatives and Objectives

These endpoints provide the public read model for initiatives and objectives. They follow the Portfolio API's plural list URLs and singular detail/context URLs. Internal records and broader filters are available through the separate [OSIRIS API](../initiatives.md).

## Authentication and publication

All routes use the existing Portfolio key check: `portfolio_apikey`, supplied through `apikey` or `X-API-Key`. No key is required when no Portfolio key is configured. OSIRIS API client scopes do not authenticate Portfolio requests. Both `initiatives` and `portal` must be enabled.

Publication is explicit:

1. Edit the initiative and enable **Show in the public portfolio**.
2. Publish any objectives individually through the same option on their edit pages.
3. An objective is visible only while **both it and its initiative** have `public: true`.

Missing or false `public` values remain private. Existing records are not published automatically. A private record responds with the same `404` as an unknown record. A published parent does not automatically publish its objectives or linked content.

Initiative metadata follows the Form Builder's Portfolio field settings and module defaults. Role-restricted fields are excluded. Raw person memberships, audit fields (`created_by`, `updated_by`, history, etc.) and raw initiative/objective association arrays are omitted. Published objectives expose their fixed fields: `id`, `initiative`, `code`, `name`, `description`, `type`, `start_date`, `end_date`, and `inactive`, where present.

## Endpoints

| Request | Result |
|---|---|
| `GET /portfolio/initiatives` | Public initiatives |
| `GET /portfolio/initiative/{id}` | One public initiative, its public objectives and related-content totals |
| `GET /portfolio/objectives` | Public objectives whose initiative is also public |
| `GET /portfolio/objective/{id}` | One public objective, including its public initiative's ID and related-content totals |
| `GET /portfolio/initiative/{id}/{content}` | Public content associated with this initiative |
| `GET /portfolio/objective/{id}/{content}` | Public content explicitly associated with this objective |

IDs are 24-character MongoDB ObjectID strings. Supported `{content}` values:

| Value | Visibility and output |
|---|---|
| `publications` | Publications only; allowed Portfolio subtypes, not hidden, normal Portfolio workflow rules. Includes `html` from `rendered.portfolio` and `search` from `rendered.plain`. |
| `activities` | Other activities only, using the enabled non-publication Portfolio subtypes; same hidden/workflow restrictions. |
| `all-activities` | Publications and other activities from enabled Portfolio subtypes; same hidden/workflow restrictions. |
| `projects` | Only projects with `public: true`; public identity, description, dates and topics. |
| `events` | Only events with `public: true`; public event details and dates. |
| `infrastructures` | Only infrastructures with `public: true`; public identity, description, category, dates and topics. |

Relations do not traverse from a project to its activities. Initiative relations use saved initiative links, including links derived from objectives. Objective relations use only explicit objective assignments. Content from unpublished sibling objectives may appear under a published initiative only if the **content itself** satisfies its normal Portfolio visibility rules; unpublished objective references are not exposed. No proposal or deadline endpoints are provided publicly.

## Detail data and counters

The initiative detail response includes `objectives`, containing all explicitly public objectives belonging to that initiative. Each objective uses the public field selection and requested language. Current objectives appear first, followed by expired or archived objectives. This embedded list is complete: `status`, `search`, `limit` and `offset` do not filter or paginate it. There is no separate `/portfolio/initiative/{id}/objectives` endpoint; the global objective list can still be filtered with `initiative={id}`.

Both initiative and objective details include `numbers` with `publications`, `activities` (excluding publications), `projects`, `events` and `infrastructures`. Initiative details additionally include `numbers.objectives`, equal to the length of the embedded objectives list. For example:

```json
"numbers": {
  "objectives": 2,
  "publications": 12,
  "activities": 4,
  "projects": 3,
  "events": 1,
  "infrastructures": 0
}
```

Counters use the same parent association and public visibility rules as their content endpoints, including enabled activity subtypes, hidden records and workflow settings. Disabled project/infrastructure features return zero. Counts cover all matching records, independently of `limit` and `offset`. Private objectives are neither embedded nor counted. Content counters do not require its linked objective to be public; the normal content visibility rules still apply.

## Research topics and spectrum

Research topics (`topics`) are explicitly assigned through the initiative form. They are returned only when the configured field allows Portfolio publication; topics are never inferred from linked content. Objectives do not inherit their initiative's or their content's research topics.

When both the research spectrum and its Portfolio integration are enabled, initiative and objective detail responses additionally include `spectrum`. It uses the existing weighted OpenAlex-topic aggregation, based only on linked publications passing the same subtype, hidden-record and workflow checks as the public publication list. With no qualifying publications, `spectrum` is an empty array. The list and embedded objective summaries remain unchanged.

## Parameters

| Parameter | Applies to | Description |
|---|---|---|
| `status` | Initiative/objective lists | `current` (default), `past`, or `all`; past means archived or ended before today. |
| `initiative` | Objective lists | Public parent initiative ID. |
| `search` | Initiative/objective lists | Case-insensitive search of the returned public fields, after language selection. Hidden metadata is not searched. |
| `lang` | All endpoints | Configured language or `all` for complete maps; usual content-language fallback applies. Fixed objective texts currently remain ordinary strings. |
| `limit` | All lists, including content lists | Positive maximum rows. Absent, zero or negative means no maximum. |
| `offset` | All lists, including content lists | Rows to skip; negative values become zero. Works independently of `limit`. |

Detail URLs remain accessible for archived/expired records while explicitly published. `status` does not filter related content. Parameters such as `filter`, `json`, `full`, `columns[]` and `aggregate` are not supported here and cannot widen public visibility.

Current records sort before past records, then by name. `count` is the match count before pagination; single-record responses contain an object and `count: 1`. IDs and numeric-looking text values such as `"001"` remain strings.

## Examples

```http
GET /portfolio/initiatives?apikey=YOUR_PORTFOLIO_API_KEY&lang=en
GET /portfolio/initiative/6afa00000000000000000001
GET /portfolio/objective/6afa10000000000000000001/publications
```

Abbreviated objective response:

```json
{
  "status": 200,
  "count": 1,
  "data": {
    "id": "6afa10000000000000000001",
    "initiative": "6afa00000000000000000001",
    "code": "26-01",
    "name": "Develop integrated environmental risk indicators",
    "type": "objective",
    "start_date": "2026-01-01",
    "end_date": "2027-12-31",
    "inactive": false
  }
}
```

`GET /portfolio/settings` reports `features.initiatives`, indicating that the initiative and Portfolio features are enabled. It does not imply that any records have been published. `labels.initiatives` and `labels.objectives` provide the institution-specific names for the configured content languages. These endpoints provide data; they do not add pages to a separately hosted Portfolio frontend.

## Errors and response behavior

Responses use the normal `status`, `count`, `data` envelope, or `error`/`msg` on failure. These endpoints set the actual HTTP status: `400` for malformed supported parameters, `403` for an invalid configured Portfolio key, and `404` for private/missing records or disabled features.

The endpoints support GET, set `Access-Control-Allow-Origin: *`, and advertise `X-API-Key` in allowed headers. They do not add an OPTIONS handler; cross-origin header authentication may still require server CORS configuration.

Read-model privacy, parent scoping, language maps, publication controls, workflow filters and authorization are covered by regression tests. No live Portfolio deployment or database migration is performed by this change.
