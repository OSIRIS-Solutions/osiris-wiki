# Research Spectrum

## `GET /portfolio/spectrum`

Returns institute-wide OpenAlex topic summaries for research spectrum pages.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/spectrum?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "T12345",
      "openalex_url": "https://openalex.org/T12345",
      "name": "Example Biology Topic",
      "description": "Example description",
      "keywords": [
        "Biology"
      ],
      "path": "Life Sciences → Biology",
      "domain_id": 1,
      "domain": "Life Sciences",
      "field_id": 13,
      "field": "Biology",
      "subfield_id": 1312,
      "subfield": "Molecular Biology",
      "count": 4,
      "share": 0.5,
      "timeline": [
        {
          "year": 2026,
          "count": 4
        }
      ],
      "units": [
        {
          "id": "BIO",
          "name": "Biology",
          "name_de": "Biologie",
          "count": 4
        }
      ],
      "researchers": [
        {
          "id": "665ef1234567890abcdef1234",
          "displayname": "Alex Example",
          "academic_title": "Dr.",
          "position": "Researcher",
          "position_de": null,
          "count": 4
        }
      ],
      "activities": [
        {
          "id": "665ef1234567890abcdef1235",
          "year": 2026,
          "score": 0.9,
          "html": "Rendered entry",
          "icon": "Rendered icon",
          "type": "Publications",
          "subtype": "Journal Article"
        }
      ]
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `id, openalex_url` | OpenAlex topic ID and public OpenAlex URL. |
| `name, path, domain_id, domain, field_id, field, subfield_id, subfield` | Topic labels and hierarchy from stored activity assignments. Numeric strings may become JSON numbers. |
| `description, keywords` | Enrichment from the local OpenAlex topic catalogue; missing description becomes null. |
| `count, share` | Topic assignment count and count divided by all institute activities with affiliated=true and non-empty assignments; share rounded to six decimals. |
| `timeline` | Annual `{year,count}` topic-assignment counts, sorted oldest first. |
| `units` | Up to 10 visible non-root related units `{id,name,name_de,count}`, sorted by count descending then ID. |
| `researchers` | Up to 10 active visible persons `{id,displayname,academic_title,position,position_de,count}`, matched via `rendered.users` and sorted by count descending then username. |
| `activities` | Optional, up to 10 recent high-score entries `{id,year,score,html,icon,type,subtype}`, sorted by year descending then activity ID. |

#### Response Behavior

Requires `portfolio-spectrum`; disabled features or no topics return an empty list. The source filter requires `affiliated=true` and non-empty `openalex.topics`, but does not restrict activity `type`, hide, or workflow status. Topic counts can therefore include hidden/non-publication activities; related visible people/units are filtered separately.

Topics are sorted by assignment count descending, then topic ID. Recent activities require `year >= current calendar year - 5` and topic score at least `0.8`; despite the code comment, there is no publication-type restriction. No query parameter changes the fixed related-item cap of 10. The local data had no assignments, so the non-empty example is based on the source.

#### Errors

A missing catalogue returns `404 DataNotFound` with `msg: "OpenAlex topics data not found"`. This particular error uses the main API response helper.
