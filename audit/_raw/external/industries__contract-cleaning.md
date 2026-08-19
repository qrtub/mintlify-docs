# Retrieval Audit — QRtub for Contract Cleaning

- HTML URL: https://help.qrtub.com/industries/contract-cleaning
- Markdown URL: https://help.qrtub.com/industries/contract-cleaning.md
- llms.txt description: "QR codes across client facilities for inspections, issue reporting and service records"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`Accept: text/markdown` on HTML path) | 200 |
| HTTP status (`.md` URL) | 200 |
| HTTP status (HTML, default `Accept`) | 200 |
| Content-Type (`.md` / markdown-negotiated request) | `text/markdown; charset=utf-8` |
| Content-Type (HTML request) | `text/html; charset=utf-8` |
| Content-Length — HTML | 242,119 bytes |
| Content-Length — Markdown | 7,618 bytes |
| HTML : Markdown byte ratio | **31.8×** |
| X-Robots-Tag (markdown responses) | `noindex, nofollow` |
| X-Robots-Tag (HTML response) | *(absent — not sent at all)* |
| Word count (markdown body, incl. boilerplate) | 1,020 words |
| Decoded character count | 7,578 chars (7,618 bytes — 40 bytes of multibyte UTF-8, e.g. the `→` arrow) |
| Estimated tokens (chars/4) | **~1,895 tokens** |
| Boilerplate share of tokens (top index nudge + bottom CTA) | ~82 tokens ≈ **4.3%** |
| Internal cross-links / "Related" section | **None — zero outbound links to any other help/industry/integration page** |

Requesting the HTML path with `Accept: text/markdown` content-negotiates to the same markdown body as the dedicated `.md` URL (identical 7,618-byte payload, identical headers). Both markdown-serving responses carry `Link:` discovery headers (`rel="llms-txt"`, `rel="llms-full-txt"`, `rel="mcp-server-card"`, `rel="agent-card"`, `rel="agent-skills"`, `rel="api-catalog"`) — also present on the plain HTML response.

## Heading structure (in order)

```
H1  QRtub for Contract Cleaning
H2    The Challenge
H2    One Code, One Page, All Audiences See Everything
H2    The Restaurant Kitchen Effect
H2    Bulk Deployment Across Facilities
H2    Real-World Example: Public Restroom QR Code
H2    Integration with Cleaning Software
H2    Getting Started
H2    Why Cleaning Companies Choose QRtub
H2    Ready to Deploy?
```

Flat outline: **no H3s anywhere on the page.** One H1, nine H2s, no skipped levels — structurally valid, but every section is a peer of every other, so a heading-based chunker gets no sub-topic granularity (e.g. "Getting Started"'s 6-step numbered list and "Why Cleaning Companies Choose QRtub"'s 5 bullet-point differentiators are each a single flat chunk).

As with other audited pages, an injected `> ## Documentation Index` blockquote line precedes the real H1 (see Boilerplate section) — a level-2 heading wrapped in `>` so it sits outside the document's own outline, ahead of the title.

## Boilerplate / repeated-across-pages content

Two blocks bracket the actual article, matching the same site-wide template seen on other help.qrtub.com pages:

**1. Top-of-file index nudge (before the H1), 175 chars ≈ 44 tokens:**
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

**2. Bottom-of-file CTA + contact line (after `***`), 154 chars ≈ 38 tokens:**
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

**Combined: ~329 chars ≈ 82 tokens out of ~1,895 total ≈ 4.3% of the page's token budget.** Consistent with the App Links page audited previously — a modest, non-egregious tax, but pure overhead once an agent has already been routed to this specific page.

**Notably absent here, unlike other audited pages:** there is no `## Related` section (or any equivalent links block) at all. This page has **zero outbound links** to any other documentation page — not to `/help/pages-overview`, `/help/creating-your-first-link`, `/help/custom-fields`, `/help/conditional-visibility`, `/integrations/mitti` (the exact integration it names in prose), or any sibling `/industries/*` page. The only URLs in the entire body are the top boilerplate's `llms.txt` reference, one bare (non-hyperlinked) code-formatted example domain (`` `qrtub.com/office-301` ``), and the bottom pricing/mailto CTA. For an agent doing multi-hop retrieval or trying to point a user to "how do I actually set up a Destination/Tub/Page," this page is a dead end with no forward references — worse than merely having broken relative links, since there's nothing to follow at all.

## Retrieval-quality notes

1. **Markdown response is `noindex, nofollow`; the HTML response is not** — same asymmetry documented on other pages in this audit series. `curl -I -H "Accept: text/markdown"` and the `.md` URL both return `x-robots-tag: noindex, nofollow`; the plain HTML request returns no `X-Robots-Tag` header at all. The artifact built for agents/crawlers is the one flagged non-indexable/non-followable, while the heavier HTML version is indexable by default.

2. **~48% of the page's body prose (about 472 of 979 words) restates one single insight three separate times** under three different narrative wrappers: "One Code, One Page, All Audiences See Everything" (216 words, the "guerrilla marketing" facility-manager anecdote), "The Restaurant Kitchen Effect" (110 words, the glass-walls-into-the-kitchen metaphor), and "Real-World Example: Public Restroom QR Code" (146 words, a step-by-step restroom-scan walkthrough) all make the identical point — *visible operational buttons impress curious members of the public and convert them into leads.* No new mechanism, feature, or fact is introduced across the second and third tellings. This is page-specific bloat rather than site-wide boilerplate, but it has the same practical effect on an agent: roughly half the token budget is spent on one repeated marketing claim rather than on how the product works. By contrast, the sections that actually explain mechanics — "Bulk Deployment Across Facilities" (53 words) and "Integration with Cleaning Software" (43 words) — are the shortest sections on the page.

3. **Internal numeric inconsistency in the illustrative facility count.** "Bulk Deployment Across Facilities" says "Configure once. Deploy to 100 facilities," the very next paragraph says "Switch inspection apps across all **80** facilities with one Destination update," and the closing section says "Deploy to 10, 50, or 100+ facilities." These are presented as the same illustrative scenario but use three different counts (100 / 80 / "10, 50, or 100+"). Harmless for a human skimming, but an agent asked "how many facilities does the example cover" or asked to quote a specific number from this page has no single correct answer to extract.

4. **"URL Templates" is capitalized throughout as if it were a canonical product term, but it is not in the glossary.** The page writes "**URL Templates example:**", "URL Templates auto-populate facility details," and "Deploy to 10, 50, or 100+ facilities with URL Templates" — parallel in capitalization to the genuine glossary term **Page Template**. `/workspace/qrtub/GLOSSARY.md` has no "URL Template" entry, and `/workspace/mintlify-docs/CLAUDE.md` refers to the same mechanism only descriptively, lowercase, as "field bindings in URL templates" (a Link/Destination configured with `{{item.field}}` bindings — not a distinct named feature or UI object). An agent treating "URL Templates" as a proper noun could hallucinate a dedicated "URL Templates" panel/entity in the product that doesn't exist. The actual binding syntax used in the one code example (`` {{item.facilityID}} ``, `` {{item.facilityName}} ``) is correct per `../qrtub/src/lib/page/bindings.ts` — double-brace, `item.` namespace — so the mechanism is described accurately even though its capitalized label is not a real glossary term.

5. **The lone code block has no language tag** (bare ` ``` ` fence around the `yourform.com/inspection?facility={{item.facilityID}}&name={{item.facilityName}}` example). Doesn't break markdown rendering, but it's a plain violation of this repo's own stated authoring standard ("Language tags on all code blocks," `/workspace/mintlify-docs/CLAUDE.md`) and loses a small syntax-highlighting/semantic-hint signal for any tool that treats fenced code specially.

6. **Brand-voice drift: the page reads as considerably more hyped/salesy than the documented voice guidelines call for.** `/workspace/qrtub/BRAND.md` states the brand personality is "quietly confident... doesn't need to shout" and explicitly lists "Make vague claims... without specifics" and "Overstate current capabilities" under Never. This page repeatedly uses ALL-CAPS emphasis for effect ("sees the **SAME** branded Page with **ALL** the buttons," "sees **ALL** the operational buttons") and language BRAND.md's own tone examples treat as too casual/hyped elsewhere ("guerrilla marketing," "genius," "weaponize it," "qualified sales lead/lead"). None of this is factually false, but it's a tonal outlier versus the help-page voice, and an agent quoting this page verbatim in a support answer or a sales conversation would sound noticeably more marketing-heavy than QRtub's stated voice.

7. **No false or Planned-feature capability claims found.** Checked against `/workspace/qrtub/BRAND.md` §1.6 (Claims That Are FALSE): the page correctly frames third-party inspection apps as separate systems requiring their own login ("reach your inspection app (Mitti, Swept, etc.) which requires login credentials"), doesn't claim QRtub performs inspections, tracks maintenance history, offers an API, or provides analytics dashboards. "Basic Media tracking" / cross-account transfer / permissions are not mentioned, so no Planned-feature overclaim there either.

8. **Third-party naming is handled consistently and correctly.** "Mitti (formerly SafetyCulture, and iAuditor before that)" is disclosed once, up front, in the "Integration with Cleaning Software" section, and "Mitti" is used consistently afterward (including in "Core features available"). This matches the disambiguation pattern seen on the App Links page and avoids the exact-string-matching risk noted there.

9. **Other product terminology matches the canonical glossary.** "Tub," "Page," "Destination," "Link," "Item" are all used correctly and capitalized per `/workspace/qrtub/GLOSSARY.md` (e.g., "Create Tub," "Configure Page," "one Destination update," "Generate Links"). No "Profile Page," "Asset," or "QRTub"/"QR Tub" violations found. Brand name renders correctly as "QRtub" throughout.

10. **No malformed markdown, no stray HTML tags, no broken tables** — there are no tables on this page at all (unlike the App Links page). No unclosed code fences; the single fence pair is well-formed. No raw `<...>` HTML detected in the plain-markdown body.

11. **Self-containment is weaker than the App Links page audited previously**, precisely because of point 2 (repetition crowding out substance) combined with the missing Related section (see Boilerplate notes above): an agent using only this page could answer "why would a cleaning company want this" reasonably well (the pitch is made three times, after all), but would struggle to answer "how do I actually configure a Destination," "what is a Tub," or "where do I read about conditional visibility for staff vs. client audiences" — concepts the page name-drops (Tub, Destination, Page) without linking to, or fully defining, the pages that explain them.

12. **Response header hygiene is otherwise strong**, matching the site-wide pattern: correct `Content-Type: text/markdown; charset=utf-8`, `Content-Disposition: inline`, and the full set of discovery `Link:` headers on every response variant tested.
