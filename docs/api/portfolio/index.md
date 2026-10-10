# Portfolio API Basics

The Portfolio API supplies data for public research profiles, institutional websites, and OSIRIS Portfolio pages. Its endpoints start with `/portfolio/`; the separate [OSIRIS API](../index.md) uses `/api/`.

!!! tip "OSIRIS Portfolio — our hosted SaaS solution"
    [OSIRIS Portfolio](https://osiris-portfolio.de) is our hosted SaaS service for presenting your institution's research online. Choosing this service helps fund our work on open-source OSIRIS. Visit the website to learn more about the solution.

    [Learn more](https://osiris-portfolio.de){.md-button.md-button--primary }

## Authentication

Portfolio access uses the independent OSIRIS setting `portfolio_apikey`. The main API setting `apikey` does not configure this key.

```http
GET /portfolio/publications?apikey=YOUR_PORTFOLIO_API_KEY
```

Alternatively:

```http
X-API-Key: YOUR_PORTFOLIO_API_KEY
```

If no Portfolio key is configured, requests are accepted without authentication. The implementation also accepts access if the global Settings object is unavailable. There is no authenticated-session bypass in the Portfolio key checker itself. All documented routes call this checker.

The local documentation checks succeeded with `apikey=test`, but also with an incorrect key, indicating that Portfolio authentication was not active in that installation. Successful responses therefore verify the schemas, not enforcement of a configured key.

## Response Format

Most routes use the Portfolio `rest` helper:

```json
{
  "status": 200,
  "count": 1,
  "data": [{"id": "665ef1234567890abcdef1234", "name": "Example record"}]
}
```

| Field | Meaning |
|---|---|
| `status` | Application status code in the JSON body, not necessarily the HTTP response status. |
| `count` | List row count before pagination, top-level field count for many detail objects, or an explicitly supplied endpoint-specific count. |
| `data` | A list or a detail/grouped object, depending on the endpoint. |

The helper calculates `count` from any countable payload when the supplied count is zero. Thus settings return `count: 6`, the search index returns `count: 7`, and a detail profile's count varies with its top-level fields. It is not generally `1` for a single detail object.

Country collaboration statistics explicitly return `count: 2` for their two top-level fields; cooperation statistics supply the number of unit labels. Spectrum list counts refer to retained topics, not source publications.

Responses use numeric-string conversion (`JSON_NUMERIC_CHECK`), so numeric strings can become JSON numbers. Retained MongoDB ObjectIDs use Extended JSON:

```json
{"_id": {"$oid": "665ef1234567890abcdef1234"}}
```

Some projections instead return string `id`; context activity lists retain `_id`, and infrastructure IDs normally use their stored identifiers. Each endpoint describes its own ID convention. Empty PHP maps can appear as `[]` instead of `{}`.

Rendered `html`, `print`, `icon`, image, and logo fields can contain HTML. Portfolio HTML can contain `**PORTAL**` placeholders for the consuming application to replace with its base path. Labels depend on OSIRIS language/configuration; fields ending in `_de` or objects with `en`/`de` expose bilingual content explicitly.

## Pagination

List endpoints use the shared `limit` and `offset` parameters:

| Parameter | Type | Behavior |
|---|---|---|
| `limit` | integer | Maximum returned rows; a positive value enables slicing when the calculated/supplied count is positive. Non-positive values disable it. |
| `offset` | integer | Number of rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

```http
GET /portfolio/persons?apikey=YOUR_PORTFOLIO_API_KEY&limit=20&offset=40
```

When slicing occurs, the response adds integer `limit` and `offset` fields. `count` keeps the pre-pagination value. Slicing also applies when `count <= limit`; an offset at or beyond the end returns an empty array. Empty lists with `count: 0` omit pagination metadata. Offset alone has no effect.

**Use pagination only for list responses.** The helper does not distinguish a PHP list from an associative PHP array. Supplying `limit` to an array-backed detail/grouped object can remove object fields or entire groups instead of paginating nested rows. BSON document objects may instead be left unsliced. Omit both parameters for settings, detail profiles, counters, the search index, country summaries, and cooperation matrices. Pagination is not applied independently to nested arrays.

## Visibility and Features

Portfolio routes apply endpoint-specific visibility rules. Typical rules include `hide != true` for persons/activities and `public=true` for projects/infrastructures; not every route applies every rule. For example, collaboration aggregates include non-public projects, unit navigation includes hidden groups, and some spectrum summaries include hidden activities. Consult each endpoint before interpreting a response as a public-only subset.

Activity list/detail routes that use quality workflows apply the `portfolio-workflow-visibility` setting:

- `all`: no additional workflow restriction.
- `only-approved`: `workflow.status` must equal `verified`.
- `approved-or-empty`: accepts verified activities or records without workflow/status.

Feature settings also affect response sections. Disabled news/spectrum routes usually return successful empty arrays; disabled events return an error. Separate related endpoints may apply different rules from the counters and nested lists in a detail profile.

## Identifiers

| Entity | Usual Portfolio identifier |
|---|---|
| Person, activity, news | MongoDB ObjectID as a 24-character hexadecimal string. |
| Project | Prefer MongoDB ObjectID. Some staff/map handlers also accept the exact stored `name`; other operations require ObjectID conversion. |
| Unit | Stored group `id`. Many, but not all, endpoints accept `0` as a root alias. Descendant handling varies by endpoint. |
| Institutional topic | Stored topic `id`, distinct from OpenAlex spectrum IDs. The search index currently uses MongoDB IDs for topics instead. |
| Infrastructure | Stored infrastructure `id`, distinct from its MongoDB ID. |
| OpenAlex spectrum topic | OpenAlex topic ID, e.g. `T12345`. |

URL-encode path identifiers and query values. The path patterns accept one segment, so identifiers cannot contain an unescaped path separator.

## Errors

```json
{
  "status": 403,
  "count": 0,
  "error": "PermissionDenied",
  "msg": "You need a valid API key for this request."
}
```

| Status | Error | Meaning |
|---|---|---|
| `400` | `WrongCall` | Invalid supported call. |
| `403` | `PermissionDenied` | Missing/incorrect configured Portfolio key. |
| `404` | `DataNotFound` | Missing/inaccessible record or disabled feature. |

Check the JSON `status` and validate that the response is JSON. The helpers do not set HTTP status codes; local missing-record responses used HTTP 200 with body `status: 404`. Invalid ObjectIDs or unresolved references can also cause non-JSON errors. Some handlers lack an explicit missing-record check; those exceptions are documented individually.

The Portfolio helper sets `Access-Control-Allow-Origin: *` and allows GET. Its allowed-header list does not include `X-API-Key`, and these routes do not provide an OPTIONS handler, so cross-origin browser header authentication may require CORS configuration. Query-key requests do not require that header.

## Endpoint Reference

All 56 concrete GET endpoints in `routes/api/portfolio.php` are documented separately, including every combination of context and endpoint in the grouped routes. The commented-out unit infrastructure route is not an active endpoint.

- [Settings](settings.md)
- [Publications](publications.md), [activities](activities.md), and [all activities](all-activities.md)
- [Persons](persons.md), [units](units.md), [projects](projects.md), and [topics](topics.md), with related endpoints under each entity
- [Research spectrum](spectrum.md)
- [Infrastructures](infrastructures.md)
- [Events](events.md), [conferences](conferences.md), and [news](news.md)
- [Search index](search-index.md) and [collaborators map](collaborators-map.md)

Examples are illustrative and full stored objects may be abbreviated. Response schemas were compared with local GET responses. Non-empty events, images, research/teaching sections, infrastructure activity rows, and spectra could not be verified with the available local data; their fields are documented from the route/helper code. No automated API tests were added.
