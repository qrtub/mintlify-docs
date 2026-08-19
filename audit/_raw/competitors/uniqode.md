# Competitor doc audit: Uniqode (uniqode.com)

Audited: 2026-08-19. Methods: direct fetch of www.uniqode.com, docs.uniqode.com (Intercom
help center), individual KB articles, llms.txt (curl + full read), pricing page, and
apidocs.uniqode.com. Third-party pricing-aggregator sites are cited explicitly where the
primary pricing table couldn't be fully extracted (it renders via JS and WebFetch's
markdown conversion dropped the table body) — those points are flagged as corroborated,
not firsthand.

Sources:
- https://www.uniqode.com/llms.txt (fetched directly, 200 OK, 11,208 bytes, 135 lines,
  last-modified 2026-07-02)
- https://www.uniqode.com/llms-full.txt (404 — does not exist)
- https://docs.uniqode.com (Intercom-hosted help center)
- https://docs.uniqode.com/en/articles/2833359-create-your-first-qr-code-on-uniqode
- https://docs.uniqode.com/en/articles/5984099-how-to-create-time-limited-qr-codes
- https://www.uniqode.com/pricing
- https://www.uniqode.com/about/ai-info
- https://apidocs.uniqode.com/ (200; /llms.txt on this subdomain is 404)
- Third-party corroboration for pricing grid: dupple.com, lifetimeqrcodes.com,
  hackceleration.com (2026 Uniqode pricing reviews)

---

## 1. Page granularity

Uniqode's actual documentation lives at **docs.uniqode.com**, an Intercom help center —
the www.uniqode.com domain is pure marketing. Granularity there is mostly narrow and
single-topic: "How to create time-limited QR Codes" is four bullet points plus a table of
contents and screenshots, not padded into a bigger "advanced QR settings" catch-all. Some
articles do combine adjacent flows into one longer page — "Create your first QR Code on
Uniqode" covers both dynamic (10 steps) and static (8 steps) creation in one ~1,200–1,400
word article rather than splitting them.

**Relevant to QRtub:** QRtub's `help/*.mdx` pages are already comparably single-topic and
similarly sized (99–346 lines each), so raw granularity is not a gap. The gap is in
**category breadth**: Uniqode's KB is organized into 8 top-level collections — QR Codes,
Cards, Integrations, **AI Capabilities**, **Insights**, **Account Management**, Developer,
and **FAQs**. QRtub's `docs.json` groups (Getting Started, Concepts, Items & Data, Pages,
Printing) are all product-mechanics groups — there is no billing/account-management group,
no analytics/insights group, and no FAQ group. A user with an account or billing question
today has nowhere obvious to land in QRtub's nav; Uniqode's taxonomy gives that question a
home.

## 2. Pricing/limits page

Uniqode has an easy-to-find `/pricing` page linked from primary nav and from llms.txt.
Where the numbers could be directly confirmed (via WebFetch fragments) they are **specific,
not vague**: a "$9/month" entry point, "$49/month" mid-tier ("best value for businesses"),
custom-domain add-on priced at "$2,000 per domain per year," Digital Business Card seats at
"$6 per user per month," a stated "14-day free trial," and a "30-day money-back guarantee."
Third-party 2026 pricing summaries (not independently verified against the live table, but
consistent across three separate sources) describe a QR plan ladder of Starter $5/mo (3
dynamic codes, 25,000 scans), Lite $15/mo (50 codes), Pro $49/mo (250 codes), Plus $99/mo
(500 codes), with analytics-history limits stated in days (30/90/180) — i.e., numeric
quotas per tier, not "unlimited" hand-waving. The only vague tier is the top one
(Business+ = "custom pricing").

**Relevant to QRtub:** this is a genuine, checkable content-quality bar: Uniqode states
exact quota numbers per plan (QR code counts, scan counts, retention days) rather than
qualitative terms. If QRtub's pricing page (qrtub.com/pricing, outside this docs repo) uses
vaguer language for plan limits, that's a concrete gap worth checking directly — specific
numbers are both more useful to a prospect and more retrievable/quotable by an AI answering
"how many QR codes do I get on plan X."

## 3. llms.txt / AI-facing artifacts

**Confirmed: `https://www.uniqode.com/llms.txt` exists** (curl -sI → `HTTP/2 200`,
`content-type: text/plain`, 11,208 bytes, last-modified 2026-07-02). No `llms-full.txt`
(404). The API subdomain (apidocs.uniqode.com) has no llms.txt of its own (404).

Curation quality — genuinely good, with one significant hole:
- Clean llms.txt structure: one-line canonical company description (incl. compliance
  badges: SOC 2 Type II, GDPR, HIPAA, ISO 27001:2022) up top, then `## Products`,
  `## Key Concepts`, `## Key Features`, `## Industries Served`, `## Use Cases`,
  `## Customer Stories`, `## Blog & Learning Resources`, `## Company` — each entry a link
  plus a single-sentence description. No marketing fluff, no duplicated boilerplate.
  A `## Key Concepts` block defines house terms in short glossary form (Phygital, Smart
  Rules, Scan Journey Intelligence, Two-Way Contact Sharing) — a compact, quotable
  definition list an LLM can lift directly.
- **The gap**: llms.txt contains zero links into docs.uniqode.com (the actual help
  center/KB). Every link is marketing site, blog, customer story, or the top-level API
  docs URL. An LLM relying on llms.txt would be well-equipped to describe *what Uniqode is
  and costs*, but has no path from llms.txt to *how to actually do something in the
  product* (e.g., "how do I create a time-limited QR code") — that content exists only in
  the Intercom KB, unreferenced by the AI-facing artifact.
- Separately, Uniqode maintains a **bespoke, hand-authored AI-facing page**:
  `www.uniqode.com/about/ai-info`, explicitly titled for AI consumption ("structured,
  canonical information about Uniqode, intended for AI assistants such as ChatGPT, Claude,
  Perplexity, Gemini, Google AI Overviews"). It's long (~3,500–4,000 words) and includes
  sections most companies don't bother with: "Instructions for AI Assistants" (how they
  want to be described), "Competitive Context" (how to compare them to alternatives), and
  a dedicated FAQ block — going beyond what llms.txt covers into an explicit "answer key"
  for LLM-generated descriptions of the company.

**Relevant to QRtub:** QRtub currently has neither an llms.txt nor an equivalent AI-facing
page anywhere in this repo (confirmed: no `llms*.txt` file exists in the repo). Uniqode's
approach — llms.txt for structured navigation/facts, plus a separate long-form
"instructions for AI assistants" page for framing/positioning — is a two-tier pattern
worth studying, but the more directly actionable lesson for QRtub is narrower: if/when
QRtub builds an llms.txt, don't repeat Uniqode's mistake of omitting the actual help
content — the KB/how-to pages should be the core of it, not an afterthought behind
marketing links.

## 4. Other structural choices

- **Byline + last-updated date on every KB article.** Both sampled articles carry
  "Written by [Name]" plus a specific date (e.g., "October 14, 2025"; "June 27, 2024")
  directly under the title. QRtub's help pages carry no author or last-updated stamp
  anywhere (confirmed by grep across all `help/*.mdx` — zero matches for
  "last updated"/date patterns). For a support KB, a visible last-updated date is a
  meaningful trust signal (tells a reader/LLM whether "how to do X" instructions are
  current) that QRtub's docs currently lack entirely.
- **Per-article table of contents + "Related Articles" + "Read next" + a feedback
  widget** ("Did this answer your question?" with reaction options) appear at the bottom
  of Uniqode KB articles. QRtub's sampled page has a manual "Next Steps" section with 1–2
  hand-picked links but no reader-feedback mechanism and no auto-generated TOC.
- **Dedicated FAQ collection** as its own top-level KB category (not a hidden section
  or blog post). QRtub has no FAQ page or section at all today — notably, this repo's own
  `CLAUDE.md` already contains an "FAQ answer" prompting template staged for future use,
  which confirms the gap is recognized internally but not yet built.
- **Glossary-as-data vs glossary-as-narrative.** Uniqode's llms.txt ships a compact,
  scannable term → one-line-definition block ("Key Concepts") that both humans and LLMs
  can quote directly. QRtub's closest equivalent, `help/key-concepts.mdx`, covers similar
  ground (the Item/Link/Page entity model) but as continuous narrative prose, not a
  quick-lookup term list, and — since QRtub has no llms.txt — it isn't exposed anywhere in
  a machine-scannable form for AI retrieval.

---

## Bottom line for QRtub

The differences that would plausibly move retrieval accuracy, AI answer quality, or
self-service are:
1. Uniqode's llms.txt is real and structurally clean, but excludes its own help-center
   content entirely — a cautionary example, not just a model to copy, for whenever QRtub
   builds one.
2. Uniqode states plan/quota numbers specifically (counts, scan volumes, retention days)
   rather than vaguely, and surfaces them on a page an LLM/aggregator can easily quote —
   worth checking QRtub's own pricing copy against this bar directly.
3. Author/last-updated stamps and a real FAQ category are present at Uniqode and absent at
   QRtub — both are cheap, concrete additions (QRtub's CLAUDE.md already has an FAQ
   template ready) that a reader or LLM can use as freshness/coverage signals.
4. Category breadth in the KB (Account Management, Insights, AI Capabilities, Developer)
   reflects support-topic coverage QRtub's current nav groups don't yet have a place for.
