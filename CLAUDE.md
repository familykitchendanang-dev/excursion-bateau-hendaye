# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A two-page static HTML website for a boat tour business (Excursion Bateau Hendaye). No build system, no package manager, no framework — edit the HTML files directly and deploy.

- `index.html` — French landing page (canonical)
- `en.html` — English landing page
- `_headers` — Netlify cache and security headers
- `sitemap.xml` — XML sitemap (update `<lastmod>` when pages change)

The site is deployed on GitHub Pages with the custom domain `excursionbateauhendaye.com`.

## Architecture

Both pages are fully self-contained: CSS is inlined in `<style>`, JavaScript is inlined in `<script>` tags at the bottom. There are no external JS/CSS files. The two language versions share identical CSS — any style change must be applied to **both files**.

### Reservation flow

The booking form supports two payment paths:

1. **Pay on site** → submits to Formspree via a dynamically created `<form>`:
   - FR: `https://formspree.io/f/xaqpdqaj`
   - EN: `https://formspree.io/f/xzdoyyww`

2. **PayPal** → redirects to `https://www.paypal.com/ncp/payment/XLPEL2RP5BUHA` with `?quantity=N&custom=<encoded booking details>`

### Blocked dates

To block out fully-booked dates, update the `DATES_BLOQUEES` array near the bottom of **both** `index.html` and `en.html`:

```js
const DATES_BLOQUEES = [
  '2026-06-15',
  '2026-06-20',
];
```

The date input's `data-lang` attribute (`"fr"` or `"en"`) controls which alert message is shown when a blocked date is selected.

## Content that appears in multiple places

When updating key business details, search for all occurrences across both files:

- **Price** (145 €): appears in the hero badge, tarif section heading, `updateTotal()` JS function, PayPal link builder, and form label
- **Reviews/ratings** (43 avis, 5/5): in the hero star link, `avis-score` block, and Schema.org JSON-LD `aggregateRating`
- **Schema.org JSON-LD**: in both `<head>` sections — keep `reviewCount` and reviews in sync when adding new testimonials

## CSS color palette

```css
--ocean: #0a4d6b
--ocean-mid: #1a7a9e
--ocean-light: #4fb3d4
--sand: #f5e9d3
--gold: #c9933a
--gold-light: #e8b85a
--white: #fdfaf6
--text-dark: #1a2634
--text-mid: #3d5368
```

## Deployment

Push to `main` — GitHub Pages deploys automatically. The `_headers` file sets aggressive caching on images (1 year immutable) and moderate caching on HTML (1 hour), so after updating HTML content users will see changes within an hour without any cache-busting needed.
