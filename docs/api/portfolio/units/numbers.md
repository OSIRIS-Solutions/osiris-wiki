# Unit Numbers

## `GET /portfolio/unit/{id}/numbers`

Returns counters used to show or hide unit profile sections.

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
GET /portfolio/unit/BIO/numbers?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 8,
  "data": {
    "general": 1,
    "persons": 4,
    "publications": 12,
    "activities": 3,
    "memberships": 2,
    "projects": 2,
    "cooperation": 1,
    "infrastructures": 0
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `general, research` | Optional `1` flags indicating available descriptions/public images and research sections. |
| `persons` | Active visible persons with a membership in this unit/descendants whose end date is null or not earlier than today. Start dates are not checked. |
| `publications` | Non-hidden publications in this unit/descendants, subject to workflow visibility. |
| `activities` | Non-hidden Portfolio-enabled non-publication activities, subject to workflow visibility. |
| `projects` | Public projects in this unit/descendants, or `0` when projects are disabled. |
| `infrastructures` | Number of public infrastructures only for the root group when enabled; otherwise `0`. |
| `memberships` | Count of activities in configured continuous subtypes; this particular counter does not add hide/workflow restrictions. |
| `cooperation` | For level-1 groups only: number of distinct publication units minus one, with a minimum of 0, using publications since `CURRENTYEAR - 4`. |

#### Response Behavior

Group `0` resolves to the root. Unit-related counts include descendants. Counter values are not the response-level `count`, which counts object fields. No affiliation requirement is added to publication/activity counters.

The root-only infrastructure count covers all public infrastructures. `general` and `research` are omitted when no content exists; `cooperation` is omitted for levels other than 1.

When quality workflows are enabled, `portfolio-workflow-visibility` applies: `all` imposes no extra restriction; `only-approved` requires `workflow.status=verified`; `approved-or-empty` accepts verified records or records with no workflow/status. This is a configured restriction, not a request parameter.

#### Errors

Missing groups return `404 DataNotFound` with `msg: "Group not found"`.
