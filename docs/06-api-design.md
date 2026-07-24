# API design

## Conventions

- API prefix: `/api/v1`.
- FastAPI publishes the canonical OpenAPI document at `/openapi.json` and interactive documentation at `/docs`.
- Authenticated endpoints use a short-lived bearer access token. Refresh tokens are delivered only through secure, HTTP-only cookies.
- Collection responses use cursor pagination: `items`, `next_cursor`, and `has_more`.
- All timestamps are ISO 8601 UTC strings; UUIDs are serialized as strings.
- Every user-facing AI response identifies the provider, model, and citations where applicable.

## Standard errors

```json
{
  "error": {
    "code": "ARTICLE_NOT_FOUND",
    "message": "The requested article does not exist.",
    "request_id": "uuid"
  }
}
```

Common status codes: `400` validation failure, `401` unauthenticated, `403` unauthorized, `404` not found, `409` conflict, `422` schema validation failure, and `429` rate limited.

## Authentication

| Method | Route | Purpose | Authentication |
| --- | --- | --- | --- |
| `POST` | `/auth/register` | Create an account | Public |
| `POST` | `/auth/login` | Start a session and issue access token | Public |
| `POST` | `/auth/refresh` | Exchange valid refresh cookie for an access token | Refresh cookie |
| `POST` | `/auth/logout` | Revoke the current refresh session | User |
| `GET` | `/auth/me` | Return the current user | User |
| `DELETE` | `/auth/me` | Request account deletion | User |

`POST /auth/register` accepts `email`, `password`, and optional `display_name`. Password policy and email verification requirements will be finalized in the authentication milestone.

## Profile and preferences

| Method | Route | Purpose |
| --- | --- | --- |
| `GET` | `/profile` | Get profile, explanation level, and selected topics |
| `PATCH` | `/profile` | Update display name, timezone, or explanation level |
| `PUT` | `/profile/topics` | Replace explicit topic interests |
| `GET` | `/settings/llm` | Get selected provider and model settings |
| `PATCH` | `/settings/llm` | Update Gemini/Ollama choice, model, temperature, and token limit |
| `GET` | `/settings/llm/providers` | List server-supported providers and available models |

Example topic update:

```json
{
  "topics": [
    {"slug": "llms", "weight": 1.0},
    {"slug": "ai-agents", "weight": 0.9},
    {"slug": "nvidia", "weight": 0.7}
  ]
}
```

## Feed, articles, and bookmarks

| Method | Route | Purpose |
| --- | --- | --- |
| `GET` | `/feed` | Paginated personalized feed; records an impression event |
| `GET` | `/articles` | Browse articles with source, topic, and date filters |
| `GET` | `/articles/{article_id}` | Read normalized article metadata and source link |
| `GET` | `/articles/{article_id}/analysis` | Get summary, takeaways, and labelled perspectives |
| `POST` | `/articles/{article_id}/events` | Record `open`, `complete`, `dismiss`, or related reading event |
| `GET` | `/bookmarks` | List the current user’s saved articles |
| `POST` | `/bookmarks` | Save an article |
| `DELETE` | `/bookmarks/{article_id}` | Remove a saved article |

`GET /feed` supports `cursor`, `limit`, and optional `topic`. Each card includes `recommendation_reason` only for the current authenticated user.

## Search and discovery

| Method | Route | Purpose |
| --- | --- | --- |
| `GET` | `/search` | Keyword and semantic article search |
| `GET` | `/topics` | List searchable topics |
| `GET` | `/topics/{slug}` | Topic metadata and recent related articles |

`GET /search` accepts `q`, `mode` (`keyword` or `semantic`), `topic`, `source`, `published_after`, cursor, and limit. The response returns article cards plus a `search_mode` value so the UI can explain how results were found.

## Chat and citations

| Method | Route | Purpose |
| --- | --- | --- |
| `POST` | `/chat/sessions` | Create article-, topic-, or global-scoped chat session |
| `GET` | `/chat/sessions` | List the current user’s sessions |
| `GET` | `/chat/sessions/{session_id}` | Get session metadata and messages |
| `POST` | `/chat/sessions/{session_id}/messages` | Send a question and receive a grounded answer |
| `DELETE` | `/chat/sessions/{session_id}` | Delete a user-owned conversation |

Article session creation request:

```json
{
  "scope_type": "article",
  "article_id": "uuid"
}
```

Chat message response shape:

```json
{
  "message": {
    "id": "uuid",
    "role": "assistant",
    "content": "...",
    "provider": "gemini",
    "model": "configured-model",
    "citations": [
      {"article_id": "uuid", "title": "Source article", "url": "https://example.com/article"}
    ]
  }
}
```

For the initial implementation, the endpoint returns a complete response. Streaming Server-Sent Events is a planned enhancement after the chat contract is stable.

## Recommendation transparency

| Method | Route | Purpose |
| --- | --- | --- |
| `GET` | `/recommendations/explain/{article_id}` | Explain why an article appeared in the current user’s feed |
| `POST` | `/recommendations/feedback` | Record positive or negative recommendation feedback |

## Internal operational endpoints

These endpoints are not exposed to ordinary users. They are protected by service-to-service authentication and should be inaccessible from the public frontend.

| Method | Route | Purpose |
| --- | --- | --- |
| `POST` | `/internal/ingestion/run` | Trigger approved-source ingestion |
| `POST` | `/internal/articles/{article_id}/process` | Queue article normalization, analysis, and embeddings |
| `GET` | `/internal/jobs/{job_id}` | Inspect worker status |
| `GET` | `/health` | Liveness check |
| `GET` | `/ready` | Readiness check for database, vector store, and configured provider |

## Authorization rules

- Users may only read and modify their own profile, settings, bookmarks, chat sessions, reading events, and recommendation feedback.
- Editorial data—sources, articles, topics, and published analyses—is shared read-only.
- A client must never select a provider configuration that exposes server credentials; settings only reference providers and safe model parameters.
- Internal routes require a distinct service identity and are omitted from public API clients.

## Implementation order

1. Health endpoint, auth, profile, topics, and user preferences.
2. Sources, articles, article analyses, feed, bookmarks, and reading events.
3. Search and retrieval foundations.
4. Chat sessions, messages, citations, and recommendation feedback.
5. Internal worker routes and operational controls.
