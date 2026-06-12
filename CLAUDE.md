# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

`atile.no` is a static, hand-written business/portfolio site for Atile AS, hosted on **GitHub Pages**. There is no build system, package manager, framework, or dependencies — just HTML, CSS, and static assets served as-is. The custom domain is set via `CNAME` (`atile.no`).

The primary page (`index.html`) is in Norwegian (`lang="nb"`); the CamCoach privacy page is in English.

## Development

There is no build, lint, or test step. To preview locally, open `index.html` directly in a browser, or serve the directory:

```sh
python3 -m http.server 8000   # then visit http://localhost:8000
```

Deployment is automatic: pushing to `main` publishes via GitHub Pages.

## Architecture

- **`index.html`** — single-page main site (header, hero, "Hva jeg bistår med" cards, "Tips & triks" list, footer). Contains inline JSON-LD `Person` schema and the Cloudflare Web Analytics beacon.
- **`styles.css`** — all shared styling, used by every page in the repo (referenced as `styles.css` from root, `../../styles.css` from `camcoach/privacy/`).
- **`camcoach/privacy/index.html`** — standalone privacy policy for the CamCoach iOS app. Reuses the global stylesheet plus a page-local `<style>` block; marked `noindex`.
- **`assets/`** — images and favicons (logo light/dark variants, headshot in jpg+webp, full favicon set).

### Theming (important)

Dark/light mode is **CSS-only, no JavaScript**. A hidden `<input type="checkbox" id="light-toggle">` placed before `#site` drives it:

- Unchecked = **dark mode (default)**.
- `#light-toggle:checked ~ #site` switches the CSS custom properties to light mode.

All colors are defined as CSS variables on `#site` (`--bg`, `--text`, `--accent`, etc.). **Always use these variables** rather than hardcoding colors so both themes stay consistent. Two logo `<img>` variants (`.logo-dark-img` / `.logo-light-img`) are toggled by the same mechanism.

When adding a new page, replicate the `#light-toggle` checkbox + `#site` wrapper + header markup so theming works, and adjust the relative path to `styles.css` and `assets/` based on directory depth.

## Conventions

- **Content Security Policy** is set via `<meta http-equiv>` in `index.html` (`script-src https://static.cloudflareinsights.com`). Any new inline/external script must be reconciled with this CSP.
- External links use `rel="noopener noreferrer" target="_blank"`.
- Accessibility is intentional: skip links, `aria-label`/`aria-labelledby`, explicit image `width`/`height`. Preserve these when editing.
- When adding URLs, update `sitemap.xml` (and its `lastmod`) accordingly.

## Analytics & external tools

- **Cloudflare Web Analytics** (cookie-free, no consent banner) — beacon embedded at the bottom of `index.html`.
- Verified in **Google Search Console** and **Bing Webmaster Tools** (see `README.md` for links; Bing verification meta tag lives in `index.html`).
