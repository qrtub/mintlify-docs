# Audit: Key Concepts

- Page: **Key Concepts**
- HTML URL: https://help.qrtub.com/help/key-concepts
- Markdown URL: https://help.qrtub.com/help/key-concepts.md
- llms.txt description: "The three-entity model, Link modes, Tubs, and the print-before-link workflow"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | `200` |
| HTTP status (`.md` URL directly) | `200` (identical response either way) |
| Content-Type (`.md`) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (`.md`) | `noindex, nofollow` |
| X-Robots-Tag (HTML) | *absent* — not sent on the HTML response |
| Content-Length (`.md`) | 10,683 bytes |
| Content-Length (HTML, default `Accept`) | 295,139 bytes |
| HTML : Markdown byte ratio | **27.6×** (295139 / 10683) |
| Markdown chars (unicode-aware) | 10,625 |
| Estimated tokens (chars/4) | **~2,656 tokens** |
| Headings | 1 × H1, 12 × H2, 9 × H3 |
| Boilerplate share of page tokens (banner + footer CTA only) | **~3.0%** (top banner ~1.65% + footer CTA ~1.40%) — much lower than short pages because the page-specific body is long |
| **Critical accuracy defect** | The "Media" section and its worked example present per-item cost/durability/installation-location/inventory/print-partner tracking as a real, current capability. **Verified against `../qrtub/src/`: none of this is tracked.** See §8. |

## 1–2. Raw header findings

**Request with `Accept: text/markdown`:**
```
HTTP/2 200
content-type: text/markdown; charset=utf-8
content-disposition: inline
content-security-policy: default-src 'none'
x-robots-tag: noindex, nofollow
x-llms-txt: /llms.txt
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
content-length: 10683
```

**Default request (HTML):**
```
HTTP/2 200
content-type: text/html; charset=utf-8
content-length: 295139
```
No `x-robots-tag` header on the HTML response — `noindex, nofollow` is emitted only on the markdown variant (confirmed identical whether reached via the `Accept: text/markdown` header or the literal `.md` URL). This is the same asymmetry seen on other pages of this site: the representation built specifically for machine consumption (and advertised via the rich `Link:` header set — `llms.txt`, `llms-full.txt`, an MCP server card, an agent card, an agent-skills index) is the one telling crawlers not to index or follow it. For a page whose entire content is oriented at explaining the product model to an automated reader, this is a self-defeating combination if any consuming crawler actually honors the directive (most on-demand agent fetches — a user pasting the URL, a RAG pipeline's own crawler — will ignore it and fetch anyway, but search-style AI indexes that respect robots directives would not).

## 3. HTML-to-Markdown ratio

295,139 bytes (HTML) / 10,683 bytes (Markdown) = **27.6×**. Lower than a typical short help page on this site (e.g. `creating-your-first-link` is ~154×) simply because this page has much more body content — the fixed Mintlify chrome/nav/JS payload is being amortized over a longer article, not because the markdown extraction is less clean.

## 4–5. Full markdown body + token estimate

Byte count (10,683) and unicode char count (10,625) differ by 58 bytes because of a handful of multi-byte characters: `✗` (used 4×), `▼` (used 4× in the ASCII diagram), `→` (used 1× in the footer CTA), and em dashes. Token estimate: 10,625 chars / 4 ≈ **2,656 tokens** for the whole page including boilerplate. Full body was read in full (316 lines) — see quoted excerpts throughout this report; not reproduced in full here for brevity, but every section below is drawn directly from it.

## 6. Heading structure (in order)

| Level | Heading |
|---|---|
| H1 | Key Concepts |
| H2 | The Three-Entity Model |
| H3 | Item |
| H3 | Link |
| H3 | Media |
| H2 | Example |
| H2 | Link Modes |
| H3 | Direct Mode |
| H3 | Page Mode |
| H2 | Destinations |
| H3 | Current Destination Types: |
| H3 | URL Templates |
| H2 | Tubs |
| H3 | What Tubs Provide: |
| H3 | Examples: |
| H2 | Print-Before-Link Workflow |
| H2 | Update Without Reprinting |
| H3 | Common scenarios: |
| H2 | Integration Layer |
| H2 | Common Questions |
| H2 | Next Steps |

Clean, consistent H1 → H2 → H3 nesting with no skipped levels — a heading-based chunker would produce sensible, self-titled chunks (e.g. "Link Modes > Direct Mode" as its own chunk). Three H3s end with a trailing colon ("Current Destination Types:", "What Tubs Provide:", "Common scenarios:") — cosmetically inconsistent with the rest (no other heading has one), and a heading-text-based lookup or anchor-slug guess (e.g. an agent guessing `#current-destination-types` vs `#current-destination-types-`) needs to handle the punctuation-stripping Mintlify's slugifier does. Not fatal, just a minor inconsistency. No heading-text collisions: the only other place "Examples" appears is as **bold inline text** (`**Examples:**`) under "Item", not a heading, so anchor IDs stay unique.

As on other pages of this site, the very first line of the raw file is a blockquoted pseudo-heading:
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
This is a `##`-level token wrapped in `>`, sitting before the real H1. A markdown parser that doesn't require headings to start at column 0 (unindented) could misidentify "Documentation Index" as the page's first heading/title. Confirmed via the HTML source that this exact text renders as `<blockquote class="sr-only" data-agent-docs-index="true" aria-hidden="true">` — i.e. it is deliberately hidden from *both* sighted users (`sr-only`) and assistive technology (`aria-hidden="true"`) in the human-facing page, while being fully present as plain, unhidden text in the markdown/LLM-facing representation. It's a page/site-wide "steer the agent toward llms.txt" instruction, invisible to every human channel (visual and screen-reader) and visible only to whatever fetches the `.md` document — confirmed byte-for-byte identical on `/help/creating-your-first-link.md` and `/help/pages-overview.md`.

## 7. Boilerplate vs. page-specific content

Two blocks are confirmed site-wide boilerplate (verified identical, byte-for-byte, on `/help/creating-your-first-link.md` and `/help/pages-overview.md`):

**A. Top "Documentation Index" banner** (175 chars):
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
175 / 10,625 = **~1.65%** of this page's tokens (~44 tokens).

**B. Footer CTA + contact block** (149 chars):
```
**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
149 / 10,625 = **~1.40%** of this page's tokens (~37 tokens).

**Combined ≈ 324 / 10,625 chars ≈ 3.05% of the page's token budget.** Notably smaller than the ~24% seen auditing a short page (`creating-your-first-link`, ~352 tokens total) — the same fixed-size boilerplate costs almost nothing on a long, dense page like this one but is brutal on short pages. Worth noting for the site-wide audit rollup: boilerplate tax is inversely proportional to page length.

**Borderline — templated wrapper, page-specific content:** the "Next Steps" section groups links under two bold sub-labels:
```
## Next Steps

**Related Help Pages:**

* [Creating Your First Link](/help/creating-your-first-link)
* [Pages Overview](/help/pages-overview)
* [Conditional Visibility](/help/conditional-visibility)
* [Physical Media Management Basics](/help/media-basics)

**Integration Guides:**

* [Mitti Integration](/integrations/safetyculture)
* [CMMS Systems Integration](/integrations/cmms-systems)
```
389 chars (~97 tokens, ~3.66%) — the *links* are page-specific (they genuinely point to content that follows from Key Concepts), so this isn't pure filler. But the wrapper pattern is inconsistent across the site: `creating-your-first-link.md` uses a flat, single-tier `## Next Steps` bullet list (no bold sub-groups), and `pages-overview.md` uses a differently-named `## Related` heading with a flat list. Three pages, three different shapes for "what to read next" — a RAG chunker or an agent trying to pattern-match "the related-links section" across pages can't rely on one heading name or one list shape.

## 8. Other retrieval-quality notes

### Critical: fabricated per-item Media tracking capability

The "Media" H3 and its worked example are the most consequential finding on this page. The section claims, as reasons to "track Media separately":

> * **Cost tracking** - A billboard costs \$5,000. That's infrastructure worth managing.
> * **Durability** - Metal plaques last 10+ years. Vinyl stickers last 1-3 years. Different materials have different lifecycles.
> * **Replacement** - When Media is damaged, you can replace it with new Media linking to the same Link—so the Item connection is preserved and nothing needs reconfiguring.
> * **Inventory** - Track what's been produced, what's installed, what's in stock.
> * **Production** - Manage Media Batches from different print partners or production runs.

...and backs it with a fully fleshed-out example presented as real product output:
```
MEDIA: Metal Plaque #4729
- Type: Stainless steel, engraved
- Cost: $75
- Installed: March 2025, left cab door
- Batch: #47 (500 pieces, PrintCo)
```

Per this workspace's own `/workspace/mintlify-docs/CLAUDE.md` (UC-008): *"Per-item Media is not [tracked]. There is no record of what an individual code is printed on — no material type, cost, durability or installation location, and no inventory."* Print **batches** are real (a tracked Draft→Printing→Printed→Deployed lifecycle with name/notes/tags/photo), but individual Media items are not.

I verified this independently against `/workspace/qrtub/src/`:
- `supabase/migrations/20260318000001_add_print_batch_tracking.sql` defines the actual batch schema — columns are `name`, `notes`, `printed_by`, `printed_at`, `link_count`, `item_count`, `csv_storage_path`. No material type, no per-unit cost, no durability, no installation location, no print-partner/vendor field anywhere in the batch or link tables.
- The only hits for "cost" / "durability" in `src/` are unrelated: `purchase_cost` / `unit_cost` are **Item**-level custom-field template fields (IT-asset and inventory starter templates), and "durability" appears only inside a demo product-description string ("military-grade durability") — neither is a Media/physical-material tracking feature.
- "PrintCo" as a named print partner has no corresponding field anywhere in the schema.

So a support chatbot, or ChatGPT/Perplexity/Claude answering from this page, would confidently tell a customer that QRtub tracks per-Media cost, durability, installation location, and print-partner — none of which exists today. This is exactly the failure mode `mintlify-docs/CLAUDE.md` calls out by name ("Pages have previously documented ... Media type tracking. None of these exist. All were written from assumption rather than from the code") — it is still live on this page and should be corrected or at minimum scoped down to what print batches actually track.

### Terminology and brand consistency (mostly good)

Cross-checked against `/workspace/qrtub/GLOSSARY.md` and `/workspace/qrtub/BRAND.md`:
- "Item", "Link", "Media", "Destination(s)", "Tub", "Direct Mode", "Page Mode", "Print-before-link" all match canonical glossary terms and capitalization exactly.
- "QRtub" is written correctly everywhere (capital QR, lowercase tub, one word); `qrtub.com` stays lowercase in every URL example.
- The "Integration Layer" section's "What QRtub is NOT" list (asset management / inspection / maintenance tracking / compliance platform) matches BRAND.md §1.3 almost verbatim — accurate, not overstated.
- The page never uses the word "BETA" (correctly, per `mintlify-docs/CLAUDE.md`'s instruction to retire that word) and never claims API access, analytics dashboards, or cross-account transfer — all consistent with BRAND.md's Planned/Available table.
- The URL-Template non-encoding caveat ("QRtub does not URL-encode them, so a field containing a space, `&`, `?` or `#` will produce a broken link") matches `mintlify-docs/CLAUDE.md`'s verified technical note precisely — this part is accurate and well-explained.
- `{{item.assetID}}` / `{{item.name}}` double-brace binding syntax matches the verified syntax in `../qrtub/src/lib/page/bindings.ts`.

### Self-containment

Strong. Unlike a short procedural page, this page defines every term it uses inline (Item, Link, Media, Direct/Page Mode, Destinations, URL Templates, Tubs) before using it, and closes with a 5-question FAQ that pre-empts likely follow-ups ("What's the difference between a Link and a QR code?", "Can one Link connect to multiple Items?"). A support chatbot or ChatGPT/Perplexity retrieving only this page could correctly answer most conceptual questions about the product without a second fetch — the Media-tracking claim above is the one place where "self-contained" also means "self-contained fabrication," since there's no other page pulled in to contradict it.

### Minor markdown-rendering artifacts (not visible in HTML)

- Escaped currency: `\$5,000` (backslash-escaped, presumably to avoid math-delimiter interpretation) appears once, while `$75` two lines later in the diagram code block is unescaped (fine there — inside a fenced code block, no escaping is needed). Cosmetically inconsistent but not a parsing problem for an LLM reader; a naive display of the raw markdown to a human would show a stray backslash.
- The ASCII-art entity diagram under "## Example" is inside a fenced code block with trailing whitespace on several lines (e.g. `Excavator #203 `, `Status: Active  `) used for visual column alignment — harmless, but if any downstream tool trims trailing whitespace per line before diffing/caching, the diagram's alignment (not its content) would shift.
- Relative internal links (`/help/creating-your-first-link`, `/integrations/safetyculture`, etc.) are root-relative; they resolve fine within the site but are meaningless if this markdown body is lifted out of context (e.g. pasted into a prompt without the source URL attached) — same issue observed on other pages of this site. The footer CTA and contact link, by contrast, are fully-qualified (`https://qrtub.com/pricing`, `mailto:hi@qrtub.com`), so only the "Next Steps" links have this problem here.
- No broken tables, no orphaned HTML tags, no dangling reference-style links — the rest of the markdown renders cleanly and matches the visible HTML content.
