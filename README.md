# Semantic HTML (2026) — Agent Skill & Field Guide

> An agent skill that produces **static, CSS-free, purely semantic HTML** exploiting the full 2026 element vocabulary — with SEO, ARIA, internationalization, and performance best practices baked in — plus a self-demonstrating demo page where every element is its own example.

![HTML5](https://img.shields.io/badge/HTML-Living_Standard-E34F26?logo=html5&logoColor=white)
[![Validate HTML](https://github.com/mikezupper/semantic-html-skill/actions/workflows/validate.yml/badge.svg)](https://github.com/mikezupper/semantic-html-skill/actions/workflows/validate.yml)
![W3C Validated](https://img.shields.io/badge/Nu_Checker-0_errors_·_0_warnings-brightgreen?logo=w3c&logoColor=white)
![CSS](https://img.shields.io/badge/CSS-none_by_design-blueviolet)
![JavaScript](https://img.shields.io/badge/JavaScript-none_(JSON--LD_only)-yellow)
![Elements](https://img.shields.io/badge/element_coverage-113%2F113-blue)
![Deprecated elements](https://img.shields.io/badge/deprecated_elements-0-critical)
![ARIA](https://img.shields.io/badge/ARIA-native--first-9cf)
![WCAG](https://img.shields.io/badge/WCAG-landmarks_·_labels_·_captions-005A9C?logo=w3c&logoColor=white)
![i18n](https://img.shields.io/badge/i18n-lang_·_dir_·_bdi_·_ruby-ff69b4)
![SEO](https://img.shields.io/badge/SEO-JSON--LD_·_OG_·_canonical-success)
![Baseline](https://img.shields.io/badge/Baseline-2026-blue)
![Skill format](https://img.shields.io/badge/format-Claude_Code_skill-D97757?logo=anthropic&logoColor=white)
![License](https://img.shields.io/badge/license-CC_BY_4.0-lightgrey)
![PRs](https://img.shields.io/badge/PRs-welcome-brightgreen)
![Maintained](https://img.shields.io/badge/maintained-yes-success)

---

## Contents

| File | What it is |
|---|---|
| [`skills.md`](skills.md) | The agent skill: prime directives, the full element vocabulary with when-to-use tables, page-architecture rules, SEO / ARIA / i18n / performance best practices, frontier-element reference, and a self-review checklist. |
| [`demo.html`](demo.html) | A self-demonstrating field guide. One CSS-free page exercising the entire vocabulary live — validates with **zero errors and zero warnings** against the [W3C Nu checker](https://validator.w3.org/nu/). |
| [`examples/`](examples/) | Eight focused showcases (see below) — each a realistic scenario built around one element cluster, completely CSS-free. All Nu-validated in CI. |
| `README.md` | You are here. |

## The website — html-tags.com

This skill powers **[html-tags.com](https://html-tags.com/)** — a reference site with a page for every living HTML element (when to use it, when not to, live examples, Baseline status), styled versions of the examples below, a printable cheat sheet, and an element-of-the-week spotlight. The site lives in its own repository: [mikezupper/html-tags-com](https://github.com/mikezupper/html-tags-com).

## Examples

Each example is a realistic page whose *subject matter* forces one family of elements into natural, idiomatic use — the opposite of a kitchen-sink demo.

**Every example is completely CSS-free** — the browser's default stylesheet is the only rendering, which is the point: the structure must stand on its own. Styled versions of these same pages (each with a unique theme and a pure-CSS styled-view toggle) live at [html-tags.com/examples](https://html-tags.com/examples/).

| Example | Scenario | Element cluster showcased |
|---|---|---|
| [`recipe.html`](examples/recipe.html) | Overnight rye bread recipe | `<time datetime="PT…">` durations, `<data>` quantities, ingredient/step lists, `<dl>` nutrition panel, Recipe JSON-LD |
| [`glossary.html`](examples/glossary.html) | Web-platform glossary | `<dl>`/`<dt>`/`<dd>`, `<dfn>` defining instances, `<abbr>`, fragment-link cross-references |
| [`terminal-manual.html`](examples/terminal-manual.html) | Unix-style man page for a fictional CLI | `<code>`, nested `<kbd>`, `<samp>`, `<var>`, `<pre>` sessions, options as description lists |
| [`conference-schedule.html`](examples/conference-schedule.html) | Two-track conference program | Complex tables: `headers`/`id` associations, `rowspan`, `<colgroup>`, `scope`, `<tfoot>`, Event JSON-LD |
| [`world-poetry.html`](examples/world-poetry.html) | Multilingual poetry anthology | `lang` switches, `dir="rtl"` Arabic, `<ruby>`/`<rt>`/`<rp>` furigana, `<bdi>`, `<bdo>`, locale-aware `<q>`, `translate="no"` |
| [`help-center.html`](examples/help-center.html) | Product support page | Script-free interactivity: `<search>`, `<details name>` exclusive accordion, declarative `popover`, `<dialog>`, `<mark>`, FAQPage JSON-LD |
| [`product-order.html`](examples/product-order.html) | Print-edition order form | `<fieldset>`/`<legend>`, typed inputs + `autocomplete`, `<datalist>`, `<optgroup>`, `<meter>`, `<progress>`, `<output for>`, schema.org **microdata** (the attribute-level alternative to JSON-LD) |
| [`release-notes.html`](examples/release-notes.html) | Software changelog | `<ins>`/`<del>` with `datetime` + `cite`, `<s>` for superseded facts, `<ol reversed>`, nested `<article>`s, `<hgroup>` version headings |

## Motivation

Most HTML written today is `<div>` soup: structure decided by how things should *look*, meaning bolted on afterwards (if ever). That trade has real costs:

- **Accessibility** — screen readers navigate by landmarks, headings, and roles. A page built from `<div>`s is a featureless plain; a semantic page is a labeled map.
- **SEO** — crawlers reconstruct topic structure from headings, `<article>` boundaries, `<time>` freshness signals, and structured data. Semantic markup *is* the ranking signal.
- **Machine readability** — LLMs, reader modes, RSS extractors, and translation tools all parse meaning from element choice. `<data>`, `<time datetime>`, and `<abbr>` are APIs for machines embedded in prose for humans.
- **Longevity** — CSS frameworks churn yearly; `<article>` has meant the same thing since 2008 and will mean it in 2040.

This skill takes the constraint to its logical extreme: **no CSS at all**. When there is no stylesheet to lean on, every structural decision must be justified by meaning alone — which is precisely the discipline that makes markup good even when you *do* add styling later. The demo page proves the point: rendered with nothing but the browser's default stylesheet, it is fully navigable, readable, interactive (accordion, popover, dialog — zero script), and translatable.

A second motivation is **coverage**. HTML has ~113 living elements; typical pages use fifteen. The neglected hundred — `<hgroup>`, `<search>`, `<dfn>`, `<samp>`, `<data>`, `<ruby>`, `<bdi>`, `<meter>`, `<output>`, `<menu>` — exist because someone needed exactly that meaning. The skill forces an agent to actively look for them instead of defaulting to the same dozen tags.

## Principles

The skill is built on six prime directives (see [`skills.md`](skills.md) for the full text):

1. **Meaning over appearance.** Never choose an element for how it renders.
2. **The element-selection ladder.** Specific element → structural element → `<div>`/`<span>` as a *defect to justify*, not a default.
3. **No presentation layer.** No `<style>`, no `style=""`, no stylesheets, no classes, no deprecated presentational elements.
4. **First rule of ARIA: don't use ARIA.** Native semantics beat retrofitted roles; ARIA fills only genuine gaps (`aria-label` on repeated navs, `aria-current`, `aria-describedby`).
5. **No deprecated elements, ever.** `<center>`, `<font>`, `<marquee>` and friends are absent because they are wrong, not because they are old.
6. **Respect the content model.** No block content in `<p>`, no interactive-inside-interactive, `<summary>` first in `<details>` — validity is a parser-speed feature, not just hygiene.

Layered on top: page-architecture rules (landmark order, the RSS-feed test for `<article>` vs `<section>`, shallow DOM), and four best-practice tracks — **SEO** (title/description discipline, canonical, JSON-LD, descriptive links), **ARIA** (the short list of attributes that are legitimate in static pages), **i18n** (`lang` on every language switch, `dir="auto"`, `<bdi>`, `translate="no"`, hreflang), and **performance** (intrinsic dimensions, `loading="lazy"` + `fetchpriority`, resource hints, content chunking — adapted from MDN's fast-loading guide).

## Using the skill

### With Claude Code

Install it as a project or personal skill:

```bash
# Project-scoped
mkdir -p .claude/skills/semantic-html
cp skills.md .claude/skills/semantic-html/SKILL.md

# Or personal (all projects)
mkdir -p ~/.claude/skills/semantic-html
cp skills.md ~/.claude/skills/semantic-html/SKILL.md
```

The frontmatter `description` triggers automatic loading whenever a task involves creating or modifying static HTML; you can also invoke it explicitly with `/semantic-html`.

### With any other agent or LLM

`skills.md` is plain Markdown with YAML frontmatter — paste it into a system prompt, attach it as context, or wire it into any framework's instruction slot. It is written as *operating instructions* ("you are building…", "forbidden:", checklists), not as an essay, so it works verbatim.

### As a human reference

The when-to-use tables (inline semantics, sectioning, tables, forms) and the self-review checklist stand alone as a code-review rubric for HTML PRs.

## How to best leverage it

**1. Generating new pages.** State the content and audience; let the skill drive structure. Good prompt shape:

> Using the semantic-html skill, build `pricing.html` for a bakery: three tiers, an FAQ, contact info, English with one French tagline.

The skill will force tiers into a properly-scoped `<table>` or `<dl>`, the FAQ into a `<details name>` exclusive accordion, contact into `<address>`, the tagline into `<span lang="fr">` — decisions you'd otherwise have to review for.

**2. Auditing existing HTML.** Paste a page and ask for a review against the skill's checklist. The div-count rule ("every `<div>`/`<span>` is a defect to justify") and the ARIA-redundancy rule surface most real-world problems quickly.

**3. Close the loop with the validator.** The skill treats validation as non-negotiable. Automate it:

```bash
curl -s -H "Content-Type: text/html; charset=utf-8" \
  --data-binary @demo.html \
  "https://validator.w3.org/nu/?out=json"
# → {"messages":[]} means clean
```

**4. Use the demo as a living cheat-sheet.** Open [`demo.html`](demo.html) in a browser and view source side by side. Every section demonstrates the element it discusses — the accordion closes its sibling because of `name="faq"`, the popover opens from a bare `popovertarget` button, the quotation marks around the German proverb come from `lang="de"`. It is the skill's claims, executable.

**5. Adding CSS later.** The no-CSS constraint is a forcing function, not a religion. Markup produced under this skill is the *ideal* styling target: landmarks and elements give you semantic selectors (`article > header`, `nav[aria-label="Primary"]`), so when you do add a stylesheet you rarely need to touch the HTML — and the page still works perfectly the day the stylesheet fails to load.

**6. Frontier elements.** `<geolocation>`, `<selectedcontent>`, and `<fencedframe>` (experimental, Chromium-first, JS-required) are documented in the skill with working code and feature-detection patterns — reference material for when a project explicitly relaxes the static constraint. The demo shows them as captioned code listings.

## What "full coverage" means

- **113 / 113** living elements accounted for: used live in the demo, shown as frontier listings, or explicitly justified as omitted (`<base>`, `<template>`, `<slot>`, `<canvas>`, `<noscript>`, `<object>`, `<embed>` — each with its reason in the demo's aside).
- **0** deprecated elements, **0** classes, **0** inline styles, **0** presentational attributes.
- The only `<script>` on the demo page is `type="application/ld+json"` — data, not behavior.

## References

The skill's full reference set (MDN element/attribute/content-category references, WHATWG Living Standard, Nu checker, ARIA Authoring Practices Guide, Using ARIA, WebAIM, Schema.org, Google structured-data docs, htmlhead.dev, web.dev Learn HTML, HTMHell, MDN fast-loading guide) is maintained at the bottom of [`skills.md`](skills.md).

## License

Text and markup licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/).
