# vigilsquad-docs

Public documentation site for **vigilsquad**, built with [Docusaurus](https://docusaurus.io/).
Published at **https://docs.vigilsquad.com**.

- **Owner:** Hugo Marques
- **Product repo:** [nutek-devs/vigilsquad](https://github.com/nutek-devs/vigilsquad)
- **Live:** https://docs.vigilsquad.com

## What's here

User-facing product docs (guides + API reference):

```
docs/
  getting-started.mdx · authentication.mdx · webhooks.mdx
  agent-builder.mdx · multi-agent-routing.mdx · session-lifecycle.mdx · widget-embed.mdx
  api/            # API reference — AUTO-GENERATED from the OpenAPI spec
sidebars.ts       # sidebar structure
i18n/             # translations
```

> ⚠️ This is a **public** site. Internal/operational content (runbooks, infra, incidents,
> secrets) does **not** belong here — keep it in the product repo or the internal runbooks hub.

## Tech Stack

Docusaurus (preset-classic) · `docusaurus-plugin-openapi-docs` (API reference auto-gen).

## Quick Start

```bash
npm install
npm run start        # local dev server with HMR
```

## Commands

| Command | What it does |
|---------|--------------|
| `npm run start` | local dev server |
| `npm run build` | static build → `build/` |
| `npm run serve` | serve the production build locally |
| `npm run typecheck` | type-check the config and sidebars (`tsc`, also run in CI) |
| `npm run gen-api-docs all` | regenerate API reference from the OpenAPI spec |
| `npm run clean-api-docs all` | remove generated API docs |
| `npm run write-translations` | extract i18n strings |

## Deploy

- **Platform:** Cloudflare Pages — [.github/workflows/deploy.yml](.github/workflows/deploy.yml) (`Build & Deploy to CF Pages`).
- Published at `https://docs.vigilsquad.com` (`url`/`baseUrl` in [docusaurus.config.ts](docusaurus.config.ts)).

## Updating the API reference

The `docs/api/` pages are generated from vigilsquad's OpenAPI spec — do **not** edit them by
hand. The source of truth is `docs/openapi.json` on `nutek-devs/vigilsquad@main` (the spec the
production API serves); `static/openapi.json` here is only a local copy.

- **CI:** every deploy checks that file out, copies it to `static/openapi.json`, and regenerates
  `docs/api/` before building — the site never publishes a stale copy.
- **Trigger:** vigilsquad's `openapi-sync.yml` dispatches this workflow when `docs/openapi.json`
  changes on its `main`, and fails its own CI if the spec was not regenerated after a route change.
- **Locally:** copy the spec over, then `npx docusaurus clean-api-docs all && npx docusaurus gen-api-docs all`.
