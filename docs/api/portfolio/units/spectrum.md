# Unit Spectrum

## `GET /portfolio/unit/{id}/spectrum`

Returns the aggregated OpenAlex research spectrum for one unit.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `id (path)` | string | Exact group ID, or `0` for the root group. |
| `apikey` | string | Portfolio API key, if configured; alternatively use `X-API-Key`. |
| `limit` | integer | Maximum list rows. A positive value enables shared pagination. |
| `offset` | integer | List rows to skip with `limit`. Defaults to `0`; negative values become `0`. |

Authentication, pagination, and error envelopes are described in [Portfolio API Basics](../index.md). Parameters such as `json`, `filter`, `columns[]`, `full`, and `aggregate` are not supported unless listed here.

#### Example Request

```http
GET /portfolio/unit/BIO/spectrum?apikey=YOUR_PORTFOLIO_API_KEY
```

#### Example Response

```json
{
  "status": 200,
  "count": 1,
  "data": [
    {
      "_id": "T12345",
      "count": 4,
      "sumScore": 3.2,
      "topic": {
        "id": "T12345",
        "name": "Example Biology Topic",
        "score": 0.8
      },
      "total": 8,
      "avg_score": 0.8,
      "share": 0.5,
      "weight": 0.2
    }
  ]
}
```

The example uses illustrative values. Optional stored or rendered fields may be omitted. Response `count` is the number of returned list rows before shared pagination.

#### Returned Fields

| Field | Description |
|---|---|
| `_id` | OpenAlex topic ID, e.g. `T12345`. |
| `count` | Number of assigned topic occurrences in matching publications. |
| `sumScore` | Sum of stored topic scores. |
| `topic` | First stored OpenAlex topic object for this group, including available ID/name/score/hierarchy metadata. |
| `total` | Number of matching publications with non-empty assigned OpenAlex topics. |
| `avg_score` | `sumScore / count`. |
| `share` | `count / total`. |
| `weight` | `(count / total) * (sumScore / total)`. |

#### Response Behavior

Matches publication `units` by exact group ID, without descendants. `0` resolves to the level-0 root ID.

Requires publication `type=publication` and non-empty `openalex.topics`. The spectrum helper does not add hide, affiliation, or workflow restrictions. Topics below 5% share are discarded; remaining topics are sorted by `weight` descending and capped at 25 before shared pagination. Response `count` is the retained number of topics, not source publications.

When `portfolio-spectrum` is disabled, or no matching assignments exist, returns a successful empty array. The local data contained no spectrum rows; the non-empty example follows the helper implementation.
