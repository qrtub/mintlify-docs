# Audit: QRtub for Arboriculture & Tree Management (help.qrtub.com/industries/arboriculture-tree-management)

- HTML URL: https://help.qrtub.com/industries/arboriculture-tree-management
- Markdown URL: https://help.qrtub.com/industries/arboriculture-tree-management.md
- llms.txt description: "QR codes for tree populations: inspections, works history, and public transparency"
- Audited: 2026-08-19

## Summary table

| Metric                               | Value                                                                |
| ------------------------------------- | --------------------------------------------------------------------- |
| HTTP status (both variants)          | 200                                                                   |
| Content-Type (Accept: text/markdown) | `text/markdown; charset=utf-8`                                       |
| Content-Type (default Accept)        | `text/html; charset=utf-8`                                           |
| X-Robots-Tag (markdown variant)      | `noindex, nofollow` — present on both the negotiated `.md` response and the literal `.md` URL |
| X-Robots-Tag (HTML variant)          | absent (not sent)                                                     |
| Content-Length, HTML                 | 257,205 bytes                                                         |
| Content-Length, Markdown             | 11,020 bytes                                                          |
| HTML : Markdown byte ratio           | **23.3×** (257205 / 11020)                                            |
| Markdown length                      | 10,992 characters (11,020 bytes UTF-8 — 3 non-ASCII chars used: `→ ← —`) |
| Estimated tokens (chars/4)           | **~2,748 tokens**                                                     |
| Boilerplate share of page tokens     | **~3.0%** hard boilerplate (~82 of ~2,748 tokens); **~5.8%** if the templated "Core features available" footer bullets are also counted (see below) |

Both `Accept: text/markdown` on the HTML URL and the direct `.md` URL return byte-identical
bodies (11,020 bytes each, same `age`/cache state) — content negotiation is implemented
correctly. The HTML response carries no `X-Robots-Tag`, while both markdown paths carry
`noindex, nofollow` — the lightweight machine-readable twin of this page is deliberately kept
out of Google's index (avoiding duplicate-content flags against the canonical HTML page), which
is a reasonable choice but means any crawler that respects the header won't index the ~23×
lighter markdown directly; it would have to fall back to the heavier HTML or ignore the header
(most LLM retrieval fetchers do ignore `X-Robots-Tag`, since it's a `noindex` search-engine
signal, not an agent-access control).

The response headers on both HTML and markdown also advertise a `Link` header pointing to
`/llms.txt`, `/llms-full.txt`, `/.well-known/api-catalog`, `/.well-known/mcp/server-card.json`,
and `/.well-known/agent-card.json` — this site is well set up for agent discovery at the
platform level (confirmed further by the embedded page config showing Mintlify's contextual
"open in ChatGPT / Claude / Perplexity / MCP / Cursor / VSCode" menu is enabled site-wide).

## Heading structure (markdown, in document order)

```
H1  QRtub for Arboriculture & Tree Management
H2  The Challenge
H2  One Code, One Page, All Audiences See Everything
H2  The Urban Forest Dashboard Effect
H2  Bulk Deployment Across Tree Population
H2  Real-World Example: Park Tree QR Code
H2  Public Education as Core Value
H2  Integration with Tree Management Software
H2  Getting Started
H2  Why Arborists & Councils Choose QRtub
H2  Use Cases
H2  Ready to Deploy?
```

No H3/H4 headings anywhere in the body (verified: `grep -o '<h3'` on the raw HTML returns
exactly 1 hit, and it belongs to an unrelated sidebar-title UI element, not page content — the
markdown correctly has zero `###` lines). All 11 content H2s in the markdown match 1:1 with the
11 content `<h2 id="...">` anchors in the HTML (`the-challenge`,
`one-code-one-page-all-audiences-see-everything`, `the-urban-forest-dashboard-effect`,
`bulk-deployment-across-tree-population`, `real-world-example-park-tree-qr-code`,
`public-education-as-core-value`, `integration-with-tree-management-software`,
`getting-started`, `why-arborists-&-councils-choose-qrtub`, `use-cases`, `ready-to-deploy`) — the
markdown export is structurally faithful, nothing dropped or reordered.

**Sub-structure is bold text, not real headings.** Several H2 sections contain de-facto
sub-sections rendered only as **bold lead-in text**, never as H3/H4:

- *One Code, One Page...*: "**Example Page:**", "**Here's the value:**", "**Note on
  security:**", "**Public engagement meets professional operations:**"
- *Real-World Example*: "**Arborists** (during scheduled inspection):", "**Council staff**
  (checking maintenance history):", "**Park visitors** (curious about the tree):", "**Result:**"
- *Public Education as Core Value*: "**Botanical information:**", "**Local significance:**",
  "**Engagement:**"
- *Why Arborists & Councils Choose QRtub*: six bold-led value props ("Public education meets
  operations:", "Scalable across tree population:", "50-year asset lifecycle:", etc.)
- *Use Cases*: six bold-led scenarios ("Council street trees:", "Heritage trees:", "Memorial
  trees:", "Strata/commercial properties:", "School grounds:", "Storm response:")

A heading-only chunker (splitting strictly on `#`/`##`) will fuse every one of these into one
large chunk per H2 — e.g. the entire "Use Cases" section (6 distinct scenarios) becomes a single
retrievable unit, so a query about just "memorial trees" or just "storm response" pulls in the
other five scenarios too. The bold-lead-in formatting is also inconsistent (`**Term:**` vs
`**Term** (aside):`), so a regex-based sub-heading extractor would need two patterns.

## Boilerplate identified

**1. Top-of-file "agent discovery" callout (before the real H1), 175 chars / ~44 tokens (~1.6% of page):**

```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

Confirmed site-wide and byte-identical: this exact block also opens
`https://help.qrtub.com/industries/civil-construction.md`. In the HTML it is a screen-reader-only
element (`<blockquote ... data-agent-docs-index="true" aria-hidden="true">` containing an `h2`
+ two `p` tags) — invisible to human visitors, deliberately exposed only to markdown/text
consumers. Structural quirk: it contains an H2 ("## Documentation Index") that sits **before**
the page's real H1 ("# QRtub for Arboriculture & Tree Management") in document order.

**2. Footer CTA + contact block (after the `***` rule), 154 chars / ~38 tokens (~1.4% of page):**

```
**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

Also verbatim-identical on `civil-construction.md`'s footer. Present in the HTML too (same
anchor text/href) — a real, visible marketing CTA repeated at the bottom of every page, not a
markdown-only artifact.

**Combined hard boilerplate: 329 of 10,992 characters ≈ 82 of ~2,748 tokens ≈ 3.0%** of this
page's token budget is generic, repeated-on-every-page filler (agent-discovery pointer + pricing
CTA/contact footer) unrelated to arboriculture-specific content.

**3. Semi-boilerplate — "Core features available" bullet list (~314 chars / ~78 tokens / ~2.9%
of page), just above the footer CTA:**

```
**Core features available:**

* Bulk Link generation for tree populations
* Pages with public education + operational transparency
* Integration with tree inspection platforms and asset management systems
* URL Templates for tree-specific data pre-fill
* Professional presentation to ratepayers and property owners
```

The *structure* ("Ready to Deploy?" H2 → "Core features available" bullets → `***` → CTA
footer) is a template reused across industry pages, but the bullet *content* is reworded
per-industry (compare `civil-construction.md`'s equivalent list: "Bulk Link generation" /
"Pages with multiple destinations" / "Integration with Mitti, CMMS platforms" / "URL Templates
for bulk deployment with equipment IDs" — same shape, different nouns). Counting this as
boilerplate is debatable; if included, total filler rises to **~160 of ~2,748 tokens ≈ 5.8%**.

## Retrieval-quality notes

- **Self-contained, text-only:** Yes. The raw HTML has exactly 2 `<img>` tags on the whole page,
  and both are the light/dark nav logo — zero in-content screenshots, diagrams, or embedded
  media. Nothing is lost by reading the markdown instead of the HTML; there's no `<video>` or
  `<iframe>` either.
- **Absolute, working links:** Both outbound links (`https://qrtub.com/pricing`,
  `mailto:hi@qrtub.com`) are fully-qualified, so they resolve correctly even if this single
  page is retrieved with no other context (e.g., pasted into ChatGPT/Perplexity in isolation).
- **No cross-links to canonical concept pages — a real gap for isolated retrieval.** The page
  uses several capitalized QRtub product terms — "Item field," "Destination," "Page,"
  "Configure Page," "Create Tub," "URL Templates" — without ever linking to or defining them
  in-page (e.g., no link to `help/key-concepts.md`, `help/pages-overview.md`, or
  `help/custom-fields.md`, all of which exist per `llms.txt` and define exactly these terms).
  An agent that fetches only this URL (the common case for a chat citation or a single web
  search hit) has no in-page path to the entity-model definitions and could misread "Item,"
  "Tub," or "Destination" as generic English words rather than QRtub's specific data-model
  terms. The only mitigating pointer is the generic top-of-file "fetch llms.txt" banner, which
  requires the agent to already know to look for it and then do a second fetch + look-up.
- **Field-binding example does not match this page's own field names — likely to mislead a
  reader or coding agent who copies it verbatim.** The "Bulk Deployment Across Tree Population"
  section gives this URL Template:

  ```
  yourinspectionapp.com/tree?treeID={{item.assetID}}&species={{item.species}}&location={{item.location}}
  ```

  Two sections later, "Getting Started" step 3 says to "Create Tub - Define tree fields (**Tree
  ID**, Species, Location, Planted date, DBH, Height, Conservation status)" — i.e., the field
  this page tells the reader to create is named **"Tree ID,"** not "assetID." Per the field-
  binding mechanics (`{{item.fieldName}}` resolves a named Item field, and "a missing or empty
  field inserts an empty string" with no error — confirmed against
  `../qrtub/src/lib/page/bindings.ts` via the sibling `mintlify-docs/CLAUDE.md`), a reader who
  follows this page's own instructions literally (create a "Tree ID" field, then paste the URL
  Template example as shown) would get a silently-empty `treeID=` query parameter, because no
  field named "assetID" exists on their Tub. This reads like a generic template snippet
  (`item.assetID` appears verbatim in this repo's own field-binding documentation as a
  cross-industry placeholder) that wasn't localized for the tree/arborist field names this
  specific page otherwise uses consistently. This is the single highest-impact accuracy issue
  found on the page — it's copy-pasteable, plausible-looking, and wrong for this page's own
  worked example.
- **Terminology vs. GLOSSARY.md/BRAND.md — otherwise consistent.** "Link," "Page," "Tub,"
  "Item," "Destination," "URL Templates," "Configure Page," "Create Tub" all match the canonical
  glossary casing and usage. No banned synonyms ("Asset" as an entity name, "Profile Page,"
  "smart link," etc.) appear — mentions of "asset management systems" and "50-year asset
  lifecycle" refer to the customer's own domain vocabulary (council/tree assets) or third-party
  "asset management" software QRtub integrates with, which GLOSSARY.md/BRAND.md explicitly
  permit ("It integrates with asset management systems, doesn't replace them").
- **No claims that QRtub itself performs inspection, maintenance-tracking, or reporting.**
  Consistent with BRAND.md's "Claims That Are FALSE" list — every operational button
  ("Start Tree Inspection," "Log Maintenance," "View Inspection History," "Report Tree Issue")
  is described as a Destination that hands off to the arborist's own inspection app / council
  system, requiring separate login. One borderline phrase: the "Storm response" use case says
  "Reports auto-route to correct council team with tree ID and location pre-filled" — this
  reads a little like QRtub performs routing logic itself; it's more accurately a Destination
  (possibly conditional, via CEL) pointing at the right form with pre-filled data, and the page
  doesn't clarify that the "routing" happens on the receiving system, not inside QRtub. Not
  flatly false, but slightly loose phrasing next to a section of the brand guide that is
  otherwise scrupulously careful about this exact distinction.
- **Markdown well-formed, no rendering artifacts.** All bullet lists, the numbered "Getting
  Started" list, the fenced code block (the URL Template), the italicized botanical Latin names
  (`*Eucalyptus leucoxylon*`), and the `***` thematic break parse cleanly in the plain-text
  `.md` file — no broken list markers, no stray HTML, no leaked entities. The only artifact a
  plain-text (non-rendering) consumer would see is the literal `***` line before the footer,
  which renders as an `<hr>` in HTML but is just three asterisks as raw text.
- **Content-heavy, low noise otherwise.** Once the ~3–6% templated header/footer is set aside,
  essentially all remaining tokens are arboriculture-specific (tree-population workflows,
  botanical education content, named tree-inspection platforms — Arborcheck, TreePlotter,
  CONFIRM/Abacus, TreeWorks, Mitti). This is a denser, more industry-specific page than a
  generic "Use Cases" filler page would be — the low boilerplate ratio is a genuine strength for
  retrieval (few wasted tokens once the templated blocks are stripped), and the
  {{item.assetID}} mismatch is the main quality issue rather than any structural/retrieval
  defect.
