# Search and Aggregation

The entity-specific search endpoints share the behavior below. Each endpoint has its own page with parameters, default fields, and response examples:

- [Activities](../activities/search.md)
- [Projects](../projects/search.md)
- [Proposals](../proposals/search.md)
- [Conferences](../conferences/search.md)
- [Journals](../journals/search.md)
- [Persons](../persons/search.md)

## Filtering

`json` is a JSON-encoded MongoDB filter and overrides `filter`. A non-object/array decoded value is replaced by an empty filter; an empty top-level `$and` is removed. `filter` expects bracket notation, not a JSON string.

The route recursively casts values named `public` to PHP booleans and values named `projects` to ObjectIDs. Use typed JSON booleans; the string `"false"` is truthy. The recursive conversion checks leaf keys, so array/object filters should be verified against the actual stored data types.

`search` always replaces the supplied filter with a case-insensitive regular expression on the stored `title` field. This applies to every entity, including journals and persons: it does not automatically search `journal`, `name`, `first`, or `last`. Use `json` to search those fields.

```http
GET /api/search/persons?apikey=YOUR_API_KEY&json=%7B%22last%22%3A%7B%22%24regex%22%3A%22Example%22%2C%22%24options%22%3A%22i%22%7D%7D
```

## Selecting Columns

`columns[]` selects stored fields plus string `id`; `_id` is excluded. Missing fields may be omitted. For activity-specific rendered columns, see [activity search](../activities/search.md).

Selecting a field whose first path component is one of the following arrays unwinds that array before filtering and projecting:

```text
authors, editors, supervisors, persons, collaborators, topics,
metrics, impact, units
```

For example, `columns[]=authors.user` produces one row per author, with an `authors` object containing `user`, rather than one row per activity. Nested projected paths become nested output objects. Empty or missing arrays can remove a record; several selected array fields can multiply rows. Each selected field adds an unwind stage, even when several fields refer to the same array. Activity columns redirected to rendered fields skip this step.

Normal results are sorted by stored `year` descending. `count` is the number of projected rows before slicing, including rows created by unwinding; it need not equal the number of distinct documents. `full` and `formatted` are not supported here.

## Aggregation

A non-empty `aggregate` selects a stored grouping field. `aggregate_function` defaults to `count` and accepts `count`, `sum`, `mean`, or `median`. The latter three require a numeric `aggregate_value` field.

Grouping/value paths must consist of dot-separated components starting with a letter or underscore, followed by letters, digits, underscores, or hyphens. Array fields from the list above are automatically unwound once per distinct array, preserving null and empty arrays. If two distinct arrays are involved, their combinations can multiply rows.

Aggregation uses stored field values, including for activities; rendered column mapping and project display formatting do not apply. `columns[]` is ignored.

#### Count

```http
GET /api/search/activities?apikey=YOUR_API_KEY&aggregate=year
```

```json
{
  "status": 200,
  "count": 2,
  "data": [
    {"count": 30, "value": 2026},
    {"count": 15, "value": 2025}
  ]
}
```

Each row contains the group `value` and its `count`. Groups are sorted by count descending. The response-level `count` is the number of groups, not the number of source documents.

#### Numeric Functions

```http
GET /api/search/activities?apikey=YOUR_API_KEY&aggregate=type&aggregate_function=mean&aggregate_value=year
```

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {"value": "publication", "result": 2025.5}
  ]
}
```

Each row contains the group `value` and numeric `result`; groups are sorted by result descending. Only MongoDB numeric types (`int`, `long`, `double`, `decimal`) are included. Numeric strings, nulls, and other types are excluded; groups without numeric values disappear.

`sum` adds numeric values, `mean` calculates their arithmetic average, and `median` selects the middle sorted value (or averages the two middle values). Median uses MongoDB's `$sortArray` operator and requires server support for it.

#### Errors

| Status | Error | Message |
|---|---|---|
| `400` | `WrongCall` | `Unsupported aggregation function.` |
| `400` | `WrongCall` | `Invalid aggregation field.` |
| `400` | `WrongCall` | `A valid numeric value field is required for this aggregation.` |

These validation errors use the standard error envelope. Empty `aggregate` skips aggregation and returns normal rows; function/value parameters alone do not activate it.

## Permissions and Pagination

Projects and proposals have an additional entity-view permission check; without that permission, results are restricted to records linked to or created by the current session username. The API key does not bypass it.

For activities, an authenticated session without an `apikey` query parameter gets the user's activity visibility filter, even with a header API key.

Normal and aggregate modes both use the shared [pagination behavior](../index.md#pagination). `offset` skips the requested number of rows when a positive `limit` is used. Examples on these pages are illustrative unless explicitly stated otherwise.
