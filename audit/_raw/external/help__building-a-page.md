# Retrieval Audit — "Building a Page"

- HTML URL: https://help.qrtub.com/help/building-a-page
- Markdown URL: https://help.qrtub.com/help/building-a-page.md
- llms.txt description: "Use the Page Editor to lay out sections, bind Item data, and theme the page people see when they scan"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
| --- | --- |
| HTTP status (Accept: text/markdown) | 200 |
| Content-Type (markdown request) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (markdown request) | `noindex, nofollow` — present |
| HTTP status (default Accept: text/html) | 200 |
| Content-Type (default request) | `text/html; charset=utf-8` |
| X-Robots-Tag (HTML request) | absent (no header sent; HTML has `<link rel="canonical">` instead) |
| Content-Length — HTML | 256,895 bytes |
| Content-Length — Markdown | 7,047 bytes |
| HTML : Markdown byte ratio | **36.45 : 1** |
| Markdown char count (UTF-8 aware) | 7,029 |
| Estimated tokens (chars/4) | **~1,757** (≈1,762 using raw byte count) |
| Word count | 1,062 |

Both routes to the markdown body agree exactly: `curl -H "Accept: text/markdown" .../building-a-page` and `curl .../building-a-page.md` both return 7,047 bytes, identical `text/markdown; charset=utf-8` content type, and identical `x-robots-tag: noindex, nofollow`. The plain HTML request (no special Accept header) returns no `x-robots-tag` at all and instead carries `<link rel="canonical" href="https://help.qrtub.com/help/building-a-page"/>` in the document head.

Full header dumps (for reference):

```
# Accept: text/markdown
HTTP/2 200
content-type: text/markdown; charset=utf-8
content-disposition: inline
x-robots-tag: noindex, nofollow
x-llms-txt: /llms.txt
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt",
      </.well-known/api-catalog>; rel="api-catalog",
      </.well-known/mcp/server-card.json>; rel="mcp-server-card",
      </.well-known/agent-card.json>; rel="agent-card",
      </.well-known/agent-skills/index.json>; rel="agent-skills"
content-length: 7047

# default (HTML)
HTTP/2 200
content-type: text/html; charset=utf-8
etag: "kwl0a26ko25i5t"
x-nextjs-prerender: 1
content-length: 256895
(no x-robots-tag header)
```

## Heading structure (in order)

1. `# Building a Page` (H1)
2. `## Before you start`
3. `## Opening the editor`
4. `## The layout`
5. `## Adding and arranging sections`
6. `## Section types`
7. `## Putting Item data into a section`
8. `## Showing a section only sometimes`
9. `## Previewing with real data`
10. `## Theming and layout`
11. `## Saving: the whole Tub, or one Item`
12. `## Related`

No H3s anywhere on the page — it's a flat H1 → H2 outline, which is clean for chunked retrieval (every section is a self-contained top-level unit, no ambiguous sub-heading nesting to worry about). Note there is also a synthetic `## Documentation Index` heading, but it lives inside a blockquote at the very top of the file (see Boilerplate section) and is not part of the page's real content outline — an agent building a heading-based table of contents should recognize it as injected front-matter, not the page's first section.

## Boilerplate vs. page-specific content

Three blocks are template/boilerplate rather than content specific to "Building a Page." Combined they are a modest share of the page's tokens, but worth flagging since they recur on every help article:

**1. Top-of-file "Documentation Index" nudge (lines 1–3, before the real H1):**

```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

- 175 chars ≈ 44 tokens ≈ **2.5% of the page's total tokens**.
- Confirmed generic/injected: in the HTML version this exact text is rendered inside `<blockquote class="sr-only" data-agent-docs-index="true" aria-hidden="true">` — i.e. Mintlify injects it as a screen-reader-only, visually-hidden element on every page, purely to steer crawlers/agents toward `/llms.txt`. Good for discoverability, but it is not "Building a Page" content and eats a small slice of every single page's token budget in a corpus of many pages.

**2. Bottom footer CTA (after the Related list):**

```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

- 154 chars ≈ 38.5 tokens ≈ **2.2% of the page's total tokens**.
- This is a standard marketing/support footer, visible in the human HTML too (not hidden) — appears to be a fixed block appended to every help article regardless of topic. For a single-page fetch it's harmless, but across an entire corpus ingested for RAG it's pure repeated noise (same pricing pitch, same support email, unrelated to page-editor mechanics).

**3. "Related" links list:**

```
## Related

* [Pages Overview](/help/pages-overview)
* [Using Fields](/help/using-fields)
* [Conditional Visibility](/help/conditional-visibility)
* [Custom Fields](/help/custom-fields)
```

- 185 chars ≈ 46 tokens ≈ **2.6% of the page's total tokens**.
- This one is a judgment call: the *pattern* ("## Related" + bullet list of links) is a standard structural block repeated across the docs site, but the actual four links are specific to this page's topic and plausibly useful for an agent doing multi-hop retrieval (e.g. "how do I use fields in bindings?" → follow to Using Fields). I would not call this pure boilerplate — it is templated in form but genuinely page-relevant in content.

**Combined:** the two genuinely generic blocks (Documentation Index nudge + footer CTA) together are **~4.7%** of the page's ~1,757 estimated tokens. If the Related list is also counted as templated overhead, the combined figure is **~7.3%**. Neither is large in isolation, but the top+bottom pair is identical boilerplate repeated verbatim on every help page in the corpus — for an agent doing retrieval across many pages, that's several dozen wasted tokens per page, every page, forever.

## Other retrieval-quality notes

- **Self-containment / relative links:** all four "Related" links and the one inline cross-reference link (`[Pages Overview](/help/pages-overview)` under "Before you start") are root-relative paths with no domain (`/help/pages-overview`, not `https://help.qrtub.com/help/pages-overview`). If an agent has only this single markdown document in context (e.g., a chunk returned by a vector search with no surrounding page metadata), it cannot resolve these into working URLs without independently knowing the site's base domain. This is a common pattern and not unique to this page, but it does mean the page is not fully self-contained as a standalone document — link-following requires out-of-band knowledge of `https://help.qrtub.com`.
- **X-Robots-Tag mismatch is worth flagging even though it's likely intentional:** the machine-readable variant (`text/markdown`, both via content negotiation and the literal `.md` URL) is marked `noindex, nofollow`, while the human HTML page is indexable and canonical. This is standard practice to avoid duplicate-content indexing of the same content at two URLs/representations, so it's not a bug — but it does mean any AI crawler that respects `X-Robots-Tag` while building a search index (as opposed to fetching a specific URL on a user's request) will never index the lightweight markdown copy directly, only the 36x-heavier HTML. Systems that do on-demand fetch-and-summarize (typical RAG/agent browsing) are unaffected since they request the specific URL rather than crawling a link graph.
- **No malformed markdown found.** Both GFM tables (left-panel tabs; section-type categories) have consistent column counts and valid header/separator rows. The two fenced code blocks (binding syntax, CEL condition example) have no language tag but are otherwise well-formed — a minor missed opportunity (e.g. ` ```txt ` or a CEL-specific hint) rather than a defect. The horizontal rule (`***`) before the footer CTA renders correctly as a block boundary.
- **Internal factual consistency check passed:** the page claims "Seventeen sections are available" and then lists exactly 17 across the five categories (7 + 3 + 4 + 2 + 1) — this is the kind of claim an agent might be asked to verify/quote, and it checks out.
- **Terminology matches the canonical glossary** (`/workspace/qrtub/GLOSSARY.md`): uses "Item," "Tub," "Destination," "Page Editor," "Page" correctly throughout. The two bolded UI-label references — "**Profile page** tab" and "**Edit profile page**" button — quote the *current app's actual UI text* rather than misusing "Profile Page" as a product term; this matches GLOSSARY.md's "Not Yet Aligned in the Product" note that the App UI still says "Profile page" pending a rename. So the page is accurately describing today's UI while keeping prose terminology (Page, Page Editor) aligned with the glossary — not an error, but worth knowing if some other page in the corpus quotes stale UI text without that same care.
- **No overclaiming vs. BRAND.md feature-status table.** Nothing here claims Planned features (API, advanced Media lifecycle, etc.) as available; the conditional-visibility (CEL) references match BRAND.md's "Available (advanced)" status.
- **Good agent-discovery infrastructure at the HTTP layer:** both HTML and markdown responses carry `Link` headers advertising `/llms.txt`, `/llms-full.txt`, `/.well-known/api-catalog`, `/.well-known/mcp/server-card.json`, `/.well-known/agent-card.json`, and `/.well-known/agent-skills/index.json`, plus an `x-llms-txt` header. This is site-wide infrastructure, not page-specific, but it's a meaningfully positive signal for agent retrieval that a per-page audit should note.
- **Content-to-boilerplate signal-to-noise is otherwise good.** Excluding the ~4.7–7.3% templated portion, the remaining ~93–95% of the page is dense, specific, non-marketing instructional content (concrete UI navigation paths, a worked binding-syntax example, an explicit Override ON/OFF saving-semantics explanation) — exactly the kind of material a support chatbot or coding agent would want verbatim.
