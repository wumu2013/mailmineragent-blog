# SEO/GEO Shortcode Guide

> Project-level shortcode suite for MailMiner Agent Blog's SEO and GEO (Generative Engine Optimization) workflows. Replaces the "copy-paste" patterns documented in `POST-OPTIMIZATION-GUIDE.md` §2.4 (TL;DR) and §3.2 (E-E-A-T) with typed, validated, theme-isolated building blocks.
>
> **Authored:** 2026-06-11 · **Status:** Stable · **Backs the build of:** all `content/posts/*.md` (zero migration required for existing posts).

---

## Table of Contents

- [0. Overview](#0-overview)
- [1. `tldr` — TL;DR summary block](#1-tldr--tl-dr-summary-block)
- [2. `faq` — FAQ HTML + JSON-LD](#2-faq--faq-html--json-ld)
- [3. `eeat` — About + Past coverage + Disclosure + CTA](#3-eeat--about--past-coverage--disclosure--cta)
- [4. `schema` — Inline JSON-LD for any Schema.org type](#4-schema--inline-json-ld-for-any-schemaorg-type)
- [5. Conventions & gotchas](#5-conventions--gotchas)
- [6. JSON-LD emission priority order](#6-json-ld-emission-priority-order)
- [7. Migration playbook (frontmatter → shortcode)](#7-migration-playbook-frontmatter--shortcode)
- [8. Adding a new shortcode](#8-adding-a-new-shortcode)

---

## 0. Overview

Four shortcodes live in `layouts/_shortcodes/`:

| Shortcode | Visible HTML? | Emits JSON-LD? | Source of truth |
|---|---|---|---|
| `tldr`    | ✅ | — | Inline body |
| `faq`     | ✅ | ✅ (paired with extend_head) | Inline params OR `Params.faq` |
| `eeat`    | ✅ | — | Inline params |
| `schema`  | — | ✅ | Inline params (typed) |

**Two JSON-LD emission paths exist** (deliberately complementary, not duplicated):

1. **Auto path** — `layouts/_partials/extend_head.html` reads `.Params.faq` (frontmatter) OR `page.Scratch.Get "faq_inline"` (set by `{{< faq >}}` in inline mode). No shortcode call required for the head to receive JSON-LD when the frontmatter form is used.
2. **Explicit path** — `{{< schema >}}` shortcode emits a `<script type=application/ld+json>` block at the call site. Use it for any Schema.org type the auto path doesn't cover (Article, HowTo, BreadcrumbList, Person, Organization, Product).

A shared partial — `layouts/_partials/seo/schema_jsonld.html` — is the single source of truth for JSON-LD byte formatting. Both paths call it. **Always byte-stable** under `hugo --minify`.

**Theme isolation:** every shortcode and the shared partial live under `layouts/`. The theme directory `themes/PaperMod/` is not modified — PaperMod upgrades will not clobber this work.

---

## 1. `tldr` — TL;DR summary block

Wraps a markdown body in a semantic `<aside role="note">` with a leading "TL;DR" label. Markdown inside the body is processed via `markdownify` (supports `**bold**`, `*italic*`, `code`, `[link](url)`).

### Usage

```
{{< tldr >}}
A regular employee with zero e-commerce background found a hyper-specific
niche — custom keyboard risers for women with long nail art. By modifying
one dimension (height), they built a steady **30–50 orders per day** with
**$0 ad spend**.
{{< /tldr >}}
```

### Output

```html
<aside class="tldr" role="note">
  <p><strong>TL;DR</strong> A regular employee with zero e-commerce background found a hyper-specific niche — custom keyboard risers for women with long nail art. By modifying one dimension (height), they built a steady <strong>30–50 orders per day</strong> with <strong>$0 ad spend</strong>.</p>
</aside>
```

### When to use

- Every post on the blog (per `POST-OPTIMIZATION-GUIDE.md` §2.4, the TL;DR blockquote is the first non-frontmatter element on the page).
- The blockquote `> **TL;DR** ...` form in raw markdown still works; the shortcode is preferred when the author wants semantic markup (`<aside>` + `role="note"`) for assistive tech and screen readers.

### Notes

- The shortcode consumes `.Inner`, so it MUST be paired (`{{< tldr >}}...{{< /tldr >}}`). Self-closing form `{{< tldr />}}` will fail with "does not evaluate .Inner".
- The leading `**TL;DR**` label is added by the shortcode — do not put `TL;DR` at the start of the body, it will be double-rendered.

---

## 2. `faq` — FAQ HTML + JSON-LD

Renders a `<section class="faq">` block with full Schema.org microdata, AND (when used in inline mode) populates `page.Scratch.Get "faq_inline"` so `extend_head.html` emits the JSON-LD `<script>` block in `<head>`.

### Two modes

#### Mode A — Frontmatter-driven (default, back-compat with existing posts)

```markdown
---
faq:
  - q: "Is a keyboard riser a good niche product?"
    a: "Yes — a custom keyboard riser targeting nail-art typists reached 30-50 orders per day with $0 ad spend."
  - q: "How much profit can a keyboard riser make per month?"
    a: "$4,000-$8,000 per month at 30-50 orders per day, $15-$20 AOV, and 30-40% gross margin."
---

{{< faq />}}
```

- The visible HTML block renders the Q&A list from `.Params.faq`.
- `extend_head.html` auto path emits JSON-LD from the same frontmatter field.
- The `{{< faq />}}` shortcode call is a "render trigger" — the shortcode template is also paired (consumes `.Inner`), so use either form (self-closing here is the cleanest).

#### Mode B — Inline-driven (new posts, no frontmatter coupling)

```markdown
{{< faq
    questions="Is a keyboard riser a good niche product?|How much profit can it make?"
    answers="Yes — ...|$4,000-$8,000 per month."
/>}}
```

- Q&A strings are pipe-separated (`|`).
- The shortcode writes the same data to `page.Scratch.Set "faq_inline"`, which `extend_head.html` reads to emit JSON-LD.
- No `faq:` frontmatter field required.

### Optional params

| Param     | Default                       | Purpose |
|-----------|-------------------------------|---------|
| `heading` | `"Frequently Asked Questions"`| Override the `<h2>` text |

### Hybrid mode

If a page has BOTH `faq:` frontmatter AND inline params, the inline params win for **visible HTML** (explicit > implicit) while the frontmatter wins for **JSON-LD** (per the priority order in §6 below). The visible HTML and the JSON-LD will then describe slightly different Q&As — this is almost always a bug. If you migrate a post from mode A to mode B, **delete the `faq:` frontmatter block**.

### Visible output (both modes)

```html
<section class="faq" itemscope itemtype="https://schema.org/FAQPage">
  <h2>Frequently Asked Questions</h2>
  <div itemscope itemprop="mainEntity" itemtype="https://schema.org/Question">
    <h3 itemprop="name">Is a keyboard riser a good niche product?</h3>
    <div itemscope itemprop="acceptedAnswer" itemtype="https://schema.org/Answer">
      <p itemprop="text">Yes — a custom keyboard riser ...</p>
    </div>
  </div>
</section>
```

### JSON-LD output (auto path, both modes)

```html
<script type=application/ld+json>{"@context":"https://schema.org","@type":"FAQPage","mainEntity":[{"@type":"Question","name":"Is a keyboard riser a good niche product?","acceptedAnswer":{"@type":"Answer","text":"Yes — a custom keyboard riser ..."}}]}</script>
```

Microdata is rendered alongside the JSON-LD — Google reads both. The microdata is a fallback for crawlers that strip `<script>`.

---

## 3. `eeat` — About + Past coverage + Disclosure + CTA

Implements the "About the MailMiner Editorial Team" 3-paragraph E-E-A-T block from `POST-OPTIMIZATION-GUIDE.md` §3.2. Replaces the manual "copy-paste this 25-line template" workflow with a typed, validated, link-checking shortcode.

### Usage

```markdown
{{< eeat
    focus="TikTok Shop organic commerce, niche product selection, 1688 sourcing, and AI tooling for e-commerce operators"
    past="/posts/from-tiktok-to-shopify-spanish-ecommerce/|Spanish TikTok-to-Shopify||/posts/amazon-refined-selection-90-percent-success-framework/|Amazon refined-selection 90% framework"
    disclosure="Revenue figures ($450-$1,000/day, $4K-$8K/month) and operational details (30-50 orders/day) are reported by the seller, not independently audited. Margin estimates assume a 30-40% gross margin, typical for this category but variable by supplier and shipping terms."
    cta="Found a similar niche or have questions about the niche-within-a-niche framework?"
/>}}
```

### Param reference

| Param | Required | Format | Notes |
|---|---|---|---|
| `focus` | Yes (or one of the others) | plain text | Sentence describing the team's focus areas. Rendered as "Our focus areas include ... ." |
| `past` | No | `path\|title\|\|path\|title\|\|...` | Pipe-separated `path\|title` pairs, joined with `\|\|`. Each path is validated against the site page index; typos emit a `WARN` at build time, not a 404 in production. |
| `disclosure` | No | plain text | Rendered with a leading `<strong>Disclosure:</strong>` label. |
| `cta` | No | plain text | Defaults to "Found a similar niche or have questions about this framework?". Wrapped in a `<blockquote>` and linked to `/about/`. |

### Why `||` as the row separator?

Hugo shortcode parameters are **single-line** — they cannot span newlines. So multi-row inputs (multiple past-coverage links, multiple breadcrumb items) must use an inline separator. `||` was chosen because it never appears in real URL paths or human-readable titles, and the underscore-separated underscore-form keeps the parameter readable in source.

### Output (visible HTML only — no JSON-LD)

```html
<hr>
<h2>About the MailMiner Editorial Team</h2>
<p>The MailMiner Editorial Team is a group of cross-border e-commerce operators, TikTok Shop sellers, and AI tooling builders. We publish case studies drawn from real seller interviews and our own product experiments — never generic theory, never fabricated case studies.</p>
<p><strong>Our focus areas</strong> include TikTok Shop organic commerce, niche product selection, 1688 sourcing, and AI tooling for e-commerce operators. Past coverage includes a <a href="/posts/from-tiktok-to-shopify-spanish-ecommerce/">Spanish TikTok-to-Shopify</a>, and the <a href="/posts/amazon-refined-selection-90-percent-success-framework/">Amazon refined-selection 90% framework</a>.</p>
<p><strong>Disclosure:</strong> Revenue figures ($450-$1,000/day, $4K-$8K/month) and operational details (30-50 orders/day) are reported by the seller, not independently audited. Margin estimates assume a 30-40% gross margin, typical for this category but variable by supplier and shipping terms.</p>
<blockquote>
  <p>Found a similar niche or have questions about the niche-within-a-niche framework? Reach out via the <a href="/about/">About page</a> — we read every message.</p>
</blockquote>
```

### Link validation

If any `path` in the `past` param does not resolve to a regular page in the site index, the build emits:

```
WARN  shortcode/eeat: past-coverage path "/posts/typo-here/" does not resolve to any page in site index (page=/posts/this-post/). Possible typo or unpublished target.
```

The shortcode does NOT fail the build — the link is still rendered. The WARN is your signal to fix the typo before committing.

---

## 4. `schema` — Inline JSON-LD for any Schema.org type

Emit any of the supported Schema.org types as a `<script type=application/ld+json>` block at the call site. Use this for JSON-LD that `extend_head.html` doesn't auto-emit, or for cases where you want the JSON-LD adjacent to the relevant content in the page body (less common; head injection is usually preferred).

### Supported types

`FAQPage`, `Article`, `HowTo`, `BreadcrumbList`, `Person`, `Organization`, `Product`.

Unknown types **fail the build** with `errorf`. This is intentional — typos in JSON-LD pollute Google's Rich Results. Catch them at compile time.

### Examples

#### `Article` (drop into a post to override the auto-emitted BlogPosting with a more specific type)

```markdown
{{< schema type="Article"
    headline="Kitchen Supply Wholesale in Laos: A $280K Vientiane Playbook"
    author="MailMiner Editorial Team"
    datePublished="2026-05-29"
    description="Pan built a $280K, 1,200 sqm kitchen supply warehouse in Vientiane, Laos."
/>}}
```

#### `BreadcrumbList` (override the auto-emitted one with a custom path)

```markdown
{{< schema type="BreadcrumbList"
    items="Home|/|Posts|/posts/|Kitchen supply in Laos|/posts/kitchen-supply-wholesale-laos-sichuan-entrepreneur/"
/>}}
```

#### `HowTo` (for playbooks and step-by-step guides)

```markdown
{{< schema type="HowTo"
    name="How to find a niche-within-a-niche product"
    description="Five steps to identify and validate a hyper-specific e-commerce niche."
    steps="Identify a hyper-specific pain point|Check whether standard products solve it|Modify one dimension of an existing product|Demonstrate the before-and-after visually|Let the algorithm match your content to the right audience"
/>}}
```

#### `FAQPage` (alternative to `{{< faq >}}` if you want only the JSON-LD without the visible HTML block)

```markdown
{{< schema type="FAQPage"
    questions="Is this a separate JSON-LD block?|When would I use it alone?"
    answers="Yes. Use one or the other, not both.|Use it when you only want the head-injected JSON-LD without rendering the visible FAQ section."
/>}}
```

### Param reference

| Type | Required params | Optional params |
|---|---|---|
| `FAQPage` | `questions`, `answers` (both `\|`-separated) | — |
| `Article` | — | `headline`, `author`, `datePublished`, `dateModified`, `image`, `description` (each defaults to a page-level value) |
| `HowTo` | `steps` (`\|`-separated) | `name`, `description` |
| `BreadcrumbList` | `items` (`Name\|Path\|\|Name\|Path\|\|...`) | — |
| `Person` | `name` | `url`, `role` |
| `Organization` | `name` | `url`, `logo` |
| `Product` | `name` | `description`, `image`, `sku` |

### Multi-row params

Same rule as `eeat`: use `||` to separate rows inside a single-line parameter. Use `|` to separate fields inside a row. Example: `items="Home|/|Posts|/posts/||Post|/posts/post-slug/"` represents three rows: `("Home","/")`, `("Posts","/posts/")`, `("Post","/posts/post-slug/")`.

---

## 5. Conventions & gotchas

### Self-closing form

Three of the four shortcodes (`faq`, `eeat`, `schema`) consume `.Inner` (so the template gets paired-shortcode parsing, which is the only way Hugo's parser will accept a self-closing `{{< name arg="x" />}}` form). Use self-closing form in source for readability:

```markdown
{{< eeat focus="..." past="..." disclosure="..." cta="..." />}}
```

`{{< tldr >}}` does NOT consume `.Inner` only because it is the only shortcode whose body IS the payload. It must be written as paired:

```markdown
{{< tldr >}}body text{{< /tldr >}}
```

### Quoted strings

Shortcode parameter values can be single- or double-quoted. Prefer **double quotes** for consistency with Hugo's YAML frontmatter style. If the value contains a double quote, escape it with `\"` — but in practice, you should not need to put quotes inside shortcode param values (use plain prose).

### Pipe character escaping

The pipe `|` is the field separator inside multi-item params. If your content legitimately contains a pipe (e.g., a code snippet like `a | b`), DO NOT pass it through a shortcode — the parser will split it. Put the content in a code block or a regular markdown paragraph instead.

### Site base URL

The `BreadcrumbList` type emits absolute URLs by prepending `$.Site.BaseURL` to each path. Make sure `config.toml`'s `baseURL` is correct (currently `https://mailmineragent.com`).

---

## 6. JSON-LD emission priority order

`extend_head.html` consults sources in this order. The first one with data wins:

1. **`.Params.faq`** (frontmatter) — wins if non-empty.
2. **`page.Scratch.Get "faq_inline"`** (set by `{{< faq >}}` in inline mode).
3. None → no FAQPage JSON-LD emitted.

This ordering means:

- Existing posts with `faq:` frontmatter need NO migration — the auto path keeps working byte-identically.
- Posts that want shortcode-only authoring should delete their `faq:` frontmatter when adding `{{< faq >}}` inline params.
- Mixed form (frontmatter + inline) is **not supported** — the JSON-LD will come from frontmatter (possibly stale), and the visible HTML from the inline params. The author should resolve the duplication by deleting one or the other.

The `{{< schema >}}` shortcode does NOT participate in this priority order — it always emits at its call site, independently of `extend_head.html`. This is by design: you can use `{{< schema type="FAQPage" >}}` for a headless-JSON-LD-only injection on a page that already has a visible FAQ but no frontmatter data.

---

## 7. Migration playbook (frontmatter → shortcode)

To convert an existing post from frontmatter form to shortcode form:

1. **Backup:** record the current JSON-LD output:
   ```bash
   cp public/posts/SLUG/index.html /tmp/before.html
   ```

2. **Edit the post:**
   - Remove the `faq:` block from frontmatter.
   - Replace the `## FAQ: ...` markdown section with a `{{< faq questions="..." answers="..." />}}` call.
   - Replace the trailing `## About the MailMiner Editorial Team` markdown section with a `{{< eeat ... />}}` call.

3. **Rebuild and diff:**
   ```bash
   rm -f .hugo_build.lock && hugo --gc --minify
   python3 -c "
   import re
   a = open('/tmp/before.html').read()
   b = open('public/posts/SLUG/index.html').read()
   a_ld = re.findall(r'<script type=application/ld\+json>(.*?)</script>', a, re.DOTALL)
   b_ld = re.findall(r'<script type=application/ld\+json>(.*?)</script>', b, re.DOTALL)
   # Diff block 0 (FAQPage)
   print('EQUAL' if a_ld[0] == b_ld[0] else f'DIFF:\n--- before ---\n{a_ld[0][:200]}\n+++ after +++\n{b_ld[0][:200]}')
   "
   ```

4. **Validate in Google Search Console / Rich Results Test.**

For most posts, the diff is *not* byte-identical (because microdata attributes and the visible HTML container change), but the JSON-LD content is identical and the visible Q&A is identical.

---

## 8. Adding a new shortcode

Three files to touch:

1. `layouts/_shortcodes/NAME.html` — the template. **Always** add `{{- $_ := .Inner -}}` at the top so the template is paired (unless it actually uses `.Inner` for the body, like `tldr`).
2. `docs/SHORTCODE-GUIDE.md` — usage docs.
3. (Optional) `layouts/_partials/seo/schema_jsonld.html` — if the new shortcode emits JSON-LD, add a new branch in the type map.

Build-time validation policy for new shortcodes:

- **Unknown types** → `errorf` (catches typos in production JSON-LD).
- **Mismatched param counts** → `errorf`.
- **Unresolvable internal links** → `warnf` (don't fail builds over typos; the link is still rendered).
- **Empty / missing data** → `warnf` (visible HTML degrades to nothing; operator can fix).

This policy keeps the build honest: structural errors fail, content typos warn.

---

## Appendix A: File map

```
mailmineragent-blog/
├── layouts/
│   ├── _shortcodes/                  # NEW — SEO/GEO building blocks
│   │   ├── tldr.html
│   │   ├── faq.html
│   │   ├── eeat.html
│   │   └── schema.html
│   ├── _partials/
│   │   ├── extend_head.html          # REFACTORED — delegates to schema_jsonld.html
│   │   ├── seo/                      # NEW
│   │   │   └── schema_jsonld.html    # Single source of JSON-LD formatting
│   │   ├── author.html
│   │   ├── author_link.html
│   │   └── post_meta.html
│   ├── _default/home.llms.txt
│   └── partials/extend_footer.html
├── content/posts/                     # 0 changes
│   └── shortcode-smoke-test.md        # NEW — draft: true smoke test harness
└── docs/
    ├── SHORTCODE-GUIDE.md             # NEW — this file
    ├── POST-OPTIMIZATION-GUIDE.md     # updated §3.2 to point here
    └── GEO-tracking.md
```

## Appendix B: Verification commands

```bash
# 1. Production build (must report 0 errors, 0 warnings)
cd /home/wumu/projects/mailmineragent-blog
rm -f .hugo_build.lock
hugo --gc --minify

# 2. Build with drafts (smoke test page)
hugo --gc --minify --buildDrafts

# 3. Diff the JSON-LD output of a known-good post (should be byte-identical
#    to the pre-refactor baseline)
python3 -c "
import re
h = open('public/posts/keyboard-riser-niche-tiktok-hustle/index.html').read()
for m in re.findall(r'<script type=application/ld\+json>(.*?)</script>', h, re.DOTALL):
    print(m[:80], '...')
"

# 4. Validate all smoke-test JSON-LD blocks with Google's Rich Results Test
#    (manual step — open https://search.google.com/test/rich-results and
#    paste the JSON-LD content from the smoke test page)
```
