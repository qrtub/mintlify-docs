# Competitor doc audit: QR Tiger (qrcode-tiger.com)

Audited: 2026-08-19
Auditor: martin@smcmarine.com.au (via Claude)
Method: direct fetch of homepage, llms.txt, llms-full.txt (curl -I + curl), FAQ, pricing (/payment),
troubleshooting page (/qr-code-not-working), API docs (/api-documentation), help center landing
(/blog/help-center), plus one web search to locate their real API reference (Stoplight).
Everything below was directly observed; nothing here is inferred/guessed.

## 0. Site shape (context)

qrcode-tiger.com is fundamentally a **marketing site with SEO content pages**, not a dedicated docs
subdomain like help.qrtub.com. There is no /docs or /help subdomain — "help" content lives at
`/faq`, `/blog/help-center` (a blog-tag landing page, not a real category structure), and dozens of
individual marketing/how-to URLs (e.g. `/qr-code-tracking`, `/qr-code-with-logo`,
`/qr-code-not-working`). The one genuine structured docs product they have is their **API
reference, hosted separately on Stoplight** (`qrtiger.stoplight.io`), found via web search, not
linked from their main nav.

This matters for the comparison: QRtub's help.qrtub.com is already a purpose-built docs site, which
is structurally ahead of QR Tiger's "help pages scattered across a marketing blog" approach. The
findings below are about the few things QR Tiger nonetheless does better or differently.

## 1. Page granularity — mixed, and the split is informative

- **Highly granular** for product/use-case surface area: each QR code type (vCard, Wi-Fi, menu,
  event, GS1, app store, etc.) and each industry vertical (restaurants, real estate, healthcare,
  logistics...) gets its own single-topic URL. ~20+ QR-type pages, ~10 industry pages, confirmed via
  the llms.txt listing and spot-checked live.
- **Long, single-page-with-anchors** for reference/support content that QRtub would recognize as
  "docs": the FAQ (~55-60 Q&As, no category grouping, one continuous page), the on-site
  `/api-documentation` (one long page with a table of contents and anchor links rather than
  separate endpoint pages), and the troubleshooting page (25 problems + pro-tips + FAQ, all on one
  URL with a jump-to-section TOC at the top).
- **The real API reference is small-page granular, but lives on a different platform.** Their actual
  developer docs are on Stoplight (`qrtiger.stoplight.io/docs/qrtiger-api/...`), where each guide/
  endpoint (e.g. "How to get your API key," "Guide to Track data API," "Create Dynamic QR Code," "How
  to customize redirectUrl parameters") is its own page/slug. This is the opposite granularity
  choice from the marketing-site `/api-documentation` long page, which itself says the long page is
  only a lightweight walkthrough and defers to Stoplight for full specs.

**Takeaway for QRtub:** QR Tiger's single continuous long pages for FAQ/API/troubleshooting are
navigable for a human skimming top-to-bottom, but poor for retrieval-by-fragment (a RAG/LLM answer
engine or a search deep-link can't cite "the answer" without pulling a huge page). If QRtub already
does one-topic-per-page for its help articles, that is a retrieval advantage worth stating plainly
in the audit rather than something to imitate. The one thing worth noting as a *deliberate, working*
choice is: QR Tiger keeps deep API reference detail on a separate small-page-per-endpoint system
(Stoplight) rather than trying to cram it into the same long marketing page — a "match your
granularity to content type" lesson if QRtub ever adds an API reference.

## 2. Pricing/limits page — easy to find, mostly specific numbers

- Pricing is linked from the homepage nav and is at a clean, guessable URL: `/payment`.
- Quotas are stated with **specific numbers** for the things that differentiate plans:
  - Free: 3 dynamic QR codes, 500 scans per code
  - Regular ($7/mo): 12 dynamic QR codes, 500 API requests/month, 5MB file upload
  - Advanced ($16/mo): 200 dynamic QR codes, 3,000 API requests/month, bulk batch up to 3,000, 10MB
    file upload
  - Premium ($37/mo): 600 dynamic QR codes, 10,000 API requests/month, 20MB file upload
- **Vague/marketing terms are reserved for the things they want unconstrained-sounding**: "unlimited"
  is used for static QR codes, scans on paid plans, downloads, and folders — i.e., the plan-limiting
  numbers are precise, the upsell/non-limiting language is where "unlimited" appears. This is a
  consistent, honest-reading pattern (not vague on the numbers that matter, vague only where
  genuinely unbounded).
- No comparison table on the pricing page itself (feature lists are per-plan cards with a
  "show more features" expander); no FAQ embedded on the pricing page (it links out to `/faq`
  instead). The FAQ page separately restates plan numbers in prose (e.g., "3 free dynamic QRs with a
  500 scan limit," data retention "maximum period of 1 year" after subscription expiry), which is a
  second, independent place the same numbers live — a minor consistency-drift risk if the two pages
  are edited independently, but currently they agree.

**Takeaway for QRtub:** the specific-numbers-on-limits, vague-only-on-truly-unbounded-features
pattern is worth confirming QRtub's own pricing page follows — this is exactly the kind of page an
AI answer engine or a prospect scans for "how many X do I get," and vague plan pages are a common
self-service failure point.

## 3. llms.txt / llms-full.txt — llms.txt exists and is genuinely well-curated; llms-full.txt is a false positive

Checked directly:

```
curl -sI https://www.qrcode-tiger.com/llms.txt       -> HTTP/2 200, content-type: text/plain
curl -sI https://www.qrcode-tiger.com/llms-full.txt  -> HTTP/2 200, content-type: text/html
```

- **`llms.txt` is real and well-built.** ~104 lines, proper llms.txt structure (H1 summary blockquote,
  then `##` sections), organized into: Core Product, QR Code Types (23 entries), Pricing Plans (with
  the same specific numbers as the /payment page), Industry Solutions, Features & How-Tos,
  Integrations, Resources & Learning, Company, Mobile Apps. Each link has a one-line description.
  This is a curated content map, not an auto-dump — genuinely useful as an AI-ingestion index and a
  reasonable model for what a QRtub `llms.txt` should look like (product summary → categorized link
  list with one-liners, including pricing numbers inline).
- **`llms-full.txt` does NOT actually exist, despite returning HTTP 200.** Content-type is
  `text/html`, not `text/plain`; body is the full Next.js app shell (all JS chunks, fonts, meta tags)
  with `<title>QR Code Generator | Create Free QR Codes With Logo | QR Tiger</title>` — i.e., it's
  their client-side-routed "not found" page rendered inside the normal site chrome, served with a
  soft-200 instead of a 404 (confirmed: a random garbage path on the same domain correctly returns
  404, so the app *can* 404 — `llms-full.txt` just isn't a registered route and falls through to a
  200'd not-found shell). Anyone or any crawler that requests `llms-full.txt` because they saw
  `llms.txt` exists and assumed the pair would get ~440KB of HTML/JS junk, not full-text docs, and
  because it's a 200 rather than 404 a naive scraper may cache it as if it were valid content.

**Takeaway for QRtub:** two concrete, checkable actions —
  1. If QRtub doesn't have an `llms.txt` yet, QR Tiger's is a solid template: short product
     description, then flat categorized link list with one-line descriptions per doc page, pricing
     numbers included inline.
  2. If QRtub ever publishes only `llms.txt` (no full-text companion), make sure the non-existent
     `llms-full.txt` (or any other guessed convention URL) returns a real 404, not a 200'd app shell
     — this is a one-line routing check but avoids exactly the failure mode observed here.

## 4. Other structural observations

- **No glossary found.** Checked `/glossary` and `/qr-code-glossary` directly — both 404. For a
  product with jargon (dynamic vs static QR, GS1 Digital Link, geofencing, white-label domain, etc.)
  this is a gap; QR Tiger relies on inline explanation inside the FAQ/how-to pages instead of a
  standalone reference.
- **Troubleshooting index exists and is well-formed** at `/qr-code-not-working`: opens with a
  quick-reference TOC of all 25 problems, gives a "quick fix" pass, then a detailed section per
  problem, then a pro-tips section, then its own 9-question FAQ. This is a genuinely good
  troubleshooting-index pattern (problem-first TOC, then depth) that's worth QRtub cross-checking its
  own troubleshooting/known-issues content against, if it has any single-long-page equivalents.
- **"Last updated" stamps are inconsistent, not absent.** The troubleshooting page shows both
  "Published on: February 13, 2026" and "Update: July 20, 2026" at the top. The API documentation
  page shows "Last Updated: August 19, 2025." The FAQ page, by contrast, has **no visible
  published/updated date at all**. So freshness signaling exists on some page types (long-form
  guides) but not on the page type that most needs it for trust (FAQ, which states plan numbers and
  policy specifics like the 1-year data retention window) — a reader has no way to tell if the FAQ's
  numbers are current.
- **FAQ format**: single continuous list, no categorization/accordion-by-topic, no per-question
  anchors observed. Combined with no "last updated" stamp, this is the weakest-structured page of
  the set reviewed, despite carrying some of the most important content (plan limits, data retention,
  feature availability by plan).

**Takeaway for QRtub:** the one clearly transferable lesson here is to put a visible last-updated
date on *any* page stating plan quotas, limits, or retention policy (their own FAQ fails this test
even though their long-form guides pass it) — stale, undated numbers on a pricing-adjacent FAQ are a
trust and accuracy risk for both human readers and an AI system citing the page.

## Sources

- https://www.qrcode-tiger.com (homepage/nav)
- https://www.qrcode-tiger.com/llms.txt (fetched directly)
- https://www.qrcode-tiger.com/llms-full.txt (fetched directly, confirmed soft-200 non-page)
- https://www.qrcode-tiger.com/faq
- https://www.qrcode-tiger.com/payment
- https://www.qrcode-tiger.com/qr-code-not-working
- https://www.qrcode-tiger.com/api-documentation
- https://www.qrcode-tiger.com/blog/help-center
- https://qrtiger.stoplight.io/docs/qrtiger-api/ (found via web search, not linked from main nav)
- https://www.qrcode-tiger.com/robots.txt (sitemap reference, checked for completeness)
