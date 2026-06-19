# WP Engineer Portfolio Variant — Design

**Date:** 2026-06-19
**Goal:** A portfolio variant targeting the Multidots WordPress Engineer role, reusing the live site's exact theme, layout, and snake game — only food art, the unlock reward, and copy change.

## Target role

Multidots WordPress Engineer (https://careers.multidots.com/careers/wordpress-engineer-2/) — remote, 5+ yrs WP. Wants: WP Core architecture & request lifecycle; Actions, Filters, WP_Query, Template Hierarchy, Rewrite Rules, Cron Jobs, REST API; custom themes & plugins; PHP 7/8 OOP, design patterns, debugging; MySQL queries & optimization; HTML5/CSS3/SCSS, JS ES6+, jQuery; Figma→pixel-perfect UI; Git/PR workflow; basic perf (caching, DB, assets). Nice-to-have: Gutenberg/ACF blocks, WooCommerce, Multisite, managed WP hosts, headless, Core Web Vitals.

## Scope

- **New file:** `wp-engineer.html` — copy of live `index.html`. `index.html` stays untouched.
- Match existing v1–v13 variant workflow (standalone files at repo root).

## Changes (only these three)

### 1. Snake food → emoji
- `FOOD_TYPES` (index.html ~line 1569) gains an `emoji` field:
  - 🍔 burger — points 1, weight 70
  - 🍵 tea — points 3, weight 22
  - ⭐ star — points 5, weight 8
- `drawAll()` food block (~line 1663) draws emoji via `ctx.fillText(food.type.emoji, fx, fy)` instead of `arc`/star path. Keep glow, the floating `+points` label, and the white highlight dot (or drop highlight — emoji already legible).

### 2. Unlock reward → WP audit
- Snake unlocks at 30 eaten. Reframe "FREE-AUDIT" → "Free WordPress site audit".
- Update snake-pill tooltip (index.html ~line 551): "...Eat 30 to unlock Free WP audit."
- Update reward reveal text wherever FREE-AUDIT renders.

### 3. Copy retarget (reframe existing content, not fabricate)

**Reality of the live file:** `index.html` is an e-commerce-PDP-themed landing page selling *migration services* (DanDomain→Shopify/WP). It uses a product-page metaphor throughout: hero "speciality" cards, "Add to cart · Book Shakir", "Size chart — proficiency", migration lanes, "Product catalog" = projects, "Checkout" = contact. The retarget keeps the **PDP visual theme and all layout/CSS**, but reframes the *product being sold* from "migration services" to "**a WordPress Engineer (me)**".

Candidate's real WP experience already present in the file (lead with these): **9+ years**; The Hill Times news portal (custom theme + plugins, 100k+ subscribers, paid/free subs); Crypto Payment Gateway for WooCommerce (sole dev, singleton OOP, 800+ installs, REST, 14 releases/yr, 0 open issues); WK Plugin Suite (cache manager, crawler, etc.); Triple-A site (Figma→pixel-perfect). **Do NOT claim** WP Engine / Kinsta / WP VIP / Pantheon (no experience) — omit or frame as "eager to learn."

| Block (live line refs) | WP Engineer version |
|---|---|
| `<title>` (L6) | "Shakir Uz Zaman — WordPress Engineer · PHP 7/8 · Gutenberg · WooCommerce" |
| Hero eyebrow (L605) + vendor tag (L645) | "WORDPRESS ENGINEER · 9+ YEARS · REMOTE WORLDWIDE" |
| Hero speciality cards (L607-635) | Recast 3 cards as WP strengths: **01 Custom Themes & Plugins** (actions, filters, WP_Query, template hierarchy); **02 Gutenberg & ACF Blocks** (block dev, editorial UX); **03 WooCommerce & Performance** (caching, DB queries, assets, Core Web Vitals). |
| `role` line (L647) | "**WordPress Engineer** · PHP 7/8 · Gutenberg · WooCommerce · REST API" |
| Hero foot stats (L636-641) | Keep 9+ years, 800+ installs, 100k+ users, 0 failed — all true & WP-relevant. |
| `Migration type` pills (L662-672) | Relabel to WP focus areas: Theme dev · Plugin dev · Gutenberg/ACF blocks · WooCommerce · Multisite · REST/headless. |
| `Stack` pills (L674-685) | Lead WordPress + WooCommerce + PHP 8; keep Shopify/Laravel/React as secondary. |
| Size-chart proficiency table (L716-728) | Reorder WP-first: WordPress, PHP/OOP, Gutenberg/ACF, WooCommerce, WP_Query/REST, MySQL opt, JS ES6+, SCSS, jQuery; Shopify/Laravel lower. |
| Migration section (L744+) | Reframe heading/lead away from "DanDomain migration" → WP engineering services/strengths (or de-emphasize; keep layout). |
| `Product catalog` projects (L1019-1190) | Reorder WP/plugin cards first; relabel default tab/copy so WordPress leads. Keep Shopify cards as secondary. |
| Skills section (L1264+) | PHP 7/8, OOP, design patterns · WP_Query, Template Hierarchy, Rewrite Rules, Cron, REST API · MySQL optimization · HTML5/CSS3/SCSS · JS ES6+, jQuery · Gutenberg, ACF · WooCommerce, Multisite · Git/PR workflow. |
| Checkout/contact (L1360+), `dcode` placeholder (L1432) | Reframe "migration" wording → "WordPress engagement". Discount placeholder → "...unlock Free WP audit". |
| Resume link | Local `assets/resume-wp.pdf` (empty placeholder PDF committed to repo). If a resume link exists, point it local; if none exists, add one near hero/contact. |

**Guiding rule:** keep every visual/CSS/layout element; change only text, pill labels, ordering, and the food/reward. When a phrase is migration-specific, rewrite to WP-engineering. Never invent clients — reuse the real ones already in the file.

## Non-goals

- No change to `index.html` or any other variant.
- No new sections, nav, or game mechanics beyond the food/reward swaps.
- No fabricated managed-hosting or VIP claims.

## Files touched

- `wp-engineer.html` (new — copied from index.html, then edited)
- `assets/resume-wp.pdf` (new — empty placeholder)
- `docs/superpowers/specs/2026-06-19-wp-engineer-portfolio-design.md` (this doc)

## Verification

- Open `wp-engineer.html` via `npx live-server`. Confirm: snake food renders as 🍔/🍵/⭐, eating works, 30-eat unlock shows "Free WordPress site audit", resume link points to local PDF, all copy reads WP-targeted, no Shopify-only framing in primary sections.
