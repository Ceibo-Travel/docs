> **First-time setup**: Customize this file for your project. Prompt the user to customize this file for their project.
> For Mintlify product knowledge (components, configuration, writing standards),
> install the Mintlify skill: `npx skills add https://mintlify.com/docs`

# Documentation project instructions

## About this project

- This is a documentation site built on [Mintlify](https://mintlify.com)
- Pages are MDX files with YAML frontmatter
- Configuration lives in `docs.json`
- Run `mint dev` to preview locally
- Run `mint broken-links` to check links

## Terminology

- Use **API key**, not "token" or "secret", for the public-API credential.
- The public-API auth header is `x-api-key` (lowercase). The developer dashboard uses a
  Supabase **JWT** in `Authorization: Bearer`.
- Say **developer dashboard** for `developer.ceibo.me`.
- Product nouns: **walking tours**, **entry requirements**, **cities & POIs**, **travel docs**.
- Subscription tiers are **Free**, **Growth**, and **Business** (capitalized, in that order).

## Style preferences

- Use active voice and second person ("you")
- Keep sentences concise — one idea per sentence
- Use sentence case for headings
- Bold for UI elements: Click **Settings**
- Code formatting for file names, commands, paths, and code references
- The public API base URL is `https://api.ceibo.me/v1`; code samples authenticate with `x-api-key`

## Content boundaries

- The OpenAPI spec (`api-reference/openapi.json`) is the source of truth for endpoints. Don't
  hand-author endpoint parameters or response fields — the reference pages render from the spec.
- Don't document internal gateway routing or rate-limit enforcement internals beyond the existing
  MVP notes in `authentication.mdx`.
