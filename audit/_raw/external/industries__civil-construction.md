# Retrieval Audit — QRtub for Civil Construction

- Page: QRtub for Civil Construction
- HTML URL: https://help.qrtub.com/industries/civil-construction
- Markdown URL: https://help.qrtub.com/industries/civil-construction.md
- llms.txt description: "One QR code per machine, linking to inspections, maintenance and operator manuals"
- Audited: 2026-08-19

## Summary data table

| Metric | Value |
|---|---|
| HTTP status (both variants) | 200 |
| Content-Type (`Accept: text/markdown`) | `text/markdown; charset=utf-8` |
| Content-Type (default `Accept: text/html`) | `text/html; charset=utf-8` |
| X-Robots-Tag (markdown variant) | `noindex, nofollow` |
| X-Robots-Tag (html variant) | **absent** (not sent on the HTML response at all) |
| Content-Length — HTML | 293,589 bytes |
| Content-Length — Markdown | 7,746 bytes |
| HTML : Markdown byte ratio | **37.9 : 1** (293589 / 7746 = 37.90) |
| Markdown body — Unicode chars | 7,732 |
| Markdown body — word count | 1,069 |
| Estimated tokens (chars/4 heuristic) | **~1,933 tokens** |
| Estimated tokens (words × ~1.3, alt. heuristic) | ~1,390 tokens |

Other headers of note (both variants): `link:` header advertises `</llms.txt>; rel="llms-txt"`, `</llms-full.txt>; rel="llms-full-txt"`, plus `.well-known` api-catalog / mcp-server-card / agent-card / agent-skills entries — good machine-discoverability signaling at the HTTP layer, present consistently on both content types.

## Heading structure (in order)

```
H1  QRtub for Civil Construction
H2  The Construction Challenge
H2  How QRtub Helps Construction Teams
  H3  One Code Per Asset
  H3  Print Before Deployment
  H3  Update Without Reprinting
  H3  Different Information for Different People
H2  Real-World Construction Use Cases
  H3  Heavy Equipment Fleet
  H3  Multi-Site Deployment
  H3  Mixed Equipment Sources
H2  Integration with Construction Software
  H3  Inspection Platforms
  H3  CMMS & Maintenance
  H3  Compliance & Safety
  H3  Fleet Management
H2  Getting Started with Construction Equipment
  H3  Step 1: Generate Links
  H3  Step 2: Order Professional Codes
  H3  Step 3: Deploy on Equipment
  H3  Step 4: Connect to Systems
  H3  Step 5: Update as Needed
H2  Why Construction Companies Choose QRtub
  H3  Designed for Physical Deployments
  H3  No Equipment Downtime
  H3  Future-Proof Investment
  H3  Professional Presentation
H2  Common Construction Scenarios
  H3  Daily Pre-Start Inspections
  H3  Service Due Notification
  H3  New Operator Onboarding
  H3  Equipment Transfer Between Sites
H2  Ready to Deploy?
```

Note: there is also a pseudo-heading at the very top of the markdown body (`> ## Documentation Index`) — see "Malformed / structural" below. It's not counted in the outline above because it isn't page content; it's an injected agent-discovery nudge (see next section).

31 real headings (1×H1, 10×H2, 20×H3) for ~1,930 tokens of body — a reasonably dense, well-segmented outline. Every H3 is a short, scannable label, which is good for an agent doing selective-section retrieval/chunking.

## Boilerplate vs. page-specific content

Two boilerplate blocks were identified, both **verified present verbatim** (byte-for-byte) and both clearly not authored specifically for this page:

### 1. The "Documentation Index" agent nudge (top of file)

Exact text:
> `> ## Documentation Index`
> `> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt`
> `> Use this file to discover all available pages before exploring further.`

- 175 characters — **2.3% of the page's total tokens**.
- Confirmed via the HTML source that this is a site-wide injected element, not page-authored content: in the HTML it renders as `<blockquote class="sr-only" data-agent-docs-index="true" aria-hidden="true">...</blockquote>`. That is, it's deliberately hidden from sighted users (`sr-only`) *and* from assistive tech (`aria-hidden="true"`) — a combination that only a raw-DOM-reading crawler/agent (not a screen reader, not a human) will ever encounter in the HTML rendering. In the markdown variant it surfaces as visible blockquote text at the very top of the body. This is clearly a deliberate, site-wide (not page-specific) mechanism aimed at steering agents toward `llms.txt`, present identically on every page.

### 2. Footer CTA block

Exact text:
> `***`
>
> `**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)`
>
> `Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)`

- 154 characters — **2.0% of the page's total tokens**.
- Generic, product-level CTA with no civil-construction-specific content; the `hr` + bold-question + pricing-link + "Questions? Email us" pattern reads exactly like a template block copy-pasted across every industry/vertical page on the site (confirmed in the HTML: it's inline in the page's compiled MDX/JSX, i.e. authored per-page but templated by copy-paste, not componentized).

### 3. Semi-templated section (borderline — not counted in the % above)

The `## Ready to Deploy?` heading + `**Core features available:**` bullet list immediately before the footer CTA follows what reads as a standard per-industry-page template (heading wording and bullet-list shape repeated, only the bullet *contents* vary per industry — here: "Bulk Link generation," "Pages with multiple destinations," "Integration with Mitti, CMMS platforms," "URL Templates for bulk deployment with equipment IDs"). Content is civil-construction-flavored so it isn't pure boilerplate, but the section's presence/shape is predictable across pages. Including this heading+list plus the footer CTA together = 360 chars (~4.7% of tokens); adding the top doc-index nudge brings the combined "templated/boilerplate" share to **~6.9% of the page's total tokens** — modest, not a major tax on this page's budget.

No "Related articles" / "See also" list is present on this page at all (neither templated nor page-specific) — see self-containment notes below.

## Retrieval-quality notes

**Self-containment gaps:**
- The body uses capitalized, product-specific nouns — **Link**, **Page**, **Item**, **URL Template** — as if already known to the reader (e.g. "Use custom IDs matching your equipment numbering," "Set up Pages linking to your inspection app," "auto-populate equipment IDs from QRtub Item fields"). There is **zero internal cross-linking** anywhere in the body to a concepts/glossary page that defines these terms. The only two outbound links in the entire body are `https://qrtub.com/pricing` and `mailto:hi@qrtub.com` (plus the boilerplate `/llms.txt` link at the top). An agent that fetches only this URL (e.g., a ChatGPT/Perplexity browse action landing here from search) has no path to the primary-concept definitions and must guess at what a "Page" vs. a "Link" is from context alone.
- `CMMS` is used four times and never expanded (Computerized Maintenance Management System) or linked; `Mitti`, `UpKeep`, `Fiix`, `Maintenance Connection` are named with no descriptor beyond category, assuming reader familiarity.

**Internal factual inconsistency (same page, two different labels for the same tool):**
- Line ~30: `**Mitti (formerly SafetyCulture) inspections**`
- Line ~123: `**Mitti (iAuditor)** - Pre-start inspections, daily checks, incident reporting`

Within a single ~1,900-token page, the tool "Mitti" is glossed two different ways ("formerly SafetyCulture" vs. "(iAuditor)") with no indication of how the two labels relate (sequential rebrand history? alternate legacy names?). An agent extracting either sentence in isolation to answer "what is Mitti" or "does QRtub integrate with SafetyCulture" would produce a different, non-reconciled answer depending on which line it retrieved. This is a concrete citation-accuracy risk for a support chatbot summarizing or quoting this page.

**Terminology/claim-accuracy check against `/workspace/qrtub/GLOSSARY.md` and `/workspace/qrtub/BRAND.md`:**
- Product nouns (**Link**, **Page**, **Item**, **URL Templates**) are used correctly and match the canonical glossary throughout — no instances of deprecated synonyms ("Access Link," "Asset," "profile page" capitalized, etc.).
- Glossary's own rule of thumb assigns physical-production/printing language ("Order Professional Codes," bulk-ordering vinyl stickers/metal plaques, replacement across ownership types) to the term **Media** ("Physical asset tracking," "Replacement workflows," "Production/batch management" rows all map to "Media" in GLOSSARY.md's QR-code-vs-Media table). This page never uses "Media" once — it stays in "QR code / sticker / plaque" language even when describing bulk ordering and physical deployment, which is exactly the context GLOSSARY.md earmarks for the "Media" term. Minor, but a drift point if "Media" terminology is meant to be standardized across docs.
- The bullet **"Integration with Mitti, CMMS platforms"** (in the "Ready to Deploy?" feature list) risks overstating scope: BRAND.md's "Claims That Are FALSE" list explicitly rules out "QRtub tracks maintenance history (it links to systems that do)" and lists "API access" as **Planned**, not available. "Integration with X" read out of context by an agent could be repeated as "QRtub integrates with UpKeep/Fiix" implying a native connector/API, when the mechanism described elsewhere on the same page is just a configurable URL destination (URL Templates auto-populating a query param) — i.e., "connects to" / "links to," not a certified software integration. Worth softening the word "Integration" here to match BRAND.md's preferred "connects to / links to" framing used correctly elsewhere on the same page (e.g., "QRtub connects your physical equipment to the digital tools you already use").

**Rendering artifacts specific to the plain-markdown variant (not visible in HTML):**
- The top-of-file "Documentation Index" block renders in Markdown as a blockquoted heading — `> ## Documentation Index` — which is unconventional/uncommon Markdown (a heading marker nested inside a blockquote marker). Most Markdown parsers will render this fine as a blockquote containing an H2, but it is easy for naive line-based chunkers (e.g., something that finds headings via a regex like `^#{1,3}\s`) to **miss this as a heading** (since the line starts with `> ##`, not `##`), or conversely for something that finds `> #` blockquote-headings to misfile it as page content when it's actually a global nav aid. In the HTML rendering, by contrast, this content is a fully separate hidden accessibility-tree element (`sr-only`, `aria-hidden="true"`) that a human or screen-reader user never sees at all — so the plain-markdown reader experiences this text as visible, in-band body content immediately before the real H1, while the HTML reader never encounters it. This is the one clear "shows differently across formats" artifact found.
- Everything else in the Markdown body maps cleanly 1:1 to the HTML content (bullets, bold, three-column "Operators / Maintenance teams / Site managers see" lists, numbered Getting Started steps) — no broken tables, no stray HTML leaking into the `.md`, no truncation.

**Other observations:**
- No "Related pages" / "See also" list exists on this page (a genuine absence, not boilerplate to discount) — meaning an agent has no page-level signal pointing it to sibling industry pages (Marine, Lifesaving, Equipment Hire per BRAND.md §"Early Industries") or to foundational concept pages. Combined with the self-containment gap above, this page depends entirely on the reader already having consulted `llms.txt` or another entry point for QRtub fundamentals.
- HTML-to-Markdown ratio of ~38:1 is a strong signal that the `.md` content-negotiated route is doing its job — an agent fetching with `Accept: text/markdown` gets ~97.4% less payload for materially the same information, and the reduction is due to standard Next.js/Mintlify chrome (nav, scripts, RSC payload) rather than any content loss.
- The `X-Robots-Tag: noindex, nofollow` sent only on the markdown-negotiated response (absent on the HTML response) is worth flagging to whoever owns the Mintlify config: any crawler that respects `X-Robots-Tag` and fetches with `Accept: text/markdown` (as an increasing number of AI/agent crawlers do, by design, to get cheaper content) will be told not to index or follow links from this representation, while the same crawler fetching default HTML gets no such instruction. If this is intentional (e.g., to avoid duplicate-content indexing of the `.md` URL variant in traditional search) it's reasonable; if it's accidental, it could suppress exactly the AI-agent traffic this `.md` route exists to serve.
