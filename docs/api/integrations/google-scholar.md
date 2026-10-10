# Google Scholar

## `GET /api/google`

Reads a Google Scholar profile or publication details by scraping Scholar pages. This route does not check the OSIRIS API key.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `user` | string | Required Google Scholar profile ID, not an OSIRIS username. |
| `doc` | string | Optional publication identifier (the suffix after `user:` in `citation_for_view`). Its presence selects document-detail mode. |

`apikey`, `limit`, and `offset` are not read. These responses do not use the standard OSIRIS response envelope.

#### Profile Response

```http
GET /api/google?user=SCHOLAR_USER_ID
```

```json
{
  "total_citations": "42",
  "citations_per_year": {"2025": "12", "2026": "30"},
  "publications": [
    {
      "title": "Example publication",
      "link": "DOCUMENT_ID",
      "authors": "A Example, B Example",
      "venue": "Example Journal",
      "citations": "12",
      "year": "2025"
    }
  ]
}
```

| Field | Description |
|---|---|
| `total_citations` | Scraped total citation text, potentially `null` if absent. |
| `citations_per_year` | Year-to-citation-text map; an empty result is serialized as `[]`. |
| `publications` | At most the first Scholar page, requested with `pagesize=100` and sorted by publication date. |
| `publications[].link` | Document identifier, not a complete URL. |
| `publications[].title, authors, venue` | Scraped publication text. |
| `publications[].citations, year` | Scraped strings; unavailable/non-numeric citations and blank years may become integer `0`. |

#### Document Response

```http
GET /api/google?user=SCHOLAR_USER_ID&doc=DOCUMENT_ID
```

```json
{
  "title": "Example publication",
  "doi": "10.1234/example",
  "Autoren": ["A Example", "B Example"]
}
```

Document mode returns `title`, `doi` (empty string if not extracted), and additional fields keyed by the labels displayed by Scholar. The request uses Scholar's German interface (`hl=de`); the `Autoren` field is split into an array, while other scraped values remain strings. These dynamic keys are not a fixed schema.

#### Availability and Errors

Missing `user` produces an empty response body, not a structured parameter error. Scholar blocking, unavailable pages, or markup changes can yield empty fields or a non-JSON PHP error. The local profile test returned `total_citations: null` and empty arrays; the document test failed to parse an unavailable page. The successful examples above describe the scraper's output schema from the code and are illustrative.
