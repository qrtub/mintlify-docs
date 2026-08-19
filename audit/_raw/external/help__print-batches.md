# Audit: Print Batches (agent/LLM retrieval quality)

- HTML URL: https://help.qrtub.com/help/print-batches
- Markdown URL: https://help.qrtub.com/help/print-batches.md
- llms.txt description: "Track a production run from export to installation, and see which codes are actually deployed"
- Audited: 2026-08-19

## Summary table

| Metric | Value |
|---|---|
| HTTP status (`.md`, `Accept: text/markdown`) | 200 |
| HTTP status (HTML, default `Accept`) | 200 |
| Content-Type (`.md`) | `text/markdown; charset=utf-8` |
| Content-Type (HTML) | `text/html; charset=utf-8` |
| X-Robots-Tag (`.md` response) | `noindex, nofollow` |
| X-Robots-Tag (HTML response) | *(header absent — no directive)* |
| Content-Length, HTML | 238,058 bytes |
| Content-Length, Markdown | 3,841 bytes |
| HTML : Markdown byte ratio | **~61.98×** (238058 / 3841) |
| Word count (markdown body) | 626 words |
| Estimated tokens (chars/4) | **~960 tokens** (3841 / 4) |

Raw commands run:
```
curl -sI -H "Accept: text/markdown" https://help.qrtub.com/help/print-batches
curl -sI https://help.qrtub.com/help/print-batches
curl -s  https://help.qrtub.com/help/print-batches.md
```

## Heading structure (in order)

- H1: Print Batches
  - H2: Creating a batch
  - H2: Finding your batches
  - H2: Batch details
  - H2: Status
  - H2: Tracking what is actually installed
  - H2: The CSV is kept
  - H2: Filtering Items by batch
  - H2: Why some links cannot be deleted
  - H2: Archiving
  - H2: What is not tracked
  - H2: Related

No H3s anywhere on the page. Structure is flat and shallow — 10 H2 sections under one H1, each 1–5 sentences or a short list/table. This is a good shape for chunked RAG retrieval (each section is independently graspable) but the heading text itself is low-signal for embedding search: "The CSV is kept," "Archiving," "Status" are generic out of context and don't restate "batch" or "print" in every heading, so a retriever matching on heading embeddings alone (rather than full-chunk text) may under-rank this page for queries like "how do I archive a print batch."

## Boilerplate inventory (present in the `.md` body, not visible as "extra" in rendered HTML)

The `.md` response is not just the page's MDX source through a converter — Mintlify's markdown export wraps every page with two extra blocks that do not exist in the source `.mdx` file at all (verified against `/workspace/mintlify-docs/help/print-batches.mdx`):

**1. Top-of-file "Documentation Index" directive block (injected, not in source):**
```
> ## Documentation Index
> Fetch the complete documentation index at: https://help.qrtub.com/llms.txt
> Use this file to discover all available pages before exploring further.
```
- 177 characters / ~44 tokens ≈ **4.6% of the page's estimated tokens**.
- This is sitewide boilerplate — the identical block (pointed at a different llms.txt) appears verbatim in Mintlify's own documentation when fetched the same way (confirmed against `/workspace/mintlify-docs/audit/_raw/raw_organize_pages.md`, a raw pull of Mintlify's own docs site), so it is a platform-level template, not QRtub-authored content.
- Notable for retrieval quality in two ways: (a) it is a directive addressed to an AI reader ("Fetch...", "Use this file to discover...") sitting inside what is nominally a content payload — worth flagging as machine-directed instruction text riding along with the article body, since some agents may act on it (fetch llms.txt) before reading the actual requested page; (b) it consumes real token budget on every single page of the site for zero page-specific value.

**2. Footer CTA block (injected, not in source's rendered position — see note below):**
```
***

**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)

Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)
```
- 157 characters / ~39 tokens ≈ **4.7% of the page's estimated tokens**.
- This *does* exist in the source `.mdx` (lines 95–99), so unlike the Documentation Index block it's QRtub-authored, but it is the standardized sitewide close specified in `/workspace/mintlify-docs/CLAUDE.md` ("pages close with 'See plans and pricing' ... and the support address") — i.e. deliberately identical boilerplate across the whole help section, not page-specific content.

**3. "Related" list (structural boilerplate, page-specific payload):**
```
## Related
* [Media Basics](/help/media-basics)
* [Creating Your First Link](/help/creating-your-first-link)
* [Key Concepts](/help/key-concepts)
```
- 148 characters / ~37 tokens ≈ **3.1% of tokens**. The section *pattern* ("## Related" + bullet links) is standard across pages, but the three links themselves are page-specific, so this is only partially boilerplate — noted separately from the two blocks above rather than folded into the boilerplate total.

**Combined hard-boilerplate total (Documentation Index + footer CTA):** ~334 of 3,841 characters ≈ **10.2% of the page's total tokens** are sitewide template, not this page's actual answer content. If the "Related" block's scaffolding (not its links) is counted too, boilerplate structure accounts for roughly 13% of tokens.

For a page this short (~960 tokens total), spending ~10% of the budget on a repeated nav/CTA wrapper is a meaningful tax — in an LLM context window or a support-bot snippet budget, this is real content displaced by template on every single retrieval.

## Other retrieval-quality notes

- **Self-contained:** Yes, for its stated scope. The page defines "batch" from scratch in its opening paragraph, defines all four batch statuses and all three per-code deployment statuses inline (as tables/lists, not by reference), and explicitly states its own boundary ("Batches record production runs, not individual pieces of media... See Media Basics for what is planned") rather than silently omitting it. This self-scoping is good for an agent answering a question without needing to also fetch `/help/media-basics` — the "what this page does NOT cover" is stated in-page.
- **X-Robots-Tag divergence is the most important structural finding:** the `.md` variant is served with `X-Robots-Tag: noindex, nofollow`; the HTML variant has no such header at all (i.e., indexable). Any crawler or agent that respects `X-Robots-Tag` will treat the clean, cheap, high-signal Markdown representation as explicitly non-indexable/non-followable, while the 62× heavier HTML page (which embeds full Next.js/React hydration payload, nav chrome, etc.) remains the indexable canonical. This is backwards from what you'd want for AI-answer-engine visibility (ChatGPT/Perplexity-style retrieval tends to prefer/allow crawling cleaner representations) — worth flagging to whoever owns the Mintlify deployment config, since it looks like a platform-default rather than an intentional per-page choice.
- **HTML bloat ratio (~62×) confirms the markdown endpoint is the only sane ingestion path** for this site for any agent — one more reason the noindex on `.md` is a problem rather than a neutral default.
- **No Mintlify components (`<Card>`, `<Tabs>`, `<Steps>`, etc.) in the source** — the page is plain Markdown (lists, one table, bold, links), so the `.md` rendering has no leftover raw JSX, unrendered component tags, or component-only content that vanishes in plain-markdown consumption. This is a real strength versus pages that lean on Mintlify components.
- **Markdown table renders cleanly** — the Status table (Draft/Printing/Printed/Deployed) is a well-formed GFM pipe table in the `.md` output and will parse correctly in any standard Markdown-to-text or Markdown-to-structured-data pipeline.
- **Frontmatter `description` is duplicated into the body as a blockquote** directly under the H1 (`> Track a production run from export to installation...`) — this is not present as a blockquote in the source `.mdx` (the source's first line is the plain opening paragraph); Mintlify's markdown export injects the frontmatter description as a visible summary line. This is a positive for retrieval: an agent gets a one-line abstract before the full body, matching the llms.txt description verbatim.
- **Terminology check against `../qrtub/GLOSSARY.md`:** the canonical term for a production run is "Media Batch," but this page (and its own title, "Print Batches") uses the bare word "batch" throughout and never once uses "Media Batch" or "Media" to describe the run itself (it does correctly use lowercase "media" once, for the physical photo: "a photo of the finished media"). This isn't flagged as an error — `/workspace/mintlify-docs/CLAUDE.md`'s own use-case notes ("Print batches are shipped... Per-item Media is not") treat "print batch" as the accurate, shipped-feature name distinct from "Media" (the physical-material tracking concept), and the glossary's "Print run / production run / order" column is about avoiding those specific synonyms, not about avoiding "batch" — but it means an agent asked "what's a Media Batch in QRtub?" would need to infer that this page (titled "Print Batches") is the answer, since the string "Media Batch" never appears here to anchor a keyword or embedding match.
- **Internal links use root-relative paths** (`/help/media-basics`, `/help/creating-your-first-link`, `/help/key-concepts`) rather than absolute URLs. Fine for a browser or a same-origin agent, but an agent that has ingested only this page's raw markdown text (e.g., pasted into a chat, or retrieved via a generic scraper without base-URL context) cannot resolve these to fetch the related pages without independently knowing the origin is `https://help.qrtub.com`.
- **No images, no code blocks, no accuracy red flags found** — cross-checked the "batches are a shipped, working feature" framing against `/workspace/qrtub/BRAND.md`'s feature-status table (which lists "Media Batch management" as Planned) and confirmed via `/workspace/mintlify-docs/CLAUDE.md` that this apparent conflict is already resolved and intentional: print/deployment-status batch tracking is shipped (backed by real code paths per `/workspace/qrtub/CLAUDE.md` §4 — `app/api/print-batches/`, `server-print-batches.ts`, `media_batches`/`media_batch_access_urls`/`access_link_print_batches` tables), while the *Planned* item refers to richer per-item Media inventory (material, cost, durability) — which this very page correctly disclaims in its "What is not tracked" section. Not a defect; noted here only so a future auditor doesn't re-flag it as a false positive.
