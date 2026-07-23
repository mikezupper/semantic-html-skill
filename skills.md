---
name: semantic-html
description: Build static, CSS-free, purely semantic HTML pages that exploit the full 2026 HTML element vocabulary, with baked-in best practices for SEO, ARIA/accessibility, and internationalization. Use whenever creating or modifying static .html documents where meaning, machine-readability, and accessibility matter more than presentation.
---

# Semantic HTML (2026) — Static, Style-Free, Meaning-First

You are building **static HTML documents with zero CSS and zero JavaScript for presentation**. Every byte of markup must carry meaning. The browser's default rendering is the only styling. Your output is judged on: correctness of element choice, accessibility-tree quality, SEO signal quality, and internationalization readiness.

## Prime Directives

1. **Meaning over appearance.** Never choose an element for how it looks. Choose it for what it *is*. If you catch yourself thinking "this will render bold/indented/large," stop and re-derive the choice from semantics.
2. **The element-selection ladder.** For any piece of content, ask in order:
   1. Is there a *specific* element for this? (`<time>`, `<address>`, `<dfn>`, `<samp>`, `<output>`, …) → use it.
   2. Is there a *structural* element for this region? (`<nav>`, `<article>`, `<aside>`, `<search>`, …) → use it.
   3. Only if nothing fits, fall back to `<div>` / `<span>` — and treat every `<div>`/`<span>` in the final document as a defect to justify or remove.
3. **No presentation layer.** Forbidden: `<style>`, `style=""`, `<link rel="stylesheet">`, `<font>`, `align`/`bgcolor`/`border`/`width`/`height`-as-styling attributes, and `class` attributes (there is no CSS to hook; classes are noise here). `id` is allowed — it serves anchors, `<label for>`, `aria-labelledby`, and fragment links.
4. **First rule of ARIA: don't use ARIA.** Native HTML semantics beat `role` and `aria-*` retrofits. Reach for ARIA only to fill gaps native HTML cannot express (see the ARIA section).
5. **No deprecated elements, ever:** `<acronym>` `<big>` `<center>` `<dir>` `<font>` `<frame>` `<frameset>` `<marquee>` `<nobr>` `<param>` `<plaintext>` `<strike>` `<tt>` `<xmp>` `<menuitem>` `<rb>` `<rtc>` and friends. Replacements: `<abbr>`, `<s>`/`<del>`, `<iframe>`, `<ruby>`+`<rt>`+`<rp>`.
6. **Validate mentally against the content model.** Respect content categories (flow, phrasing, sectioning, heading, embedded, interactive, metadata): no block content inside `<p>`, no interactive content nested in interactive content (`<a>` inside `<button>` is invalid), `<li>` only inside `<ul>`/`<ol>`/`<menu>`, `<dt>`/`<dd>` only inside `<dl>` (optionally via `<div>` wrappers), `<summary>` first child of `<details>`.

## Canonical Document Skeleton

Every page starts from this shape (adapt values, keep the order — charset and viewport within the first bytes of `<head>`, title early):

```html
<!doctype html>
<html lang="en" dir="ltr">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>Primary Topic — Site Name</title>
  <meta name="description" content="One compelling, unique sentence of 150–160 characters describing THIS page.">
  <link rel="canonical" href="https://example.com/this-page/">
  <link rel="alternate" hreflang="en" href="https://example.com/this-page/">
  <link rel="alternate" hreflang="es" href="https://example.com/es/this-page/">
  <link rel="alternate" hreflang="x-default" href="https://example.com/this-page/">
  <link rel="icon" href="/favicon.ico">
  <meta property="og:type" content="article">
  <meta property="og:title" content="Primary Topic">
  <meta property="og:description" content="Same intent as meta description.">
  <meta property="og:url" content="https://example.com/this-page/">
  <meta property="og:image" content="https://example.com/img/social.png">
  <meta name="twitter:card" content="summary_large_image">
  <script type="application/ld+json">
  { "@context": "https://schema.org", "@type": "Article",
    "headline": "Primary Topic",
    "datePublished": "2026-01-15",
    "author": { "@type": "Person", "name": "Author Name" } }
  </script>
</head>
<body>
  <a href="#main">Skip to main content</a>
  <header>
    <p>Site Name</p>
    <nav aria-label="Primary">
      <ul>
        <li><a href="/" aria-current="page">Home</a></li>
        <li><a href="/about/">About</a></li>
      </ul>
    </nav>
    <search>
      <form action="/search" method="get">
        <label for="q">Search this site</label>
        <input type="search" id="q" name="q">
        <button type="submit">Search</button>
      </form>
    </search>
  </header>
  <main id="main">
    <article>
      <header>
        <h1>Primary Topic</h1>
        <p>Published <time datetime="2026-01-15">January 15, 2026</time> by <a rel="author" href="/authors/x/">Author Name</a></p>
      </header>
      <!-- content -->
      <footer>…article-scoped footer…</footer>
    </article>
    <aside aria-labelledby="related-heading">
      <h2 id="related-heading">Related reading</h2>
      …
    </aside>
  </main>
  <footer>
    <address>
      Site Name · <a href="mailto:hello@example.com">hello@example.com</a>
    </address>
    <p><small>© 2026 Site Name. All rights reserved.</small></p>
  </footer>
</body>
</html>
```

`<script type="application/ld+json">` is data, not behavior — it is the one permitted `<script>` in a static page.

## The Full Element Vocabulary — Use It

A page that only uses `<div>`, `<p>`, `<h*>`, `<a>`, `<ul>` is a failure for this skill. Actively look for opportunities to deploy the *entire* modern vocabulary. Reference: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements

### Sectioning & landmarks
| Element | Use when |
|---|---|
| `<main>` | The unique dominant content region. **Exactly one per page.** |
| `<header>` / `<footer>` | Intro/outro of the page *or of any `<article>`/`<section>`* — they nest and scope. |
| `<nav>` | Major navigation blocks only (primary nav, table of contents, breadcrumbs, pagination). Multiple navs each need a distinguishing `aria-label`. |
| `<article>` | Self-contained, independently distributable composition: blog post, comment, product card, forum reply. Each gets its own heading. |
| `<section>` | Thematic grouping **with a heading**. A `<section>` without a heading is a smell — use `<div>` or restructure. |
| `<aside>` | Tangential content: pull quotes, related links, sidebars, glossary callouts. |
| `<search>` | Wrapper for search/filter functionality (2023+; fully baseline by 2026). Prefer it over `role="search"`. |
| `<address>` | Contact info for the nearest `<article>` or `<body>` — not generic postal addresses. |
| `<h1>`–`<h6>` | One `<h1>` per page. Never skip levels descending. Choose the level by outline depth, never by size. |
| `<hgroup>` | Heading plus subtitle/tagline `<p>`s grouped as one unit. |

#### Page architecture rules

- **Canonical landmark order:** skip link → `<header>` (banner) → `<nav>` → `<main>` → `<footer>` (contentinfo). A `<header>`/`<footer>` whose nearest ancestor is `<body>` becomes the page banner/contentinfo — there must be **exactly one of each at that level**; headers/footers nested inside `<article>`/`<section>` are scoped and unlimited.
- **All perceivable content lives inside a landmark.** Nothing floats directly in `<body>` except the skip link; screen-reader users navigate by landmark, and orphaned content is invisible to that navigation.
- **`<article>` vs `<section>` test:** would this chunk make sense on its own in an RSS feed or search result? Self-contained → `<article>`. Merely a chapter of the current page → `<section>`. Neither (pure grouping, no heading) → `<div>`.
- **Articles nest:** comments on a blog post are `<article>`s inside the post's `<article>`; each carries its own `<header>` with author + `<time>`.
- **Avoid `<base>`:** it rewrites *every* relative URL — including fragment links like the skip link's `#main` and SVG `url(#id)` references. On a static multi-page site, correct relative paths beat one fragile global rewrite.
- **Keep the DOM shallow.** Wrapper stacks (`<div><div><div>`) are a styling habit; with no CSS there is nothing to hang them on. Shallow trees parse, render, and read faster.

### Text-level semantics (the most-neglected elements — use them deliberately)
| Element | Meaning |
|---|---|
| `<em>` vs `<i>` | Stress emphasis that changes meaning vs. alternate-voice text (taxonomy, ship names, foreign phrases — add `lang` on `<i>` for foreign terms). |
| `<strong>` vs `<b>` | Strong importance/seriousness/urgency vs. attention-drawn keywords with no extra importance. |
| `<cite>` | Title of a creative work (book, article, film, spec) — not the author's name. |
| `<q>` / `<blockquote>` | Inline vs. block quotation. Use `cite="url"` attribute for the source; attribute the quote in a sibling element (e.g. a `<figcaption>` when wrapped in `<figure>`), not inside the `<blockquote>`. |
| `<abbr title="…">` | Abbreviations/acronyms; spell out on first use. |
| `<dfn>` | The defining instance of a term (pair with `<dl>` glossaries or `id` for cross-references). |
| `<code>`, `<kbd>`, `<samp>`, `<var>` | Code fragments, user input/keystrokes, program output, variables/placeholders. Combine: `<kbd><kbd>Ctrl</kbd>+<kbd>S</kbd></kbd>`. Block code = `<pre><code>`. |
| `<time datetime="…">` | Every date, time, and duration. Machine format in `datetime` (`2026-07-22`, `PT2H30M`), human format as content. |
| `<data value="…">` | Machine-readable value for non-temporal content (SKUs, quantities, IDs). |
| `<mark>` | Contextual relevance highlight (search-hit marking, referenced passage) — not decoration. |
| `<del>` / `<ins>` | Document edits, with `datetime` and `cite` where known. `<s>` for no-longer-accurate content that isn't an "edit" (old price). |
| `<u>` | Non-textual annotation only (misspelling marker, Chinese proper-name mark). Almost never correct; links must never be faked with it. |
| `<small>` | Side comments and legal fine print (copyright, disclaimers). |
| `<sub>` / `<sup>` | Chemistry, math, footnote markers (`H<sub>2</sub>O`, `x<sup>2</sup>`). |
| `<ruby>` + `<rt>` + `<rp>` | East Asian pronunciation annotations (see i18n). |
| `<bdi>` / `<bdo>` | Bidirectional isolation of user-generated/unknown-direction text; explicit direction override (see i18n). |
| `<wbr>`, `<br>` | Word-break opportunity in long tokens/URLs; line break only where breaks are content (poems, addresses) — never for spacing. |
| `<span>` | Last resort; must carry an attribute that justifies it (`lang`, `id`, `translate`, microdata). |

### Grouping content
- `<p>` for paragraphs; `<hr>` for a *thematic shift* (scene change, topic pivot) — not a visual divider.
- `<ul>` / `<ol>` (with `reversed`, `start`, `type` when meaningful) / `<li>`; nest lists inside `<li>`.
- `<menu>` for toolbars/lists of commands (semantically a `<ul>` of interactive items).
- `<dl>` / `<dt>` / `<dd>` for name–value pairs: metadata blocks, glossaries, FAQs, spec sheets.
- `<figure>` + `<figcaption>` for any self-contained referenced content: images, code listings, tables, quotes, diagrams.
- `<div>` only as a grouping device of last resort (e.g. wrapping `<dt>`/`<dd>` pairs, microdata scopes).

### Tables — for tabular data only, never layout
```html
<table>
  <caption>Quarterly revenue by region, 2026</caption>
  <colgroup><col><col span="2"></colgroup>
  <thead><tr><th scope="col">Region</th><th scope="col">Q1</th><th scope="col">Q2</th></tr></thead>
  <tbody><tr><th scope="row">EMEA</th><td>…</td><td>…</td></tr></tbody>
  <tfoot><tr><th scope="row">Total</th><td>…</td><td>…</td></tr></tfoot>
</table>
```
Always: `<caption>`, `scope` on every `<th>` (or `headers`/`id` for complex tables), `<thead>`/`<tbody>`/`<tfoot>`.

### Forms (static pages may still contain working GET forms)
- Every control gets a `<label for>` (or wraps the control). Never rely on `placeholder` as a label.
- Group related controls in `<fieldset>` + `<legend>` (mandatory for radio/checkbox groups).
- Use the *specific* input type: `email`, `url`, `tel`, `search`, `number`, `date`, `time`, `datetime-local`, `month`, `week`, `range`, `color` — plus `required`, `min`/`max`/`step`, `pattern`, `minlength`/`maxlength`, `autocomplete` (use standard tokens: `name`, `email`, `postal-code`…), `inputmode`, `enterkeyhint`.
- `<datalist>` for suggestion lists; `<optgroup label>` inside `<select>`; `<output>` for computed results; `<meter>` for scalar gauges (with `min`/`max`/`low`/`high`/`optimum`); `<progress>` for task completion.
- `<button type="submit|reset|button">` — always explicit `type`.

### Interactive & disclosure (JS-free interactivity — use these instead of scripts)
- `<details>` + `<summary>` for accordions/FAQ/disclosure; `name="group"` attribute makes an exclusive accordion; `open` for default-expanded.
- `<dialog>` only when scriptable behavior exists — in purely static pages, prefer `<details>` or a plain section.
- `popover` attribute + `popovertarget` on a `<button>`: fully declarative, JS-free popovers — valid in static pages.
- `<a href>` for navigation, `<button>` for actions. Never a link that does nothing, never `href="#"`.

### Media & embedded content
- `<img src alt width height loading="lazy" decoding="async">` — intrinsic `width`/`height` (integer pixels, not styling) to prevent layout shift.
- `<picture>` + `<source media|type|srcset|sizes>` for art direction and format negotiation (AVIF/WebP with fallback).
- `<audio controls>` / `<video controls>` with nested `<source>` fallbacks and `<track kind="captions|subtitles|descriptions" srclang label>` — captions are non-negotiable for video with speech.
- `<map>` + `<area alt>` for image maps; `<iframe title="…" loading="lazy">` for embeds; `<object>`/`<embed>` only for legacy needs.
- `<svg>` inline for diagrams (give it `role="img"` and `<title>`), `<math>` for MathML formulas.
- `<canvas>`, `<template>`, `<slot>`, `<noscript>` exist but are script-serving — omit from static pages.
- Experimental 2026 elements (`<geolocation>`, `<selectedcontent>`, `<fencedframe>`) require JavaScript and are not Baseline — see **Frontier Elements** below for when and how to use them if the user relaxes the static/no-JS constraint.

## Frontier Elements (experimental, 2026 — reference only)

These three elements exist in the 2026 platform but are **not Baseline** (Chromium-first, behind flags in some cases) and **all require JavaScript** to do anything useful. In a pure static page, do not use them live — demonstrate them as code listings instead. Use them for real only when the user explicitly relaxes the "static, no-JS" constraint, and always behind feature detection with a working fallback.

### `<geolocation>` — declarative location sharing

**What it is.** A browser-rendered control (Chrome shows a "map pin" button) that asks for and delivers the user's location. The browser owns the permission UI; your code just listens. Interface: `HTMLGeolocationElement`, firing a `location` event and exposing `.position` (a `GeolocationPosition`) or `.error`.

**Attributes:** `autolocate` (boolean — fetch position on render if permission already granted), `watch` (boolean — continuously update as the device moves; otherwise one-shot).

**When to use:** store finders, weather, "near me" features — anywhere you'd previously hand-roll a button around `navigator.geolocation.getCurrentPosition()`. Prefer it because permission handling and UI are built in. **When not to:** any page needing cross-browser support today — always ship the classic Geolocation API fallback.

```html
<geolocation watch>
  <!-- fallback content renders in non-supporting browsers -->
  <button id="geo-fallback" type="button">Use my location</button>
</geolocation>
<p id="geo-output" aria-live="polite"></p>

<script>
  const out = document.querySelector("#geo-output");
  if (typeof HTMLGeolocationElement === "function") {
    const geo = document.querySelector("geolocation");
    geo.addEventListener("location", () => {
      if (geo.position) {
        const { latitude, longitude } = geo.position.coords;
        out.textContent = `You are at ${latitude}, ${longitude}`;
      } else if (geo.error) {
        out.textContent = geo.error.message;
      }
    });
  } else {
    document.querySelector("#geo-fallback").addEventListener("click", () => {
      navigator.geolocation.getCurrentPosition(
        (pos) => { out.textContent = `You are at ${pos.coords.latitude}, ${pos.coords.longitude}`; },
        (err) => { out.textContent = err.message; },
      );
    });
  }
</script>
```

### `<selectedcontent>` — customizable `<select>`

**What it is.** Mirrors the currently selected `<option>`'s content into the closed `<select>`'s button. Structure is strict: `<selectedcontent>` lives inside a `<button>` that must be the **first child** of the `<select>`. The browser `cloneNode()`s the selected option's content into it and swaps the clone on every selection change. Everything inside the button is inert.

**When to use:** rich option content (flag icons, avatars, swatches) where the closed control should echo the selection — the modern replacement for JS "fake select" libraries, keeping native keyboard/AT behavior. It is intrinsically a *styling* feature: it only activates when CSS opts in with `appearance: base-select`, so it is by definition out of scope for a no-CSS page. **Gotchas:** frameworks that re-render `<option>` content won't auto-refresh the clone; if you omit the `<button>`, the browser makes an implicit one you can't target.

```html
<select id="fruit">
  <button>
    <selectedcontent></selectedcontent>
  </button>
  <option value="apple">🍎 Apple</option>
  <option value="orange">🍊 Orange</option>
  <option value="banana">🍌 Banana</option>
</select>

<style>
  /* Opt-in switch — without this, it's a normal native select */
  select, select::picker(select) { appearance: base-select; }
</style>

<script>
  // Optional: selection still fires normal events; no JS needed for basic use
  document.querySelector("#fruit").addEventListener("change", (e) => {
    console.log("Selected:", e.target.value);
  });
</script>
```

### `<fencedframe>` — privacy-preserving embeds

**What it is.** An `<iframe>` variant with the communication channel cut: no `postMessage` to/from the embedder, no shared storage/cookies, no URL visible to the page, no programmatic focus across the boundary. The embedded document is chosen *opaquely* by a Privacy Sandbox API — `navigator.runAdAuction()` (Protected Audience) or `sharedStorage.selectURL()` — which resolves to a `FencedFrameConfig` you assign to `frame.config`. You never see the winning URL. Attributes: `width`, `height`, `allow` (Privacy Sandbox policies only: `attribution-reporting`, `private-aggregation`, `shared-storage`, `shared-storage-select-url`).

**When to use:** post-third-party-cookie ad slots, frequency-capped or interest-group-selected content, any embed where the embedder must be provably unable to learn what was shown. **When not to:** general embedding (use `<iframe>`), anything needing parent↔frame communication or standard permissions (camera, geolocation — blocked by design), or cross-browser reach (Chromium-only, flag-gated). Accessibility: always set `title`.

```html
<fencedframe id="ad-slot" width="640" height="320"
  title="Advertisement, selected privately"></fencedframe>

<script>
  async function loadAd() {
    const frameConfig = await navigator.runAdAuction({
      seller: "https://ssp.example",
      decisionLogicUrl: "https://ssp.example/decision-logic.js",
      interestGroupBuyers: ["https://dsp-a.example", "https://dsp-b.example"],
      auctionSignals: { adSlot: "top-banner" },
      resolveToConfig: true, // REQUIRED — otherwise you get a urn: usable only in <iframe>
    });
    document.querySelector("#ad-slot").config = frameConfig; // opaque: no URL exposed
  }
  if ("HTMLFencedFrameElement" in window) loadAd();
</script>
```

MDN references: [`<geolocation>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/geolocation) · [`<selectedcontent>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/selectedcontent) · [`<fencedframe>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/fencedframe)

**Watch list:** Chromium is also trialing a `<permission>` element (Page-Embedded Permission Control — a browser-rendered button for camera/mic/location prompts). It is not yet on the MDN element index; verify current status before recommending it.

## SEO Best Practices

1. **One `<h1>`**, containing the primary keyphrase naturally; logical `<h2>`→`<h3>` outline (crawlers reconstruct topic structure from it).
2. **`<title>`**: unique per page, ~50–60 chars, most important words first, `Page Topic — Site Name` pattern.
3. **`<meta name="description">`**: unique, 150–160 chars, written as the search-result pitch.
4. **`<link rel="canonical">`** on every page; self-referencing is correct.
5. **Structured data as JSON-LD** (schema.org): `Article`, `BreadcrumbList`, `FAQPage`, `Product`, `Organization`, `WebSite` as appropriate. Prefer JSON-LD over microdata; if the user asks for attribute-level markup instead, use microdata globals (`itemscope`, `itemtype`, `itemprop`, `itemid`, `itemref`).
6. **Descriptive link text** — never "click here"/"read more"; the anchor text should predict the destination. Use `rel="external"`, `rel="nofollow"`/`rel="sponsored"`/`rel="ugc"` where applicable, `rel="author"`, `rel="license"`.
7. **`alt` text on every `<img>`**: describes function/content in context; `alt=""` (empty, present) for purely decorative images.
8. **Semantic landmarks double as SEO signals**: `<main>`, `<article>`, `<nav>`, `<time datetime>` (freshness), `<address>` (entity/contact) all feed crawlers.
9. Clean URL hygiene in links: trailing-slash consistency, no fragment-only navigation for content.
10. Open Graph + Twitter Card meta for share previews (see skeleton).
11. `<meta name="robots">` only when you need to *restrict* (`noindex`); omit otherwise.

## ARIA & Accessibility Best Practices

1. **Native first.** `<button>` not `role="button"`, `<nav>` not `role="navigation"`, `<search>` not `role="search"`, `<main>` not `role="main"`. Adding a redundant role to a semantic element is an error.
2. **Legitimate ARIA in static pages:**
   - `aria-label` on repeated landmarks: `<nav aria-label="Primary">`, `<nav aria-label="Breadcrumb">`, `<nav aria-label="Pagination">`.
   - `aria-labelledby` to bind a `<section>`/`<aside>` to its visible heading.
   - `aria-current="page"` on the current nav link; `aria-current="step|date|location"` where applicable.
   - `aria-describedby` linking form controls to hint/error text.
   - `aria-hidden="true"` on purely decorative inline SVG/emoji duplicating adjacent text.
   - `role="img"` + `aria-label` on meaningful emoji or ASCII art; `role="img"` + `<title>` on inline SVG.
   - `role="doc-*"` (DPUB) for long-form documents if warranted (`doc-toc`, `doc-endnotes`).
3. **Skip link** as the first focusable element, targeting `<main id="main">`.
4. **Heading integrity**: every landmark region reachable and labeled; no heading-level skips; nothing that *looks* like a heading marked up as `<p><strong>`.
5. **Forms**: programmatic labels always; `<fieldset>`/`<legend>` for groups; error text linked via `aria-describedby`; `required` + clear indication in the label text (not color/asterisk alone).
6. **Tables**: `<caption>` + `scope` (see above).
7. **Media**: captions tracks, transcripts adjacent to audio (`<details><summary>Transcript</summary>…</details>` works well).
8. **Never**: `tabindex` > 0; `aria-*` contradicting native semantics; `title` attribute as the only label; `accesskey` (conflicts with AT); positive redundancy like `role="list"` on `<ul>` (exception: none needed in CSS-free pages).
9. **Reading order = DOM order.** With no CSS, what you write is what AT users get — order content logically.

## Internationalization Best Practices

1. **`lang` on `<html>` always** (BCP 47: `en`, `en-GB`, `zh-Hans`, `pt-BR`). **`lang` on every element whose language differs** from its ancestor: `<i lang="fr">déjà vu</i>`, `<blockquote lang="de">…</blockquote>`. Screen readers switch pronunciation engines on this.
2. **`dir`**: `dir="rtl"` on `<html>` for Arabic/Hebrew/Persian/Urdu documents; `dir="auto"` on elements holding user-generated or unknown-direction content.
3. **`<bdi>`** around any interpolated text of unknown direction (usernames, titles in lists) to prevent bidi spillover: `<p>User <bdi>אורי</bdi> — 5 posts</p>`. `<bdo dir="…">` only to force direction.
4. **`<ruby>` annotations** for CJK pronunciation: `<ruby>漢<rp>(</rp><rt>kan</rt><rp>)</rp></ruby>` — always include `<rp>` fallbacks.
5. **`translate="no"`** on brand names, code, addresses, and other do-not-localize spans.
6. **`hreflang`** on: `<link rel="alternate" hreflang="…">` for every language version of the page *including itself*, plus `hreflang="x-default"`; and on `<a hreflang>` links pointing to other-language documents.
7. **`<meta charset="utf-8">`** first in `<head>`; author files as UTF-8 without BOM.
8. **Dates/numbers**: `<time datetime>` machine values are locale-neutral; write the human-readable content in the page's locale conventions.
9. **Quotes**: prefer `<q>` (browsers insert locale-correct quotation marks based on `lang`) over typing quote characters.
10. Don't encode text in images; if unavoidable, mirror content in `alt` so it can be translated.

## Performance — Fast-Loading Static Pages

Adapted from MDN's [Author fast-loading HTML pages](https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages) for the static/no-CSS context (where you already win by default: zero render-blocking stylesheets and scripts):

1. **Valid markup is fast markup.** Invalid HTML forces the parser into error recovery; a Nu-validated page parses in one clean pass. This is a speed feature, not just hygiene.
2. **Intrinsic dimensions everywhere:** `width`/`height` (integer pixels) on every `<img>`, `<video>`, and `<iframe>` so the browser reserves space before the asset arrives — zero layout shift. For big tables, `<colgroup>`/`<col span>` lets rendering begin before all rows parse.
3. **Lazy-load below the fold:** `loading="lazy"` on images and iframes that start off-screen — but **never** on the first/hero image (it delays Largest Contentful Paint). Add `fetchpriority="high"` to the LCP image, `fetchpriority="low"` to genuinely unimportant ones, and `decoding="async"` generally.
4. **Media discipline:** `preload="none"` (or `"metadata"`) on `<audio>`/`<video>` so multi-megabyte files don't download speculatively; modern formats (AVIF/WebP) via `<picture>` with fallback; compress and strip metadata from images and SVGs; `data:` URIs only for tiny assets (they can't be cached independently).
5. **Resource hints in `<head>`** when assets come from another origin: `<link rel="preconnect" href="https://media.example.com">` (or `dns-prefetch` as the cheaper hint), `<link rel="preload">` only for late-discovered critical assets. A fully self-contained page needs none — fewer domains means fewer DNS lookups; that's the ideal.
6. **Head order matters:** `<meta charset>` within the first 1024 bytes, `<title>` early. Nothing render-blocking follows because there is nothing to block on.
7. **Chunk long content across pages.** One 5000-node DOM beats nothing; five linked 1000-node documents beat it — browsers render semantic chunks (`header`, first sections) progressively as they stream in, so put the primary content early in source order.
8. **Weight is markup's job too:** no dead wrappers, no redundant attributes, comments trimmed for production. Compression (gzip/brotli), caching headers, and CDN placement are the server's half of the contract.
9. **Fewer embeds:** every `<iframe>` is a nested browsing context with its own fetch cost — use them sparingly and always `loading="lazy"` + `title`.

## Self-Review Checklist (run before delivering any page)

- [ ] Validates conceptually against the WHATWG content model (paste-check at https://validator.w3.org/nu/ if tooling available).
- [ ] Zero `<style>`, `style=""`, stylesheets, classes, deprecated elements, presentational attributes.
- [ ] Count your `<div>`s and `<span>`s — each one individually justified or eliminated.
- [ ] Exactly one `<h1>`, one `<main>`, logical heading outline, skip link present.
- [ ] Every landmark labeled when duplicated; every image has `alt`; every control has a label; every table has `caption` + `scope`.
- [ ] Every date/time in `<time>`, every abbreviation in `<abbr>`, every work title in `<cite>`, every code token in `<code>`.
- [ ] `lang` + `dir` correct at root and at every language switch; `<bdi>` around untrusted inline text.
- [ ] `<title>`, meta description, canonical, hreflang set, JSON-LD present and matching visible content.
- [ ] Every `<img>`/`<video>`/`<iframe>` has intrinsic `width`/`height`; below-fold media is `loading="lazy"`; the hero image is not.
- [ ] All content sits inside a landmark; exactly one body-level `<header>` and `<footer>`; no `<base>`.
- [ ] Read the document top-to-bottom with styles off (that's how it ships): the raw order and structure alone must tell the whole story.

## Reference Set

**Primary (MDN):**
- Elements: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements
- Global attributes: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Global_attributes
- All attributes: https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Attributes
- Content categories: https://developer.mozilla.org/en-US/docs/Web/HTML/Guides/Content_categories
- Semantic HTML curriculum: https://developer.mozilla.org/en-US/curriculum/core/semantic-html/
- HTML guides: https://developer.mozilla.org/en-US/docs/Web/HTML/Guides
- Fast-loading pages: https://developer.mozilla.org/en-US/docs/Web/HTML/How_to/Author_fast-loading_HTML_pages

**Specs & validation:**
- WHATWG HTML Living Standard (the source of truth): https://html.spec.whatwg.org/multipage/
- Nu HTML Checker (validate every deliverable): https://validator.w3.org/nu/
- Baseline / browser support status: https://webstatus.dev/ and https://caniuse.com/

**Accessibility:**
- ARIA Authoring Practices Guide (patterns, when ARIA is warranted): https://www.w3.org/WAI/ARIA/apg/
- W3C "Using ARIA" (the five rules of ARIA): https://www.w3.org/TR/using-aria/
- WebAIM (practical screen-reader-tested guidance): https://webaim.org/

**Structured data & head:**
- Schema.org vocabulary: https://schema.org/
- Google structured-data reference (what actually earns rich results): https://developers.google.com/search/docs/appearance/structured-data
- htmlhead.dev (everything that can go in `<head>`): https://htmlhead.dev/

**Craft:**
- web.dev Learn HTML: https://web.dev/learn/html
- HTMHell (real-world anti-patterns, annotated): https://www.htmhell.dev/
