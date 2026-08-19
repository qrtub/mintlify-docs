# Agent-Retrieval Audit — Custom Fields

- **HTML URL:** https://help.qrtub.com/help/custom-fields
- **Markdown URL:** https://help.qrtub.com/help/custom-fields.md
- **llms.txt description:** "Define the fields your Items need, set allowed values and defaults, and link Items to other records"
- **Audited:** 2026-08-19

## Summary table

| Metric | Value |
| --- | --- |
| HTTP status (both variants) | 200 |
| Content-Type (`Accept: text/markdown`) | `text/markdown; charset=utf-8` |
| Content-Type (default `Accept`) | `text/html; charset=utf-8` |
| X-Robots-Tag (markdown variant) | `noindex, nofollow` |
| X-Robots-Tag (HTML variant) | *(absent — not sent)* |
| Content-Length, HTML | 241,108 bytes |
| Content-Length, Markdown | 5,035 bytes |
| HTML : Markdown byte ratio | **47.9×** (241108 / 5035 = 47.89) |
| Estimated tokens (markdown, chars/4) | **~1,259 tokens** |
| Headings | 1×H1, 10×H2, 0×H3 |
| Boilerplate share (banner + footer CTA only) | ~334 bytes ≈ 83 tokens ≈ **6.6%** of page tokens |

## 1–2. Raw header dumps

**`curl -sI -H "Accept: text/markdown" https://help.qrtub.com/help/custom-fields`**
```
HTTP/2 200
content-disposition: inline
content-security-policy: default-src 'none'
content-type: text/markdown; charset=utf-8
link: </llms.txt>; rel="llms-txt", </llms-full.txt>; rel="llms-full-txt", </.well-known/api-catalog>; rel="api-catalog", </.well-known/mcp/server-card.json>; rel="mcp-server-card", </.well-known/agent-card.json>; rel="agent-card", </.well-known/agent-skills/index.json>; rel="agent-skills"
strict-transport-security: max-age=63072000
vary: rsc, next-router-state-tree, next-router-prefetch, next-router-segment-prefetch
x-frame-options: DENY
x-llms-txt: /llms.txt
x-matched-path: /_mintlify/_markdown/_sites/[subdomain]/[[...slug]]
x-robots-tag: noindex, nofollow
content-length: 5035
```

**`curl -sI https://help.qrtub.com/help/custom-fields`** (default Accept: text/html)
```
HTTP/2 200
cache-control: public, max-age=0, must-revalidate
content-security-policy: worker-src * blob: data: 'unsafe-eval' 'unsafe-inline'; object-src data: ; base-uri 'self'; upgrade-insecure-requests; frame-ancestors 'self' https://dashboard.mintlify.com https://app.mintlify.com; form-action 'self' https://codesandbox.io;
content-type: text/html; charset=utf-8
etag: "2jzuu88f3755z4"
link: (same llms.txt / api-catalog / mcp-server-card / agent-card / agent-skills set as above)
strict-htransport-security: max-age=63072000
x-frame-options: DENY
x-llms-txt: /llms.txt
x-matched-path: /_sites/[subdomain]/[[...slug]]
x-mintlify-client-version: 0.0.3469
x-nextjs-prerender: 1
x-nextjs-stale-time: 60
content-length: 241108
```

No `x-robots-tag` header appears on the HTML response at all — it is only sent on the markdown variant.

## 3. HTML : Markdown ratio

241,108 / 5,035 = **47.89×**. The markdown twin is ~98% smaller than the rendered HTML — a large efficiency win for any agent that knows to request it (via `.md` suffix or `Accept: text/markdown`) instead of scraping/parsing the HTML.

## 4–5. Markdown body and token estimate

Full body fetched from `https://help.qrtub.com/help/custom-fields.md` (5,035 bytes, matches the content-negotiated response exactly). Estimated tokens at chars/4 ≈ **1,259 tokens** — a small, cheap page to ingest.

## 6. Heading structure (in order)

- **H1:** Custom Fields
  - H2: Where fields are configured
  - H2: Core fields and custom fields
  - H2: Field keys
  - H2: Field types
  - H2: Allowed values
  - H2: Defaults
  - H2: Reference fields
  - H2: Required fields and validation
  - H2: How fields get used elsewhere
  - H2: Related

No H3s anywhere on the page. The heading set is flat (H1 + H2 only), which is good for chunking — every section is a clean, equally-weighted retrieval unit; no deeply nested subsections to lose context on.

## 7. Boilerplate content

Two blocks read as boilerplate injected on every page rather than content specific to Custom Fields:

**(a) Top-of-file "Documentation Index" banner** (lines 1–3, 177 bytes ≈ 44 tokens ≈ **3.5%** of page tokens):

> ```
> > ## Documentation Index
> > Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> > Use this file to discover all available pages before exploring further.
> ```

This is a blockquote prepended to the markdown output, presumably by Mintlify's agent-facing tooling, on every page site-wide — not specific to Custom Fields at all.

**(b) Bottom CTA + contact footer** (lines 118–123, 157 bytes ≈ 39 tokens ≈ **3.1%** of page tokens):

> ```
> ***
>
> **Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)
>
> Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
> ```

This exact CTA + "Questions? Email us at hi@qrtub.com" pairing is the kind of standard footer block that almost certainly repeats verbatim across every help article on the site (a generic pricing pitch has no particular connection to the Custom Fields topic).

**Combined (a)+(b): 334 bytes ≈ 83 tokens ≈ 6.6% of this page's ~1,259 estimated tokens** are pure repeated boilerplate carrying zero page-specific information.

**Borderline / not counted as boilerplate:** the `## Related` section (lines 112–116, 130 bytes ≈ 2.6% of tokens) —
```
## Related

* [Using Fields](/help/using-fields)
* [Building a Page](/help/building-a-page)
* [Key Concepts](/help/key-concepts)
```
The *heading + bullet-list* pattern is templated and repeats structurally across pages, but the three links themselves are topically relevant to Custom Fields specifically (not a generic same-list-everywhere block), so this is useful contextual content wearing a boilerplate shape rather than filler. Not included in the 6.6% figure above.

If Related were also counted as non-page-specific scaffold, the boilerplate share would rise to ≈**9.2%** (464/5035 bytes).

## 8. Retrieval-quality notes

- **Self-contained:** Yes, largely. The page explains core-vs-custom fields, key-naming rules, the six field types, allowed-value pickers with colour propagation to Item pages, Tub-level defaults (and the destination-URL default's stamp-on-create exception), reference fields (team member / Item / Tub), and required-field validation on both UI entry and CSV import — all inline, with concrete syntax examples (`{{item.serial_number}}`, `item.status == "operational"`). It does lean on two pieces of assumed vocabulary — **Tub** and **Item** — without redefining them here, but correctly delegates full depth to linked pages (`/help/using-fields`, `/help/building-a-page`, `/help/key-concepts`) rather than duplicating them, which is appropriate chunking for a RAG index as long as those linked pages are also indexed.
- **Terminology check against GLOSSARY.md/BRAND.md:** Correct canonical usage throughout — "Tub," "Item," "Page," "Destination URL" all match the glossary; no instances of banned synonyms ("Asset," "Folder," "Access Link," etc.). No brand-name misspellings ("QRtub" not used in body copy at all, which is fine — this is a feature-mechanics page, not marketing copy). No false/planned-feature claims from BRAND.md §1.6 appear.
- **Tables render cleanly** in plain markdown — pipe alignment is consistent in both the Field Types table and the Reference-field-choices table, so a naive plain-text (non-markdown-parsing) consumer would still read them as structured rows reasonably well.
- **One markdown-escaping artifact:** line 19 has `**item\_id**` — a backslash-escaped underscore inside bold text (needed so `_id**` doesn't get misparsed as an emphasis marker). Every other field-key example on the page (`serial_number`, `item.status`, `item.serial_number`) is wrapped in a code span instead, where no escaping is needed and the key reads literally. A consumer that treats the `.md` file as raw text rather than parsing markdown escapes (e.g., a naive substring search for `item_id`) would fail to match against the literal `item\_id`. Minor, single-occurrence, easy fix: wrap it in backticks like the other keys.
- **No images, embedded widgets, or JS-dependent content** on this page — nothing is lost in the HTML→Markdown conversion; the .md is a complete, faithful representation of the page's substance.
- **Links are root-relative** (`/help/using-fields`, `/help/building-a-page`, `/help/key-concepts`) rather than absolute. Fine for a browser resolving against the same origin, and fine for an agent that keeps track of the site's base URL, but an agent that ingests this markdown body in isolation (e.g., pasted into a context window without the source URL attached) cannot resolve these to fetchable URLs without already knowing the domain is `help.qrtub.com`.
- **x-robots-tag asymmetry:** the markdown variant (both via content negotiation and the literal `.md` URL) is served with `x-robots-tag: noindex, nofollow`, while the HTML variant carries no such header at all. This is very likely an intentional anti-duplicate-content measure (keep the canonical HTML page indexable by search engines, keep the machine-readable twin out of search-engine result pages) and it does **not** block direct retrieval — the content is served with 200 regardless. But it's worth flagging because some conservative ingestion/crawling pipelines treat `noindex` as a broader "don't use this" signal rather than narrowly "don't rank this in search results," and could skip the cheap 5KB markdown twin in favor of scraping the 241KB HTML page it's paired with, discarding the 47.9× efficiency win entirely.
- **Discoverability signals are strong at the transport level:** every response (HTML or markdown) carries a `Link` header advertising `/llms.txt`, `/llms-full.txt`, `/.well-known/api-catalog`, `/.well-known/mcp/server-card.json`, `/.well-known/agent-card.json`, and `/.well-known/agent-skills/index.json`, plus an `x-llms-txt` header. Combined with the in-body "Documentation Index" banner pointing at `/llms.txt`, an agent landing on this one page has multiple independent paths to discover the rest of the site.
