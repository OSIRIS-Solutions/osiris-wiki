# Project Details

## `GET /portfolio/project/{id}`

Returns a public project profile, team, collaborators, and linked information.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Project MongoDB ObjectID. Prefer this to a project name because later operations require ObjectID conversion. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/project/665ef1234567890abcdef1234?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 24,
  "data": {
    "id": "665ef1234567890abcdef1234",
    "name": "EXAMPLE",
    "title": "Example research project",
    "funder": "Example funder",
    "funding_number": [
      "GRANT-001"
    ],
    "role": "coordinator",
    "type": "Research project",
    "start": {
      "year": 2025,
      "month": 1
    },
    "end": null,
    "teaser_en": "Example project",
    "teaser_de": null,
    "acronym": "EX",
    "abstract": "Example abstract",
    "start_date": "2025-01-01",
    "end_date": null,
    "persons": [],
    "activities": 5,
    "subprojects": [],
    "collaborators": [],
    "website": null,
    "img": null,
    "funding_organization": "Example funder",
    "scholarship": "-",
    "university": "-"
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `id, acronym, name, name_de, title, title_de` | Project identity and bilingual titles. |
| `type` | Translated configured project type label. |
| `abstract, abstract_de` | Bilingual abstract; empty/null fallbacks. |
| `funder, funding_number, coordinator, role` | Stored funding, coordinator, and role values; role defaults to `partner`. |
| `funding_organization, scholarship, university` | Organization names resolved by the project renderer in Portfolio mode, stored fallback values, or `"-"`. |
| `start, end, start_date, end_date` | Legacy and stored date ranges. |
| `persons` | Team membership fields retained except `user`; adds person string `id`, bilingual `role`, and level-1 `depts` map `{group_id:{en,de}}`. |
| `activities` | Count of non-hidden linked activities, subject to workflow visibility; see the current restriction below. |
| `parent, subprojects` | Public parent/subproject summaries `{id,name,title}` when found. |
| `collaborators` | Resolved organizations: `id`, `role`, `name`, `type`, `location`, `country`, `ror`, `lat`, and `lng`. |
| `website, img` | Stored website and absolute uploaded project image URL, or null. |
| `topics` | Optional active topic objects `{id,name,name_de,color}`. |
| `news` | Optional public project news dated up to today, newest first. |

#### Response Behavior

Requires a public project. Use its MongoDB ObjectID: although the initial lookup accepts a project name, subsequent ObjectID conversion expects a valid ObjectID and a name can fail. Hidden/unresolved team members are skipped, but inactive members remain. Team memberships follow role order `applicant`, `PI`, `Co-PI`, `worker`, `associate`, `student`.

The activity counter normally matches the project name or ObjectID. With workflow visibility `approved-or-empty`, the handler overwrites that project `$or` with the workflow `$or`, so the counter can include unrelated non-hidden activities. This is documented as implemented; this change only fixes pagination. Public subprojects are found by `parent_id`, while the parent summary reads `parent`.

#### Errors

Missing or non-public projects return `404 DataNotFound` with `msg: "Project not found"`.
