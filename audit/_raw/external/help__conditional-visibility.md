# Audit: Conditional Visibility

- HTML URL: https://help.qrtub.com/help/conditional-visibility
- Markdown URL: https://help.qrtub.com/help/conditional-visibility.md
- llms.txt description: "Show or hide Destinations on Pages based on conditions"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (both HTML and `.md`) | 200 |
| Content-Type (`.md`, via `Accept: text/markdown`) | `text/markdown; charset=utf-8` |
| Content-Type (default, `Accept: text/html`) | `text/html; charset=utf-8` |
| X-Robots-Tag (`.md` response) | `noindex, nofollow` |
| X-Robots-Tag (HTML response) | **absent** (no header sent) |
| Content-Length — HTML | 317,717 bytes |
| Content-Length — Markdown | 8,751 bytes |
| HTML : Markdown byte ratio | **36.3×** (317717 / 8751) |
| Markdown word count | 1,205 words |
| Estimated tokens (chars/4) | **~2,185 tokens** (8,741 decoded chars / 4) |

Other response details worth noting:
- Both responses are served from Vercel edge cache (`x-vercel-cache: HIT`) and both advertise the same discovery `Link` headers: `</llms.txt>; rel="llms-txt"`, `</llms-full.txt>; rel="llms-full-txt"`, plus `.well-known` entries for an API catalog, MCP server card, agent card, and agent-skills index — good machine-discoverability signal at the transport level.
- The HTML response is cached publicly (`cache-control: public, max-age=0, must-revalidate`) while the markdown response is marked `private, no-cache` — the markdown copy is never held in a shared/browser cache, only Vercel's own edge cache (age was already 43,138s / ~12 hours at fetch time, so it is being served stale-ish from edge despite the `no-cache` client directive).

## Heading structure (in document order)

```
H1  Conditional Visibility
H2  When You Need This
H2  Common Use Cases
  H3  1. Equipment Type-Specific Inspections
  H3  2. Tag-Based Routing
  H3  3. Test Status-Based Routing
H2  Using AI to Generate Conditions
  H3  Example Prompt Template
  H3  Example: Multiple Tags
H2  Advanced: Device-Specific Destinations
  H3  Mobile App vs Desktop Links
  H3  iOS Safari Workaround (Mitti Example)
H2  Available Fields
  H3  Quick Device Field Reference
H2  Tips
H2  When NOT to Use This
H2  Getting Help
H2  Related
```

No skipped heading levels (H1→H2→H3 throughout); hierarchy is clean and would chunk sensibly for RAG.

## Boilerplate / repeated-across-pages content

Two clearly templated, non-page-specific blocks were found:

**1. Top-of-file "Documentation Index" nudge** (lines 1-3 of the markdown, appears before the H1):

> `> ## Documentation Index`
> `> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt`
> `> Use this file to discover all available pages before exploring further.`

This is a Mintlify-injected block, not authored content — it is generic instruction text pointing an agent at `/llms.txt` and is presumably identical on every page of the site. ~176 chars ≈ **44 tokens**.

**2. Footer CTA + support line** (last 4 lines of the file, after the `Related` section and a `***` rule):

> `**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)`
>
> `Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)`

This is a standard marketing/support footer, unrelated to conditional visibility specifically, and almost certainly repeated verbatim across all (or most) help articles. ~154 chars ≈ **38 tokens**.

**Combined boilerplate: ~82 tokens out of ~2,185 total ≈ 3.75% of the page's token budget.** Not huge in isolation, but it recurs on every page an agent ingests, so it compounds across a multi-page crawl (e.g., an agent pulling 20 help pages pays this ~82-token tax 20 times for identical content).

A borderline third case: the "Getting Help" section ends with "Need help? Contact [hi@qrtub.com](mailto:hi@qrtub.com) with your use case and we can help you construct the right condition." This duplicates the footer's "Questions? Email us at hi@qrtub.com" contact prompt within the same page (two separate invitations to email support, ~30 lines apart). Not cross-page boilerplate, but an internal redundancy.

The "## Related" section (4 links: Using Fields, Device Detection, Pages Overview, Key Concepts) is templated in *pattern* (most/all pages likely end with a "Related" list) but its *content* is page-specific and genuinely useful, so it was not counted as boilerplate.

## Other retrieval-quality notes

- **Metadata consistency is good**: the markdown's subtitle blockquote ("Show or hide Destinations on Pages based on conditions") exactly matches the llms.txt description passed in — no drift between the two.
- **Relative links won't resolve out of context.** The three "Related"/inline cross-references use root-relative paths with no domain: `/help/using-fields`, `/help/device-detection`, `/help/pages-overview`, `/help/key-concepts`. On the live HTML site these resolve fine against `help.qrtub.com`, but if this markdown file is fetched standalone (e.g., pasted into a different chat, or read by a coding agent without surrounding page context), an LLM has to *infer* the `https://help.qrtub.com` prefix rather than being given it. A safer pattern for llms.txt-style markdown mirrors is to emit absolute URLs for every link.
- **Code fences have no language tag.** All CEL expression examples (`item.type == "forklift"`, `device.isDesktop || !device.isIOS || ...`, etc.) and the AI prompt template are in bare ``` ``` ``` fences with no `cel`/`text` language hint. Harmless for meaning, but it means syntax highlighting is lost and a coding agent can't easily distinguish "this is a CEL expression to copy verbatim" from "this is a prompt template to fill in" purely from fence metadata — it has to rely on the surrounding prose.
- **Self-contained, but deliberately partial by design.** The page explains CEL basics, gives 3 concrete use-case recipes, a full device-field quick reference, and an AI-prompt-generation workflow — genuinely useful for an agent to act on without leaving the page. However it explicitly defers the *complete* Item/Tub/Session field list to "Using Fields" (`/help/using-fields`), so an agent asked "what fields can I use in a condition besides device.*" cannot fully answer from this page alone and must follow the (relative) link above.
- **Terminology matches the canonical glossary** (`/workspace/qrtub/GLOSSARY.md`): "Destination," "Page," "Item," "Tub" are all used correctly and capitalized per the glossary; no instances of deprecated terms ("Asset," "Profile Page," "Action Link," etc.) were found. "QRtub" brand casing is correct throughout (checked against `/workspace/qrtub/BRAND.md` — capital QR, lowercase tub, one word) except inside the literal URL `https://qrtub.com/pricing`, which is correctly lowercase per the glossary's URL rule.
- **One real-world third-party claim to verify against BRAND.md's truth table**: the page states "Mitti (formerly SafetyCulture)" and gives `iauditor://` as its deep-link scheme. BRAND.md's "Claims That Are TRUE" list only says QRtub "links to inspection tools like SafetyCulture" — it doesn't itself assert the Mitti rebrand, so this specific factual detail (SafetyCulture → Mitti rename, and the `iauditor://` scheme) sits outside what BRAND.md/GLOSSARY.md can confirm or deny; flagging for a human to verify it's still accurate, since a wrong deep-link scheme in a doc an AI agent might paste into generated code would silently break the customer's link.
- **No malformed markdown rendering was observed.** Nested lists, code fences, bold/italic emphasis, and the blockquote-then-heading transition at the top all render cleanly as plain markdown; nothing that renders fine in HTML (e.g., via custom Mintlify components) appears broken or stripped down in the `.md` version.
- **`noindex, nofollow` is scoped to the markdown representation only.** Because the header appears solely on the `Accept: text/markdown` response and not on the default HTML response, general web/AI search-index crawlers hitting the canonical HTML URL are still indexed normally; the directive only tells crawlers not to index the raw `.md` mirror itself. This is very likely intentional (avoids duplicate-content indexing of the HTML/MD pair) and shouldn't block an agent that fetches the `.md` URL on direct instruction — `noindex`/`nofollow` governs crawler-driven discovery/indexing, not one-off fetches — but it does mean the markdown mirror won't be independently surfaced by a search-driven AI answer engine that relies on its own crawl index rather than live fetching.
