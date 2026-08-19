# Audit: QRtub for Electrical Test and Tag

- HTML URL: https://help.qrtub.com/industries/electrical-test-and-tag
- Markdown URL: https://help.qrtub.com/industries/electrical-test-and-tag.md
- llms.txt description: "QR codes on tested equipment, linking to your test platform and compliance records"
- Audit date: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (Accept: text/markdown) | 200 |
| HTTP status (default Accept: text/html) | 200 |
| Content-Type (markdown request) | `text/markdown; charset=utf-8` |
| Content-Type (html request) | `text/html; charset=utf-8` |
| X-Robots-Tag (markdown request) | `noindex, nofollow` |
| X-Robots-Tag (html request) | *absent* (not sent) |
| Content-Length — HTML | 269,069 bytes |
| Content-Length — Markdown | 11,817 bytes |
| HTML : Markdown byte ratio | **22.77 : 1** |
| Markdown char count (decoded) | 11,787 chars |
| Estimated tokens (chars/4) | **~2,950 tokens** |
| Boilerplate share of page tokens | **~3.2%** (~95 tokens) |

Full raw headers are reproduced below for traceability.

### 1. `curl -sI -H "Accept: text/markdown" .../electrical-test-and-tag`

```
HTTP/2 200
age: 43282
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
content-length: 11817
```

Note: content negotiation via `Accept: text/markdown` on the extensionless URL works — it returns the same markdown body as the explicit `.md` URL (same content-length, same `x-matched-path` internal route). Good: an agent that only respects `Accept` headers (rather than URL-rewriting to `.md`) still gets clean markdown.

### 2. `curl -sI .../electrical-test-and-tag` (default `Accept: text/html`)

```
HTTP/2 200
age: 56925
cache-control: public, max-age=0, must-revalidate
content-security-policy: worker-src * blob: data: 'unsafe-eval' 'unsafe-inline'; object-src data: ; base-uri 'self'; upgrade-insecure-requests; frame-ancestors 'self' https://dashboard.mintlify.com https://app.mintlify.com; form-action 'self' https://codesandbox.io;
content-type: text/html; charset=utf-8
etag: "4mhykcks5m5rdx"
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", ...
server: Vercel
x-frame-options: DENY
x-llms-txt: /llms.txt
x-mintlify-client-version: 0.0.3469
x-nextjs-prerender: 1
x-vercel-cache: HIT
content-length: 269069
```

No `x-robots-tag` header appears on the HTML response at all — the `noindex, nofollow` directive is applied **only** to the markdown/agent-facing representation, not the human-facing HTML page.

### 3. Ratio

269,069 bytes (HTML) / 11,817 bytes (Markdown) = **22.77×**. The HTML payload is almost 23 times heavier than the plain-markdown alternative for materially the same content — consistent with a typical Mintlify/Next.js SPA shell (React hydration payload, nav/sidebar chrome, inline JS) versus a clean content extract. This is a strong argument for any agent to prefer the `.md` endpoint or `Accept: text/markdown`.

### 4–5. Markdown body + token estimate

Fetched via `curl -s https://help.qrtub.com/industries/electrical-test-and-tag.md`. 11,787 characters decoded (11,817 bytes — the ~30-byte gap is multi-byte UTF-8 characters: em dashes "—", arrows "→"). At ~4 chars/token that's **~2,950 tokens** for the full page body, including the injected header/footer boilerplate described below.

## 6. Heading structure (in order)

```
H1  # QRtub for Electrical Test and Tag
H2  ## The Challenge
H2  ## Two Use Cases, Same Solution
H3    ### Use Case 1: In-House Testing (Company Manages Own Equipment)
H3    ### Use Case 2: Contract Testing (Service Provider Offers Client Access)
H2  ## The Professional Compliance Register Effect
H2  ## Bulk Deployment and Ongoing Management
H2  ## Real-World Example: Contract Provider Serving Office Building
H2  ## Integration with Test and Tag Software
H2  ## Getting Started
H3    ### For In-House Teams:
H3    ### For Contract Providers:
H2  ## Why Test and Tag Operations Choose QRtub
H2  ## Use Cases
H2  ## Ready to Deploy?
```

Hierarchy is clean and monotonic — no skipped levels (H1→H2→H3 only), each H2 is a self-contained topical section. This is good for chunked retrieval (e.g. a RAG pipeline splitting on headings would get coherent, non-overlapping chunks).

## 7. Boilerplate content (repeated across pages, not page-specific)

Verified by diffing against `https://help.qrtub.com/industries/civil-construction.md`, which carries **byte-identical** copies of both blocks below (only the page-specific bullet lists between them differ).

**a) Header block (top of every page, 176 chars / ~44 tokens):**

```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```

This is a Mintlify-injected agent-discovery hint (mirrors the `Link: </llms.txt>; rel="llms-txt"` HTTP header) — it's aimed at AI consumers specifically, not at human readers of the HTML page, and is not part of the HTML rendering at all (confirmed: it's absent from the human-facing page, only appears in the markdown/agent representation).

**b) Footer CTA block (bottom of every page, 154 chars / ~39 tokens):**

```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```

**c) Semi-boilerplate section scaffold (49 chars / ~12 tokens):**

The `## Ready to Deploy?` heading + the `**Core features available:**` lead-in line are identical wording on both pages audited; only the bullet list underneath is page-specific (this page: 5 bullets about bulk link generation, client-isolated Tubs, branded Pages, test-platform deep links, URL Templates; civil-construction: a different 4-bullet list). So the section *frame* is templated, the *content* is not.

**Total boilerplate:** 176 + 154 + 49 = 379 characters ≈ **95 tokens**, against a total page body of ~2,950 tokens → **≈3.2% of this page's tokens are non-page-specific boilerplate.** That's a small fraction — this page is mostly unique, substantive content. (The header block is arguably a feature rather than a cost for agent retrieval, since it actively points the agent to the full site index; only the footer CTA is "pure" repeated marketing boilerplate, and it alone is only ~1.3% of tokens.)

No standard "Related articles" / "Next steps" link list appears on this page (unlike some docs sites) — the only repeated structural elements are the two above.

## 8. Other retrieval-quality notes

- **Self-contained:** Yes, largely. The page defines its own two use cases (in-house vs. contract provider) with concrete worked examples, a real-world scenario, a bulk-deployment/URL-Template code example, and a "Getting Started" checklist. A support chatbot or coding agent could answer most user questions about this industry vertical from this page alone without needing sibling pages. It does lean on reader familiarity with QRtub product nouns (**Tub**, **Page**, **Link**, **URL Templates**) that are defined more fully elsewhere (`help/key-concepts.md`, `help/pages-overview.md`, `help/using-fields.md`) — an agent answering questions using *only* this page in isolation (e.g. a RAG chunk without the rest of the site) would understand *what to do* but might not fully understand the underlying entity model (e.g., that a "Tub" is a category-based workspace, not just "a register"). The page does give a serviceable inline gloss for Tub ("Each client gets their own Tub (isolated equipment register)"), which mitigates this.
- **Terminology consistency with GLOSSARY.md / BRAND.md:** Good. Uses "Link", "Tub", "Page", "Item" (as "equipment item") correctly and capitalized where the glossary expects. Correctly refers to "Mitti (formerly SafetyCulture)" rather than the deprecated name only. Does not violate any BRAND.md "Claims That Are FALSE" (no claim of being asset-management/inspection/compliance software — third-party test platforms and OH&S systems are consistently framed as external integrations QRtub links to, not features QRtub provides). One minor terminology gap: the glossary's canonical term for the physical printed material is "Media", but this page (and the industry generally) uses "test tag(s)" / "tags" throughout — reasonable per BRAND.md's "use industry terminology, change the nouns not the verbs" rule, but it means an agent cross-referencing this page with `help/media-basics.md` would need to infer that "test tag" ≈ "Media" in this context; the page never makes that equivalence explicit.
- **Malformed-in-plain-markdown artifact:** Two instances of an unnecessary backslash-escaped ampersand — `OH\&S` (line "**Auditor** (during OH\&S inspection):" and "**Compliance tracking:** OH\&S systems, audit management platforms"). This renders correctly as "OH&S" through a markdown renderer (the backslash just escapes a character that didn't need escaping), but it does **not** show up as a problem in the HTML view (already rendered to "OH&S" there) — it's only visible/latent in the raw markdown export. Any agent or pipeline that treats the `.md` payload as plain text without running it through a markdown parser (e.g., a naive text-search index, or an LLM asked to quote the source verbatim) will surface the literal string "OH\&S" with a stray backslash. Low severity, but a genuine plain-markdown-only defect invisible to a human auditing the HTML page.
- **robots signal split between representations:** `X-Robots-Tag: noindex, nofollow` is sent only on the markdown representation (both via `Accept: text/markdown` and the `.md` URL); the HTML page has no `X-Robots-Tag` at all. This is presumably deliberate (avoids the `.md` URL being separately indexed by Google as duplicate content), and doesn't block direct fetches by user-driven agents (ChatGPT/Claude/Perplexity fetching a URL a user pasted, or a coding agent's `curl`/`WebFetch`, ignore `noindex` — it's a search-indexing directive, not a fetch-blocking one). But it is worth flagging: an AI crawler that *does* respect `noindex` when discovering pages by crawling (rather than being pointed at a URL directly) could skip indexing the clean markdown alternative entirely, even though `Link: rel="llms-txt"` and the in-body "Documentation Index" blockquote are actively inviting that same class of crawler to explore further. The two signals (llms.txt discovery vs. noindex on the discovered content) point in slightly different directions for crawler-type agents, though not for direct-fetch agents.
- **Content negotiation works cleanly:** requesting the extensionless URL with `Accept: text/markdown` returns byte-identical content to the explicit `.md` URL (same `content-length: 11817`, same internal `x-matched-path`). Good — agents don't need to know the `.md` convention specifically; the `Accept` header alone is sufficient.
- **No code fences/HTML leakage in the markdown:** the one code-like block (URL Template example, `yourtestapp.com/test?equipmentID={{item.assetID}}...`) is inside a proper fenced code block and renders as plain text — no stray HTML tags, JSX artifacts, or broken tables were found in the markdown body.
