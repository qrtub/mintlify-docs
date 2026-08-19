# Audit: Device Detection & Routing

- HTML URL: https://help.qrtub.com/help/device-detection
- Markdown URL: https://help.qrtub.com/help/device-detection.md
- llms.txt description: "Route users to different Destinations based on their device type, operating system, or browser"
- Audited: 2026-08-19

## Summary Table

| Metric | Value |
| --- | --- |
| HTTP status (both variants) | 200 |
| Content-Type (`Accept: text/markdown`) | `text/markdown; charset=utf-8` |
| Content-Type (default `Accept: text/html`) | `text/html; charset=utf-8` |
| X-Robots-Tag (markdown response) | `noindex, nofollow` |
| X-Robots-Tag (HTML response) | *(absent — not sent)* |
| Content-Length, HTML | 298,890 bytes |
| Content-Length, Markdown | 9,060 bytes |
| HTML : Markdown byte ratio | **32.99×** (298890 / 9060) |
| Markdown body length (downloaded) | 9,060 bytes / 9,006 Unicode chars (multi-byte punctuation: em dashes, `→`, curly quotes) |
| Estimated token count (chars/4) | **~2,250–2,265 tokens** |

## Raw header dumps

**1. `curl -sI -H "Accept: text/markdown" .../help/device-detection`**
```
HTTP/2 200
content-type: text/markdown; charset=utf-8
content-length: 9060
x-robots-tag: noindex, nofollow
x-llms-txt: /llms.txt
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
cache-control: private, no-cache
x-vercel-cache: HIT
```

**2. `curl -sI .../help/device-detection` (default Accept)**
```
HTTP/2 200
content-type: text/html; charset=utf-8
content-length: 298890
(no x-robots-tag header present)
link: </llms.txt>; rel="llms-txt", ... (same discovery links)
x-matched-path: /_sites/[subdomain]/[[...slug]]
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
cache-control: public, max-age=0, must-revalidate
x-vercel-cache: HIT
```

Note: neither response's `Vary` header includes `Accept`, even though the response body materially differs by `Accept` header (markdown vs. HTML). On a CDN (Vercel here) that's a latent cache-key risk — a shared edge cache keyed only on URL (not on `Accept`) could in principle serve the wrong representation to a client that didn't send the same `Accept` value as whichever request populated that cache slot. Both requests above did return the correct content-type despite this (likely because Mintlify's markdown route is a distinct matched path, `/_mintlify/_markdown/...`, rather than true `Accept`-based negotiation on one path) — noted as a fragility, not an observed failure.

## Heading Structure (order, as it appears in the .md)

```
H1  Device Detection & Routing
H2  Why Device Detection Matters
H2  Common Use Cases
  H3  1. Mobile App vs Web App
  H3  2. Platform-Specific App Downloads
  H3  3. iOS Safari Deep Link Workaround (Mitti Example)
  H3  4. Tablet-Optimized Experience
  H3  5. Browser-Specific Features
H2  Available Device Information
  H3  Device Type
  H3  Operating System
  H3  Browser
H2  How to Set Up Device Routing
H2  Combining Device Detection with Item Data
H2  Important Notes
H2  Testing Your Setup
H2  When NOT to Use Device Detection
H2  Related
```

Well-formed hierarchy: single H1, no skipped levels, H3s only ever nest under a preceding H2. Good structural signal for chunked retrieval (a RAG splitter using headers will produce sensible, self-describing chunks — e.g. "Available Device Information > Operating System" is a coherent standalone chunk with its field name, values, flags, and example).

## Boilerplate Content

Two boilerplate blocks bracket the page-specific content, both clearly templated (identical wording expected on other help pages, only the URLs/domain are fixed constants, not this-page-specific data):

**1. Top-of-file "Documentation Index" banner** (present only on the `.md`/`Accept: text/markdown` variant — this is a Mintlify-injected banner for the markdown/agent-facing rendering, not part of the human HTML page at all):

```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
— 177 characters (~44 tokens).

**2. Bottom-of-file CTA + contact footer:**

```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
— 155 characters (~39 tokens).

**Combined boilerplate: 332 of 9,006 characters ≈ 3.7% of the page's total tokens** (~83 of ~2,250 estimated tokens). This is a small fraction — not a major retrieval-quality problem on its own for this page — but it is dead weight that contributes nothing page-specific, and it recurs on every help page identically, so at index/corpus level (llms-full.txt, or any RAG ingestion of all help pages) it's pure redundant tokens duplicated across every article. A chatbot quoting "Ready to get started..." verbatim from a retrieved chunk would produce an oddly salesy non-sequitur mid-answer if the chunker doesn't strip it.

Additionally, the **`## Related` section** (lines 274–280, 473 chars / ~118 tokens / ~5.2% of the page) is a *structural* template pattern repeated across pages (every help article likely ends with a "Related" list) but its *content* is page-specific (links genuinely relevant to device detection: app-links, using-fields, conditional-visibility, pages-overview, key-concepts) — so it is not counted as boilerplate above, just flagged as a recurring pattern worth knowing about if building page-uniqueness heuristics.

## Retrieval-Quality Notes

- **Self-contained: yes, largely.** The page explains the feature from scratch (why device detection matters → use cases → full field/value/flag reference for `device.type`, `device.os`, `device.browser` → setup steps → testing → when not to use it) without assuming the reader has another page open. A chatbot could answer most device-routing questions from this page alone.
- **Cross-references are relative paths** (`/help/app-links`, `/help/using-fields`, `/help/conditional-visibility`, `/help/pages-overview`, `/help/key-concepts`) with no domain. An agent that fetches only the raw `.md` body (e.g. via a generic URL-to-markdown tool rather than a browser resolving the page's `<base>`) needs to already know the `help.qrtub.com` origin to follow them. Not a defect specific to this page, but worth flagging: the `.md` response contains no equivalent of an HTML `<base href>` to anchor those relative links.
- **ASCII tree diagram renders as an unlabeled fenced code block** (the "Example Page" box-drawing tree under "How to Set Up Device Routing"):
  ```
  Page: Equipment Inspector
  ├── "Open Mobile App"
  │   └── Condition: device.isMobile
  ...
  ```
  This is fine for a human reading rendered Markdown, and it's unambiguous to an LLM reading raw text too (indentation + box-drawing characters are legible enough), but it is **not marked with a language hint** and carries no alt-text/caption beyond the "Example Page:" line before it — a screen-reader/very-literal parser would just see an opaque code block. Low severity; the surrounding prose ("Example Page:") already labels it adequately for an LLM.
- **No malformed Markdown found** in the plain-text rendering — nested bullets, bold labels used as pseudo-headers ("**Setup:**", "**Scenario:**", "**Result:**"), and inline code spans for CEL expressions and field names all degrade cleanly to plain text. Nothing that renders fine in HTML but breaks/garbles in the `.md` (no leftover component syntax, no unresolved MDX/JSX, no broken table syntax).
- **`X-Robots-Tag: noindex, nofollow` is set only on the markdown response**, not the HTML response. This is very likely deliberate and correct Mintlify behavior (prevents the `.md` alternate from being indexed as duplicate content by traditional search crawlers that respect X-Robots-Tag), and it doesn't block the intended AI-agent consumption path (agent/LLM fetchers generally don't treat X-Robots-Tag as a fetch-permission signal, only search-indexing crawlers that honor it do) — but it's worth recording explicitly since a support chatbot's retrieval crawler, if it happens to respect robots directives literally, could skip this representation.
- **Machine-discovery headers present and correct:** both responses carry `Link` headers advertising `/llms.txt`, `/llms-full.txt`, `/.well-known/api-catalog`, `/.well-known/mcp/server-card.json`, `/.well-known/agent-card.json`, `/.well-known/agent-skills/index.json`, plus a custom `X-Llms-Txt: /llms.txt` header. This is good practice for agent discoverability at the site level (this isn't page-specific, same on every page, but confirms the page participates correctly in the site's agent-discovery scaffolding).
- **HTML:Markdown ratio (~33×) is large but expected** for a Mintlify/Next.js-rendered page (full RSC payload, hydration script, nav/sidebar chrome, etc.) versus the plain markdown alternate — this is exactly the kind of page where an agent should strongly prefer the `.md`/`Accept: text/markdown` path over scraping rendered HTML, and the site correctly offers that path.
- **Terminology/brand check (against `/workspace/qrtub/GLOSSARY.md` and `BRAND.md`):** the page is clean. It correctly uses "Destination" (capitalized, never "Action Link"/"Destination Link"), "Item" (never "Asset"), "Page"/"Pages" (never "Profile Page" or "Landing Page" as a proper noun), and "Conditional Visibility" matching the glossary's CEL feature naming. No forbidden synonyms ("smart link," "simple redirect," "Access Link," etc.) appear. No false capability claims from BRAND.md §1.6 (no claims about API, asset management, analytics, etc.) — the "Not for Security" and "Fallback Behavior" callouts are accurate, appropriately scoped, and match BRAND's "Honest about scope" voice attribute. The Mitti/SafetyCulture aside ("Mitti (formerly SafetyCulture)") is presented correctly as a third-party integration example, consistent with BRAND's "QRtub is NOT inspection software... links to inspection tools like SafetyCulture" positioning.
