# API Basics

!!! info "Under Construction"
    The API documentation is currently being updated. Some information may be incomplete. Please check back later for the latest details.

The OSIRIS API provides programmatic access to research information stored in OSIRIS. It can be used to retrieve activities, projects, persons, organizational structures, events, and other data managed by the system.

Unless stated otherwise, all API endpoints:

- return JSON responses
- use HTTP GET requests
- require authentication via an API key
- follow the standard OSIRIS response format described below

The API is designed to support reporting, system integration, data synchronization, and external presentation of research information.

---

# Authentication

API access is controlled through a configurable API key.

The API key can be provided either as a query parameter:

```http
GET /api/activities?apikey=YOUR_API_KEY
```

or as an HTTP header:

```http
X-API-Key: YOUR_API_KEY
```

Depending on the OSIRIS configuration, API authentication may be disabled. In this case all API requests are accepted without an API key.

Requests originating from authenticated OSIRIS sessions may also be allowed automatically.

---

# Response Format

Successful requests return a JSON object with the following structure:

```json
{
    "status": 200,
    "count": 2,
    "data": []
}
```

## Fields

| Field | Description |
|---------|-------------|
| `status` | HTTP-like status code returned by the API |
| `count` | Total number of matching records |
| `data` | Array containing the requested data |

The structure of objects inside `data` depends on the endpoint.

---

# Pagination

Some endpoints support pagination using the `limit` and `offset` parameters.

Example:

```http
GET /api/activities?limit=50&offset=100
```

Paginated responses contain additional metadata:

```json
{
    "status": 200,
    "count": 350,
    "limit": 50,
    "offset": 100,
    "data": []
}
```

## Parameters

| Parameter | Description |
|------------|-------------|
| `limit` | Maximum number of records returned |
| `offset` | Number of matching records to skip |

The `count` field always refers to the total number of matching records before pagination is applied.

Not all endpoints support pagination. Endpoint-specific documentation will indicate whether pagination is available.

---

# Filtering

Many endpoints support filtering of results.

The preferred way to provide filters is through the `json` parameter containing a JSON-encoded MongoDB query.

Example:

```http
GET /api/activities?json={"year":2025}
```

URL-encoded:

```http
GET /api/activities?json=%7B%22year%22%3A2025%7D
```

Example filter:

```json
{
    "year": 2025,
    "type": "publication"
}
```

Supported filter fields depend on the endpoint and underlying data model.

Endpoint-specific documentation may define additional filtering options or restrictions.

---

# Error Handling

Errors are returned as JSON objects.

Example:

```json
{
    "status": 403,
    "count": 0,
    "error": "PermissionDenied",
    "msg": "You need a valid API key for this request."
}
```

## Common Error Responses

| Status | Error | Description |
|---------|---------|-------------|
| `400` | `WrongCall` | Invalid request or parameters |
| `403` | `PermissionDenied` | Missing or invalid API key |
| `404` | `DataNotFound` | Requested resource could not be found |
| `500` | Internal server error | Unexpected server-side error |

Applications consuming the API should always check the `status` field before processing the returned data.