# Competitor audit: Linear (linear.app/docs)

Fetched directly: `https://linear.app/docs` (real docs site, not a marketing page — confirmed
sidebar nav with topic categories), plus `https://linear.app/llms.txt`, several individual
`https://linear.app/docs/<slug>.md` raw pages, and `https://linear.app/pricing`.
All findings below are from direct fetch/curl, not inference.

---

## 1. Page granularity — small, single-topic pages

Confirmed by word count on the raw markdown export of individual doc pages:

| Page | Word count |
| --- | --- |
| `priority.md` | 243 |
| `start-guide.md` | 262 |
| `issue-relations.md` | 412 |
| `parent-and-sub-issues.md` | 752 |
| `sla.md` | 994 |

Every nav item is its own page at its own URL (e.g. "Priority," "SLAs," "Issue relations,"
"Parent and sub-issues" are four separate pages, not sections of one "Issues" mega-page). The
`llms.txt` index confirms this 1:1 mapping — each sidebar entry links to one dedicated `.md` file.

**Contrast with QRtub:** QRtub's `/help/*.mdx` pages run 600–1,600+ words each (`key-concepts.mdx`
1,566, `using-fields.mdx` 1,373, `device-detection.mdx` 1,219, `conditional-visibility.mdx` 1,185),
i.e. roughly 2–6x Linear's per-page length, with multiple distinct sub-topics folded into a single
page. This matters for retrieval: a RAG/AI-answer system (or a user's Ctrl+F) has to pull a much
larger, more heterogeneous chunk to answer a narrow question on QRtub's docs than it would on
Linear's, which lowers precision and makes it easier for an LLM to cite the right page but the
wrong section of it.

## 2. Pricing/limits page — easy to find, specific numbers

`linear.app/pricing` is a dedicated page (linked from the docs' `billing-and-plans.md`, which
explicitly defers to it: *"Please see Pricing for rates and a feature-by-feature comparison by
plan"* rather than duplicating numbers that could drift out of sync).

Limits stated are **specific numbers where a cap exists, "Unlimited" where it doesn't** — not
vague hand-waving:

| Feature | Free | Basic | Business | Enterprise |
| --- | --- | --- | --- | --- |
| Members | Unlimited | Unlimited | Unlimited | Unlimited |
| Teams | **2** | **5** | Unlimited | Unlimited |
| Issues | **250** | Unlimited | Unlimited | Unlimited |
| File upload size | **10MB** | Unlimited | Unlimited | Unlimited |
| Integrations | **15** | Unlimited | Unlimited | Unlimited |
| Sub-team nesting | — | **1 level** | **5 levels** | Unspecified |

Pricing itself is also exact: Free $0, Basic $10/user/mo, Business $16/user/mo (yearly billing),
Enterprise "Custom." The billing-and-plans doc additionally states a concrete behavioral
consequence of a limit — *"If you have over 250 issues, you will no longer be able to create new
issues"* after downgrade/cancellation — which is the kind of precise, actionable statement that
resolves a support question outright rather than prompting a follow-up.

**Relevance to QRtub:** no plan-limits or pricing content exists anywhere in this docs repo
(`docs.json`/`help/`/`industries/`/`integrations/` — confirmed by search, none found). A user or
an AI assistant answering "how many pages/items can I have on the Starter plan" from QRtub's docs
today has nothing specific to cite.

## 3. llms.txt — exists and is well-curated; llms-full.txt does NOT exist despite a 200 response

Checked directly:

```
curl -sI https://linear.app/llms.txt        → HTTP 200, content-type: text/plain  (real file)
curl -sI https://linear.app/llms-full.txt   → HTTP 200, content-type: text/html   (fake — this is
                                               the Next.js SPA shell / soft-404, x-robots-tag: noindex,
                                               NOT an actual llms-full.txt; confirmed by fetching the
                                               body, which is an HTML `<head>` with app bootstrap JS,
                                               not text)
```

So Linear ships a curated `llms.txt` index but **not** a full-text dump — the opposite pairing
from what's often assumed. Quality of the one they do ship is high:

- Opens with a one-line product description, then organizes links under the **same category
  headers as the docs sidebar** (Getting Started, Account, AI, Teams, Issues, Issue properties,
  Projects, Initiatives, Cycles, Views, Find and filter, Integrations, Analytics, Administration),
  plus a separate `## Developers` section for the API/SDK docs.
- Every entry links to a clean `.md` raw-markdown counterpart of the page
  (`linear.app/docs/<slug>.md`), not the rendered HTML — directly consumable by an LLM without
  scraping/boilerplate-stripping.
- Total size is small (9.7KB for the index itself), so it's cheap to include in a system prompt or
  retrieval seed.
- Checked the linked `.md` pages themselves (`sla.md`, `priority.md`, `issue-relations.md`,
  `parent-and-sub-issues.md`): **none carry a repeated marketing footer/CTA** — each page ends on
  actual content, no "Sign up now" boilerplate appended.

**Relevance to QRtub:** QRtub's own `llms-full.txt` (Mintlify auto-generated, already audited
separately in `audit/_raw/external/llms-full.md`) carries the identical marketing CTA + email
footer on **19/19** pages verbatim:

```
***
**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)
Questions? Email us at hi@qrtub.com
```

Linear's approach — clean per-page markdown, zero repeated boilerplate, a curated small index
file rather than (or in addition to) a full dump — is the more retrieval-friendly pattern. Every
chunk of QRtub's current llms-full.txt spends part of its token budget on identical CTA text that
carries no answering signal; at RAG-chunk granularity this either wastes context or (if chunked
naively) produces near-duplicate low-information chunks across all 19 pages.

## 4. Other structural choices

- **Inline, per-page FAQ instead of a site-wide FAQ page.** `sla.md` ends with a `## FAQ` section
  using semantic `<details><summary>` collapsible blocks (4 Q&A pairs specific to SLAs). This
  keeps FAQ content co-located with the topic it answers rather than siloed in a separate,
  hard-to-connect FAQ page — and it round-trips cleanly into the markdown export, so an AI
  assistant retrieving `sla.md` gets the FAQ for free in the same chunk. QRtub's docs currently
  have no FAQ pattern (inline or standalone) in any `/help/*.mdx` file.
- **No "last updated" timestamps anywhere.** Checked rendered page chrome and raw markdown
  (`sla.md`) and the HTTP response headers (`curl -sI` on a docs page) — no `Last-Modified` header,
  no visible "updated on" date in content or layout. This is a genuine gap on Linear's side, not a
  strength — worth noting so QRtub doesn't over-index on copying everything Linear does. (Not
  recommending QRtub match this omission.)
- **No glossary and no troubleshooting index found.** Neither the `llms.txt` category list nor the
  visible sidebar (Getting Started/Account/AI/Teams/Issues/.../Administration/Developers) contains
  a "Glossary" or "Troubleshooting" section. Linear substitutes per-feature FAQ blocks (see above)
  for what a troubleshooting index would otherwise cover. This is a plausible area where neither
  side has an example worth copying — flagging it as absent on both, not as a Linear strength.

---

## Bottom line for QRtub (only the changes plausibly worth making)

1. **Split long `/help/*.mdx` pages by sub-topic.** Pages over ~1,000 words that cover multiple
   distinct actions (e.g. `key-concepts.mdx`, `using-fields.mdx`, `device-detection.mdx`,
   `conditional-visibility.mdx`) are candidates to break into smaller single-topic pages, matching
   Linear's pattern of one URL per concept. This is the change most likely to improve retrieval
   precision for both search and AI-assistant citations.
2. **Publish a pricing/limits page with specific numbers**, mirroring Linear's "specific cap or
   explicit 'Unlimited', never vague" convention, and link to it from feature docs instead of
   repeating numbers inline (reduces drift risk).
3. **Strip the repeated CTA/email footer from `llms-full.txt`** (or regenerate it without the
   Mintlify default footer injection) — every one of Linear's per-page `.md` exports is boilerplate-
   free, and QRtub's current 19/19-page repetition is a concrete, fixable drag on AI-answer quality
   from that file.
4. Consider inline per-page FAQ blocks (`<details><summary>`) on pages that field recurring support
   questions, rather than planning a separate FAQ page — this keeps the answer co-located with the
   page an AI assistant or search result would already retrieve.
