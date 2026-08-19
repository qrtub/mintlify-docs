# Audit: CMMS Systems Integration

- HTML URL: https://help.qrtub.com/integrations/cmms-systems
- Markdown URL: https://help.qrtub.com/integrations/cmms-systems.md
- llms.txt description: "Connect QR codes to your maintenance system using deep links and URL Templates"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
| --- | --- |
| HTTP status (both variants) | 200 |
| Content-Type (`Accept: text/markdown`) | `text/markdown; charset=utf-8` |
| Content-Type (default `Accept: text/html`) | `text/html; charset=utf-8` |
| X-Robots-Tag (markdown response) | `noindex, nofollow` ⚠️ |
| X-Robots-Tag (HTML response) | *(header absent — not sent at all)* |
| Content-Length, HTML | 252,299 bytes |
| Content-Length, Markdown | 4,233 bytes |
| HTML : Markdown byte ratio | **59.6 : 1** (252299 / 4233 = 59.60) |
| Word count (markdown body) | 544 words |
| Estimated token count (chars/4) | **~1,058 tokens** |

Full raw response headers are reproduced below for the record.

**`curl -sI -H "Accept: text/markdown" https://help.qrtub.com/integrations/cmms-systems`**
```
HTTP/2 200
age: 43289
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
x-vercel-cache: HIT
content-length: 4233
```

**`curl -sI https://help.qrtub.com/integrations/cmms-systems`** (default Accept)
```
HTTP/2 200
age: 4209
cache-control: public, max-age=0, must-revalidate
content-security-policy: worker-src * blob: data: 'unsafe-eval' 'unsafe-inline'; object-src data: ; base-uri 'self'; upgrade-insecure-requests; frame-ancestors 'self' https://dashboard.mintlify.com https://app.mintlify.com; form-action 'self' https://codesandbox.io;
content-type: text/html; charset=utf-8
etag: "5z5otl1bzd5ekb"
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
server: Vercel
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-llms-txt: /llms.txt
x-mintlify-client-version: 0.0.3469
x-nextjs-prerender: 1
x-nextjs-stale-time: 60
x-vercel-cache: HIT
x-vercel-project-id: prj_3kakCEKDVpOxnQIJmKyTWs83RXEa
x-version: dpl_DRdAt1tmryV3mWMHqNwiMeLsBe72
content-length: 252299
```

`<title>` (HTML `<head>`): `CMMS Systems Integration - QRtub documentation`
`<meta name="description">`: matches the llms.txt description exactly.
`<link rel="canonical">`: `https://help.qrtub.com/integrations/cmms-systems` (correct, self-referential).

## Heading structure (markdown body, in order)

```
H1  CMMS Systems Integration
H2    Overview
H2    Integration Approaches
H3      Option 1: Direct Deep Links (If Supported)
H3      Option 2: Web-Based Access
H3      Option 3: URL Templates
H2    Example Setup: UpKeep
H3      Create Page Destinations
H2    Multi-System Page
H2    Common CMMS Platforms
H2    Best Practices
H2    Troubleshooting
H2    Resources
```

One H1, clean strict-nesting (no skipped levels, no H2 appearing after an H3 unexpectedly). That said, see the "pseudo-headings" note below — several structurally-important subsections are **not** real headings at all.

## Boilerplate vs. page-specific content

Fetched a sibling page (`https://help.qrtub.com/integrations/safetyculture.md`) to confirm which blocks are templated across pages rather than specific to this one. Confirmed identical byte-for-byte boilerplate in three places:

**1. Top-of-file "Documentation Index" banner (176 bytes) — identical on both pages, verbatim:**
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
This sits *above* the real H1 and is itself marked up as an H2 inside a blockquote (see malformation note below).

**2. Bottom-of-file CTA block (157 bytes) — identical on both pages, verbatim:**
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

**3. Two of the three "Resources" bullets (78 bytes) — identical on both pages, verbatim:**
```
* [Pages Overview](/help/pages-overview)
* [Key Concepts](/help/key-concepts)
```
On the CMMS page these are 2 of the section's 3 bullets (the third, "Contact your CMMS vendor for deep link documentation," is page-specific but is plain text, not a link). On the SafetyCulture page the same two generic links appear appended after that page's own specific resource links, confirming this is a standard "Related links" tail rather than curated per-page.

**Boilerplate total: 176 + 157 + 78 = 411 bytes out of 4,233 bytes total ≈ 9.7% of the page's tokens**, delivering zero CMMS-specific information. (If the whole "## Resources" section header and wrapper are counted too, it nudges slightly higher, but the substantive boilerplate content is ~10%.)

For a page whose llms.txt-declared purpose is narrowly "connect QR codes to your maintenance system using deep links and URL Templates," roughly 1 in 10 tokens an agent pays to ingest is generic site chrome (index-discovery notice, pricing CTA, contact email, two generic doc links) rather than CMMS integration content. Not severe, but a measurable tax repeated across (at minimum) every page in the `/integrations/` section, and probably sitewide.

## Retrieval-quality notes

**1. `X-Robots-Tag: noindex, nofollow` on the machine-readable endpoint specifically.** The `.md` response carries `noindex, nofollow`; the HTML response for the identical URL sends no `X-Robots-Tag` header at all. `robots.txt` is otherwise permissive site-wide (`Content-Signal: ai-train=yes, search=yes, ai-input=yes`, no `Disallow` for `/integrations/`). This is an inconsistency worth flagging: the URL the site's own `llms.txt`/`Link: rel="llms-txt"` mechanism is actively advertising as the preferred machine-readable representation is simultaneously told (via a header some crawlers/agents do respect) not to be indexed and not to have its links followed. If any AI crawler honors `X-Robots-Tag` per-URL, it could end up excluding the clean markdown mirror from its index while still indexing the much noisier HTML page — the opposite of what an `llms.txt`-style setup is meant to achieve. Worth confirming with Mintlify whether this is intentional (e.g., avoiding duplicate-content indexing of the `.md` twin) or a platform default that undercuts the llms.txt strategy.

**2. Pseudo-headings hidden inside bold text, invisible to heading-based chunking/outline extraction.** Several structurally important subsections are formatted as bold inline text rather than real Markdown headings, so any agent that chunks by heading level (a common RAG pattern) will merge these into their parent H2/H3 with no addressable anchor:
   - Under "Example Setup: UpKeep" / "Create Page Destinations": **View Asset Record**, **Create Work Order**, **View Maintenance History** (three distinct URL patterns, no sub-heading distinguishing them)
   - Under "Best Practices": **Field Mapping**, **User Testing**, **Vendor Lock-in Protection**
   - Under "Troubleshooting": **"Page not found" errors:**, **Authentication issues:**
   
   A question like "how do I fix authentication issues with my CMMS link" would retrieve the whole Troubleshooting section (both sub-topics) rather than a scoped chunk, and a coding agent looking to distinguish the three UpKeep URL patterns has to parse bold-then-code-block adjacency rather than reading discrete headings.

**3. Blockquoted H2 sits above the real H1.** The "## Documentation Index" boilerplate block (line 1) is itself a level-2 heading, nested inside a blockquote, appearing *before* the page's actual H1 ("# CMMS Systems Integration"). A naive heading-structure parser that doesn't account for blockquote-nesting would report the document's first heading as an H2, with the "real" title only appearing second — a minor but real structural wrinkle for any tool doing pure regex heading extraction rather than a proper Markdown AST parse.

**4. Content is otherwise self-contained and faithfully mirrors the HTML.** Checked the raw HTML for Mintlify interactive components (Tabs, Accordion, CodeGroup, Card, Steps, Frame) that sometimes fail to flatten cleanly into `.md` — found none embedded as component markers in this page's HTML, and the rendered HTML text (asset ID examples, platform table, troubleshooting list) matches the markdown 1:1. No images, no embedded video, no JS-only content; nothing is lost by consuming the markdown instead of the HTML.

**5. Both "Related-links"-style internal links resolve.** `/help/pages-overview` and `/help/key-concepts` both return HTTP 200 (checked directly), so no broken-link risk for an agent that tries to follow them for more context. Note the URLs live under a `/help/` prefix that doesn't match this page's own `/integrations/` prefix — mildly inconsistent site IA, but not a retrieval defect since it resolves.

**6. Terminology consistency (checked against `/workspace/qrtub/GLOSSARY.md` and `BRAND.md`).** The page correctly says "Item field" and `{{item.cmmsAssetID}}` / `{{item.assetID}}` rather than misusing QRtub's own glossary term "Asset" for the QRtub-side concept (glossary explicitly reserves "Item," not "Asset," for that — "qrtub doesn't position itself as asset management"). The field names `cmmsAssetID`/`assetID` are the *external* CMMS's vocabulary being mapped into an Item field, which is the correct pattern and doesn't contradict the glossary. "Destination"/"Destinations," "Page," and "URL Templates" are all used per-canon. The page's closing claim — "if you switch CMMS vendors, update Destinations in QRtub—don't reprint QR codes" — matches BRAND.md's approved positioning (vendor lock-in protection / "print once, change forever") and correctly avoids the disallowed claim that QRtub replaces or *is* a CMMS ("What QRtub IS NOT: A maintenance tracking system... It connects to CMMS systems, doesn't track maintenance itself"). No brand or terminology violations found on this page.

**7. Size/ratio context.** The 59.6:1 HTML-to-Markdown ratio reflects standard Mintlify SPA chrome (nav, JS bundles, framework payload) rather than anything specific to this page's content bloat — the markdown itself, at ~1,058 estimated tokens, is a reasonably compact, single-topic page once the ~10% boilerplate tax (see above) is set aside.
