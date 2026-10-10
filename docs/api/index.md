# API Basics

The OSIRIS API provides programmatic access to research information stored in OSIRIS. It can be used to retrieve activities, projects, persons, organizational structures, events, and other data managed by the system.

Unless stated otherwise, all API endpoints:

- return JSON responses
- use HTTP GET requests
- require authentication via an API key
- follow the standard OSIRIS response format described below

The API is designed to support reporting, system integration, data synchronization, and external presentation of research information.

---

## Authentication

API access is controlled through a configurable API key.

The API key can be provided either as a query parameter:

```http
GET /api/activities?apikey=YOUR_API_KEY
```

or as an HTTP header:

```http
X-API-Key: YOUR_API_KEY
```

Depending on the OSIRIS configuration, API authentication may be disabled. In this case all API requests are accepted without an API key.

Requests originating from authenticated OSIRIS sessions may also be allowed automatically when `Sec-Fetch-Site` indicates the same site/origin, or (if that header is absent) the Referer host matches.

[Google Scholar](integrations/google-scholar.md) and [OpenAlex topics](research/openalex-topics.md) currently do not check the API key. Projects and proposals also apply separate session permission checks; a valid key alone does not guarantee access to their records.

---

## Response Format

Successful requests return a JSON object with the following structure:

```json
{
    "status": 200,
    "count": 2,
    "data": []
}
```

## Fields

| Field | Description |
|---------|-------------|
| `status` | Application status code returned in the JSON body |
| `count` | Endpoint-specific count: usually matching rows before pagination; see exceptions below |
| `data` | Array or object containing the requested data |

The structure of `data` depends on the endpoint. Person details and person unit memberships return an object; most other endpoints return arrays. Google Scholar and duplicate detection return direct objects without this envelope.

`count` has endpoint-specific semantics:

| Endpoints / mode | Meaning of `count` |
|---|---|
| Most lists | Matching output rows before slicing. Search projections may produce several rows per stored document. |
| Aggregation | Number of groups before slicing. |
| Full streamed exports and activity table | Number of streamed records. |
| Publication HTML | Number returned after the endpoint's own `limit`. |
| Reviews | Number of source activities, not grouped person rows. |
| Conferences, events, deadlines, spectrum activities | Currently always `0`, even when `data` contains rows. |
| OpenAlex topics | Number of retained matches, capped at 20, before shared slicing. |

ObjectIDs in full/raw documents use MongoDB Extended JSON (`{"$oid":"..."}`). Projection-based endpoints often supply a separate string `id`. The shared response helper and stream writer use `JSON_NUMERIC_CHECK`, so numeric strings can become JSON numbers. Rendered HTML and labels depend on OSIRIS language and settings.

Examples use illustrative values and abbreviated stored documents. Optional projected fields may be absent when the stored field is missing.

---

## Pagination

Endpoints using the shared response helper read `limit` and `offset`:

| Parameter | Type | Description |
|---|---|---|
| `limit` | integer | Maximum rows returned. A non-positive value disables pagination. |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

```http
GET /api/activities?apikey=YOUR_API_KEY&limit=50&offset=100
```

For an array response with a positive `count` and positive `limit`, the helper skips `offset` rows and returns up to `limit` rows. Pagination applies even when `count <= limit`. An offset at or beyond the end of the array returns `data: []`.

Paginated responses include integer `limit` and `offset` values. `count` keeps the total supplied by the endpoint before pagination:

```json
{
  "limit": 50,
  "offset": 100,
  "status": 200,
  "count": 350,
  "data": []
}
```

The array is abbreviated in this example. Without a positive `limit`, `offset` is ignored and pagination metadata is omitted. Empty results with `count: 0` also omit pagination metadata.

Streamed full exports and the activity table ignore both parameters. Endpoints reporting `count: 0` do not slice. Publication HTML applies its own `limit` during retrieval and has no effective offset. The full person list uses the shared helper rather than streaming.

---

## Filtering

Many endpoints support filtering of results, but parameter support and precedence are endpoint-specific. Sending an unsupported parameter does not automatically filter the response.

The preferred way to provide filters is through the `json` parameter containing a JSON-encoded MongoDB query.

Example:

```http
GET /api/activities?json={"year":2025}
```

URL-encoded:

```http
GET /api/activities?json=%7B%22year%22%3A2025%7D
```

With HTTP query parameters, bracket-notation `filter` values are usually strings. Prefer `json` when numbers, booleans, or MongoDB operators must retain their types. URL-encode JSON and regular expressions in actual requests. Filters and projections use the underlying stored fields, which can differ from rendered output labels.

Example filter:

```json
{
    "year": 2025,
    "type": "publication"
}
```

Supported filter fields depend on the endpoint and underlying data model.

Endpoint-specific documentation may define additional filtering options or restrictions.

---

## Error Handling

Errors are returned as JSON objects.

Example:

```json
{
    "status": 403,
    "count": 0,
    "error": "PermissionDenied",
    "msg": "You need a valid API key for this request."
}
```

## Common Error Responses

| Status | Error | Description |
|---------|---------|-------------|
| `400` | `WrongCall` | Invalid request or parameters |
| `403` | `PermissionDenied` | Missing or invalid API key |
| `404` | `DataNotFound` | Requested resource could not be found |

Applications consuming enveloped responses should check the JSON `status` before processing `data`. The shared helpers do not set the HTTP response status; local tests returned HTTP 200 even for JSON `status` values 400, 403, and 404. Unhandled database/parameter errors can instead produce non-JSON PHP output or an empty body, so check whether the body is valid JSON as well.

## Endpoints

- [Activities](activities.md): compact records, [HTML citations](activities/html.md), [activity table](activities/all.md), [spectrum](activities/spectrum.md), [reviews](activities/reviews.md), and [duplicate detection](activities/duplicates.md).
- [Persons](persons.md): list, [details](persons/profile.md), and [unit memberships](persons/units.md).
- [Projects](projects.md), [proposals](proposals.md), and [funding-number lookup](projects/funding-numbers.md).
- [Conferences](conferences.md), [events](events.md), and [deadlines](deadlines.md).
- [Journals](journals.md) and [full journal records](journals/records.md).
- [Teaching](teaching.md), [infrastructures](infrastructures.md), and [organizations](organizations.md).
- [Search and aggregation](search/index.md), with separate pages for all six entity endpoints.
- [Google Scholar](integrations/google-scholar.md) and [OpenAlex topics](research/openalex-topics.md).

This documentation covers all 28 concrete GET endpoints declared with `/api/` in `routes/api/api.php`. The POST enrichment route is outside this GET endpoint reference.
