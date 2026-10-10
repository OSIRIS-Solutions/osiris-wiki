# Person Images

## `GET /portfolio/person-images`

Returns image URLs for persons who permit public profile images.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/person-images?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "path": "http://osiris.test/img/users/alex.example.jpg"
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Person MongoDB ObjectID as string. |
| `path` | Absolute image URL, based on request scheme/host and OSIRIS root path. |

#### Response Behavior

Requires `hide != true` and `public_image=true`; there is no active-person filter. The `db_pictures` feature selects `/image/{username}` URLs; otherwise only existing `img/users/{username}.jpg` files are returned. Rows follow surname order. Unlike person details, no placeholder is returned for missing images. The local check returned an empty list; the non-empty example is based on the code.
