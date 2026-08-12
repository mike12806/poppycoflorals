# GitHub Copilot Instructions

## Project Overview
This repository is the website for **Poppy & Co. Florals**, a small one-person floral
studio that designs arrangements for weddings and events. It is a hand-written,
single-page static site deployed to **Cloudflare Pages** at `poppycoflorals.com`.
All changes are version-controlled in Git and deployed automatically via GitHub Actions.

## Repository Structure

```
.github/
  workflows/
    deploy.yml              # Deploy to Cloudflare Pages on push to main
    renovate-validate.yml   # Validate Renovate config on pull requests
  copilot-instructions.md   # This file
site/
  index.html                # The whole site — one page
  style.css                 # All styles
  favicon.svg               # Poppy mark
  robots.txt
  sitemap.xml
  img/hero.webp             # Watercolour hero illustration
renovate.json               # Renovate dependency update configuration
README.md
```

## Technology Stack

- **Frontend**: Static HTML5 and CSS3 — no framework, no build step, no external requests
- **Hosting**: Cloudflare Pages, served at `poppycoflorals.com` and `www.poppycoflorals.com`
- **Deployment**: GitHub Actions using `cloudflare/wrangler-action`
- **Dependency updates**: Renovate (tracking Wrangler and GitHub Actions versions)

## Content Guidelines

**Keep it small.** This is a side business run by one person. Resist adding pages,
navigation, pricing tables, galleries, booking forms, or invented detail (hours,
delivery areas, product menus). The site exists to explain what she does and let
someone email her.

- Contact is a plain `mailto:` link — there is no form and no backend
- Don't invent facts about the business; ask before adding specifics

## HTML & CSS Guidelines

- Use **semantic HTML5 elements** (`<header>`, `<section>`, `<footer>`, etc.)
- Keep the site **fully self-contained** — no CDN scripts, external fonts, or trackers
- Keep all styles in `style.css`; avoid inline styles
- Use the CSS custom properties defined in `:root` for colours rather than literals
- Use **2 spaces** for indentation in HTML and CSS files
- Every image needs meaningful `alt` text and explicit `width`/`height`

## Design Notes

The palette is drawn from the hero watercolour: poppy red (`--poppy`), meadow green
(`--meadow`), and warm sand neutrals. Headings use a serif stack; body copy uses the
system sans stack.

## Deployment

### How It Works
Pushing to the `main` branch triggers the `deploy.yml` workflow, which:
1. Creates the Cloudflare Pages project if it does not already exist
2. Deploys the `site/` directory using Wrangler
3. Registers the custom domains `poppycoflorals.com` and `www.poppycoflorals.com`
4. Configures proxied CNAME records in the `poppycoflorals.com` Cloudflare zone
   (the apex record relies on Cloudflare's CNAME flattening)
5. Triggers domain verification and purges the Cloudflare cache

### Required Secrets
- `CF_API_TOKEN` — Cloudflare API token with Pages and DNS permissions
- `CF_ACCOUNT_ID` — Cloudflare account ID

### Wrangler Version
The Wrangler CLI version is pinned in `deploy.yml` via the `WRANGLER_VERSION` env
variable and tracked by Renovate.

## Renovate Configuration

- **Automerge is disabled** — all updates require manual review before merging
- **GitHub Actions** workflow dependencies are tracked and updated automatically
- **Wrangler** npm package version is tracked via a custom regex manager

## CI/CD Workflow

1. **Make changes** in a feature branch
2. **Open a pull request** — Renovate config is validated automatically
3. **Review** changes for correctness
4. **Merge** to `main` — triggers automatic deployment to Cloudflare Pages

## Best Practices

- **Keep secrets out of code** — use GitHub Actions secrets for API tokens
- **Pin dependency versions** — use specific version tags for actions and Wrangler
- **Use meaningful commit messages** — describe what changed and why
- **Preview locally** (`python3 -m http.server 8000 --directory site`) before opening a PR
