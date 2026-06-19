# WP Engineer Portfolio Variant — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Produce `wp-engineer.html` — a copy of the live `index.html` fully pivoted to a WordPress Engineer job application (Multidots role), with snake food as emoji (🍔🍵⭐) and the unlock reward reframed to "Free WordPress site audit".

**Architecture:** Static single-file HTML site, no build step (CLAUDE.md: no tests/linter/bundler). Verification is visual via `npx live-server`. We copy `index.html` → `wp-engineer.html` and edit only that copy; `index.html` stays untouched. All CSS/layout/JS structure is reused; we change text, pill labels, card order, the `FOOD_TYPES`/draw code, and the resume link.

**Tech Stack:** HTML, CSS (Materialize-adjacent custom + the file's own styles), vanilla JS canvas snake game.

**Source-of-truth content** (real, already in the file — never invent new clients):
- 9+ years experience (the live file says "10+"; user states 9+ — use **9+**).
- WP projects: The Hill Times (100k+ subs, custom theme+plugins), Skate Ontario (theme+plugin member portal), Aagaard Jewellery, Ewers Energi, Triple-A site (Figma→pixel-perfect).
- Plugins: Crypto Payment Gateway for WooCommerce (sole dev, singleton OOP, 800+ installs, REST, 14 releases/yr, 0 open issues), WK Plugin Suite.
- Roles: WK (current), Triple-A (2023–2025), Grype Digital (2019–2023, WP for NA clients), Neural Semiconductor, Sycorax.
- **Do NOT claim** WP Engine / Kinsta / WP VIP / Pantheon.

**Honesty note on the 9+ vs 10+ discrepancy:** user said 9+ years. Use "9+ years" consistently in the new file. (Plain edit, applies everywhere "10+" appears as a years claim.)

---

## File Structure

- **Create:** `wp-engineer.html` (copy of `index.html`, then edited).
- **Create:** `assets/resume-wp.pdf` (empty placeholder PDF, committed).
- **Untouched:** `index.html` and all other variants.

---

## Task 1: Scaffold the new file + placeholder resume

**Files:**
- Create: `wp-engineer.html`
- Create: `assets/resume-wp.pdf`

- [ ] **Step 1: Copy index.html to the new file**

```bash
cp index.html wp-engineer.html
```

- [ ] **Step 2: Create a minimal valid empty PDF placeholder**

```bash
printf '%%PDF-1.4\n1 0 obj<</Type/Catalog/Pages 2 0 R>>endobj\n2 0 obj<</Type/Pages/Kids[3 0 R]/Count 1>>endobj\n3 0 obj<</Type/Page/Parent 2 0 R/MediaBox[0 0 612 792]>>endobj\nxref\n0 4\n0000000000 65535 f \n0000000009 00000 n \n0000000052 00000 n \n0000000101 00000 n \ntrailer<</Size 4/Root 1 0 R>>\nstartxref\n164\n%%%%EOF\n' > assets/resume-wp.pdf
```

- [ ] **Step 3: Verify both files exist**

Run: `ls -la wp-engineer.html assets/resume-wp.pdf`
Expected: both listed, `wp-engineer.html` ~117K, `resume-wp.pdf` non-empty.

- [ ] **Step 4: Verify the PDF opens (header check)**

Run: `head -c 8 assets/resume-wp.pdf`
Expected: `%PDF-1.4`

- [ ] **Step 5: Commit**

```bash
git add wp-engineer.html assets/resume-wp.pdf
git commit -m "chore: scaffold wp-engineer.html from index.html + placeholder resume"
```

---

## Task 2: Snake food → emoji (🍔🍵⭐)

**Files:**
- Modify: `wp-engineer.html` (`FOOD_TYPES` ~L1569; `drawAll()` food block ~L1663-1681)

**Note:** line numbers are from `index.html` at copy time; if shifted, locate by the literal strings shown.

- [ ] **Step 1: Add emoji field to FOOD_TYPES**

Find:

```js
    const FOOD_TYPES=[
      {color:'#ff5fa2',points:1,weight:70,glow:'#ff5fa2'},
      {color:'#67e8f9',points:3,weight:22,glow:'#67e8f9'},
      {color:'#a78bfa',points:5,weight:8,glow:'#a78bfa',star:true}
    ];
```

Replace with:

```js
    const FOOD_TYPES=[
      {color:'#ff5fa2',points:1,weight:70,glow:'#ff5fa2',emoji:'🍔'},
      {color:'#67e8f9',points:3,weight:22,glow:'#67e8f9',emoji:'🍵'},
      {color:'#a78bfa',points:5,weight:8,glow:'#a78bfa',star:true,emoji:'⭐'}
    ];
```

- [ ] **Step 2: Draw emoji instead of circle/star in drawAll()**

Find the food-draw block:

```js
      // food
      if(food){
        const fx=food.x*CELL+CELL/2,fy=food.y*CELL+CELL/2,c=food.type.color,t=Date.now()/200;
        ctx.shadowColor=c;ctx.shadowBlur=22+Math.sin(t)*4;
        if(food.type.star){
          // 5-point star
          ctx.fillStyle=c;ctx.beginPath();
          for(let i=0;i<10;i++){const ang=(Math.PI*2*i)/10-Math.PI/2;const r=i%2?CELL*.22:CELL*.5;ctx.lineTo(fx+Math.cos(ang)*r,fy+Math.sin(ang)*r)}
          ctx.closePath();ctx.fill();
        } else {
          ctx.fillStyle=c;
          ctx.beginPath();ctx.arc(fx,fy,CELL*.45,0,Math.PI*2);ctx.fill();
        }
        ctx.shadowBlur=0;
        ctx.fillStyle='rgba(255,255,255,.9)';
        ctx.beginPath();ctx.arc(fx-CELL*.14,fy-CELL*.14,CELL*.1,0,Math.PI*2);ctx.fill();
        // point label above
        ctx.fillStyle=c;ctx.font=`bold ${CELL*.4}px Inter,system-ui`;ctx.textAlign='center';
        ctx.fillText('+'+food.type.points,fx,fy-CELL*.7);
      }
```

Replace with (draw the emoji centered; keep glow + the floating `+points` label; drop the circle/star path and the white highlight dot):

```js
      // food (emoji)
      if(food){
        const fx=food.x*CELL+CELL/2,fy=food.y*CELL+CELL/2,c=food.type.color,t=Date.now()/200;
        ctx.shadowColor=c;ctx.shadowBlur=18+Math.sin(t)*4;
        ctx.font=`${CELL*1.05}px "Apple Color Emoji","Segoe UI Emoji","Noto Color Emoji",system-ui`;
        ctx.textAlign='center';ctx.textBaseline='middle';
        ctx.fillText(food.type.emoji,fx,fy);
        ctx.shadowBlur=0;
        // point label above
        ctx.fillStyle=c;ctx.font=`bold ${CELL*.4}px Inter,system-ui`;ctx.textBaseline='alphabetic';
        ctx.fillText('+'+food.type.points,fx,fy-CELL*.7);
      }
```

- [ ] **Step 3: Visual verify**

Run: `npx live-server` (open `http://127.0.0.1:8080/wp-engineer.html`). Steer the snake with arrows/WASD toward a highlighted text target.
Expected: food renders as 🍔 / 🍵 / ⭐ (not colored dots), `+1/+3/+5` label floats above, eating still increments score.

- [ ] **Step 4: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: snake food renders as emoji (burger/tea/star)"
```

---

## Task 3: Reframe unlock reward → "Free WordPress site audit"

**Files:**
- Modify: `wp-engineer.html` (snake-pill title ~L551; reward logic ~L1531; `dcode` placeholder ~L1432; autofill ~L1806)

- [ ] **Step 1: Update snake-pill tooltip**

Find (the `title=` on the `snake-pill` div, ~L551):

```
title="Use arrows or WASD to guide the snake. Eat 30 letters to unlock FREE-AUDIT.">
```

Replace with:

```
title="Use arrows or WASD to guide the snake. Eat 30 to unlock your Free WP audit code.">
```

- [ ] **Step 2: Update the discount-code input placeholder**

Find (~L1432):

```html
<input placeholder="Discount code (complete Migration Quest → unlock FREE-AUDIT)" id="dcode">
```

Replace with:

```html
<input placeholder="Code (finish the snake game → unlock Free WP audit)" id="dcode">
```

- [ ] **Step 3: Keep the unlock CODE value, change only user-facing wording**

The code value `FREE-AUDIT` is the token the game grants. Keep the token working but present it as a WP audit. Find (~L1531):

```js
if(c==='FREE-AUDIT'||c==='FREEAUDIT'){da.textContent='✓ Unlocked';
```

Leave the matched token as-is (`FREE-AUDIT`/`FREEAUDIT`) so the game still unlocks, but verify the surrounding success message references a WP audit. Read 3 lines of context around L1531; if any visible label says "audit"/"migration", ensure it reads "Free WordPress site audit". If the success text is only `'✓ Unlocked'`, no change needed here.

- [ ] **Step 4: Update the autofill value to match new framing (optional cosmetic)**

Find (~L1806):

```js
const inp=document.getElementById('dcode');if(inp&&!inp.value){inp.value='FREE-AUDIT'
```

Leave the value `FREE-AUDIT` (matches the unlock check). No change unless it displays user-facing copy; if so, keep token but it's fine as the code.

- [ ] **Step 5: Visual verify**

Reload `wp-engineer.html`. Hover the snake pill → tooltip says "Free WP audit code". Eat 30 → unlock still fires and the discount field shows the unlocked state.
Expected: no remaining visible "Migration Quest" / "FREE-AUDIT" *labels* (the internal token value may remain in JS).

- [ ] **Step 6: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: reframe game reward to Free WordPress site audit"
```

---

## Task 4: Pivot hero + positioning to WordPress Engineer

**Files:**
- Modify: `wp-engineer.html` (`<title>` L6; hero eyebrow L605; speciality cards L607-635; vendor L645; role L647; hero foot L636-641)

- [ ] **Step 1: Title tag**

Find (L6):

```html
<title>Shakir Uz Zaman — Migration Expert · DanDomain → Shopify · WordPress → Shopify · DanDomain → WordPress</title>
```

Replace with:

```html
<title>Shakir Uz Zaman — WordPress Engineer · PHP 7/8 · Gutenberg · WooCommerce</title>
```

- [ ] **Step 2: Hero eyebrow**

Find (L605):

```html
        <span class="pip"></span>3 SPECIALITIES · IN STOCK · SHIPS WORLDWIDE
```

Replace with:

```html
        <span class="pip"></span>WORDPRESS ENGINEER · 9+ YEARS · REMOTE WORLDWIDE
```

- [ ] **Step 3: Recast the three speciality cards**

Find the three `<a class="hero-spec ...">` blocks (L607-635) and replace their tags/headings/copy (keep classes, hrefs, structure):

Card 01 (`hero-spec dd`):
```html
            <div class="hero-spec-tag">★ CORE</div>
            <h3>Custom Themes &amp; Plugins</h3>
            <p>WP Core architecture, request lifecycle, actions, filters, WP_Query, template hierarchy, rewrite rules, cron. Clean OOP PHP 7/8.</p>
```

Card 02 (`hero-spec wp`):
```html
            <div class="hero-spec-tag">EDITORIAL UX</div>
            <h3>Gutenberg &amp; ACF Blocks</h3>
            <p>Custom blocks and ACF fields that give editors a fast, predictable authoring experience. ES6+ and React where it helps.</p>
```

Card 03 (`hero-spec speed`):
```html
            <div class="hero-spec-tag">⚡ FAST</div>
            <h3>WooCommerce &amp; Performance</h3>
            <p>WooCommerce builds plus performance work: caching, DB query &amp; asset optimization, Core Web Vitals. Sub-1s FCP targets.</p>
```

Also change the third card's CTA text from `Audit →` to `See →` if it still reads "Audit"; the first two `Plan →` CTAs can become `See →`.

- [ ] **Step 4: Vendor tag + role line**

Find (L645):
```html
      <div class="vendor"><span class="pip"></span>CMS MIGRATION SPECIALIST</div>
```
Replace:
```html
      <div class="vendor"><span class="pip"></span>WORDPRESS ENGINEER</div>
```

Find (L647):
```html
      <div class="role"><b>Migration Expert</b> · DanDomain → Shopify · DanDomain → WP · WP cache &lt; 1s</div>
```
Replace:
```html
      <div class="role"><b>WordPress Engineer</b> · PHP 7/8 · Gutenberg · WooCommerce · REST API</div>
```

- [ ] **Step 5: Hero foot stats — change "10+" to "9+"**

Find (L637):
```html
        <span class="hero-foot-stat"><b>10+</b> years</span>
```
Replace:
```html
        <span class="hero-foot-stat"><b>9+</b> years</span>
```
Find the "0 failed migrations" stat (L640) and reframe:
```html
        <span class="hero-foot-stat"><b>0</b> open plugin issues</span>
```

- [ ] **Step 6: Visual verify**

Reload. Hero reads "WordPress Engineer · 9+ years", three cards = Themes/Plugins, Gutenberg/ACF, WooCommerce/Performance. No "Migration Expert" / "DanDomain" in hero.

- [ ] **Step 7: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: pivot hero + positioning to WordPress Engineer"
```

---

## Task 5: Reframe the buy/spec panel (pills, proficiency, CTAs)

**Files:**
- Modify: `wp-engineer.html` (migration pills L662-672; stack pills L674-685; eng pills L687-695; buy CTA L697-707; size-chart table L716-728)

- [ ] **Step 1: "Migration type" field → "Focus area"**

Find the field label + pills group `data-group="migration"` (L662-672). Replace the label text "Migration type" → "Focus area", "DanDomain → Shopify selected" → "Custom plugin dev selected", and the pills with:

```html
          <span class="pill on hot">Custom plugin dev</span>
          <span class="pill">Custom theme dev</span>
          <span class="pill">Gutenberg / ACF blocks</span>
          <span class="pill">WooCommerce</span>
          <span class="pill">Multisite</span>
          <span class="pill">REST / headless</span>
```

- [ ] **Step 2: Stack pills — lead WordPress**

Find `data-group="stack"` (L674-685). Reorder so the `on` pill is WordPress and WooCommerce/PHP lead; Shopify/Laravel/React/Svelte follow:

```html
          <span class="pill on"><span class="glow"></span>WordPress<span class="sub">^6.x</span></span>
          <span class="pill">WooCommerce<span class="sub">^9.x</span></span>
          <span class="pill">PHP<span class="sub">7 / 8</span></span>
          <span class="pill">Gutenberg<span class="sub">blocks</span></span>
          <span class="pill">ACF<span class="sub">pro</span></span>
          <span class="pill">Shopify<span class="sub">Liquid</span></span>
          <span class="pill">Laravel<span class="sub">^11</span></span>
          <span class="pill">React<span class="sub">^18</span></span>
```

Also update the picked label "Shopify Liquid selected" → "WordPress selected".

- [ ] **Step 3: Buy CTA wording**

Find (L701) `Add to cart · Book Shakir` → `Hire me · WordPress Engineer`.
Find (L704) the mailto subject `Hire%20Shakir%20-%20Migration%20Project` → `Hire%20Shakir%20-%20WordPress%20Engineer`.
Find (L706) `Buy now · Email directly` → keep as `Email me directly`.

- [ ] **Step 4: Size-chart proficiency table — WP first**

Find the table (L716-728) and reorder rows WP-first, adding WP-specific rows; keep the `bar` width style pattern:

```html
              <tr><td>WordPress (themes &amp; plugins)</td><td><span class="bar-wrap"><span class="bar" style="width:96%"></span></span>expert</td></tr>
              <tr><td>PHP 7/8 · OOP</td><td><span class="bar-wrap"><span class="bar" style="width:92%"></span></span>expert</td></tr>
              <tr><td>Gutenberg / ACF blocks</td><td><span class="bar-wrap"><span class="bar" style="width:88%"></span></span>strong</td></tr>
              <tr><td>WooCommerce</td><td><span class="bar-wrap"><span class="bar" style="width:92%"></span></span>expert</td></tr>
              <tr><td>WP_Query · REST API</td><td><span class="bar-wrap"><span class="bar" style="width:90%"></span></span>strong</td></tr>
              <tr><td>MySQL · query optimization</td><td><span class="bar-wrap"><span class="bar" style="width:85%"></span></span>strong</td></tr>
              <tr><td>JavaScript ES6+ · jQuery</td><td><span class="bar-wrap"><span class="bar" style="width:85%"></span></span>strong</td></tr>
              <tr><td>SCSS · responsive UI</td><td><span class="bar-wrap"><span class="bar" style="width:88%"></span></span>strong</td></tr>
              <tr><td>Shopify (Liquid)</td><td><span class="bar-wrap"><span class="bar" style="width:90%"></span></span>strong</td></tr>
              <tr><td>Laravel</td><td><span class="bar-wrap"><span class="bar" style="width:75%"></span></span>working</td></tr>
```

- [ ] **Step 5: "Care instructions" accordion — drop Shopify-only phrasing**

Find the "Care instructions — how I work" body (L733). Replace "Always pull before push on Shopify themes." with "Always pull before push; PR-based workflow." Keep the rest.

- [ ] **Step 6: Visual verify**

Reload. Focus-area + stack pills lead WordPress; proficiency table is WP-first; CTA says "Hire me · WordPress Engineer".

- [ ] **Step 7: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: reframe spec panel pills, proficiency, CTAs to WP Engineer"
```

---

## Task 6: Reframe the "Migration services" section to WP engineering

**Files:**
- Modify: `wp-engineer.html` (section `id="migration"` L744-onwards: eyebrow L747, h2 L749, lead L750, lanes L752+)

- [ ] **Step 1: Section eyebrow + heading + lead**

Find (L747) eyebrow text "Migration services" → "What I build".
Find (L749):
```html
    <h2>Migrate from <span class="hl">DanDomain</span> to <span class="hl">Shopify</span> or <span class="hl">WordPress</span>.</h2>
```
Replace:
```html
    <h2>I build and maintain <span class="hl">WordPress</span> themes, <span class="hl">plugins</span> and <span class="hl">blocks</span>.</h2>
```
Find (L750):
```html
    <p class="lead">SEO equity preserved. Zero-downtime cutover. e-conomic / Business Central integrations re-wired.</p>
```
Replace:
```html
    <p class="lead">Custom themes &amp; plugins, Gutenberg/ACF blocks, WooCommerce, REST integrations, and performance work — on PHP 7/8 with a clean PR workflow.</p>
```

- [ ] **Step 2: Recast the "lanes" as WP service/strength cards**

Read the lanes block (the `<a class="lane ...">` items starting L752). For each lane, replace the migration `flow`/heading/list with a WP strength. Keep the card markup and the mailto hrefs (update subjects to WP). Use these three (mirror the existing number of lanes; if more than 3 exist, collapse extras or repurpose — read the block first to count):

Lane 1:
```html
        <h3>Custom themes &amp; plugins</h3>
        <ul>
          <li>Actions, filters, WP_Query</li>
          <li>Template hierarchy &amp; rewrite rules</li>
          <li>Clean OOP PHP 7/8</li>
```
Lane 2:
```html
        <h3>Gutenberg &amp; ACF blocks</h3>
        <ul>
          <li>Custom blocks &amp; fields</li>
          <li>Editor-friendly UX</li>
          <li>ES6+ / React where useful</li>
```
Lane 3:
```html
        <h3>WooCommerce &amp; performance</h3>
        <ul>
          <li>WooCommerce builds &amp; extensions</li>
          <li>Caching, DB &amp; asset optimization</li>
          <li>Core Web Vitals</li>
```
Update each lane's mailto subject from migration wording to e.g. `subject=WordPress%20Engineering`.

- [ ] **Step 3: Check the "Any WordPress site under 1 second" sub-hero (L809)**

Read around L809. This block (`id="cache"`) is already WP-flavored — keep it, it supports the performance pitch. No change needed unless it references migration; if so, light edit only.

- [ ] **Step 4: Visual verify**

Reload. The former migration section now reads "What I build" with three WP strength cards; no "DanDomain"/"migration" copy remains in this section.

- [ ] **Step 5: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: reframe migration section to WordPress engineering strengths"
```

---

## Task 7: Reorder projects WP-first + relabel catalog

**Files:**
- Modify: `wp-engineer.html` (projects head L1019-1022; tabs L1023-1028; card order L1029-1188)

- [ ] **Step 1: Catalog heading + sub**

Find (L1020-1021):
```html
    <h2>Product catalog <span class="eyetag">12 PRODUCTS</span></h2>
    <div class="sub">Selected Shopify themes, WordPress sites and plugins shipped to production.</div>
```
Replace:
```html
    <h2>Selected work <span class="eyetag">SHIPPED</span></h2>
    <div class="sub">WordPress sites, plugins and blocks shipped to production — plus Shopify &amp; Laravel.</div>
```

- [ ] **Step 2: Reorder filter tabs so WordPress is the default/active**

Find the tabs (L1023-1028). Make `wp` the active (`on`) tab and order WordPress, Plugins, Shopify:
```html
    <button class="tab on" data-filter="all">All <span class="count">12</span></button>
    <button class="tab" data-filter="wp">WordPress <span class="count">5</span></button>
    <button class="tab" data-filter="plugin">Plugins <span class="count">3</span></button>
    <button class="tab" data-filter="shopify">Shopify <span class="count">4</span></button>
```
(Keep `all` first/active so the filter JS default still works; just reorder wp before shopify. If the filter JS keys off `data-filter="all"` being first, do NOT remove it.)

- [ ] **Step 3: Move WordPress + plugin project cards above Shopify cards**

In the `<div class="projects rv">` block, cut the WP cards (Hill Times L1121, Triple-A site L1136, Skate Ontario L1150, Aagaard L1160, Ewers L1174) and the plugin cards (Crypto Gateway L1085, WK Plugin Suite L1100, BV Dashboard L1110) and paste them **before** the four Shopify cards (Papirladen L1031, Ovellie L1046, QLiving L1060, Webkonsulenter L1070). Order WP cards first, then plugin cards, then Shopify cards. Do not edit card internals — only move them. (`data-cat` drives filtering; visual order is DOM order.)

- [ ] **Step 4: Visual verify**

Reload. "Selected work" grid shows WordPress + plugin cards first, Shopify after. Tab filtering still works (click WordPress / Plugins / Shopify).

- [ ] **Step 5: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: lead projects with WordPress + plugin work"
```

---

## Task 8: Tighten metrics, work history, skills, education + finalize "9+"

**Files:**
- Modify: `wp-engineer.html` (metrics L1195-1200; work history L1203-1261; skills L1264+; education section ~L1344; any remaining "10+")

- [ ] **Step 1: Metrics — change "10+" years to "9+", keep WP-relevant blurbs**

Find (L1196):
```html
    <div class="metric"><div class="big">10+</div><div class="lbl">Years</div><div class="blurb">CMS development</div></div>
```
Replace:
```html
    <div class="metric"><div class="big">9+</div><div class="lbl">Years</div><div class="blurb">WordPress &amp; PHP development</div></div>
```

- [ ] **Step 2: Work history — eyebrow + lead-in + current role title**

Find (L1204) `Work history <span class="eyetag">10+ YRS</span>` → `Work history <span class="eyetag">9+ YRS</span>`.
Find (L1205) the sub describing "Shopify themes and WordPress plugin product engineering" → reword WP-first: "A decade across remote roles — WordPress theme &amp; plugin engineering, WooCommerce, plus Shopify and Laravel."
Find (L1210) current role `<h3>CMS Developer · Shopify &amp; WordPress</h3>` → `<h3>WordPress &amp; CMS Developer</h3>`. Reorder its bullet list so the WordPress/plugin bullet (L1214) comes first, Shopify bullet second.

- [ ] **Step 3: Skills section — sub copy stays honest; ensure WP-first ordering**

Read the skillgrid (L1268+). Ensure the "Daily driver" column lists WordPress before Shopify (find L1276-1277 rows and swap order so WordPress ★★★★★ is first). No fabricated skills.

- [ ] **Step 4: Global sweep for leftover migration/Shopify-lead wording + "10+"**

Run a search in the new file:

```bash
grep -niE "10\+|migration expert|migration quest|free-audit|dandomain|book shakir|add to cart" wp-engineer.html
```

Expected after edits: no user-facing "10+", "Migration Expert", "Migration Quest", "Book Shakir", "Add to cart", or "DanDomain" in visible copy. The internal `FREE-AUDIT` token in JS (Task 3) may remain — that is the only allowed match. Fix any visible leftovers inline using the same reframing rules (WP-engineering wording, never invent clients).

- [ ] **Step 5: Education section sanity**

Read around L1344 (`Education`). Leave factual (B.Sc CSE). No change unless it references migration/Shopify; if so, none expected.

- [ ] **Step 6: Visual verify (full page pass)**

Reload `wp-engineer.html` and scroll the entire page top to bottom.
Expected: consistent "WordPress Engineer / 9+ years" story; WP work leads everywhere; snake food = emoji; unlock = Free WP audit; resume link (Task 9) works; no migration-sales framing visible.

- [ ] **Step 7: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: WP-first metrics, work history, skills; normalize to 9+ years"
```

---

## Task 9: Resume link → local placeholder PDF

**Files:**
- Modify: `wp-engineer.html` (resume/CV link — location TBD by search)

- [ ] **Step 1: Find any existing resume/CV link**

```bash
grep -niE "resume|cv|curriculum|drive\.google" wp-engineer.html
```

- [ ] **Step 2a: If a resume link exists** — point it to the local PDF

Replace its `href` with `/assets/resume-wp.pdf` (site-absolute path, per repo convention — resolves on GitHub Pages and live-server). Add `download` attribute. Example shape:
```html
<a href="/assets/resume-wp.pdf" download>Download résumé</a>
```

- [ ] **Step 2b: If NO resume link exists** — add one in the hero info panel

Insert directly after the `buy-now` mailto link (after L707 region) a résumé link styled to match the existing `buy-now` class:
```html
      <a class="buy-now" href="/assets/resume-wp.pdf" download>
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2"><path d="M12 3v12m0 0l-4-4m4 4l4-4M5 21h14"/></svg>
        Download résumé (PDF)
      </a>
```

- [ ] **Step 3: Visual verify**

Reload. Click the résumé link → browser downloads / opens `resume-wp.pdf` (blank page is expected — placeholder).

- [ ] **Step 4: Commit**

```bash
git add wp-engineer.html
git commit -m "feat: link résumé to local placeholder PDF"
```

---

## Task 10: Final review pass + deploy decision

**Files:**
- Modify: `wp-engineer.html` (only if review finds issues)

- [ ] **Step 1: Cross-check against the Multidots requirements**

Confirm the page surfaces, in real terms: WP Core architecture; actions/filters/WP_Query/template hierarchy/rewrite/cron/REST; PHP 7/8 OOP; HTML5/CSS3/SCSS/JS ES6+/jQuery; Git/PR workflow; perf (caching, DB, assets); Gutenberg/ACF; WooCommerce; Multisite. Anything missing → add to skills or a card using real evidence only.

- [ ] **Step 2: Confirm no fabricated managed-hosting claims**

```bash
grep -niE "wp engine|kinsta|wp vip|wordpress vip|pantheon" wp-engineer.html
```
Expected: no matches (we did not claim these).

- [ ] **Step 3: Final visual QA via live-server**

Open `http://127.0.0.1:8080/wp-engineer.html`. Play the snake to 30, unlock the audit code, click résumé, click a few project tabs. Confirm no console errors (`F12`).

- [ ] **Step 4: Decide deployment with the user**

Do NOT push without explicit user confirmation. Ask: "Commit/push `wp-engineer.html` to `master` (goes live on GitHub Pages), or keep it local as a draft variant like the others?"

- [ ] **Step 5 (only if user approves push):**

```bash
git push origin master
```

---

## Self-Review (author check)

- **Spec coverage:** food→emoji (T2 ✓), reward→WP audit (T3 ✓), copy retarget across title/hero/pills/proficiency/migration/projects/skills/history (T4–T8 ✓), resume→local PDF (T1+T9 ✓), no fabricated hosts (T10 ✓), index.html untouched (only wp-engineer.html edited ✓).
- **Placeholder scan:** every code step shows literal find/replace strings; line numbers flagged as "locate by literal string if shifted". No TODO/TBD.
- **Consistency:** unlock token `FREE-AUDIT` kept in JS while only user-facing labels change (T3) — intentional and noted so the game keeps working. "9+ years" applied everywhere "10+" appeared (T4, T8). Site-absolute `/assets/...` path matches repo convention.
- **Known soft spots for executor:** Task 6 (lanes) and Task 7 (card reorder) require reading the live block first to count items before editing — both steps say so.
