# Projects

## `GET /api/projects`

Returns projects as display-ready rows, selected stored fields, grouped counts, or full streamed documents.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `json` | string | JSON-encoded MongoDB filter. Overrides `filter`. |
| `filter` | object/array | Alternative MongoDB filter using bracket notation. |
| `search` | string | Trimmed, case-insensitive title regular expression. Replaces the supplied filter. |
| `columns[]` | array | Optional stored field names. String `id` is included unless explicitly requested as a stored column. |
| `raw` | flag | Skips display formatting whenever present, including `raw=0`. |
| `aggregate` | string | Groups by a stored field and returns counts. Takes precedence over `full` and `formatted`. |
| `full` | flag | Streams full stored records whenever present, including `full=0`. Takes precedence over `formatted` and ignores `columns[]`, `limit`, and `offset`. |
| `formatted` | flag | Builds an additional display-table response. Currently produces invalid JSON; see below. |
| `limit` | integer | Maximum rows in normal/aggregate mode under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Default Response

The default projected fields are:

```text
id, type, acronym, funder, scholarship, start_date, end_date, role, applicant, proposal_id, units, topics, funding_organization, name, title, persons, subproject, timeline, tags
```

Do not include `id` in `columns[]` if you need the generated MongoDB ID: this route replaces it with the stored `id` field when explicitly selected.

Missing stored fields may be omitted. Without `raw`, every projected value is passed through the project renderer: empty values become `"-"`, type/funder/role/status codes become display labels, person and unit arrays become text, and other arrays/objects may become JSON-encoded strings. Labels and HTML depend on the OSIRIS configuration and language.

#### Example Request

```http
GET /api/projects?apikey=YOUR_API_KEY&raw=1&columns[]=name&columns[]=title&columns[]=type
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "name": "EXAMPLE",
      "title": "Example research project",
      "type": "third-party"
    }
  ]
}
```

#### Returned Fields

These descriptions refer to stored values in `raw` mode; display transformations are noted where applicable.

| Field | Description |
|---|---|
| `id` | Record ID as string. |
| `name, acronym, title` | Stored project/proposal identifier, acronym, and title. |
| `type, funder, role` | Stored codes, converted to display labels unless `raw` is present. |
| `start_date, end_date` | Stored dates; these particular field names are not date-formatted by `printField`. |
| `applicant` | Stored applicant value. |
| `units` | Stored unit array; joined into a comma-separated string in display mode. |
| `topics, tags` | Stored arrays; rendered HTML in display mode. |
| `persons` | Stored person objects; names joined by commas in display mode. |
| `scholarship, funding_organization` | Stored organization references, resolved to HTML links in display mode when possible. |
| `proposal_id` | Linked proposal identifier. |
| `subproject, timeline` | Stored subproject flag and timeline data. |

#### Filtering and Permissions

A top-level `public` filter value is cast to a PHP boolean. Use actual JSON booleans (`true`/`false`), since the string `"false"` is truthy. `search` replaces earlier filters with a title expression.

A valid API key does not bypass the separate `projects.view` permission check. If that permission is missing, results are additionally restricted to records where the current session username occurs in `persons.user` or `created_by`. A request without a session can therefore return an empty list despite existing records.

Live requests in the local test installation returned empty lists. The non-empty example and field descriptions are based on the route's projection and renderer.

#### Aggregation

```http
GET /api/projects?apikey=YOUR_API_KEY&aggregate=type
```

```json
{
  "status": 200,
  "count": 1,
  "data": [{"count": 3, "value": "third-party"}]
}
```

If the field name contains `persons`, that array is unwound; otherwise, a field name containing `collaborators` unwinds that array. Values are stored values, not display labels. Results are sorted by group count descending, and response `count` is the number of groups.

This route inserts `$match` even for an empty filter. In contexts where no permission filter is added, supply a non-empty filter to avoid a MongoDB error, or use [projects search](projects/search.md) for unfiltered aggregation.

#### Full Export

```http
GET /api/projects?apikey=YOUR_API_KEY&full=1
```

Full mode streams the complete stored records, sorted by `_id` descending. `_id` and other ObjectIDs retain Extended JSON representation (`{"$oid":"..."}`). `count` is the number of streamed documents. Normal projected lists are sorted by `year` descending.

#### Formatted Mode

The presence of `formatted` builds rows containing:

```text
id, name, title, type, status, date_range, start, funder,
funding_organization, funding_numbers, applicant, activities,
role, topics, units, persons, subproject, tags
```

`date_range` is formatted by OSIRIS; `start` comes from `start_date`; `funding_numbers` uses `; ` as a separator; `applicant` resolves `contact` or `supervisor`; `activities` counts activities whose `projects` field matches the record's `name`; `persons` is an array of names; `units` is resolved from person memberships. `subproject` currently falls back to `false` because the route reads an unrelated variable.

Currently the handler continues after emitting this response and appends the normal list response. Even an empty result is therefore two adjacent objects:

```text
{"status":200,"count":0,"data":[]}{"status":200,"count":0,"data":[]}
```

This is not valid JSON. Use `raw`, `columns[]`, or `full` for a single parseable response.

#### Related Endpoints

- [Projects search and aggregation](projects/search.md)
- [Projects by funding number](projects/funding-numbers.md)
