# Activities

## `GET /api/activities`

Returns activities stored in OSIRIS.

This endpoint can be used to retrieve activity records, select specific fields, filter results, aggregate records by a field, or export full activity data.

The API key can be passed as query parameter:

```http
GET /api/activities?apikey=YOUR_API_KEY
```

or as HTTP header:

```http
X-API-Key: YOUR_API_KEY
```

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. |
| `json` | string | JSON-encoded MongoDB filter. Overrides `filter` if both are provided. |
| `filter` | object/array | Alternative MongoDB filter using bracket notation, e.g. `filter[year]=2025`. Values supplied this way are strings; use `json` for typed values. |
| `columns[]` | array | Optional list of fields to return. |
| `aggregate` | string | Groups activities by the given field and returns counts. |
| `full` | flag | Returns full activity records as streamed JSON when present, including `full=0`. Ignored if `aggregate` is present. |
| `limit` | integer | Limits the number of returned records. |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Default Response

Without additional parameters, the endpoint returns a compact activity list.

```http
GET /api/activities?apikey=YOUR_API_KEY
```

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "activity": "Rendered activity entry",
      "icon": "ph-book-open",
      "type": "Publication",
      "subtype": "Journal Article",
      "year": 2025
    }
  ]
}
```

Default fields:

| Field | Description |
|---|---|
| `id` | Activity ID as string. |
| `activity` | Rendered HTML representation for web display. |
| `icon` | Icon representing the activity type. |
| `type` | Rendered activity type. |
| `subtype` | Rendered activity subtype. |
| `year` | Activity year. |

#### Selecting Columns

Use `columns[]` to request specific fields.

```http
GET /api/activities?apikey=YOUR_API_KEY&columns[]=title&columns[]=year&columns[]=type
```

Some fields are read from the rendered activity representation:

```text
web, print, icon, type, subtype, authors, title, departments
```

All other fields are read directly from the activity record. Do not explicitly select `columns[]=id` if you need the generated MongoDB ID: this replaces it with the stored `id` field.

Example:

```http
GET /api/activities?apikey=YOUR_API_KEY&columns[]=title&columns[]=doi&columns[]=year
```

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "title": "Rendered publication title",
      "doi": "10.1234/example",
      "year": 2025
    }
  ]
}
```

#### Filtering

Use the `json` parameter to pass a MongoDB filter.

```http
GET /api/activities?apikey=YOUR_API_KEY&json={"year":2025}
```

URL-encoded:

```http
GET /api/activities?apikey=YOUR_API_KEY&json=%7B%22year%22%3A2025%7D
```

Example: publications from 2025:

```json
{
  "type": "publication",
  "year": 2025
}
```

Example with MongoDB operator:

```json
{
  "year": {
    "$gte": 2023
  }
}
```

#### Pagination

The compact list and aggregation use the shared `limit` and `offset` parameters. `count` contains the number of matching rows before slicing (the number of groups for aggregation).

```http
GET /api/activities?apikey=YOUR_API_KEY&limit=50
```

`offset=100` skips the first 100 matching rows, and the response reports the integer `offset: 100`. Pagination also applies when `count <= limit`; offsets at or beyond the end return an empty array. Full exports ignore both parameters. See [API Basics](index.md#pagination).

#### Aggregation

Use `aggregate` to group activities by one field.

```http
GET /api/activities?apikey=YOUR_API_KEY&aggregate=year&json=%7B%22year%22%3A%7B%22%24gte%22%3A2020%7D%7D
```

```json
{
  "status": 200,
  "count": 2,
  "data": [
    {
      "count": 42,
      "value": 2025
    },
    {
      "count": 37,
      "value": 2024
    }
  ]
}
```

Aggregation can be combined with filters:

```http
GET /api/activities?apikey=YOUR_API_KEY&aggregate=year&json=%7B%22type%22%3A%22publication%22%7D
```

The current implementation requires a non-empty filter for this aggregation: without one, the empty PHP array passed to MongoDB as `$match` can cause a database error and a non-JSON response. Use an explicit filter as above, or [activity search](activities/search.md) for unfiltered aggregation.

If the aggregation field contains `authors`, OSIRIS unwinds the `authors` array before grouping.

Example:

```http
GET /api/activities?apikey=YOUR_API_KEY&aggregate=authors.user&json=%7B%22type%22%3A%22publication%22%7D
```

#### Full Export

Use `full=1` to retrieve full activity records.

```http
GET /api/activities?apikey=YOUR_API_KEY&full=1
```

This returns streamed JSON:

```json
{
  "status": 200,
  "data": [
    {
      "_id": {"$oid": "665ef1234567890abcdef1234"},
      "type": "publication",
      "year": 2025
    }
  ],
  "count": 1
}
```

This mode is intended for exports or synchronization. It may return large datasets.

#### Response Behavior

Compact records are sorted by `year` descending. Projected fields may be omitted when the corresponding stored or rendered field is missing. Rendered fields contain display text or HTML and depend on the OSIRIS language and configuration; `icon` can contain HTML rather than an icon class alone.

Full records are sorted by `_id` descending and retain all stored fields, including `_id` in MongoDB Extended JSON form (`{"$oid":"..."}`). The presence of `full` activates this mode; `columns[]`, `limit`, and `offset` are ignored. `count` is the number of streamed records.

For authenticated sessions without an `apikey` query parameter, OSIRIS also applies the user's activity visibility filter, even if the API key was supplied as a header.

#### Related Endpoints

- [Publication citations](activities/html.md)
- [Activity table](activities/all.md)
- [Research spectrum activities](activities/spectrum.md)
- [Reviews](activities/reviews.md)
- [Duplicate detection](activities/duplicates.md)
- [Activity search and aggregation](activities/search.md)
