# Person Details

## `GET /portfolio/person/{id}`

Returns a public person profile with contact settings and related research information.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Person MongoDB ObjectID; usernames are not supported. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/person/665ef1234567890abcdef1234?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 16,
  "data": {
    "id": "665ef1234567890abcdef1234",
    "displayname": "Alex Example",
    "first": "Alex",
    "last": "Example",
    "academic_title": "Dr.",
    "position": "Researcher",
    "position_de": "Forschende",
    "depts": [],
    "cv": [],
    "contact": {
      "mail": "alex@example.org"
    },
    "biography": {
      "en": "Example biography",
      "de": null
    },
    "research_profile": {
      "en": "Example research profile",
      "de": null
    },
    "img": "http://osiris.test/img/no-photo.png",
    "show_image": false,
    "numbers": {
      "publications": 7,
      "activities": 4,
      "teaching": 0,
      "projects": 2,
      "infrastructures": 1
    },
    "infrastructures": []
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `id, displayname, first, last, academic_title` | Person ID and stored name fields. |
| `position, position_de` | Stored bilingual position. |
| `inactive` | Only added as `true` when `is_active` is false. |
| `depts` | Flattened readable current unit hierarchy: `id`, `name_en`, `name_de`, `unit_en`, `unit_de`, `indent`, and `hasChildren`. |
| `cv` | Stored CV array or an empty array. |
| `contact` | Public email/phone plus non-empty alternative mail, comment, ORCID, Scholar ID, and social links. |
| `biography, research_profile` | Bilingual objects `{en,de}` with nullable values. |
| `research` | Optional array of bilingual `{en,de}` research entries. |
| `img, show_image` | Absolute profile image or placeholder URL, plus image-display flag. |
| `highlighted` | Optional visible highlighted activities: `{id,icon,html,print}`; workflow restrictions apply and `**PORTAL**` is removed from HTML. |
| `numbers` | Counts for `publications`, `activities`, `teaching`, `projects`, and `infrastructures`; source-activity counts may differ from list lengths. |
| `teaching` | Optional module rows `{id,name,title,affiliation,count}`; only added when a linked module resolves. |
| `projects` | Optional object with `current` and `past` arrays; each row has `id`, `name`, `title`, funding values, project `role`, legacy `start`/`end`, and bilingual `personRole`. |
| `infrastructures` | Array of public person infrastructure rows `{id,name,role,start,end}`; empty when absent/disabled. |
| `topics` | Optional active topic objects `{id,name,name_de,color}`. |
| `spectrum` | Optional aggregated spectrum; the embedded person profile suppresses it when fewer than four publications have assigned topics. |
| `news` | Optional public linked news dated up to today, newest first. |

#### Response Behavior

Requires a person ObjectID and excludes hidden persons. Inactive persons remain accessible. Email and phone default to public unless their visibility flag is false; alternative contact and social fields are included whenever non-empty.

`img` defaults to `/img/no-photo.png`. Public images use the `db_pictures` feature or an existing JPEG; `show_image` indicates whether an actual image is shown. Unit hierarchy excludes memberships whose end timestamp is not in the future; start dates are not checked.

Activity counters use `authors.user`, whereas the standalone person activity lists use `rendered.users`; results can differ for non-author roles. `public_other_activities` and `public_teaching` control embedded activity/teaching counts and sections. Standalone related endpoints do not enforce those switches. Public projects are split into current/past by project dates.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.

#### Errors

Missing or hidden persons return `404 DataNotFound` with `msg: "Person not found"`.
