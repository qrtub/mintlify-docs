# Competitor doc audit — Flowcode (flowcode.com)

Fetched 2026-08-19. Two properties exist and were both checked:
- Marketing site: `https://www.flowcode.com` (Webflow-hosted)
- Help center: `https://help.flowcode.com` (Intercom-hosted, redirects `help.flowcode.com` → `/en/`)

No `/docs`, `/help`, or `/support` path exists on the marketing domain itself (all 404) — the real docs live entirely on the separate Intercom subdomain. `help.flowcode.com` is not linked from `robots.txt` or `llms.txt` on the marketing domain, so a crawler that only checks `www.flowcode.com` would miss the actual help content entirely.

---

## 1. Page granularity

Small, single-question pages — classic Intercom help-center style, not long all-in-one guides.

- Example: "Flowcode Scan Data Caps: FAQs" — one specific policy, ~350–400 words, structured as several H2 questions ("What is a Flowcode Scan Data Cap?", "Will my Flowcode stop working if I hit my cap?", etc.).
- Example: "How many scans is my Flowcode allowed?" — ~180–200 words, answers exactly one question.
- 13 collections, ~158 articles total across the help center (FAQs & How To's: 24, Data & Analytics: 18, Account Management: 25, Flowpage: 16, Flowcode: 13, Advanced Features: 10, Domains: 7, Teams: 7, Flowcode Pixel: 6, Printing & Design: 5, API & Developer Portal: 3, Flowtag: 3, Table of Contents: 1).
- Titles are literal user questions ("Can I delete my Flowcode account?", "What's the difference between Flowcode, Flowtag, and Flowpage?") rather than feature names — this maps well onto how people phrase search queries and how an AI assistant would phrase a retrieved answer.

**Relevance to QRtub:** this granularity is good practice for retrieval precision (a RAG system or AI answer engine can cite one narrow, self-contained chunk instead of extracting a fragment from a long page). QRtub's `help/*.mdx` pages are already reasonably scoped by topic, so this is confirmation of a direction already underway rather than a gap — but it's worth explicitly keeping single-question FAQ articles (as opposed to folding "can I do X" questions into feature-overview pages) as new content is added.

---

## 2. Pricing/limits page

Easy to find (`flowcode.com/pricing`, linked from primary nav) and states **specific numbers**, not vague tiers:

| Plan | Price | Flowcodes/Landing Pages | Scan analytics cap | Seats |
|---|---|---|---|---|
| Free | $0/mo | Up to 2 | 500 scans | 1 |
| Pro Plus | $25/mo (billed annually) | Up to 50 | 6,000 scans | 3 collaboration seats |
| Growth | $250/yr | Starts at 150 | 10,000 scans (first) | Custom |
| Growth Plus (Enterprise) | Usage-based | Unlimited (rate card) | — | — |
| Enterprise | Custom | Unlimited (rate card) | — | Unlimited workspaces |

Beyond the pricing page, Flowcode maintains a **dedicated, dated FAQ article specifically about the limits**: "Flowcode Scan Data Caps: FAQs" gives a plan-by-plan numeric table (Basic 500 / Pro 2,000 / Pro+ 6,000 / Growth 10,000 / Enterprise unlimited), states the policy's effective date ("in effect as of 02/29/2024"), and explicitly clarifies the failure mode in plain language: codes/pages keep working past the cap, only analytics visibility is lost, and there's no overage charge. A companion FAQ, "How many scans is my Flowcode allowed?", reinforces the same point (scanning itself is unlimited; only the analytics dashboard is capped) — so two different phrasings of the same user question both land on a consistent, precise answer.

**Relevance to QRtub:** this is the one finding most likely to change AI-answer quality. A generic AI QR/link-in-bio question ("does my QR code stop working after X scans?" or "what's the scan limit on the free plan?") is exactly the kind of query an LLM assistant gets asked, and Flowcode has pre-written a precise, unambiguous, dated answer to it, duplicated across two article phrasings and reflected identically in the pricing page and in `llms.txt`. If QRtub states plan quotas only on the pricing page (or states them differently in different places), a model synthesizing from crawled/RAG content risks vague or inconsistent answers where Flowcode's would be unambiguous. Worth checking that QRtub's own limits (scans, codes, seats) are (a) stated as exact numbers somewhere in the help docs, not only marketing pricing copy, and (b) consistent between the pricing page and any help articles that mention them.

---

## 3. llms.txt / AI-facing artifacts

Flowcode has **two separate `llms.txt` files** on two different subdomains, of very different quality. No `llms-full.txt` exists on either domain (404 on both).

**a) `www.flowcode.com/llms.txt`** (marketing site, Webflow-hosted, 200 OK, `text/plain`, but marked `x-robots-tag: noindex`)
Covers only marketing pages: Products, Industries, Pricing, "Get started", a "Style guide" reference link (despite `/styleguide` being disallowed in the marketing site's own `robots.txt` — an internal inconsistency), and an "Optional" section that reads as an unfinished draft rather than a curated summary:
> "Customer stories: To be added at /customer-stories. Currently, the CLEAR story is featured on the homepage."
> "Blog: Not yet launched. Planned for /blog."
This file contains **zero product-usage or help content** — nothing about how Flowcodes/Flowpages actually work, no limits detail beyond the one-line pricing summary. Curation quality here is weak: it reads like a lightly-reviewed auto-generated sitemap description, not a deliberately written AI-facing index.

**b) `help.flowcode.com/llms.txt`** (Intercom-hosted help center, 200 OK, `text/plain`)
This is the one that matters. It is a genuinely well-curated, machine-readable index of the actual help content:
- Organized by the same 13 collections as the human-facing help center, in the same order.
- Every one of the ~158 articles is listed as `[Title](url).md` — Intercom auto-generates a clean Markdown version of every article at `<article-url>.md` (confirmed by fetching one directly: clean headers, bold key facts/dates, bullet lists, no nav chrome).
- A handful of entries carry a one-line description pulled from the article (e.g. "Auto-sharing FAQs: Automatically share new assets with people in your organization with auto-sharing.").
- Total size is small (~180 lines / ~18.7 KB) — cheap for a model to ingest in full as a sitemap before deciding what to fetch.

**Relevance to QRtub:** the split is the real lesson, not the mere existence of a file. An `llms.txt` that only indexes marketing/pricing pages (as Flowcode's marketing-domain one does) provides little help-content signal to an AI assistant and can even look neglected (unfinished placeholder text, `noindex` header, inconsistency with robots.txt). The one that actually helps AI answer quality is the help-center index with per-article clean-Markdown mirrors. If QRtub's `llms.txt` (referenced in this repo's audit files) is closer to the marketing-only pattern, prioritize making sure the *help docs* — not just marketing pages — are what's indexed, and that each listed page has (or resolves to) a clean Markdown/plain-text version, since that's what materially affects retrieval and citation accuracy.

---

## 4. Other structural choices

- **"Last updated" stamps on individual articles**, visible in the article byline (e.g. "Flowcode Scan Data Caps: FAQs" shows Feb 28, 2024; "How many scans is my Flowcode allowed?" shows Oct 26, 2023). This lets a reader (or a model) judge whether a stated limit/policy is current. Not visible on the collection-listing pages, only on the article page itself.
- **A dated API changelog article** ("Flowcode API Changelog") sits inside the API & Developer Portal collection alongside the API overview and API FAQs — a lightweight version of a changelog, scoped to API behavior specifically.
- **Policy-change FAQs as a pattern**: rather than silently updating the pricing page when a limit changes, Flowcode publishes a standalone dated FAQ article explaining the change, its effective date, and the exact before/after behavior. This appears more than once (Scan Data Caps FAQ, "End of Trials FAQs").
- **No visible glossary.** The one article named "Table of Contents" is a navigational hub duplicating the collection structure, not a term glossary.
- **No visible dedicated troubleshooting index** — troubleshooting-style content is folded into the FAQ collections rather than given its own section.
- Search bar present on the help center homepage ("Search for articles...").

**Relevance to QRtub:** the two patterns worth adopting if not already present are (1) visible last-updated dates on individual help articles, so both readers and AI systems can weigh currency of a stated number/policy, and (2) treating a limit/policy change as its own dated FAQ artifact rather than only editing the pricing page in place — this creates an explicit, citable, dated record of "what changed and when" that plain pricing-page edits don't provide. A glossary and a dedicated troubleshooting index are both things Flowcode itself lacks, so there's no competitive gap to close there.
