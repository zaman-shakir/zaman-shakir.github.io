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
Candidate's real WP experience to feature: **9+ years**, custom themes/plugins, Gutenberg/ACF blocks, WooCommerce/Multisite. **Do NOT claim** WP Engine / Kinsta / WP VIP / Pantheon (no experience) — omit or frame as "eager to learn."

| Section | WP Engineer version |
|---|---|
| Intro / typing strings | "WordPress Engineer" · "Custom themes & plugins" · "Gutenberg & ACF blocks" · "PHP 7/8 · WP_Query · REST API" |
| About | 9+ yrs WordPress. Core architecture, request lifecycle, actions/filters, custom themes+plugins, Gutenberg/ACF blocks, WooCommerce/Multisite. |
| Experience | Reframe achievements toward WP: theme/plugin builds, block dev, WooCommerce, perf (caching, DB queries, assets). |
| Projects | Lead with WP (custom plugins, Gutenberg/ACF blocks, WooCommerce/Multisite). Keep Shopify/Laravel as secondary "also worked with". Reframe existing project copy; real client names/details added by user later. |
| Skills | PHP 7/8, OOP, design patterns · WP_Query, Template Hierarchy, Rewrite Rules, Cron, REST API · MySQL optimization · HTML5/CSS3/SCSS · JS ES6+, jQuery · Gutenberg, ACF · WooCommerce, Multisite · Git/PR workflow |
| Resume link | Local `assets/resume-wp.pdf` (empty placeholder PDF committed to repo). Replace all Google Drive resume occurrences in this file with the local path. |

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
