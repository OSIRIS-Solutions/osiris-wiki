# Unit Details

## `GET /portfolio/unit/{id}`

Returns a unit profile with public images, hierarchy, research sections, and section counters.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Stored group ID, or `0` for the root group. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 13,
  "data": {
    "id": "BIO",
    "name": "Biology Unit",
    "name_de": "Biologie",
    "parent": "EXAMPLE",
    "unit": {
      "name": "Department",
      "name_de": "Abteilung",
      "head": "Head of Department",
      "head_de": "Abteilungsleitung"
    },
    "level": 1,
    "hide": false,
    "order": 1,
    "images": [],
    "heads": [
      {
        "id": "665ef1234567890abcdef1235",
        "name": "Alex Example",
        "img": "<img class=\"profile-img small\" src=\"/img/no-photo.png\">",
        "position": "Researcher",
        "position_de": "Forschende"
      }
    ],
    "children": [],
    "research": [],
    "numbers": {
      "persons": 4,
      "publications": 12,
      "activities": 3,
      "collaborators": 2,
      "projects": 2,
      "infrastructures": 0
    }
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `_id and other stored fields` | The underlying group document is retained except for `head`; `_id` remains Extended JSON. |
| `unit` | Resolved organizational unit-type object with bilingual `name`/`name_de` and head-role labels `head`/`head_de`. |
| `images` | Filtered public image metadata; see below. |
| `heads` | Visible head persons with `id`, stored display `name`, profile-image HTML `img`, `position`, and `position_de`. Absent when no head IDs are stored. |
| `parent_details, children` | Compact parent and visible child objects with `id`, bilingual names, `level`, and `hide`. Parent may be null. |
| `topics` | Active topic detail objects `{id,name,name_de,color}` when topic IDs were stored. |
| `research` | Research sections with bilingual headings/info and resolved activities. Activity `html` uses `rendered.print` here. |
| `numbers` | Section counters described below; includes distinct collaborator organizations, with no project-public filter for that collaborator count. |
| `spectrum` | Optional aggregated research spectrum when `portfolio-spectrum` is enabled. |
| `nav_topics, nav_units` | Topic navigation and flattened unit hierarchy. |

#### Response Behavior

`0` resolves to the level-0 root group; normal IDs are matched against group `id`. This handler does not explicitly reject hidden or missing groups; a missing group can cause incomplete output or an unhandled error rather than a structured 404.

Public `images` entries contain `id`, `filename`, absolute `url`, nullable `thumbnail_url`, `mime`, `size`, `width`, `height`, bilingual `caption`/`caption_de`, `taken_at`, `credits`, `public`, `uploaded_at`, and `order`. Only records with `public`, `file`, and `id` are included. Images are sorted by `order` ascending, then upload timestamp descending.

`nav_topics` supplies the Portfolio topic list. `nav_units` is a flattened navigation hierarchy with `id`, `parent`, `name`, `name_de`, `order`, `level`, `active`, `open`, `openable`, and `hide` per row. Navigation is built from Portfolio's cached/fetched topic and unit data.

Counter definitions follow [unit numbers](numbers.md), except that this profile omits `memberships` and `cooperation`, and adds `collaborators`.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.
