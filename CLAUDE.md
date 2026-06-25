# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
npm run dev       # dev server at localhost:4321
npm run build     # output to dist/
npm run preview   # preview the build locally
npm run astro     # direct Astro CLI access
```

No test suite or linter is configured.

## Architecture

**Astro 6 + Tailwind CSS 4** static site for NPO法人国際AKIBA総合支援協会 (AKIBA JAPAN), deployed to `akiba-japan.jp` on Netlify.

### Page pattern

Every page uses `BaseLayout.astro` as its single wrapper:

```astro
---
import BaseLayout from "../layouts/BaseLayout.astro";
---
<BaseLayout title="ページタイトル | AKIBA JAPAN" description="...">
  <!-- page content -->
</BaseLayout>
```

`BaseLayout` handles all `<head>` meta (SEO, Open Graph, Twitter Card, canonical URL, JSON-LD schema), imports global styles, and wraps content with `<Header>` and `<Footer>`. The `<slot />` is where page content lands.

### Routing

File-based: `src/pages/*.astro` → URL path. Current pages: `/` `/about` `/shops` `/coin` `/events` `/rules` `/join` `/contact`.

### Data layer

Shop data lives in `src/data/shops.js` as plain JS arrays (`shops`, `categories`). Pages import and map over them at build time — there is no CMS or API. To add a shop, append an object to the `shops` array matching the existing shape (`id`, `name`, `category`, `categoryLabel`, `description`, `coinDiscount`, `featured`).

### Tailwind v4 setup

Tailwind is loaded via the `@tailwindcss/vite` plugin (no `tailwind.config.js`). Design tokens are defined in `src/styles/global.css` using the `@theme` directive:

| Token | Value |
|---|---|
| `--color-primary` | `#e8546a` (used as `text-primary`, `bg-primary`) |
| `--color-primary-dark` | `#c73d55` |
| `--color-black` | `#0f0f0f` |
| `--font-mono` | Space Mono |

Body font is Noto Sans JP. Both fonts are loaded from Google Fonts with a print-then-all lazy load pattern in `BaseLayout`.

### Navigation

Nav links in `Header.astro` are driven by a `navLinks` array at the top of the file. Each `href` must be unique — duplicate hrefs cause both links to appear active simultaneously (active state is detected by matching `Astro.url.pathname`).

Footer has three link columns (`nav1`/`nav2`/`nav3`) plus an `sns` array for social links. The copyright year is computed automatically via `new Date().getFullYear()`.

### Client-side JS

Astro ships zero JS by default. The one exception is `shops.astro`, which includes an inline `<script>` for client-side category filtering (show/hide `.shop-wrapper` elements by `data-category`).
