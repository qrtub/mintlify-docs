# Retrieval Audit — App Links & Fallback URLs

- HTML URL: https://help.qrtub.com/help/app-links
- Markdown URL: https://help.qrtub.com/help/app-links.md
- llms.txt description: "Use deep links to open mobile apps directly, with automatic fallback when the app isn't installed"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | 200 |
| HTTP status (HTML, default `Accept`) | 200 |
| Content-Type (`.md` request) | `text/markdown; charset=utf-8` |
| Content-Type (HTML request) | `text/html; charset=utf-8` |
| Content-Length — HTML | 262,938 bytes |
| Content-Length — Markdown | 7,107 bytes |
| HTML : Markdown byte ratio | **37.0×** |
| X-Robots-Tag (`.md` request) | `noindex, nofollow` |
| X-Robots-Tag (HTML request) | *(absent — not sent at all)* |
| Word count (markdown body) | 930 words |
| Estimated tokens (chars/4, 7,107 chars) | **~1,777 tokens** |
| Boilerplate share of tokens (top index nudge + bottom CTA) | ~82 tokens ≈ **4.6%** |

Both requests returned `200`. The markdown endpoint additionally advertises discovery links via the `Link:` response header (`rel="llms-txt"`, `rel="llms-full-txt"`, `rel="mcp-server-card"`, `rel="agent-card"`, `rel="agent-skills"`, `rel="api-catalog"`) — present on both the HTML and markdown responses.

## Heading structure (in order)

```
H1  App Links & Fallback URLs
H2    What Are App Links?
H2    The Problem: App Not Installed
H2    How QRtub Handles App Links
H2    Setting a Fallback URL
H2    Setting a Custom Fallback Message
H2    Using Bindings in Fallback URLs
H2    Where Fallback Settings Live
H2    Common Examples
H3      Mitti (iAuditor)
H3      Generic Enterprise App
H3      App Download Link (No Web Version)
H2    App Links vs Device Detection
H2    Related
```

Note: above the H1, the response body also contains an injected `> ## Documentation Index` line — a level-2 heading, but wrapped inside a blockquote (`>`), so it precedes the real H1 and sits outside the document's own outline. See quality notes below for why this matters to naive chunkers.

The outline itself is clean otherwise: no skipped levels, one H1, logical grouping, H3s correctly nested under the "Common Examples" H2.

## Boilerplate / repeated-across-pages content

Two blocks are template injection rather than page-specific content, bracketing the actual article:

**1. Top-of-file index nudge (before the H1), 175 chars ≈ 44 tokens:**
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
This is a Mintlify-wide injection (confirmed present verbatim on other help.qrtub.com pages during prior audits in this series) — not specific to App Links.

**2. Bottom-of-file CTA + contact line (after `***`), 154 chars ≈ 38 tokens:**
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
Generic marketing CTA + support email, unrelated to app links/deep-linking specifically — the exact same "Ready to get started with QRtub?" block is the site-wide footer pattern.

**Combined: ~329 chars ≈ 82 tokens out of ~1,777 total ≈ 4.6% of the page's token budget.** This is a modest tax — not egregious — but it's pure overhead for an agent that has already been routed to this specific page (e.g. via llms.txt or a support-bot retrieval) and doesn't need to be told to go read llms.txt again or to see a pricing CTA in the middle of a technical answer about fallback URLs.

**Not counted as boilerplate:** the `## Related` section (4 links: Mitti Integration, Device Detection, Conditional Visibility, Pages Overview). Although "a Related-links block" is a structural pattern likely repeated across many pages, its *contents* here are specific and genuinely relevant to app-links/fallback behavior (not a generic sitewide link list), so it's counted as page-specific content, not filler.

## Retrieval-quality notes

1. **Markdown response is `noindex, nofollow`; the HTML response is not.** `curl -I -H "Accept: text/markdown"` returns `x-robots-tag: noindex, nofollow`, while the plain HTML request returns no `X-Robots-Tag` header at all (i.e., indexable by default). This is backwards for the stated use case: the `.md` endpoint is exactly the artifact meant for AI agents/crawlers, yet it's the one flagged non-indexable/non-followable. Any crawler that honors `X-Robots-Tag` (several AI-search crawlers do) will decline to index or follow links from the version built for it, while indexing the heavier HTML version instead.

2. **All in-page navigational links are root-relative, with no base domain.** Every link to another doc (`/integrations/safetyculture`, `/help/device-detection` ×3, `/help/conditional-visibility`, `/help/pages-overview`) is written as a bare path with no scheme/host. Only the external pricing link and the `mailto:` are absolute. If this markdown is ingested standalone (pasted into a chat, pulled into a RAG index without preserving source origin, fetched by a generic reader that doesn't resolve relative to `https://help.qrtub.com`), all six internal cross-references silently break — an agent citing "see /help/device-detection" without the domain gives the end user a dead reference.

3. **Page is otherwise well self-contained.** It defines the concept ("app links" = custom-scheme URLs) before using it, gives concrete examples with real scheme syntax (`iauditor://…`, `spotify://…`), explains the exact mechanism (2.5-second timer, fallback precedence), documents the three-level override hierarchy (Destination > Link > Item) via a clear table, and gives three worked "Common Examples." An agent could answer most support questions from this page alone without needing sibling pages loaded.

4. **No HTML-only artifacts leak into the markdown, and nothing is malformed structurally** — no stray raw HTML tags, unclosed code fences, or broken tables. One cosmetic nit: the "App Links vs Device Detection" comparison table has inconsistent internal padding on one row (`Wrong browser on iOS (app links blocked)      | [Device Detection routing]` — extra spaces before the pipe) — harmless for both human GFM rendering and markdown parsers, but a signal of hand-edited/inconsistent table source.

5. **Third-party naming drifts across the page in a way worth flagging for consistency, not correctness.** The same integration is called "Mitti (formerly SafetyCulture)" on first mention, then "Mitti" and "Mitti (iAuditor)" later, with the actual link path still `/integrations/safetyculture` and the app-link scheme example still `iauditor://…`. The page discloses the rename up front so a human reader tracks it fine, but an agent doing exact-string matching against a user's phrase ("SafetyCulture" vs "Mitti" vs "iAuditor") could treat these as three different products if only skimming a chunk that lacks the opening disambiguation sentence.

6. **Product terminology matches QRtub's canonical glossary.** Cross-checked against `/workspace/qrtub/GLOSSARY.md` and `BRAND.md`: "Destination," "Link," "Item," and bindings syntax (`{{item.field}}`) are all used correctly and consistently; brand name appears correctly as "QRtub" throughout (no "QRTub"/"QR Tub" slips); no capitalized "Profile Page"-style violations. No terminology defects found on this page.

7. **Response header hygiene is otherwise strong for agent consumption**: correct `Content-Type: text/markdown; charset=utf-8`, `Content-Disposition: inline` (not forcing a download), and discovery breadcrumbs via `Link: rel="llms-txt"`/`rel="llms-full-txt"`/`rel="agent-card"`/`rel="mcp-server-card"` on every response — a genuinely agent-friendly touch that most docs sites lack.
