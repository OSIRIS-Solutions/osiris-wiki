# Persons

## `GET /portfolio/persons`

Returns a compact list of visible persons.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/persons?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "displayname": "Alex Example",
      "academic_title": "Dr.",
      "position": "Researcher",
      "position_de": "Forschende",
      "is_active": true
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Person MongoDB ObjectID as string. |
| `displayname, academic_title` | Stored display name and academic title. |
| `position, position_de` | Stored bilingual position text. |
| `depts` | Legacy stored department references, if present; not resolved from current unit memberships. |
| `is_active` | Stored active flag, if present. |

#### Response Behavior

Persons with `hide=true` are excluded; inactive persons remain included. Rows are sorted by `last` ascending. This list does not include contact details or image URLs.

#### Related Endpoints

- [Person Details](persons/profile.md)
- [Person Activities](persons/activities.md)
- [Person All Activities](persons/all-activities.md)
- [Person Images](persons/images.md)
- [Person Infrastructures](persons/infrastructures.md)
- [Person Projects](persons/projects.md)
- [Person Publications](persons/publications.md)
- [Person Spectrum](persons/spectrum.md)
- [Person Teaching](persons/teaching.md)
