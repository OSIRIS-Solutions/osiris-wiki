# OpenAlex Topics

## `GET /api/openalex/topics`

Searches OSIRIS's local OpenAlex topic catalogue. The API-key check is disabled in this route; no API key is required, and no live OpenAlex request is made.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `q` | string | Required search text. Trimmed and lowercased; must contain at least two characters. |
| `limit` | integer | Can reduce the final list using the shared [pagination behavior](../index.md#pagination), after the fixed 20-result cap. |
| `offset` | integer | Number of rows to skip when `limit` is used. Defaults to `0`; negative values become `0`. |

#### Example Request

```http
GET /api/openalex/topics?q=biology
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "id": "T12345",
      "name": "Example Biology Topic",
      "subfield_id": 1312,
      "subfield": "Molecular Biology",
      "field_id": 13,
      "field": "Biochemistry, Genetics and Molecular Biology",
      "domain_id": 1,
      "domain": "Life Sciences",
      "path": "Life Sciences → Biochemistry, Genetics and Molecular Biology → Molecular Biology",
      "search": "life sciences molecular biology example biology topic",
      "description": "Example topic description.",
      "keywords": [
        "Biology"
      ]
    }
  ]
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `id, name` | OpenAlex topic ID and topic name. |
| `subfield_id, subfield` | Subfield identifier and label. |
| `field_id, field` | Field identifier and label. |
| `domain_id, domain` | Domain identifier and label. |
| `path` | Human-readable domain → field → subfield hierarchy. |
| `search` | Stored combined searchable text. |
| `description, keywords` | Stored topic description and keywords. |

#### Matching and Ordering

The query is matched against each topic's `search` text using a case-insensitive substring search. Matching topics are ranked by topic name: exact match, prefix match, substring match, then other matches in the search text. Ties are sorted alphabetically by name.

Only the first 20 matches are retained. `count` is the retained count (at most 20) before any additional shared pagination, not the total number of catalogue matches. The internal `_relevance` value is removed.

#### Errors

| Status | Error | Message |
|---|---|---|
| `400` | `WrongCall` | `Query too short` (also when `q` is omitted). |
| `404` | `DataNotFound` | `OpenAlex topics data not found` if the local catalogue file is missing. |
