# MoonWitness Documentation Hub

Centralized documentation for all MoonWitness repositories. This repo is the single source of truth for long-form docs; project repos keep only short READMEs.

## Repositories

- [mw-core](mw-core/) — contracts and boundary specs
- [mw-xv](mw-xv/) — board, execution engine, guides
- [mw-stack](mw-stack/) — fleet orchestration and VPS blueprint
- [mw-dock](mw-dock/) — BaaS addons
- [mw-design](mw-design/) — UI catalog and assets
- [mw-isp](mw-isp/) — ISP decisions

## Rules

1. No `docs/` folder is allowed inside any project repo.
2. Long-form docs live here: `docs/<repo>/index.md` (relative to workspace root).
3. This repo is deployed to Cloudflare Pages on every push to `main`.

## Deployment

Cloudflare Pages via Wrangler: `wrangler pages deploy public --project-name mw-docs`.
