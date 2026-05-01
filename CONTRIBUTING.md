# Contributing to the Ceibo docs

Thanks for the interest. This site backs [docs.ceibo.me](https://docs.ceibo.me) —
patches that fix typos, clarify endpoints, or add examples are all
welcome.

## How to contribute

### Option 1: Edit on GitHub

1. Navigate to the page you want to edit.
2. Click the pencil icon (Edit this file).
3. Make your changes and open a pull request.

### Option 2: Local development

1. Fork and clone this repository.
2. Install the Mintlify CLI: `npm i -g mint`.
3. Create a branch for your changes.
4. Run `mint dev` from the repo root and preview at `http://localhost:3000`.
5. Validate links: `mint broken-links`.
6. Commit and open a pull request.

See the project [README](./README.md) for the full repository layout
and a guide to extending the API reference.

## Writing guidelines

- **Use active voice**: "Run the command" not "The command should be run".
- **Address the reader directly**: use "you" instead of "the user".
- **Keep sentences concise**: aim for one idea per sentence.
- **Lead with the goal**: start instructions with what the reader wants to accomplish.
- **Use consistent terminology**: don't alternate between synonyms for the same concept.
- **Include examples**: show, don't just tell. Prefer real Ceibo endpoints over placeholders.

## What not to add

- Mintlify-internal documentation (component reference, styling guides, etc.).
  This site documents the Ceibo API, not how to author Mintlify docs.
- Backend implementation details. Engineering documentation lives in
  `ceibo-backend/docs/`; only API contract and consumer-facing
  guidance belongs here.
