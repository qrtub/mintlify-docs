# Audit: Using Fields in Pages

- HTML URL: https://help.qrtub.com/help/using-fields
- Markdown URL: https://help.qrtub.com/help/using-fields.md
- llms.txt description: "Reference Item data, Tub information, user session, and device details in URLs and conditions"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`Accept: text/markdown`) | 200 |
| Content-Type (markdown) | `text/markdown; charset=utf-8` |
| X-Robots-Tag (markdown response) | `noindex, nofollow` |
| X-Robots-Tag (default HTML response) | *(absent — header not sent at all)* |
| Content-Length, HTML (default Accept) | 412,897 bytes |
| Content-Length, Markdown | 12,460 bytes |
| HTML : Markdown byte ratio | **33.14 : 1** |
| Markdown char count | 12,458 |
| Estimated tokens (chars/4) | **~3,115 tokens** |

## 1–2. Raw headers

**`curl -sI -H "Accept: text/markdown" https://help.qrtub.com/help/using-fields`**
```
HTTP/2 200
age: 43185
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
content-length: 12460
```

**`curl -sI https://help.qrtub.com/help/using-fields`** (default `Accept: text/html`)
```
HTTP/2 200
age: 62652
cache-control: public, max-age=0, must-revalidate
content-security-policy: worker-src * blob: data: 'unsafe-eval' 'unsafe-inline'; object-src data: ; base-uri 'self'; upgrade-insecure-requests; frame-ancestors 'self' https://dashboard.mintlify.com https://app.mintlify.com; form-action 'self' https://codesandbox.io;
content-type: text/html; charset=utf-8
etag: "yt8kjhitve8ug1"
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
content-length: 412897
```

**Notable asymmetry:** the plain-markdown response carries `x-robots-tag: noindex, nofollow`; the HTML response carries **no `x-robots-tag` header at all** (i.e., indexable by default). This means the machine-readable `.md` twin of the page is explicitly telling crawlers not to index/follow it, while the human HTML page has no such restriction. Any bot that honors `X-Robots-Tag` (Googlebot and others do; unclear how many AI-agent fetchers check it) would be directed to skip the very artifact that's cheapest and cleanest for it to consume, and instead fall back to the 33x-heavier HTML. This looks like an artifact of Mintlify's default markdown-serving config rather than an intentional per-page choice, but it works against the stated purpose of serving agents lean markdown.

## 3. HTML-to-Markdown ratio

412,897 / 12,460 = **33.14×**. The HTML payload is over 33 times larger than the markdown for identical content — expected for a Mintlify React/Next.js shell (nav chrome, client JS bundle refs, CSS) but confirms the markdown route is the correct target for any agent that can use it.

## 4–5. Markdown body / token estimate

Full body fetched via `curl -s https://help.qrtub.com/help/using-fields.md`: 12,460 bytes / 12,458 chars / 386 lines. Estimated tokens ≈ 12,458 / 4 ≈ **3,115 tokens**. That's a moderate-sized page for a single "how do I reference fields" answer — a chat agent pulling this into context pays ~3K tokens, most of which is legitimate reference tables (see below), not padding.

## 6. Heading structure (in order)

```
H1  Using Fields in Pages
H2  Two Ways to Use Fields
H3    1. URL Templates (Double Curly Braces)
H3    2. Conditional Visibility (Direct References)
H2  Available Fields
H3    Item Fields
H4      Standard Item Fields
H4      Custom Item Fields
H3    Tub Fields
H3    Session Fields
H3    Device Fields
H3    Theme Fields
H2  URL Template Examples
H3    Basic Field Insertion
H3    Inspection App Integration
H3    CMMS Integration
H3    Custom Application
H2  Conditional Visibility Examples
H3    Item Status
H3    Tags
H3    Equipment Type
H3    Combining Multiple Conditions
H3    Device-Based Conditions
H2  Common Patterns
H3    Pattern 1: Equipment-Specific Inspections
H3    Pattern 2: Location-Based Routing
H3    Pattern 3: Status-Dependent Actions
H3    Pattern 4: Device + Item Type Routing
H2  Important Notes
H2  Getting Help
H2  Related
```

One structural quirk: before the real H1, the file opens with a **blockquote containing a markdown H2** (`> ## Documentation Index`, see boilerplate section below). A heading-extraction script that naively regexes `^#{1,6} ` without checking for a leading `>` will misread the page as starting with an H2 titled "Documentation Index" rather than the real H1 "Using Fields in Pages" three lines later. Otherwise the outline itself is clean and logical: two top-level teaching sections (concepts, then the field reference) followed by worked examples, patterns, caveats, and a help/related footer — good for an agent that wants to jump to a specific H2/H3 rather than read linearly.

## 7. Boilerplate vs. page-specific content

Three blocks read as site-wide template rather than content specific to "Using Fields in Pages":

**(a) Top-of-file "Documentation Index" banner** (lines 1–3, 177 bytes ≈ 44 tokens):
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
This is generic agent-navigation scaffolding almost certainly injected identically at the top of every page's `.md` route on this site — nothing here is specific to fields, Items, or Tubs.

**(b) Bottom-of-file CTA + contact footer** (the final 5 lines, 157 bytes ≈ 39 tokens):
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
Classic marketing-site "Ready to get started?" footer CTA, unrelated to the reference-table content above it. Also somewhat redundant with the page's own "Getting Help" section four lines earlier, which already ends with "Contact support — Email hi@qrtub.com with your use case." The same support email is therefore surfaced twice in the last ~15 lines of the page.

**(c) "Related" links list** (8 lines, 454 bytes ≈ 113 tokens) — structurally a standard sitewide component (every help page appears to end with a "## Related" block of 3–5 links), though its *targets* are page-specific (Conditional Visibility, Device Detection, two integration pages, Pages Overview) so its content is legitimately useful, not filler. Counted separately from (a)/(b) because it's borderline: same recurring shape, but not throwaway text.

**Boilerplate math:**
- Pure boilerplate, (a)+(b) only: 334 bytes / 12,458 chars ≈ **2.7% of page tokens**.
- Including the templated-but-relevant Related block, (a)+(b)+(c): 788 bytes / 12,458 chars ≈ **6.3% of page tokens**.

Verdict: boilerplate overhead on this page is low in absolute terms (~44 + ~39 tokens out of ~3,115) — not a major tax — but it is 100% dead weight for an agent since it's identical (or structurally identical) across the corpus and could be stripped by any ingestion pipeline that trims a fixed header/footer template before chunking.

## 8. Other retrieval-quality notes

- **Self-containedness:** Good for its stated scope. All four field families (Item, Tub, Session, Device, plus Theme) are fully tabulated in-page with type, description, and example value — an agent doesn't need to fetch another page to answer "what fields are available" or "what does `device.isIOS` return." The page correctly cross-links out (rather than duplicating) for adjacent-but-distinct topics: full conditional-expression syntax → `/help/conditional-visibility`, device-routing depth → `/help/device-detection`, integration specifics → `/integrations/safetyculture` and `/integrations/cmms-systems`. That's the right call for keeping this page focused, but it does mean an agent answering a *conditional-expression-syntax* question (operator precedence, supported functions beyond `size()` and `in`) must follow a link rather than finding it here.
- **Undocumented expression-language name:** the page shows CEL-like expressions (`==`, `!=`, `&&`, `||`, `in`, `size(...)`) extensively but never names the underlying expression language (internally this is CEL — Common Expression Language, per the product's own docs/config). A coding agent trying to validate an expression's grammar, escaping rules, or full operator set has no anchor term here to search on or confirm against; it only has the worked examples shown. Worth adding an explicit "(CEL syntax)" mention or a link to a syntax spec if one is public.
- **Terminology consistency (checked against `/workspace/qrtub/GLOSSARY.md` and `BRAND.md`):** clean. Uses "Item" (never "Asset"), "Tub", "Destination", "Page"/"Pages" capitalized as the product term correctly throughout. The only occurrences of the word "asset" are inside third-party example URLs (`assetId=`, `/assets/{{...}}`) illustrating *external* inspection/CMMS API conventions, not QRtub's own vocabulary — appropriate, not a glossary violation.
- **Markdown rendering fidelity:** no malformed tables, code fences, or broken links found; all five tables render with consistent column counts. The one thing that only shows up in the plain-markdown rendering (not the styled HTML) is the blockquote-wrapped H2 banner noted above — in HTML this presumably renders as a styled callout box, but in raw markdown it's indistinguishable by depth-of-nesting alone from a real heading, which could confuse naive outline-extraction over the `.md` route specifically.
- **Mild content note (not a retrieval defect, but worth flagging):** the Related list links to "Mitti (formerly SafetyCulture) Integration" — confirms a product rename is in flight; if other pages/prompts still say plain "SafetyCulture" without the "formerly" qualifier, that's a cross-page consistency item outside this single-page audit's scope.
- **Positive infra note:** both HTML and markdown responses carry a `Link` header advertising `llms.txt`, `llms-full.txt`, an API catalog, an MCP server card, and an agent card/skills index — solid site-wide agent-discovery plumbing, unrelated to this specific page's content quality but good context for the overall site audit.
