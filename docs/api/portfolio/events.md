# Events

## `GET /portfolio/events`

Returns public conferences/events with linked activities. `/portfolio/events` and `/portfolio/conferences` are aliases with separate endpoint pages.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/events?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "665ef1234567890abcdef1234",
      "title": "Example symposium",
      "start": "2026-11-01",
      "end": "2026-11-02",
      "location": "Berlin",
      "type": "Conference",
      "country": "Germany",
      "topics": [],
      "activities": []
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id` | Conference/event MongoDB ObjectID as string. |
| `title, title_full, description` | Stored event title, optional full title, and description. |
| `start, end, location, url` | Stored dates, venue, and external URL. |
| `type` | Translated configured `event-type` vocabulary label, or `"-"`. |
| `country` | Translated country name rather than the stored country code. |
| `topics` | Active institutional topic objects `{id,name,name_de,color}`, when stored topics are non-empty. |
| `activities` | Non-hidden linked activities with `id`, `icon`, rendered `type`/`subtype`, `html`, `title`, `subtitle`, and `print`. |

#### Response Behavior

Requires the `events` feature and `public=true` in conference records. The query sorts by `name` ascending, even though the projection returns `title`; this is not a guaranteed title sort. Linked activities are sorted by year/month descending and filtered only by `hide != true`, without subtype or workflow restrictions. The local installation returned no public events; the non-empty schema is documented from the code.

#### Errors

If events are disabled: `status: 404`, `error: DataNotFound`, and `msg: {"message":"Feature not enabled"}`. The error message is an object here, not a string.
