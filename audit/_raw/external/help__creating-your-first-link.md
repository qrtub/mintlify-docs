# Audit: Creating Your First Link

- Page: **Creating Your First Link**
- HTML URL: https://help.qrtub.com/help/creating-your-first-link
- Markdown URL: https://help.qrtub.com/help/creating-your-first-link.md
- llms.txt description: "Generate your first Link, download the QR code, and connect it to an Item"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | `200` |
| Content-Type (`.md`) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (`.md`) | `noindex, nofollow` |
| Content-Length (`.md`) | 1,409 bytes |
| Content-Length (HTML, default `Accept`) | 216,783 bytes |
| HTML : Markdown byte ratio | **153.9×** (216783 / 1409) |
| Estimated tokens (chars/4) | **~352 tokens** |
| Headings (H1/H2/H3 count) | 1 × H1, 6 × H2, 0 × H3 |
| Boilerplate share of page tokens | **~24%** (top banner ~12.5% + footer CTA ~11.1%) |

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
content-length: 1409
```

**Default request (HTML):**
```
HTTP/2 200
content-type: text/html; charset=utf-8
content-length: 216783
```
No `x-robots-tag` header is present on the HTML response — the `noindex, nofollow` directive is emitted only on the markdown variant. That is a notable asymmetry: it tells well-behaved crawlers/SEO bots not to index the very representation that is purpose-built for machine consumption, while the human-facing HTML page carries no such restriction. (In practice this header is aimed at search engines rather than at agent fetchers hitting the URL directly or via `llms.txt`, but it is worth flagging since it sits on the "AI-facing" endpoint specifically.)

The `Link:` header set is rich and agent-friendly: it advertises `llms.txt`, `llms-full.txt`, an API catalog, an MCP server card, an agent card, and an agent-skills index — all discoverable from a single HEAD request on any page.

## 3. HTML-to-Markdown ratio

216,783 bytes (HTML) / 1,409 bytes (Markdown) = **153.9×**. The markdown representation strips essentially all of Mintlify's chrome/nav/JS/CSS payload, leaving a very lean, high-signal document — good for retrieval economy once a consumer knows to request the `.md` variant.

## 4–5. Full markdown body + token estimate

Full body (1,409 bytes / chars, ASCII throughout so bytes≈chars):

```markdown
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.

# Creating Your First Link

> Generate your first Link, download the QR code, and connect it to an Item

Links are the foundation of QRtub. Each Link is a unique URL (like `qrtub.com/r/x5fgd`) that you can encode in a QR code and manage independently.

## Step 1: Navigate to Links

From your dashboard, click **Links** in the main navigation.

## Step 2: Generate a New Link

Click the **Generate Links** button. You can create:

* **Single Link** - Generate one Link at a time
* **Bulk Links** - Generate multiple Links for professional printing

## Step 3: Choose Your Link Type

* **Random** (`qrtub.com/r/x5fgd`) - Default, automatically generated
* **ID-based** (`qrtub.com/cra001`) - Sequential or branded patterns
* **Custom** (`qrtub.com/mylink`) - Memorable, custom URLs

## Step 4: Print or Connect

Once generated, you can:

* Download QR codes for printing
* Connect the Link to an Item
* Set up a Page with multiple Destinations

## Next Steps

* Learn about [Pages](/help/pages-overview)
* Understand [Direct Mode vs Page Mode](/help/key-concepts#link-modes)

***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

Token estimate: 1,409 chars / 4 ≈ **352 tokens** for the entire page, including boilerplate.

## 6. Heading structure (in order)

| Level | Heading |
|---|---|
| H1 | Creating Your First Link |
| H2 | Step 1: Navigate to Links |
| H2 | Step 2: Generate a New Link |
| H2 | Step 3: Choose Your Link Type |
| H2 | Step 4: Print or Connect |
| H2 | Next Steps |

No H3s. The hierarchy is flat and sequential (no skipped levels), and each H2 is a self-contained numbered step — a good shape for chunked retrieval (a vector-search chunker splitting on H2 would get 5 clean, individually-sensible chunks plus a short intro).

One structural wrinkle: the very first line of the file is a blockquoted pseudo-heading, `> ## Documentation Index` (see boilerplate section below). It renders as a callout box in HTML, but in raw markdown it is a `##` token wrapped in a blockquote. A naive header-based chunker/parser that (a) doesn't anchor its regex to line-start (so it matches `## Documentation Index` anywhere in the line) or (b) strips leading `>` characters before scanning for headings, could mistake this for a real H2 — and worse, a naive "grab the first heading as the title" heuristic could extract **"Documentation Index"** rather than **"Creating Your First Link"** as this page's title. This same banner is injected verbatim on other pages too (verified on `/help/pages-overview.md`), so it is a site-wide template artifact, not page content.

## 7. Boilerplate vs. page-specific content

Two blocks are confirmed site-wide boilerplate (verified identical, byte-for-byte, on `/help/pages-overview.md`):

**A. Top "Documentation Index" banner** (176 bytes incl. trailing blank line ≈ 172 bytes of text):
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
≈ 172 / 1409 = **~12.2%** of the page's tokens.

**B. Footer CTA + contact block** (157 bytes):
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
≈ 157 / 1409 = **~11.1%** of the page's tokens.

**Combined boilerplate ≈ 329 / 1409 bytes ≈ 23.3–24% of the entire page's token budget** is generic site-wide chrome that carries zero information about "creating your first link" specifically. For a ~352-token page, that's roughly 82 tokens spent on a repeated banner and a repeated pricing/contact CTA before an agent gets to any page-specific instruction.

A third block is borderline: the **"Next Steps"** section (`Learn about [Pages]…`, `Understand [Direct Mode vs Page Mode]…`) follows the same "Next Steps" pattern likely used across many pages, but its *links* are page-specific (they point at content that logically follows from this exact page), so it isn't pure filler — it's a templated wrapper around genuinely relevant content. Not counted in the boilerplate percentage above.

## 8. Other retrieval-quality notes

- **Self-containment is weak for an isolated fetch.** The page uses canonical product terms — Link, Item, Page, Destination, Direct Mode / Page Mode — without defining any of them inline. It relies entirely on outbound links (`/help/pages-overview`, `/help/key-concepts#link-modes`) for definitions. A support chatbot or ChatGPT/Perplexity retrieving *only* this page (a very plausible single-chunk RAG hit for a query like "how do I create a link") could correctly describe the 4-step workflow but could not answer a natural follow-up like "what's a Page?" or "what's the difference between Direct Mode and Page Mode" without a second fetch.
- **Terminology is glossary-correct.** Cross-checked against `/workspace/qrtub/GLOSSARY.md`: "Link" (not "Access Link"), "Item" (not "Asset"), "Page" and "Destinations" (correct capitalization as product terms), "QR code" lowercase generic use — all consistent with canonical usage. Brand name "QRtub" is written correctly throughout (checked against `/workspace/qrtub/BRAND.md` — capital QR, lowercase tub, one word).
- **Thin on details an agent might need to answer specific questions.** The llms.txt description promises "download the QR code" but the body only says "Download QR codes for printing" as a bullet — no file format (PNG/SVG/PDF), resolution, or where the download control lives in the UI. A coding agent or support bot asked "what format does QRtub export QR codes in" gets no answer from this page and no pointer to a page that would.
- **Relative vs. absolute link inconsistency.** Internal help links are root-relative (`/help/pages-overview`, `/help/key-concepts#link-modes`) — correct within the docs site but meaningless without the `help.qrtub.com` base if the markdown is extracted context-free (e.g., pasted into a prompt without the source URL attached). Meanwhile the footer CTA and contact link use fully-qualified URLs (`https://qrtub.com/pricing`, `mailto:hi@qrtub.com`). This mix is minor but means an agent that only has the raw markdown text (no originating URL) cannot resolve the "Next Steps" links at all, while it can resolve the footer links.
- **`x-robots-tag: noindex, nofollow` appears only on the markdown response**, not the HTML response (see header dump above) — flagged again here because it's the single most surprising header-level finding: the representation meant for machine consumption is the one telling crawlers not to index/follow it.
- **No malformed markdown syntax otherwise.** Lists, bold UI-label references (`**Links**`, `**Generate Links**`), inline code for URL patterns (`` `qrtub.com/r/x5fgd` ``), and the horizontal rule (`***`) all render correctly and match the HTML page's visible content — no broken tables, no dangling reference-style links, no orphaned HTML tags leaking into the markdown.
- **Good chunkability.** Aside from the blockquote-heading edge case noted above, the flat H1→H2 structure with short, self-descriptive step headings is well-suited to heading-based chunking strategies common in RAG pipelines.
