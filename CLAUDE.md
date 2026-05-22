# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Static personal portfolio site for Shakir Uz Zaman, deployed via GitHub Pages from the `master` branch of `zaman-shakir.github.io` (root). No build step — files served as-is.

## Commands

- Install dev dep: `npm install`
- Local dev server (live reload): `npx live-server` — serves repo root on `http://127.0.0.1:8080`
- Deploy: push to `master` on `origin`. GitHub Pages auto-publishes.

No tests, no linter, no bundler configured.

## Architecture

Single-page site. Everything renders from `index.html`.

- `index.html` (~984 lines) — entire page. All sections (`#intro`, `#about`, `#experience`, `#projects`, `#skills`, `#education`, `#contact`) are anchor-scroll targets inside one document. Large blocks are commented-out variants kept for reference (alternate cards, certifications, second education entry, ML/AI projects). Edit in place; don't split into partials — no templating engine.
- `assets/css/style.css` (~707 lines) — custom styles layered on top of Materialize. Anchored to Materialize class names (`teal-text`, `card`, `side-nav`, etc.).
- `assets/vendor/typed.js/` — local copy of Typed.js for the intro typing animation. Strings live inline in `index.html` near `</body>` (`new Typed('.typing', { strings: [...] })`).
- `assets/img/` — all images, including `favicon/` set referenced from `<head>`.
- External CDNs loaded in `<head>` / before `</body>`: Materialize 0.95.3 CSS+JS, Font Awesome 4.3.0, jQuery 1.11.2. Materialize init (`scrollSpy`, `sideNav`) runs in inline `<script>` at end of body.

## Path convention

Asset URLs use site-absolute paths (`/assets/...`, `/assets/img/favicon/...`). These resolve correctly on `zaman-shakir.github.io` (deployed at site root) and via `live-server` from repo root. Do not switch to relative paths without verifying both.

## Analytics

GTM (`GTM-PGZH8HT`) and gtag (`UA-126939217-2`) hardcoded in `<head>`. Leave intact unless explicitly changing.

## Editing notes

- Nav is duplicated 3x in `index.html` (desktop side-nav, mobile top bar, mobile slide-out). Section additions require updating all three.
- Resume link is a Google Drive URL — appears multiple times, update all occurrences.
- Many `<!-- ... -->` blocks are intentional dormant content. Don't delete without confirming.
