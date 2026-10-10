# Research Spectrum Activities

## `GET /api/spectrum-activities`

Returns activities assigned to an exact research spectrum display name, sorted by spectrum score descending.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `spectrum` | string | Required exact value of `spectrum.display_name` (case-sensitive). |

#### Example Request

```http
GET /api/spectrum-activities?apikey=YOUR_API_KEY&spectrum=Microbiology
```

#### Example Response

```json
{
  "status": 200,
  "count": 0,
  "data": [
    {
      "score": 0.95,
      "icon": "<i class=\"ph ph-file-text\"></i>",
      "activity": "Rendered activity entry",
      "type": "Publications",
      "id": "665ef1234567890abcdef1234"
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Activity ID as string. |
| `score` | Matching spectrum score. |
| `icon, activity, type` | Rendered icon, web representation, and type label. Missing rendered fields may be omitted. |

#### Response Behavior

Each matching spectrum entry produces a row. No matches return `data: []`. Currently `count` is always `0`, even when rows are present, because this route does not pass a count to the response helper. Consequently `limit` and `offset` do not slice results. `json` and `filter` are not read.

The example illustrates the projection defined in the route. The local test data contained no matching spectrum entries, so only the empty result was verified live.
