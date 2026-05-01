# Ceibo developer docs

The source for [docs.ceibo.me](https://docs.ceibo.me) — public-facing
documentation for the Ceibo travel platform API.

The site covers:

- Quickstart and authentication
- API reference (driven by `api-reference/openapi.json`)
- Subscription tiers and rate-limit model

The backing API runs at `https://api.ceibo.me/v1`. Engineering
documentation lives in
[`ceibo-backend/docs/`](https://github.com/ceibo-travel/ceibo-backend/tree/main/docs)
and is intentionally not published here.

## Local preview

Requires Node 19+.

```bash
npm i -g mint
mint dev
```

The site runs on `http://localhost:3000`. Hot reload is automatic.
Use `mint dev --port <n>` to override.

Validate links before pushing:

```bash
mint broken-links
```

## Deployment

Pushing to `main` deploys automatically via the Mintlify GitHub app.
DNS for `docs.ceibo.me` is configured in Route 53; the certificate
is issued by Mintlify via ACME.

## Editing the API reference

Endpoint pages are stubs — the body is rendered from
`api-reference/openapi.json`. To add a new endpoint:

1. Add the path and schemas to `openapi.json`.
2. Create a new MDX file under `api-reference/endpoint/` whose
   frontmatter references the operation:

   ```mdx
   ---
   title: "List walking tours"
   openapi: "GET /walking-tours"
   ---
   ```

3. Add the file path to the navigation in `docs.json`.

## Repository layout

```
.
├── docs.json              # Site config and navigation
├── index.mdx              # Landing page
├── quickstart.mdx         # Get an API key + first request
├── authentication.mdx     # API keys and JWT
├── api-reference/
│   ├── introduction.mdx   # Conventions, pagination, errors
│   ├── openapi.json       # OpenAPI 3.1 spec — source of truth
│   └── endpoint/          # Per-operation MDX stubs
├── logo/                  # Logo SVGs
├── images/                # Screenshots and hero art
└── snippets/              # Reusable MDX fragments
```

## Need help?

- Email: [support@ceibo.me](mailto:support@ceibo.me)
- Mintlify docs: https://mintlify.com/docs
