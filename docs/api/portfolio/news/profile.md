# News Details

## `GET /portfolio/news/{id}`

Returns a public news article with resolved linked entities and an optional featured card.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | News MongoDB ObjectID. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/news/665ef1234567890abcdef1234?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 16,
  "data": {
    "id": "665ef1234567890abcdef1234",
    "title": "Example research news",
    "title_de": null,
    "teaser": "Example summary",
    "teaser_de": null,
    "date": "2026-09-01",
    "content": "Example article content",
    "content_de": null,
    "image": null,
    "activities": [],
    "persons": [],
    "projects": [],
    "infrastructures": [],
    "topics": [],
    "events": [],
    "featured": null
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | News MongoDB ObjectID as string. |
| `title, title_de, teaser, teaser_de` | Stored bilingual title and teaser. |
| `date` | Stored news date. |
| `content, content_de, image` | Stored bilingual article content and image value/path; the image is not converted to an absolute URL here. |
| `topics` | Stored topic references, not expanded by this handler. |
| `activities` | Resolved non-hidden activities with string `id`, rendered `icon`, `type`, `subtype`, `html`, `title`, `subtitle`, and `print`. |
| `persons` | Resolved visible persons with `id`, `displayname`, `academic_title`, `position`, and `position_de`; `_id` is retained. |
| `projects` | Resolved public projects with `id`, `name`, `title`, `acronym`, `start_date`, `end_date`, and `type`; `_id` is retained. |
| `infrastructures` | Resolved public infrastructures with `id`, `name`, `subtitle`, and `type`; `_id` is retained. The returned `id` is the MongoDB ID, unlike the stored identifier used by infrastructure endpoints. |
| `events` | Resolved public events with `id`, `title`, `title_full`, `start`, `end`, `location`, and `country`; `_id` is retained. |
| `featured` | Resolved card `{id,type,type_label,icon,title,subtitle,text}`, or null. It represents a linked activity, person, project, infrastructure, or event. |

#### Response Behavior

Requires `visibility=public`, but unlike the list this endpoint does not restrict `date <= today`, so future public news can be retrieved by ID. If news is disabled, returns successful `data: []` instead of a detail object.

Unresolved/hidden linked entities are skipped, and public visibility is checked for linked projects/infrastructures/events. Linked activities do not receive a subtype/workflow restriction. A resolved featured entity is removed from its normal related list and shown only as the featured card. The card uses translated labels/text and an icon; if its title cannot be resolved, `featured` becomes null. Because infrastructure links return MongoDB IDs but featured references use stored IDs in some data, an infrastructure featured card may fail to resolve.

#### Errors

Missing or non-public news returns `404 DataNotFound` with `msg: "News not found"` when the feature is enabled.
