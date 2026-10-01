# SYNTHEX website

Static site for the SYNTHEX web studio. Plain HTML, CSS and JavaScript — no framework, no runtime dependencies.

## Edit → build → deploy

```
npm run build     # generates the pages from src/, plus css/site.css, sitemap.xml, robots.txt
npm run dev       # local preview at http://localhost:5174
npm run validate  # HTML validation of every generated page
```

Netlify runs `node build.mjs` on every deploy, so committing `src/` changes is enough.

## Where things live

| Want to change… | Edit |
|---|---|
| Phone, WhatsApp, email, domain, Google Analytics ID | `src/site.mjs` |
| Page content (Georgian) | `src/pages/ka/*.html` |
| Page content (English) | `src/pages/en/*.html` |
| Header / navigation | `src/partials/ka/header.html`, `src/partials/en/header.html` |
| Footer | `src/partials/ka/footer.html`, `src/partials/en/footer.html` |
| `<head>`: meta, fonts, icons | `src/partials/head.html` |
| Colors, spacing, type scale | `css/tokens.css` |
| Buttons, nav, cards, forms | `css/components.css` |
| Section layouts | `css/sections.css` |
| Behaviour | `js/*.js` (one file per concern) |

Never edit the generated files by hand: `index.html`, `work.html`, `contact.html`, `privacy.html`, `404.html`, `en/*.html`, `css/site.css`, `sitemap.xml`, `robots.txt`. The next build overwrites them.

## Page template format

Each file in `src/pages/<lang>/` starts with a comment block, then the `<main>`:

```html
<!--
title: Page title
description: Meta description
scripts: hero-tabs, pointer     (optional; nav and reveal are always included)
active: work                    (marks the nav link with aria-current)
noindex: true                   (optional; keeps the page out of sitemap.xml)
-->
<main id="main">…</main>
```

Available placeholders: `{{root}}` (`''` on Georgian pages, `'../'` on English), `{{phone}}`, `{{phoneHref}}`, `{{phoneNbsp}}`, `{{whatsapp}}`, `{{email}}`, `{{hours}}`, `{{domain}}`.

## Security headers

`netlify.toml` sets a strict Content-Security-Policy. The only inline script is the one-liner in `src/partials/head.html`; its SHA-256 hash is in the CSP. If you change that line, recompute the hash:

```
node -e "console.log('sha256-'+require('crypto').createHash('sha256').update(\"document.documentElement.classList.replace('no-js','js')\").digest('base64'))"
```

## Placeholders still to replace

Search for `PLACEHOLDER` in `src/pages/`: prices, delivery times, the 30-day support term, and the three concept portfolio cards.
