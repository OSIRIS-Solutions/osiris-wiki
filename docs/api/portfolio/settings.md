# Settings

## `GET /portfolio/settings`

Returns the configuration required to build Portfolio navigation and feature sections.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. Do not paginate this object response. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/settings?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 6,
  "data": {
    "version": "2.1.1",
    "features": {
      "projects": true,
      "infrastructures": true,
      "topics": true,
      "news": true,
      "spectrum": true
    },
    "labels": {
      "topics": {
        "en": "Research Topics",
        "de": "Forschungsbereiche"
      },
      "journals": {
        "en": "Journals",
        "de": "Zeitschriften"
      },
      "infrastructures": {
        "en": "Infrastructures",
        "de": "Infrastrukturen"
      }
    },
    "publication_types": [
      {
        "id": "article",
        "en": "Journal Article",
        "de": "Zeitschriftenartikel"
      }
    ],
    "activity_categories": [
      {
        "id": "event",
        "en": "Events",
        "de": "Veranstaltungen"
      }
    ],
    "affiliation": {
      "id": "EXAMPLE",
      "name": "Example Institute"
    }
  }
}
```

The example uses illustrative values; the object is abbreviated. Optional stored or rendered fields may be omitted. For detail objects, response `count` usually counts top-level fields; see the basics.

#### Returned Fields

| Field | Description |
|---|---|
| `version` | OSIRIS version. |
| `features` | Booleans for `projects`, `infrastructures`, `topics`, `news`, and `spectrum`. Spectrum requires both `spectrum` and `portfolio-spectrum` features. |
| `labels` | Configured bilingual labels for topics, journals, and infrastructures, with default labels. |
| `publication_types` | Portfolio-enabled publication subtypes as `{id,en,de}`, sorted by configured `order`. |
| `activity_categories` | Non-publication categories whose subtypes are Portfolio-enabled, as `{id,en,de}`, sorted by `order`. |
| `affiliation` | Configured `affiliation_details` object, or `null`. May include institute ID/name, link, ROR/OpenAlex references, location, country, and coordinates. |

#### Response Behavior

`data` is a configuration object. `count` is its top-level field count (currently 6), not a number of settings records. Do not use `limit` or `offset` on this object.
