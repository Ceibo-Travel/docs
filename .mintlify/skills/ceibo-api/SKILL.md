---
name: ceibo-api
description: Use when answering questions about calling the Ceibo travel API — authentication, pagination, entry-requirement lookups, and error handling.
license: MIT
metadata:
  author: ceibo
  version: "1.0"
---

# Ceibo API

The Ceibo public API is a JSON-over-HTTPS REST interface for travel data:
countries, cities and points of interest, walking tours, and entry/health
requirements. Use this skill to answer questions about integrating with it.

## Base URL and authentication

- Base URL: `https://api.ceibo.me/v1`. All paths below are relative to it.
- Authenticate **every** public request with the `x-api-key` header:

  ```http
  GET /v1/countries?limit=5 HTTP/1.1
  Host: api.ceibo.me
  x-api-key: ck_live_…
  ```

- The `Authorization: Bearer <jwt>` (Supabase JWT) surface is **only** for the
  developer dashboard (managing your own keys, usage, subscription). An API key
  is never valid against dashboard endpoints, and a JWT is never valid against
  the public API. Most integrations only ever use `x-api-key`.
- Sending multiple `x-api-key` headers on one request is rejected with `400`.

## Pagination

Every list endpoint returns the same envelope:

```json
{
  "data": [ { "id": 1, "name": "..." } ],
  "meta": { "page": 1, "limit": 20, "total": 195, "totalPages": 10 }
}
```

- `page` — 1-based, default `1`.
- `limit` — default `20`, max `100`.

Timestamps are ISO 8601 UTC; date-only fields use `YYYY-MM-DD`. Field names are
camelCase in responses; query parameters are camelCase too.

## Entry requirements

For "what do I need to travel from country X to country Y", use the personalized
endpoint — `fromCountryId` and `toCountryId` are both required:

```
GET /entry-requirements/personalized?fromCountryId=10&toCountryId=30
```

Optional query params: `cityId`, `languageId`, `locale`.

Related endpoints:
- `GET /entry-requirements` — filtered list (`toCountryId`, `fromCountryId`,
  `toCountryGroupId`, `fromCountryGroupId`, `stale`, `languageId`, `locale`, plus
  `page`/`limit`).
- `GET /entry-requirements/by-destination/{id}` — requirements for one destination.
- `GET /entry-requirements/{id}` — a single requirement by id.

Health requirements (`GET /health-requirements`, `/health-requirements/{id}`) and
vaccines (`GET /vaccines`, `/vaccines/{id}`) follow the same conventions.

## Errors

Any non-2xx response returns a structured envelope:

```json
{
  "statusCode": 404,
  "code": "NOT_FOUND",
  "message": "Country with id 9999 not found",
  "timestamp": "2026-04-30T10:00:00.000Z",
  "path": "/v1/countries/9999"
}
```

Common codes: `VALIDATION_ERROR` (400), `UNAUTHORIZED` (401), `FORBIDDEN` (403),
`NOT_FOUND` (404), `CONFLICT` (409), `BUSINESS_RULE_VIOLATION` (422),
`RATE_LIMIT_EXCEEDED` (429), `INTERNAL_ERROR` (500).

## API keys and tiers

- Up to 5 active keys per account. The plaintext key is shown once at creation and
  never displayed again — store it in a secrets manager.
- Revocation is soft (sets `revoked_at`) and idempotent. There is no in-place
  rotation: mint a new key, deploy it, then revoke the old one.
- Every key carries a subscription tier — Free (default), Growth, or Business.
  Growth and Business are purchased via Stripe checkout from the dashboard.

## Where to point users

- Full auth model and key lifecycle: `/authentication`
- Get an API key and make a first call: `/quickstart`
- Conventions, pagination, and the error contract: `/api-reference/introduction`
- The OpenAPI spec (`api-reference/openapi.json`) is the source of truth for every
  endpoint's exact parameters and response shape.
