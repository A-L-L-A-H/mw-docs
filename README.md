# mw-docs

## What it does
Central documentation site for the MoonWitness ecosystem, published to Cloudflare Pages.

## Quick start
```bash
mkdir -p public
cp index.md public/index.md
cp -R mw-core mw-dock mw-design mw-isp mw-stack mw-xv addons public/
```

## Verify
```bash
test -f index.md && test -f wrangler.toml
```

## Notes
- Deployed automatically from `main` via `.github/workflows/deploy.yml`.
- All ecosystem documentation lives in this repository per workspace Rule 6.
