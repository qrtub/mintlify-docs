# Competitor doc audit — Stripe (docs.stripe.com)

Fetched/verified 2026-08-19. Methods: direct `curl` against docs.stripe.com (HTML pages, their `.md`
markdown mirror of every page, sitemap.xml, robots.txt, llms.txt), plus one WebFetch render for
UI-only elements. Everything below was directly observed; nothing here is inferred from Stripe's
general reputation.

Site is a real, large docs corpus: `docs.stripe.com/sitemap.xml` lists **4,657 URLs**. Not a marketing
homepage — confirmed as a genuine docs/help section.

---

## 1. Page granularity

Mixed, and the mix looks deliberate rather than accidental:

- **Overview/index pages are short and single-topic.** Measured via their `.md` mirror (one Markdown
  file per doc URL, e.g. `docs.stripe.com/connect.md`):
  - `connect.md` — 45 lines
  - `payments/checkout.md` — 64 lines
  - `security.md` — 123 lines
  - `declines.md` — 135 lines
  - `refunds.md` — 308 lines
- **Quickstarts/tutorials are long**, but the length comes from repeating the same integration steps
  across ~7 SDK languages (Node, Ruby, Java, Python, PHP, Go, .NET) inside one page via tabbed code
  blocks, not from mashing unrelated topics together:
  - `checkout/quickstart.md` — 1,124 lines
  - `billing/quickstart.md` — 1,716 lines (confirmed by inspecting headings: the same ~10 steps,
    each repeated per-language as `### Install the Stripe Node library` / `### Install the Stripe
    Ruby library` / etc.)
  - `webhooks/quickstart.md` — 863 lines
- **Reference/index pages are deliberately thin and link out**: `api.md` is only 23 lines and is
  essentially a directory of links into per-resource reference pages.

**Takeaway for QRtub**: Stripe's rule of thumb appears to be "one URL per task or concept," and length
only grows when a page legitimately needs to show the same step N ways (N SDKs), not when it's
covering N different topics. If QRtub has any long combined pages (e.g. a single "Getting Started"
page covering account setup + QR generation + asset tagging + printing + team invites), splitting by
task — the way Stripe splits "install the CLI" from "build a webhook" from "test your integration" —
would likely improve both human scanning and chunk-level retrieval accuracy for AI answers, since each
retrievable chunk stays on one topic.

## 2. Pricing / limits page — findability and specificity

- **Pricing**: `stripe.com/pricing` (marketing domain, not docs.stripe.com) redirects to a
  localized version (`/au/pricing` from this vantage point) and is one click from the docs site's
  parent domain. Numbers are stated precisely, not vague: `1.7% + A$0.30` per successful domestic
  card charge, `3.5% + A$0.30` for certain international/premium cards, `5.49% + A$0.30` for
  Amex, and a `99.999%` historical uptime figure — all as plain text on the page, not "contact
  sales" or "starting at."
- **Usage/rate limits**: `docs.stripe.com/rate-limits` is a real docs page (not a marketing page)
  and is exceptionally specific — a table with exact figures for every resource:
  - Global API: **100 requests/second** live, **25 requests/second** sandbox
  - Individual endpoints: **25 req/s** default
  - Payment Intents: **1,000 update requests per PaymentIntent object, per hour**
  - Subscriptions: **10 new invoices/subscription/minute**, **20/day**, **200 quantity
    updates/subscription/hour**
  - Files API: **20 read/s, 20 write/s**
  - Payouts: **15 create/s**, **30 concurrent requests/business**
  - Connect account creation: **30/s** live, **5/s** sandbox
  - A separate, explicit **read-API allowance tied to transaction volume**: "must not exceed an
    average of 500 [read requests] per transaction," with a worked example ("100 transactions in 30
    days → don't exceed 50,000 read requests") and a stated floor of **10,000 read requests/month
    minimum regardless of volume**.
  - It also documents the exact `429` response headers (`Stripe-Rate-Limited-Reason` values:
    `global-rate`, `endpoint-rate`, `global-concurrency`, `endpoint-concurrency`,
    `resource-specific`) so a caller — human or AI — can identify *which* limit was hit, not just
    that a limit was hit.

**Takeaway for QRtub**: this is the sharpest, most portable finding for the audit. Every number is
concrete and worked through with an example, and failure modes (what error you get, how to
distinguish causes) are documented alongside the limits themselves. If QRtub's plan-quota
documentation currently uses ranges, "fair use," or omits what actually happens when a limit is hit
(error message, upgrade prompt, throttling behavior), that's a direct, fixable gap — vague quotas are
exactly the kind of content an AI answer engine (or a support agent) will get wrong or hedge on,
because there's no specific fact to retrieve.

## 3. llms.txt / AI-facing artifacts

Checked directly:

```
curl -sI https://docs.stripe.com/llms.txt        → HTTP 200 (empty body on HEAD; real content on GET)
curl -sI https://docs.stripe.com/llms-full.txt   → HTTP 404
curl -sI https://stripe.com/llms.txt             → HTTP 200 (empty body on HEAD, marketing domain)
```

`docs.stripe.com/llms.txt` **exists and is substantial and hand-curated**, not an auto-dump:
- 89,866 bytes, 640 lines, **454 curated links across 26 named categories** (Docs, Payment Methods,
  Checkout, Payments, Link, Billing, Elements, Connect, Issuing, Capital, Crypto, Climate, Tax,
  Invoicing, Identity, Atlas, Financial Connections, Revenue Recognition, Treasury for Platforms,
  Sigma, Payment Links, Radar, Terminal, plus "Architecture and Dashboard" and "Optional").
- Every linked page has its own one-line description ("Learn how to fix a common error when
  listening to webhook events," etc.) — this reads like a real information architecture, not a
  sitemap dump.
- Notably, **every single docs.stripe.com page is separately available as clean Markdown** at the
  same path with `.md` appended (verified on a dozen+ pages) — so the llms.txt isn't the only
  AI-facing surface, it's the index into a parallel Markdown mirror of the entire site. This is a
  meaningfully stronger setup than a single llms.txt file: any crawler or agent can fetch any page
  as clean Markdown directly, with no HTML-stripping needed, and llms.txt just curates entry points
  into that mirror.
- The file **opens with a version-pinning warning aimed specifically at LLMs**: "always check the
  npm registry for the latest version rather than relying on memorized version numbers... Never
  hardcode an old version number from training data."
- It contains a **whole embedded section titled "Instructions for Large Language Model Agents: Best
  Practices for integrating Stripe"** — explicit steering text telling an AI assistant which current
  APIs to recommend and which deprecated ones to actively avoid (e.g. "never recommend the Charges
  API," "never recommend the legacy Card Element," "always use the Accounts v2 API... never the v1
  Accounts API for new integrations unless explicitly requested," "don't recommend the outdated
  terms Standard/Express/Custom"). This is Stripe directly counteracting the fact that its API has
  evolved and LLM training data is full of now-outdated integration patterns.
- `llms-full.txt` does **not** exist (404) — Stripe chose curated links over a full-text dump, likely
  because the per-page `.md` mirror already serves that purpose.

**Curation quality assessment**: high. It's organized by product area (not alphabetical/generic), every
entry has a purpose-built description, and — most distinctively — it proactively corrects for known
LLM failure modes (recommending deprecated APIs/terminology from stale training data) rather than
just listing pages. This is a materially different bar than "here are our doc URLs."

**Beyond llms.txt — an even more aggressive AI-facing layer** (found incidentally while checking a doc
page's rendered HTML, confirmed as real page metadata, not a rendering artifact):
- `docs.stripe.com/skills.md` documents official **Claude Code / Cursor / Codex plugins**
  (`claude plugin install stripe@claude-plugins-official`, `codex plugin add stripe@openai-curated`,
  Cursor marketplace listing) plus a manual path (`npx skills add https://docs.stripe.com`) and a
  machine-readable **skills index** at `docs.stripe.com/.well-known/skills/index.json`.
- Individual doc pages carry **page-level metadata tagging which "skill" bundle is relevant to that
  page** — confirmed in the raw HTML of `docs.stripe.com/rate-limits`: embedded JSON contains
  `"skills":["stripe-docs","stripe-best-practices"]`. This means Stripe's doc pages are wired at the
  page level to a specific, versioned, machine-consumable instruction bundle for coding agents —
  well past what an llms.txt file does.

**Takeaway for QRtub**: an llms.txt alone would already be a step up if QRtub has none; but the bigger,
more copyable idea is the **parallel `.md` mirror of every page** (so agents/crawlers get clean
Markdown without scraping HTML) and the **explicit "don't recommend the old way" steering block** —
directly useful if QRtub's product has changed shape over time (renamed features, deprecated flows)
and risks AI tools/chat answering with outdated QRtub terminology or workflows.

## 4. Other structural choices

- **Glossary**: `docs.stripe.com/glossary` is a real, single standalone page (274 lines) — bolded
  term + one/two-sentence plain-language definition, e.g. "**3D Secure (3DS)**: ... additional layer
  of authentication..." Dozens of terms (3D Secure, ACH, BIN sponsor, beneficial owner, capture types,
  etc.), openly linked from the docs. Confirmed present in the sitemap and fetchable as `.md`.
- **Troubleshooting pages exist but are scoped per-integration, not one central index**: sitemap shows
  `get-started/account/sso/troubleshooting`, `use-stripe-apps/adobe-commerce/payments/troubleshooting`,
  `use-stripe-apps/cegid/troubleshooting`, `use-stripe-apps/woocommerce/troubleshooting`, etc. — i.e.
  troubleshooting content lives next to the specific integration it applies to rather than in one
  generic "Troubleshooting" hub.
- **FAQs are similarly scoped/contextual**, not one master FAQ page (e.g.
  `connect/platform-express-dashboard-taxes-faqs` sits under the Connect taxes content).
- **No "last updated" date/timestamp found** on the rate-limits page's rendered HTML (checked
  directly in source) — Stripe apparently doesn't surface page-level freshness stamps to readers.
- **"Was this page helpful" feedback widget confirmed present** in page HTML (`Was this page
  helpful` / `Feedback` strings in source) — a lightweight self-service signal-collection mechanism
  Stripe has that a doc page can carry without a dedicated support-ticket flow.
- **`robots.txt` explicitly allows AI**: `Content-Signal: ai-train=yes, search=yes, ai-input=yes` —
  a deliberate, machine-readable statement that Stripe wants its docs used for AI training and
  AI-input/retrieval, on top of just not blocking crawlers.

**Takeaway for QRtub**: the glossary is the most directly transferable idea if QRtub lacks one — it's
cheap to build, and standalone glossary pages are exactly the kind of high-precision, low-ambiguity
content that AI answer engines retrieve cleanly (a defined term maps to one unambiguous chunk). The
"troubleshooting/FAQ scoped to the relevant integration page rather than a single generic hub" pattern
is also worth noting if QRtub is deciding between a single big FAQ page vs. distributing Q&A into the
relevant feature pages — Stripe's choice suggests scoping wins for a large, varied product surface.

---

## Summary of differences most likely to move QRtub's outcomes

1. **Specific, worked-example numbers for limits/quotas, plus documented failure behavior** (what
   error you get, how to tell which limit you hit) — Stripe's rate-limits page is the clearest
   model. If QRtub's plan-quota docs are vague or omit failure-mode behavior, this is a concrete,
   fixable gap that affects both self-service and AI-answer correctness.
2. **A clean-Markdown mirror of every page (`/page-path.md`) plus a curated `llms.txt` index into
   it** — bigger lever than llms.txt alone; makes every page trivially retrievable by any AI tool
   without HTML scraping.
3. **Explicit "here's what's outdated, don't recommend it" steering content** aimed at LLMs — directly
   applicable if QRtub's product/terminology has changed and risks stale AI answers.
4. **A standalone glossary page** — cheap, high-retrieval-precision content type that's simple to
   add if missing.
5. **Page granularity discipline**: short single-topic overview pages; length is reserved for
   legitimately multi-variant content (e.g., multi-platform steps), not mixed topics.
