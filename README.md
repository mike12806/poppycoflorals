# Poppy &amp; Co. Florals

Single-page static site for Poppy &amp; Co. Florals — wedding and event flower
arrangements — deployed to Cloudflare Pages at
[poppycoflorals.com](https://poppycoflorals.com).

## Structure

```
site/               # everything served by Cloudflare Pages
  index.html
  style.css
  favicon.svg
  robots.txt
  sitemap.xml
  img/hero.webp
.github/workflows/
  deploy.yml              # deploy on push to main
  renovate-validate.yml   # validate Renovate config on PRs
renovate.json
```

## Local preview

No build step — open the file directly, or serve it:

```bash
python3 -m http.server 8000 --directory site
```

## Deployment

Pushing to `main` runs `deploy.yml`, which:

1. Creates the Pages project (`poppycoflorals`) if it doesn't exist
2. Deploys `site/` via Wrangler
3. Registers `poppycoflorals.com` and `www.poppycoflorals.com` as custom domains
4. Points both at the project's `pages.dev` hostname via proxied CNAMEs
5. Triggers domain verification and purges the Cloudflare cache

### Required secrets

- `CF_API_TOKEN` — Cloudflare API token with Pages and DNS edit permissions
- `CF_ACCOUNT_ID` — Cloudflare account ID
