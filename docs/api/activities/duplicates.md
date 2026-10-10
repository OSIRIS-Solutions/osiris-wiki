# Duplicate Detection

## `GET /api/levenshtein`

Finds the closest stored publication title using PHP's `similar_text` comparison. Despite the endpoint name, the current implementation does not use Levenshtein distance.

#### Parameters

| Parameter | Type | Description |
|---|---|---|
| `apikey` | string | API key, if required. Alternatively use the `X-API-Key` header. |
| `title` | string | Required publication title to compare. |
| `pubmed` | string | Optional exact PubMed lookup; its result is currently overwritten by the title comparison. |
| `doi` | string | Optional exact DOI lookup; its result is currently overwritten by the title comparison. |

#### Example Request

```http
GET /api/levenshtein?apikey=YOUR_API_KEY&title=Example%20publication
```

#### Example Response

```json
{
  "similarity": 100,
  "id": "665ef1234567890abcdef1234",
  "title": "example publication"
}
```

#### Returned Fields

| Field | Description |
|---|---|
| `similarity` | Percentage rounded to one decimal place. Values below 50 are reported as 0. |
| `id` | ID of the selected publication, or an empty string if none was selected. |
| `title` | Stored title after lowercasing and removing HTML tags, or an empty string. |

#### Response Behavior

The response is a direct JSON object without `status`, `count`, or `data`. Authentication failures still use the standard permission error envelope. `limit` and `offset` are ignored.

The comparison selects the title with the largest number of matching characters, then reports that match's percentage. A low score can therefore still return an `id` and `title`. Only activities with `type=publication` are considered.
