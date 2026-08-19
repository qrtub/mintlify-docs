# Audit: QRtub for Local Councils

- Page: **QRtub for Local Councils**
- HTML URL: https://help.qrtub.com/industries/local-government-councils
- Markdown URL: https://help.qrtub.com/industries/local-government-councils.md
- llms.txt description: "QR codes on council assets for inspections, contractor access and public reporting"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`Accept: text/markdown`) | `200` |
| HTTP status (`.md` URL directly) | `200` (byte-identical response either way) |
| Content-Type (`.md`) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (`.md`) | `noindex, nofollow` |
| X-Robots-Tag (HTML) | *absent* — not sent on the HTML response |
| Content-Length (`.md`) | 12,496 bytes |
| Content-Length (HTML, default `Accept`) | 266,293 bytes |
| HTML : Markdown byte ratio | **21.3×** (266293 / 12496) |
| Markdown chars (unicode-aware) | 12,468 (28-byte gap from UTF-8 multi-byte `→`/`—`/`←`) |
| Estimated tokens (chars/4) | **~3,117 tokens** |
| Headings | 1 × H1, 13 × H2, **0 × H3** |
| Boilerplate share (index banner + footer CTA only) | **~2.65%** (banner 1.41% + footer 1.24%) |
| Structural-template share ("Ready to Deploy?" block, page-specific bullets) | ~4.64% additional |
| **Notable accuracy risk** | "Integration with Council Systems" names a probably-fabricated asset-management platform ("Molo"), miscategorizes a real playground-*equipment* manufacturer (Proludic) as inspection *software*, and names a tree-software product ("Arborcheck") with no discoverable real-world match — see §8. |

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
content-length: 12496
```

**Default request (HTML):**
```
HTTP/2 200
content-type: text/html; charset=utf-8
content-length: 266293
```
No `x-robots-tag` on the HTML response — `noindex, nofollow` is emitted only on the markdown variant, confirmed identical whether reached via the `Accept: text/markdown` header or the literal `.md` URL (both returned `content-length: 12496`, same `x-vercel-cache: HIT`/age lineage). This is the same asymmetry documented on other pages of this site (see `help__key-concepts.md`): the representation built specifically for machine consumption — and advertised via a rich `Link:` header set (`llms.txt`, `llms-full.txt`, MCP server card, agent card, agent-skills index) — is the one told not to be indexed or followed. Direct fetch by name (an agent that already has the URL, e.g. from `llms.txt`) is unaffected by this; what it suppresses is discovery via a general-purpose crawler/search index that honors robots directives, which would then only ever see the 21×-heavier HTML representation of this page.

## 3. HTML-to-Markdown ratio

266,293 bytes (HTML) / 12,496 bytes (Markdown) = **21.3×**. The HTML title (`<title>QRtub for Local Councils - QRtub documentation</title>`) and meta description (matches the llms.txt description exactly) are present and correct; the size delta is entirely Mintlify/Next.js chrome (nav, sidebar, search index data, theme script, RSC payload), not extra page content — HTML `<h1>`/`<h2>` ids match the markdown headings one-for-one (`the-challenge`, `one-code-per-asset-all-audiences-see-everything`, `the-professional-council-effect`, `multi-department-unified-access`, `bulk-deployment-across-asset-population`, `real-world-example-council-playground-equipment`, `contractor-coordination`, `public-issue-reporting-with-context`, `integration-with-council-systems`, `getting-started`, `why-councils-choose-qrtub`, `use-cases`, `ready-to-deploy`).

## 4–5. Full markdown body + token estimate

Byte count (12,496) and unicode char count (12,468) differ by 28 bytes from six `→` (2 extra bytes each), one `←` (2 extra bytes), and seven `—` em dashes (2 extra bytes each) = 12+2+14 = 28. Token estimate: 12,468 chars / 4 ≈ **3,117 tokens** for the whole page, banner and footer included. Full 277-line body was read in full; relevant passages are quoted throughout below.

There are no images in the HTML page body (only the nav-logo SVGs in the site chrome), so nothing visual is lost in the markdown conversion — the page is pure prose/lists/one code block, and the markdown is a complete, faithful representation of it.

## 6. Heading structure (in order)

| Level | Heading |
|---|---|
| H1 | QRtub for Local Councils |
| H2 | The Challenge |
| H2 | One Code Per Asset, All Audiences See Everything |
| H2 | The Professional Council Effect |
| H2 | Multi-Department, Unified Access |
| H2 | Bulk Deployment Across Asset Population |
| H2 | Real-World Example: Council Playground Equipment |
| H2 | Contractor Coordination |
| H2 | Public Issue Reporting with Context |
| H2 | Integration with Council Systems |
| H2 | Getting Started |
| H2 | Why Councils Choose QRtub |
| H2 | Use Cases |
| H2 | Ready to Deploy? |

Flat two-level structure: **zero H3s** across 277 lines. That is a real chunking-granularity problem. Underneath several H2s sit 4–6 distinct, separately-labelled sub-topics carried only as bold inline lead-ins, e.g.:
- "Multi-Department, Unified Access" contains five bolded sub-blocks (`**Parks & Reserves Tub:**`, `**Buildings & Facilities Tub:**`, `**Fleet & Equipment Tub:**`, `**Urban Forest Tub:**`, `**Infrastructure Tub:**`) that read exactly like H3s but aren't marked up as headings.
- "Real-World Example: Council Playground Equipment" contains four persona sub-blocks (`**Parks officer**`, `**Council-appointed contractor**`, `**Parent/ratepayer**`, `**Auditor**`).
- "Why Councils Choose QRtub" contains eight bolded value-prop leads; "Use Cases" contains six.

39 bold-lead-in lines total (`grep -c '^\*\*'`-style pseudo-headers) vs. 0 real H3s. A heading-based RAG chunker (chunk-per-H2, common default) would lump all five department Tubs, or all four playground personas, into one retrieval unit with no addressable sub-anchor — an agent asked "what does the Fleet & Equipment Tub track?" gets the whole "Multi-Department" section back, or nothing, depending on chunk-size limits, rather than a precise citation. Promoting these bold lead-ins to real H3s would fix this at no cost to the prose.

## 7. Boilerplate vs. page-specific content

Two blocks are confirmed **site-wide, byte-for-byte identical** boilerplate (verified against `/industries/civil-construction.md`, which carries the exact same banner and footer text):

**A. Top "Documentation Index" banner** (176 chars):
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
176 / 12,468 = **~1.41%** of this page's tokens (~44 tokens).

Confirmed via the HTML source this renders as `<blockquote class="sr-only" data-agent-docs-index="true" aria-hidden="true">` — hidden from sighted users (`sr-only`) *and* from assistive technology (`aria-hidden="true"`) in the human-facing page, while fully present as plain unhidden text in the markdown/LLM-facing representation. It sits before the real H1, so a naive outline-extractor that doesn't account for blockquote nesting could mistake "Documentation Index" for the page's title.

**B. Footer CTA + contact block** (155 chars):
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
155 / 12,468 = **~1.24%** of this page's tokens (~39 tokens).

**Combined ≈ 331 / 12,468 chars ≈ 2.65% of the page's token budget.**

**Borderline — templated wrapper, page-specific content:** the whole `## Ready to Deploy?` section (578 chars, ~4.64% of tokens) follows a fixed shape — `**Core features available:**` + a bullet list — that recurs verbatim in structure (not content) on `civil-construction.md`, which has its own, much shorter 4-item version of the same heading/lead-in pair. The bullets themselves are page-specific (council-flavoured: "Department-isolated Tubs with cross-visibility options", "Public issue reporting with asset context"), so this isn't pure filler, but it's a third recurring wrapper shape (alongside the banner and footer) that a cross-page pattern-matcher would need to recognize as "the standard capabilities-recap block," distinct from the free-form "Why Councils Choose QRtub" bullets one section above it, which cover overlapping ground in different words.

## 8. Other retrieval-quality notes

### Third-party integration list: one likely-fabricated name, one miscategorized real brand

The `## Integration with Council Systems` section states, unqualified:

> QRtub connects to the systems councils already use:
> **Asset management:** TechnologyOne, Assetic, Conquest, Molo, Pathway
> **Inspections:** Mitti (formerly SafetyCulture), Proludic (playgrounds), Arborcheck (trees)
> **Work orders:** Council CMMS platforms, contractor portals
> **GIS:** Spatial asset mapping systems
> **Compliance:** Audit management, regulatory reporting systems

I cross-checked the named products with web search:
- **TechnologyOne, Assetic, Conquest, Pathway** — all confirmed as real Australian council asset-management platforms (Assetic is used by City of Stirling/Townsville/Tweed Shire; Conquest by Shellharbour Council; Pathway by Liverpool City Council; TechnologyOne Strategic Asset Management, now powered by Assetic, has ~10 recent council customers).
- **"Molo"** — no matching council asset-management product found in search. Sitting in a list of four otherwise-verifiable real platforms, this reads as a likely fabricated fifth name.
- **Proludic** — real company, but it is a **playground-equipment manufacturer** (slides, swings, multi-play structures), not inspection software. The page lists it under "**Inspections:**" alongside Mitti — a miscategorization of a real brand into the wrong product category.
- **"Arborcheck"** — no matching tree-management/inspection software found (the real products in this space are named ArborNote, ArborPlus, Arborline, ArboStar); likely fabricated.

This is the same failure pattern already flagged on this site's `help/key-concepts` page (a fabricated "PrintCo" print partner in a worked Media example) and is exactly what `/workspace/mintlify-docs/CLAUDE.md` warns is endemic to this page type: *"Industry pages have historically been the worst offenders for overclaiming... Name their tools, but never imply QRtub integrates with them."* Beyond naming, the section's framing — "**QRtub connects to** the systems councils already use" — is the specific phrasing CLAUDE.md's "What 'integration' means here" section says never to use ("Never write 'integrates with X' in a way that implies data exchange, sync, or write-back... Accurate: 'opens the inspection in X, pre-filled with this item's ID'"). The GIS and Compliance rows compound this: unlike the Asset-management/Inspections rows, they name no product at all ("Spatial asset mapping systems", "Audit management, regulatory reporting systems") and get no worked URL-template example anywhere on the page — a vague, unfalsifiable capability claim of exactly the kind BRAND.md §3.2 tells writers to avoid. A support chatbot or ChatGPT/Perplexity/coding agent answering "does QRtub work with our council's GIS system" from this page alone would have nothing concrete to check the claim against, and could easily repeat "Molo" or "Arborcheck" back to a customer as if they were verified integration partners.

### Disclaimer stated once, relied on twice more without restatement

Line 49 states the operating model clearly: *"The buttons are visible, but they're still protected... The Page shows what's available; your existing systems control who can actually access them."* This is the load-bearing fact that prevents a reader from thinking QRtub itself stores inspection/maintenance data. It is stated exactly once. The later `## Real-World Example: Council Playground Equipment` section (lines 138–172) walks through four personas tapping "Playground Safety Inspection," "Log Maintenance," and "View Inspection History" with lines like *"Access complete inspection records"* — all consistent with the disclaimer, but only if the reader still has it in context. If a heading-based chunker (see §6) retrieves the "Real-World Example" section on its own — plausible, since it's a self-contained-looking worked scenario — the caveat that these are the *customer's own* systems, not QRtub's, is not in that chunk. A chatbot answering "does QRtub keep inspection history?" from that chunk alone risks implying QRtub is the system of record.

### Terminology and brand consistency (good)

Cross-checked against `/workspace/qrtub/GLOSSARY.md` and `/workspace/qrtub/BRAND.md`:
- "QRtub" appears 13 times, always correctly capitalized (capital QR, lowercase tub, one word) — no `QRTub`/`Qrtub` casing errors found (`grep -noE 'QR ?[Tt]ub|Qrtub|qrtub'` confirms all-clear).
- "Tub", "Page", "Destination(s)", "Link(s)" are all used and capitalized per the glossary's canonical product-term rules ("Every council asset gets a single QR code. Everyone who scans sees the SAME Page..."; "the Destinations you have set up"; "Bulk Link generation").
- Never claims QRtub *is* asset-management, inspection, maintenance, or compliance software — consistently frames these as systems QRtub routes *to* (aside from the "connects to"/"Molo"/"Proludic" issue above, which is a naming/category problem, not a category-of-claim problem).
- Never uses "BETA" (correctly retired per `mintlify-docs/CLAUDE.md`) and makes no analytics-dashboard, API, or cross-account-transfer claims.
- One minor terminology gap: the page discusses physical QR-bearing material at length ("Order asset tags," "metal plaques for playgrounds, vinyl for signs, aluminum for trees," "physical tags stay unchanged for the 20-50 year life of assets") but never once uses the glossary term "Media" — the canonical product term for exactly this concept. Not a forbidden-synonym violation (glossary doesn't ban "tag"), but it is a missed opportunity for consistent product vocabulary, and a reader/agent cross-referencing `help/media-basics.md` (which uses "Media" throughout) would not obviously connect the two pages' terminology.

### Self-containment

Reasonably strong for a narrative/persuasive industry page, with the two gaps above. The page never links to any other help doc — no link to `help/key-concepts` (which would define "Tub," "Destination," "Link," "Page" the reader is assumed to already know), no link to `help/using-fields` for the URL Template syntax it demonstrates, no "Related" section at all (unlike the help-category pages audited elsewhere on this site, which close with a "Next Steps"/"Related" links block). The only outbound links on the entire page are the pricing CTA and the support mailto. This is mitigated somewhat by the top-of-page banner instructing the retrieving agent to fetch `llms.txt` first and discover the rest of the site — but that instruction is itself boilerplate the agent must act on, not a direct link to the specific definitional page a term-lookup would need.

### Minor markdown-rendering artifacts (not visible in HTML)

- The one fenced code block (URL Template example, lines 130–132) is clean — properly triple-backtick-fenced, renders as plain text with no language tag, no trailing-whitespace or escaping issues.
- No tables anywhere on the page (`grep -c '^|'` → 0) — every list is a bullet list, which is unambiguous in plain markdown.
- No broken links, no dangling reference-style link definitions, no orphaned HTML tags. The two external links (`https://qrtub.com/pricing`, `mailto:hi@qrtub.com`) are both fully-qualified, so — unlike some other pages on this site — there are no root-relative links that would break if this markdown body were lifted out of its source context.
- The Mintlify "contextual" toolbar (confirmed in the HTML's embedded JSON: `"contextual":{"options":["copy","view","chatgpt","claude","perplexity","mcp","cursor","vscode"],"display":"header"}`) is present on this page like the rest of the site — Copy/View-as-Markdown/Open-in-ChatGPT-Claude-Perplexity/MCP/Cursor/VS-Code affordances are all wired up, which is a genuine positive for agentic retrieval infrastructure even where (as above) the content itself has accuracy gaps.
