# Audit: Physical Media Management Basics

- **HTML URL:** https://help.qrtub.com/help/media-basics
- **Markdown URL:** https://help.qrtub.com/help/media-basics.md
- **llms.txt description:** "How to think about the physical material your QR codes are printed on"
- **Audited:** 2026-08-19

## Summary table

| Metric | Value |
| --- | --- |
| HTTP status (all 3 requests) | 200 |
| Content-Type (Accept: text/markdown, on `/help/media-basics`) | `text/markdown; charset=utf-8` |
| Content-Type (default Accept, on `/help/media-basics`) | `text/html; charset=utf-8` |
| Content-Type (`/help/media-basics.md`) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (markdown, both the negotiated URL and the `.md` URL) | `noindex, nofollow` |
| X-Robots-Tag (default HTML response) | **absent** (no header sent at all) |
| Content-Length — HTML | 276,671 bytes |
| Content-Length — Markdown | 7,201 bytes |
| HTML : Markdown byte ratio | **38.4 : 1** (276671 / 7201 = 38.42) |
| Token estimate (chars/4, markdown body) | ~1,790 tokens (7,161 chars of text ÷ 4; 7,201 bytes includes a few multi-byte UTF-8 chars like `→` and `—`) |
| Boilerplate share of page tokens | ~4.6% (top llms.txt banner + bottom CTA/footer combined) |

Full raw headers are in the appendix at the bottom of this file.

## Heading structure (in order)

```
H1  Physical Media Management Basics
H2    Understanding the Three Entities
H2    Why Track Media Separately?
H2    What you can track today
H2    Not tracked yet
H2    Media Types
H3      Vinyl Stickers
H3      Metal Plaques
H3      Signs (Rigid)
H3      Billboards & Large Format
H3      Real Estate Signs
H3      Printed Ads (Newspaper, Magazines, Postcards)
H3      NFC Chips
H2    Media Templates (Planned Feature)
H2    Print Batches
H2    Media Partners (Planned Feature)
H2    Choosing the Right Media
H2    Next Steps
```

12 H2s, 7 H3s (all nested under "Media Types"), 1 H1. Structure is logical and skimmable; no heading-level skips (no jumping from H1 straight to H3, etc.). The doc reads as one coherent unit, not fragments — good for chunked RAG retrieval since each H2 section is a self-contained, well-scoped chunk (Media Types' H3 subsections are also individually chunk-able: each is a 4-bullet fact card).

## Boilerplate identified

Two distinct blocks, both clearly cross-page template injections rather than content about physical media:

### 1. Top "Documentation Index" banner (Mintlify llms.txt discovery banner)

Exact text (rendered as a blockquote, the first thing in the file, appearing *before* the H1):

> \## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.

- 175 characters ≈ 44 tokens.
- This is a Mintlify-generated navigational injection into the `.md`/negotiated-markdown response, not part of the authored page — it does not appear in the HTML rendering at all. It is almost certainly identical (except URL) on every page of the site, since it's a generic "go read llms.txt first" instruction rather than anything about media/QR codes.
- Effect on retrieval: harmless and arguably useful (points an agent to the sitemap-equivalent), but it does consume the first ~44 tokens of every single page fetch with an instruction rather than content, and it front-loads the context window with something that isn't an answer to the user's question.

### 2. Bottom CTA / contact footer

Exact text (last lines of the file, after a `***` horizontal rule):

> \---
>
> **Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)
>
> Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)

- 156 characters ≈ 39 tokens.
- This is a generic "get started" sales CTA + support-email line with no reference to media, QR codes, or anything page-specific. Wording matches the kind of boilerplate CTA block BRAND.md prescribes site-wide ("Include concrete next steps or CTAs" as a general writing rule) — i.e., it reads like a shared footer partial rather than page content, and would read identically at the bottom of, say, a billing or login-help page.

### Combined boilerplate share

- Boilerplate: 175 + 156 = 331 characters ≈ 83 tokens.
- Total page: 7,161 characters ≈ 1,790 tokens.
- **≈ 4.6% of this page's total token budget is non-page-specific boilerplate** (banner + footer). Not severe, but not zero — for a page whose whole value-add is ~1,790 tokens, spending 83 of them on "go read another file" + "here's our pricing page" is a real, if small, tax on every agent fetch.

### Borderline case: "Next Steps" section

The `## Next Steps` H2 itself (heading label) is likely a templated section name reused across pages, but its *content* here is page-specific and thin — one related link plus one caveat sentence:

> \* [Key Concepts](/help/key-concepts) — how Media relates to Items and Links
>
> Guides for Media Templates, Batch management, replacement workflows and Media Partners will follow once those features exist.

This isn't pure boilerplate (the link target and the caveat sentence are specific to this page), so it wasn't counted in the 4.6% above — but the section is structurally the "Related links" pattern the task asked to watch for, just with page-specific content filled in rather than a copy-pasted list.

## Other retrieval-quality notes

1. **Relative link won't resolve out of context.** The one internal link — `[Key Concepts](/help/key-concepts)` — is a root-relative path, not an absolute URL. In the raw `.md` fetch there is nothing else in the file that states the site's domain. An agent that retrieves *only* this markdown body (e.g., pasted into a chat, or fetched by a generic crawler that doesn't know to resolve relative to `https://help.qrtub.com`) cannot reconstruct a working URL from `/help/key-concepts` alone. Every other Mintlify page on this domain will have the same issue, but it's worth flagging since it directly breaks "click through for more" for a non-browser consumer.

2. **Minor MDX-escape leakage into plain markdown.** Line 153 of the raw markdown reads:
   `Temporary (\< 1 year) → Vinyl stickers, printed materials`
   The backslash before `<` is an MDX/Mintlify escape (needed in the HTML/React rendering pipeline so `< 1 year` isn't parsed as the start of an HTML/JSX tag). It does not show up in the HTML-rendered page (renders cleanly as "< 1 year"), but it leaks through verbatim into the plain-markdown export as `\< 1 year`. A human reading raw markdown would silently understand it; an LLM ingesting the raw text token-for-token sees a literal backslash character that doesn't belong to the sentence — small, but it's exactly the kind of artifact that only shows up in the markdown channel and not in HTML, which is what this audit is looking for.

3. **Caching inconsistency between the two markdown delivery paths.** Fetching the canonical URL with `Accept: text/markdown` returns `Cache-Control: private, no-cache`, while fetching the dedicated `/help/media-basics.md` URL (no special Accept header needed) returns `Cache-Control: public, max-age=0, must-revalidate` for byte-identical content (both 7,201 bytes, same ETag-less 200). An agent that content-negotiates via the Accept header (rather than requesting the `.md` suffix directly) gets a response marked non-cacheable even though the underlying content is the same and *is* publicly cacheable via the other path. Not a correctness bug, but a missed opportunity for edge/CDN caching on one of the two supported access patterns.

4. **X-Robots-Tag divergence between HTML and markdown is worth double-checking.** Both markdown-delivery paths (content-negotiated and `.md`-suffixed) send `X-Robots-Tag: noindex, nofollow`, while the plain HTML page sends no `X-Robots-Tag` header at all (i.e., default-indexable). This is presumably deliberate — Mintlify likely wants search engines to index the canonical HTML page and not the `.md`/`llms.txt` alternate representations as separate duplicate-content URLs. But it means any AI crawler that respects `X-Robots-Tag` the same way search engines do (several major AI-crawler user agents document that they do) will decline to index/store the very representation that's structured best for it, while happily indexing the noisy 38x-heavier HTML. Worth confirming this is intentional sitewide policy rather than an oversight, since it could mean the "agent-optimized" markdown surface is invisible to crawl-based agents even though it's reachable and correct when fetched directly (as this audit did).

5. **Self-containment: undefined product terms used without a link or inline gloss.** The page uses "Tub" (`"When you export a print list from a Tub"`) and "Media Batches" (`"Manage Media Batches from different print partners or production runs"`) without defining either term on-page, and the one outbound link (`Key Concepts`) is framed as being about Media/Items/Links, not Tubs. Per `GLOSSARY.md`, "Tub" is a load-bearing product term (category-based workspace) that a reader/agent landing on this page cold — e.g., a support chatbot that retrieved only this page as its context — has no way to resolve. This slightly weakens standalone answerability for questions like "what's a Tub and how does it relate to media batches?"

6. **Terminology accuracy vs. GLOSSARY.md / BRAND.md: clean.** Checked "Item," "Link," "Media," "Media Batch," "Tub," and "Print Batches/Media Batch management" usage against the glossary and BRAND.md's feature-status table:
   - Correct casing and usage of QRtub, Item, Link, Media throughout.
   - The page correctly distinguishes **available today** ("Print batches" — batch metadata, Draft→Printing→Printed→Deployed status, CSV retention, deployment marking, archiving — matches the real `media_batches`/print-batch feature described in the product's own engineering docs) from **planned** (Media as a distinct trackable entity, Media Templates, Media Partners, cost/material/durability tracking per physical item) — each planned section is explicitly labeled "(Planned Feature)" or placed under "Not tracked yet," in line with BRAND.md's "Never promise features marked as Planned" / "Never overstate current capabilities" rules.
   - No false claims found (e.g., does not claim Media inventory management, Media Batch management, or Media Templates are available).

7. **Self-contained and coherent overall.** Apart from the two undefined terms in note 5, the page stands on its own: it defines Item/Link/Media inline (with a table), explains why media is tracked separately, clearly separates current vs. planned capability, and gives concrete, numeric, non-hand-wavy guidance (costs, durability ranges, use cases) for seven physical media types plus a decision-factor section (Environment/Duration/Budget/Application). This is well-suited to being retrieved and quoted directly by a support chatbot or answer engine without hallucination risk, aside from the noted gaps.

8. **Size efficiency.** At ~1,790 tokens for the full page (minus ~83 boilerplate tokens = ~1,707 tokens of actual content), this is a compact, dense page — good for RAG chunking or being pulled whole into a context window. The 38.4:1 HTML:Markdown ratio confirms the enormous overhead (nav chrome, scripts, CSS, React hydration payloads) an agent would eat by fetching the HTML instead of using the `.md` alternate — strong evidence for always preferring `Accept: text/markdown` / the `.md` suffix when this site is the target.

## Appendix: raw curl output

### 1. `curl -sI -H "Accept: text/markdown" https://help.qrtub.com/help/media-basics`

```
HTTP/2 200
age: 43236
cache-control: private, no-cache
content-disposition: inline
content-security-policy: default-src 'none'
content-type: text/markdown; charset=utf-8
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
server: Vercel
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-llms-txt: /llms.txt
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
x-robots-tag: noindex, nofollow
content-length: 7201
```

### 2. `curl -sI https://help.qrtub.com/help/media-basics` (default Accept: text/html)

```
HTTP/2 200
age: 51909
cache-control: public, max-age=0, must-revalidate
content-security-policy: worker-src * blob: data: 'unsafe-eval' 'unsafe-inline'; object-src data: ; base-uri 'self'; upgrade-insecure-requests; frame-ancestors 'self' https://dashboard.mintlify.com https://app.mintlify.com; form-action 'self' https://codesandbox.io;
content-type: text/html; charset=utf-8
etag: "o0qhrck3h65xbd"
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
server: Vercel
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-llms-txt: /llms.txt
x-mintlify-client-version: 0.0.3469
x-nextjs-prerender: 1
x-nextjs-stale-time: 60
content-length: 276671
```

(no `x-robots-tag` header present in this response)

### 3. `curl -sI https://help.qrtub.com/help/media-basics.md`

```
HTTP/2 200
age: 43236
cache-control: public, max-age=0, must-revalidate
content-disposition: inline
content-security-policy: default-src 'none'
content-type: text/markdown; charset=utf-8
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
server: Vercel
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
x-robots-tag: noindex, nofollow
content-length: 7201
```

### Full markdown body

Saved locally during the audit at (scratchpad, not part of this repo):
`/tmp/claude-1000/-workspace/8d8e6318-59ee-44f4-8476-e19a07fd0f4a/scratchpad/media-basics.md`
