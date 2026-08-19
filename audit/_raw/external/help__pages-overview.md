# Audit: Pages Overview (help.qrtub.com/help/pages-overview)

- HTML URL: https://help.qrtub.com/help/pages-overview
- Markdown URL: https://help.qrtub.com/help/pages-overview.md
- llms.txt description: "What a Page is, when a Link should open one, and how Destinations fit together"
- Audited: 2026-08-19

## Summary table

| Metric                              | Value                                                                 |
| ------------------------------------ | ---------------------------------------------------------------------|
| HTTP status (both variants)          | 200                                                                   |
| Content-Type (Accept: text/markdown) | `text/markdown; charset=utf-8`                                       |
| Content-Type (default Accept)        | `text/html; charset=utf-8`                                            |
| X-Robots-Tag (markdown variant)      | `noindex, nofollow` — present only on the markdown response          |
| X-Robots-Tag (HTML variant)          | absent (not sent)                                                     |
| Content-Length, HTML                 | 220,243 bytes                                                         |
| Content-Length, Markdown             | 2,570 bytes                                                           |
| HTML : Markdown byte ratio           | **85.7×** (220243 / 2570)                                             |
| Markdown length                      | 2,560 characters                                                      |
| Estimated tokens (chars/4)           | **~640 tokens**                                                       |
| Boilerplate share of page tokens     | **~12.7%** (~81 of ~640 tokens; see below)                            |

Both the `Accept: text/markdown`-negotiated response on the HTML URL and the direct `.md` URL
return identical bodies (2570 bytes) and both carry `x-robots-tag: noindex, nofollow`. The
plain HTML document at the same path does **not** carry an `X-Robots-Tag` header at all, so
crawlers are told not to index/follow the clean markdown twin of a page that otherwise appears
normally indexable in HTML form. This is presumably deliberate (avoid duplicate-content
indexing against the canonical HTML page), but it means any indexer/agent that respects
`X-Robots-Tag` will not index the lightweight markdown version — it would have to fall back to
parsing the 85×-heavier HTML, or ignore the header (most LLM retrieval fetchers do ignore it).

## Heading structure (markdown, in document order)

```
H1  Pages Overview
H2  What is a Page?
H2  Page Mode vs Direct Mode
H2  Key Benefits
H2  Creating a Page
H2  Related
```

No H3s exist in the body content. Verified against the rendered HTML (`grep -o '<h[1-3]'`):
the same five H2 ids (`what-is-a-page`, `page-mode-vs-direct-mode`, `key-benefits`,
`creating-a-page`, `related`) appear on identical anchors in the HTML — the markdown export is
structurally faithful to the HTML, no headings are dropped or reordered by the conversion.

Note: inside "Key Benefits," the three sub-items ("One Code, Every System," "Audience
Routing," "Professional Presentation") are rendered as **bold paragraph text**, not as H3/H4
markdown headings, in both the HTML and the markdown. A heading-based chunker that only splits
on `#`/`##` will keep all three benefit blurbs fused into one "Key Benefits" chunk — probably
fine here since the whole section is short (~50 words), but worth knowing if a downstream
indexer expects every visual sub-heading to be a real heading level.

## Boilerplate identified

Two blocks are site-wide boilerplate injected on every page, not page-specific content:

**1. Top-of-file "agent discovery" callout (before the real H1), 175 chars / ~44 tokens:**

```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

Confirmed site-wide: this exact block also opens `https://help.qrtub.com/help/key-concepts.md`
verbatim. In the HTML it is a screen-reader-only, `aria-hidden="true"` element
(`<blockquote class="sr-only" data-agent-docs-index="true" aria-hidden="true">`) — invisible to
human visitors, deliberately exposed to markdown/text scrapers. Structural quirk: it contains
an H2 ("## Documentation Index") that sits **before** the page's own H1 ("# Pages Overview") in
document order, so a naive/heading-hierarchy-aware chunker could see an H2 outrank an H1 that
follows it.

**2. Footer CTA + contact block (after the `***` rule), 149 chars / ~37 tokens:**

```
**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

Also confirmed verbatim-identical on `key-concepts.md`'s footer. Present in the HTML too
(same anchor text/href), so it's a real, visible marketing CTA repeated at the bottom of every
help article, not a markdown-only artifact.

**Combined: 324 of 2,560 characters ≈ 81 of ~640 tokens ≈ 12.7% of this page's total token
budget is generic, repeated-on-every-page boilerplate** (agent-discovery pointer + pricing
CTA/contact footer), unrelated to what a Page/Destination/Link is.

The "## Related" section (2 links: Key Concepts, Conditional Visibility (Advanced)) is a
recurring *pattern* across pages but its content is page-specific (a curated, relevant set of
next links) rather than reused text, so it is not counted as boilerplate here — it's useful
signal, not filler.

## Retrieval-quality notes

- **Self-contained content:** Yes. The prose explains Page vs Direct Mode, lists concrete
  benefits, and gives a 4-step creation workflow without depending on images. Confirmed via
  HTML diff that the only `<img>` tags on the page are the site nav logos (light/dark theme
  variants) — there are no in-content screenshots or diagrams whose meaning would be lost by
  reading the markdown instead of the HTML. No `<video>`/`<iframe>` embeds either.
- **Terminology accuracy against GLOSSARY.md/BRAND.md:** Good. "Page," "Link," "Destination,"
  "Tub," "Item," "Direct Mode," "Page Mode" are all used exactly per the canonical glossary.
  The one legacy-UI term ("Show a profile page") is explicitly flagged in-text as "in the
  current app" rather than presented as the product's real name — this matches
  GLOSSARY.md's "Not Yet Aligned in the Product" note that the app UI still says "profile
  page" while docs should say "Page." The doc gets this distinction right instead of just
  parroting the stale UI label. "Mitti (formerly SafetyCulture)" is used only as an example
  Destination target, consistent with BRAND.md's "IS NOT: Inspection software... links to
  inspection tools like SafetyCulture, doesn't provide inspection features."
- **Discoverability gap:** The llms.txt entry immediately after this page is "Building a
  Page" ("Use the Page Editor to lay out sections, bind Item data, and theme the page people
  see when they scan") — the natural next step after learning what a Page *is*. But this
  page's own "## Related" list only links to Key Concepts and Conditional Visibility
  (Advanced), not to Building a Page. An agent answering "how do I add Destinations to my
  Page" from this article alone has no in-page pointer to the how-to article; it would have
  to already know to consult llms.txt separately.
- **Markdown well-formed:** The table (Page Mode vs Direct Mode), the multi-line numbered
  list item under "Creating a Page" (continuation lines correctly indented 3 spaces so they
  stay inside list item 1), and the bullet lists all parse cleanly — no broken table pipes,
  no orphaned list markers, no stray HTML leaking into the `.md` body.
- **`***` thematic break:** Renders as an `<hr>` in HTML before the footer CTA; a plain-text
  consumer that doesn't render markdown will just see a literal `***` line as a low-value
  separator glyph — trivial, but adds a few characters of noise if an agent surfaces the raw
  file to a user.
- **Caching/negotiation:** Both `.md` fetch paths (`Accept: text/markdown` on the HTML URL,
  and the literal `.md` URL) return byte-identical bodies and headers (aside from
  `x-vercel-cache: MISS` vs `HIT` and `cache-control` differing between the two request
  forms) — content negotiation is implemented correctly and consistently.
