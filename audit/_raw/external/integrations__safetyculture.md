# Retrieval Audit — Mitti (formerly SafetyCulture / iAuditor)

- HTML URL: https://help.qrtub.com/integrations/safetyculture
- Markdown URL: https://help.qrtub.com/integrations/safetyculture.md
- llms.txt description: "Open Mitti inspections straight from a scan, pre-filled with the item's data. Mitti was called SafetyCulture, and iAuditor before that."
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | 200 |
| HTTP status (HTML, default `Accept`) | 200 |
| Content-Type (`.md` request) | `text/markdown; charset=utf-8` |
| Content-Type (HTML request) | `text/html; charset=utf-8` |
| Content-Length — HTML | 296,494 bytes |
| Content-Length — Markdown | 9,411 bytes |
| HTML : Markdown byte ratio | **31.5×** |
| X-Robots-Tag (`.md` request) | `noindex, nofollow` |
| X-Robots-Tag (HTML request) | *(absent — not sent at all)* |
| Word count (markdown body) | 1,193 words |
| Character count (markdown body) | 9,381 chars |
| Estimated tokens (chars/4) | **~2,345 tokens** |
| Boilerplate share of tokens (top index nudge + bottom CTA only) | ~83 tokens ≈ **3.5%** |

Both requests returned `200`. Both responses (HTML and markdown) advertise the same discovery links via the `Link:` header (`rel="llms-txt"`, `rel="llms-full-txt"`, `rel="mcp-server-card"`, `rel="agent-card"`, `rel="agent-skills"`, `rel="api-catalog"`).

## Heading structure (in order)

```
H1  Mitti (formerly SafetyCulture / iAuditor)
H2    If you know it as SafetyCulture or iAuditor
H2    Overview
H2    Integration Method
H2    Getting Your Template ID
H2    Basic Setup: Start Inspection
H3      Mobile App Deep Link
H3      Web App Deep Link
H3      Using QRtub URL Templates
H2    Advanced: Pre-Fill Inspection Questions
H3      Mobile App Pre-Fill Format
H3      Pre-Fill Multiple Questions
H2    App Not Installed? Set a Fallback URL
H2    Additional Deep Link Options
H3      View Inspection Report
H3      Edit Existing Inspection
H3      Open Asset Profile
H3      View Document/File
H2    Use Cases
H2    Troubleshooting
H2    Resources
```

Note: above the H1, the response body also contains an injected `> ## Documentation Index` line — a level-2 heading, but wrapped inside a blockquote (`>`), so it precedes the real H1 and sits outside the document's own outline (same pattern confirmed on `/help/app-links.md`, `/integrations/cmms-systems.md`, and `/help/pages-overview.md` — this is a sitewide Mintlify injection, not page content).

Outline otherwise is clean: one H1, no skipped levels, H3s correctly nested under their parent H2s. **However**, two sections use bold inline text as pseudo-subheadings instead of real Markdown headings:
- Under **Use Cases**: `**Equipment Inspections**`, `**Facility Inspections**`, `**Multi-Audience Routing**` (not H3s)
- Under **Troubleshooting**: `**Mobile app doesn't open:**`, `**Web link doesn't work:**`, `**Data not pre-filling:**`, `**Users need access:**` (not H4s)

A heading-based chunker/retriever that splits on `#`/`##`/`###` markers will lump all of "Equipment Inspections," "Facility Inspections," and "Multi-Audience Routing" into one large "Use Cases" chunk (and likewise all four failure modes into one "Troubleshooting" chunk) rather than exposing them as individually addressable sections — a minor structural cost for precision retrieval (e.g. a query about "web link doesn't work" gets the whole troubleshooting block, not just its own bullet).

## Boilerplate / repeated-across-pages content

Two blocks are template injection rather than page-specific content, bracketing the actual article (verified byte-identical against `/integrations/cmms-systems.md` and `/help/pages-overview.md`):

**1. Top-of-file index nudge (before the H1), 176 chars ≈ 44 tokens:**
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

**2. Bottom-of-file CTA + contact line (after `***`), 155 chars ≈ 39 tokens:**
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

**Combined: 331 chars ≈ 83 tokens out of ~2,345 total ≈ 3.5% of the page's token budget.** Modest overhead, but pure filler for an agent already routed to this specific page — it doesn't need a nudge to go re-fetch `llms.txt`, nor a pricing CTA injected into a technical answer about deep-link syntax.

**Partially-recurring (not counted above, flagged separately):** two of the five `## Resources` links — `[Pages Overview](/help/pages-overview)` and `[Key Concepts](/help/key-concepts)` — appear verbatim on `/integrations/cmms-systems.md` too, suggesting a standard "always link these two" pattern across integration pages. The other three Resources entries (two `help.mitti.com` links and `/help/app-links`) are page-specific. If these two recurring lines are added to the boilerplate tally, the total rises to ~410 chars ≈ 103 tokens ≈ 4.4%.

## Retrieval-quality notes

1. **Markdown response is `noindex, nofollow`; the HTML response sends no `X-Robots-Tag` at all.** `curl -I -H "Accept: text/markdown"` returns `x-robots-tag: noindex, nofollow`; the plain HTML request returns no such header (indexable by default). This is backwards for the stated purpose of the `.md` endpoint — it's the artifact meant for AI agents/crawlers, yet it's the one flagged non-indexable/non-followable. Any crawler that honors `X-Robots-Tag` will decline to index this page's markdown or follow its links, while indexing the much heavier, harder-to-parse HTML version instead.

2. **The rendered HTML is not plain readable text-in-tags — it's a Next.js RSC/hydration payload.** Grepping the raw HTML response for a distinctive string like "Getting Your Template ID" or the example template ID finds it, but embedded inside serialized `_jsx(_components.code,{children:...})` call syntax, not as a clean DOM text node a naive scraper (curl + regex, a lightweight readability extractor) could reliably pull out. This is very likely *why* the HTML page is 296KB vs. the markdown's 9.4KB (31.5× ratio) — most of the HTML bytes are JS scaffolding, font preloads, and serialized props, not article content. It strongly reinforces that the `.md` endpoint is the correct integration point for any agent that isn't executing a full browser/React runtime.

3. **All in-page navigational links to sibling docs are root-relative with no base domain**: `/help/app-links`, `/help/pages-overview`, `/help/key-concepts` all omit scheme/host. Verified these resolve correctly as absolute-path links against `help.qrtub.com` (200 OK each) — so they are *not* broken in-situ. But if this markdown is ingested standalone (pasted into a chat, pulled into a RAG index without preserving source origin), all three internal cross-references silently break for a reader/agent that doesn't know to resolve them against `https://help.qrtub.com`.

4. **Page is otherwise well self-contained.** It disambiguates the three-name history (iAuditor → SafetyCulture → Mitti) in the first section before using any of the names, explains the deep-link mechanism plainly ("No data is exchanged between the two systems; QRtub builds the URL and Mitti handles the rest"), gives concrete worked examples for every deep-link type (start inspection, pre-fill single/multiple questions, view report, edit inspection, asset profile, fallback setup), and includes a troubleshooting section. An agent could answer most support questions from this page alone. The two external `help.mitti.com` links (for "getting entity IDs") are the only content genuinely deferred to a third party, and the page still explains the *mechanism* (Template ID lives in the web-app URL) inline before pointing out.
   - Both `https://help.mitti.com/en-US/000076/` and `https://app.mitti.com` were checked live and resolve (308/200 respectively) — no dead external links on this page.

5. **No HTML-only artifacts leak into the markdown, and nothing is structurally malformed** — no stray raw HTML tags, unclosed code fences, or broken tables. The one GFM table (Recommended fallback setup: App link / Fallback URL) renders cleanly. Code fences around every deep-link example are present and correctly triple-backtick-delimited.

6. **Product terminology matches QRtub's canonical glossary and brand file.** Cross-checked against `/workspace/qrtub/GLOSSARY.md` and `/workspace/qrtub/BRAND.md`:
   - "Item," "Tub," "Destination," "Page," and the `{{item.field}}` / `{{tub.field}}` double-brace binding syntax are all used correctly and consistently (no single-brace bindings found).
   - Brand name appears correctly as "QRtub" throughout — no "QRTub"/"QR Tub" slips.
   - No capitalized "Profile Page"-style glossary violations.
   - Matches `CLAUDE.md`'s "What integration means here" rule precisely: the page explicitly states "No data is exchanged between the two systems," never implies sync/write-back, and never claims QRtub replaces Mitti/SafetyCulture's inspection functionality — consistent with `BRAND.md`'s "Claims That Are FALSE" list (`QRtub replaces SafetyCulture`).
   - The field-encoding caveat ("QRtub inserts field values exactly as stored and never URL-encodes them") matches the verified behavior documented in the docs repo's own `CLAUDE.md` ("Values are inserted exactly as stored — there is no automatic URL encoding"). No terminology or capability-claim defects found on this page.

7. **Naming drift is intentional and well-flagged, but still a risk for chunked retrieval.** The page opens with a dedicated section disambiguating "Mitti," "SafetyCulture," and "iAuditor" as one product under three historical names, and explains why `iauditor://` and `app.safetyculture.com` still work. This is good practice — but if a retrieval system chunks the page below the H1 (e.g. splits at each `##`) and a user's query says "SafetyCulture" while a later chunk only says "Mitti," the disambiguating context from the "If you know it as SafetyCulture or iAuditor" section may not be in the same context window as the chunk that actually answers the question.

8. **Response header hygiene is otherwise strong for agent consumption**: correct `Content-Type: text/markdown; charset=utf-8`, `Content-Disposition: inline` (not forcing a download), and discovery breadcrumbs via `Link: rel="llms-txt"` / `rel="llms-full-txt"` / `rel="agent-card"` / `rel="mcp-server-card"` / `rel="agent-skills"` / `rel="api-catalog"` on every response — a genuinely agent-friendly touch most docs sites lack. The `noindex, nofollow` on the `.md` variant (note 1) is the one header-level defect.
