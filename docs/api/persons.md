# Persons

## `GET /api/users`

Returns persons as display-ready profile rows, selected fields, or full stored records.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `json` | string | JSON-encoded MongoDB filter. Overrides `filter`. |
| `filter` | object/array or string | Bracket-notation filter or JSON string. Supports special handling of `is_active` and `units`. |
| `columns[]` | array | Additional stored fields. The base identity and contact fields are always included. |
| `full` | flag | Returns full stored person records whenever present, including `full=0`. |
| `path` | string | Base path for profile and department links in the display HTML. Defaults to the OSIRIS root path. |
| `subtitle` | string | Stored field to show beneath the name. `position` uses the translated position. Defaults to department links. |
| `limit` | integer | Maximum rows under the shared [pagination behavior](index.md#pagination). |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/users?apikey=YOUR_API_KEY&columns[]=orcid
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "username": "alex.example",
      "name": "Alex Example",
      "first": "Alex",
      "last": "Example",
      "position": "Researcher",
      "mail": "alex@example.org",
      "orcid": "0000-0002-1825-0097"
    }
  ]
}
```

#### Default Fields

| Field | Description |
|---|---|
| `id, username` | MongoDB ID as string and OSIRIS username. |
| `img` | Rendered profile image HTML. |
| `html` | Rendered profile card with name, links, subtitle, and optional topic/guest icons. |
| `name, first, last` | Full name and individual name components. |
| `names` | Alternative names joined by commas, or an empty string. |
| `position` | Translated position. |
| `mail, telephone, orcid, academic_title` | Stored contact details, ORCID, and academic title. |
| `dept` | Current department information resolved from unit memberships. |
| `active` | String `yes` or `no`; missing `is_active` is treated as active. |
| `public_image` | Image visibility flag; defaults to `true`. |
| `topics, keywords, roles` | Stored arrays, with empty-array fallbacks. |

#### Filtering

The default filter is `username != null`. Supplying `filter` or `json` replaces that default, rather than adding to it.

In `filter` only, a truthy `is_active` becomes `{"$ne":false}`; false becomes an exact false filter. PHP treats the string `"false"` as true, so use typed JSON when you need an exact boolean. In `json`, `is_active` is used unchanged.

A top-level `units` array in either filter is converted into a membership query requiring a matching unit and a start/end interval covering today (inclusive). Missing/null start or end dates are allowed.

```http
GET /api/users?apikey=YOUR_API_KEY&json=%7B%22units%22%3A%5B%22BIO%22%5D%7D
```

#### Selecting Columns and Full Records

`columns[]` adds stored fields to `id`, `username`, `name`, `first`, `last`, `position`, and `mail`. Requesting one of those fields again can overwrite the rendered base value with the stored value. Unknown requested fields return `null`.

`full` takes precedence over display parameters and `columns[]`. Unlike streamed activity/project exports, it still uses the shared pagination helper. Full records retain `_id` as `{"$oid":"..."}` and all stored fields. `count` is the number of matching persons before slicing. No explicit sort is applied.

A former employee always receives the former-employee subtitle, even when `subtitle` is supplied. `path` affects profile and department links, but does not rewrite all image or topic URLs.

#### Related Endpoints

- [Person details](persons/profile.md)
- [Person unit memberships](persons/units.md)
- [Person search and aggregation](persons/search.md)
