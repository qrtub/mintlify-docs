---
title: "Competitor doc-practice audit: Twilio (twilio.com/docs)"
description: "Live findings from fetching Twilio's public docs, pricing, error index, glossary, MCP server, and llms.txt/robots.txt, assessed for differences that would plausibly change retrieval accuracy, AI answer quality, or self-service for help.qrtub.com."
---

# Twilio docs vs. help.qrtub.com — live findings

**Fetched:** 2026-08-19, via direct `curl`/WebFetch against `www.twilio.com` (docs live at `twilio.com/docs`, not a separate subdomain — the marketing site and docs share a domain, with `/docs/*.md` serving raw Markdown for every page).

Reachability: fully reachable, extensive real technical documentation (not a marketing shell). All findings below are from pages actually fetched, cited by URL.

---

## 1. Page granularity

**Finding: mixed, and the mix itself is the useful signal.**

- **Reference and error content is split into very small, single-topic pages.** Each REST error code gets its own dedicated URL, e.g. `/docs/api/errors/21211` (~500–600 words: Description → Possible Causes → Possible Solutions → an E.164 formatting example table → Related resources). Confirmed by direct fetch. The full error index (`/docs/api/errors`) is a searchable table of codes linking out to these atomic pages, plus a JSON export of the whole list.
- **Feature/limits pages are also small and single-purpose**, e.g. `/docs/sip-trunking/scale-and-limits` — one page, ~350 words, two sections (Accounts, Trial Accounts), nothing else bundled in.
- **Quickstarts are the exception**, and they're long by design, not by disorganization: `/docs/messaging/quickstart` is ~1,260 words / 5,900 characters, but that length comes from repeating the same 3 steps (prereqs → send → receive) across 8 language tabs (Python, Node, PHP, two C# flavors, Java, Go, Ruby) on one URL, not from cramming unrelated topics together. A reader on one language tab reads roughly 150–200 words.

**Why this matters for QRtub:** QRtub's `help/*.mdx` pages are already single-topic and short-to-medium (194–1,566 words, `help/key-concepts.mdx` the outlier at 1,566). That part is comparable to Twilio's reference-page pattern. The gap is narrower than "Twilio = small pages, QRtub = long pages" — it isn't. The real difference is Twilio's **atomic error/troubleshooting pages with stable per-code URLs**, a granularity tier QRtub's docs don't have an equivalent of at all (see §4).

## 2. Pricing/limits page

**Finding: yes, both exist, both are numeric, and — this is the gap — the *limits* live inside the docs themselves, not just on the marketing site.**

- `/en-us/pricing` (marketing domain, but linked from docs top nav) states concrete per-unit rates, not ranges or "contact us": *"Starts at $0.0083 to send or receive a message"*, *"Starts at $0.0085/min to receive and $0.014/min to make a call"*, *"Starts at $0.05/verification"*. Enterprise-tier products (Connections, Unify, Engage, Segment CDP) do fall back to "Contact sales" — so even Twilio isn't 100% numeric, but the core self-serve products are.
- Separately, **inside the docs proper** (not the pricing page), `/docs/sip-trunking/scale-and-limits` states hard technical quotas with real numbers: *"100 unique SIP trunks"*, *"1 Trunking termination call per second (CPS) (You can increase up to 5 CPS in console)"*, *"Unlimited trunking origination calls per second (CPS)"* — with caveats disclosed inline ("Dependent on carrier support") rather than buried. Trial-account limits are also numeric: 30-day trial window, exhaustible "free product units."

**Why this matters for QRtub:** help.qrtub.com's own pages only ever link *out* to `qrtub.com/pricing` with generic anchor text ("See plans and pricing →") — confirmed via `grep -rn "pricing" help/*.mdx`, 3 hits, all the same boilerplate link, no page in the docs states an actual plan quota (e.g., QR codes per plan, items/assets per plan, print-batch size caps, seats) in a specific number. If QRtub's plans do have such caps, an AI assistant answering "how many QR codes can I create on the Starter plan" has nothing in the docs corpus to retrieve — it would have to guess or bounce the user to the marketing site. Twilio's `scale-and-limits`-style pages are exactly the artifact that closes that gap, and they're cheap to write (one page, one table, per feature area).

## 3. llms.txt / AI-facing artifacts

**Finding: Twilio has two differently-scoped llms.txt files of very different quality, plus AI-facing surfaces beyond llms.txt that QRtub doesn't have.**

| URL | Status | Size | Curation |
|---|---|---|---|
| `www.twilio.com/llms.txt` | 200, `text/markdown` | 2.3 MB / ~9,425 lines | **Poor.** This is effectively a raw crawl dump of the marketing site + full blog archive, un-curated by relevance or recency — e.g. it lists 2012-era blog posts ("JSConf 2012: Bull Rides & Bacon in Scottsdale," "Improve Your Memory with Simolio – Steve Castle Wins a Netbook!") alongside current product pages, with no signal distinguishing current docs from 14-year-old marketing filler. For an AI system trying to ground answers, this is mostly noise. |
| `www.twilio.com/docs/llms.txt` | 200, `text/plain` | 376 KB / ~1,893 lines | **Good.** Organized under H2 headings that mirror the real product taxonomy (Authy, Building with AI, Elastic SIP Trunking, Event Streams, Flex, …), each entry a real one-sentence description pulled from the page's own metadata, relative `.md` links. Comprehensive rather than minimal (nearer in spirit to an `llms-full.txt`), but scoped correctly to docs-only content — no blog/marketing noise. |
| `www.twilio.com/llms-full.txt` and `www.twilio.com/docs/llms-full.txt` | 404 | — | Neither exists; Twilio doesn't ship a separate "full" file at either path — the docs `llms.txt` already plays that role. |

**Beyond llms.txt:** Twilio also ships a hosted **MCP server** (`mcp.twilio.com/docs`, documented at `/docs/ai/mcp`) exposing a search-then-retrieve tool pair over "1,800+ endpoints across 30+ products," plus **"Twilio Skills"** — packaged Agent Skills for Claude Code/Cursor/Codex (`/docs/ai/skills`) that teach coding agents which Twilio product to reach for and known architecture pitfalls. Both are dedicated single-topic doc pages with `dateModified` metadata and an explicit Public Beta disclaimer.

**A genuine tension worth flagging:** Twilio's `robots.txt` sets `Content-Signal: ai-train=no, search=yes, ai-input=no` — it opts OUT of generic AI training and AI-input/RAG use by third-party crawlers, while still allowing search indexing. That's the opposite of what one might expect from a company simultaneously publishing llms.txt and an MCP server: the likely read is Twilio wants AI agents to use its *own* MCP/Skills channel (which it controls and can keep accurate/versioned) rather than have generic AI systems scrape and potentially misrepresent versioned API docs. QRtub's own `robots.txt` currently ships Mintlify's unconfigured default, `Content-Signal: ai-train=yes, search=yes, ai-input=yes` (per `/workspace/mintlify-docs/audit/_raw/external/robots.md`) — i.e. QRtub has taken no position at all, whereas Twilio made a deliberate, asymmetric choice. This is a policy decision, not obviously a "fix," but it's a deliberate structural choice QRtub hasn't made one way or the other.

**Why this matters for QRtub:** QRtub already has both `llms.txt` and `llms-full.txt` (Mintlify auto-generated, confirmed live via `curl -sI`). The comparison that matters isn't "does QRtub have the file" — it does — it's that Twilio's example shows a single, large, un-curated llms.txt (the root one) actively hurts retrieval by diluting the corpus with irrelevant/stale entries, while a docs-scoped, well-described one helps. Worth spot-checking QRtub's own llms.txt for the same failure mode (e.g., stale industry/integration pages with thin content still describing this rather than dropping them from the index descriptions).

## 4. Other structural choices QRtub's docs currently lack

- **Glossary.** `/docs/glossary` is a real, alphabetically-tabbed glossary (7, A–W) with one-line definitions and category tags (e.g., "API: An Application Programming Interface (API) is provided by a service owner so that others may use the features and functions enabled by the service."), each term linking to deeper docs. `grep -rli glossary` across QRtub's `help/`, `industries/`, `integrations/` returns **zero matches** — no glossary exists. For a trades/facilities audience, QRtub's docs already use domain jargon (asset tags, conditional visibility, print batches) that a glossary would help both human newcomers and AI retrieval disambiguate.
- **Troubleshooting/error index.** `/docs/api/errors` is a dedicated, searchable, categorized index of every error code with a JSON export, each linking to an atomic troubleshooting page (see §1). QRtub has no equivalent — `grep -rli troubleshoot` across the same three directories returns only 2 hits, both inside third-party *integration* pages (`integrations/cmms-systems.mdx`, `integrations/safetyculture.mdx`), i.e. troubleshooting content exists only incidentally inside integration guides, not as its own indexed section. This is the single structural gap most likely to affect AI answer quality: "why isn't my QR code scanning" / "why did my print batch fail" style questions have no canonical page to retrieve.
- **`dateModified` machine-readable timestamps.** Every Twilio doc page fetched (`scale-and-limits`, `ai/mcp`, the error page) carries a `schema.org TechArticle` frontmatter block with a real `dateModified` ISO timestamp (e.g. `2026-03-09T18:47:22.000Z`, `2026-07-20T17:19:21.000Z`) — not necessarily rendered visibly to a human reader, but present in structured data for search engines/AI crawlers to gauge freshness. Whether QRtub's Mintlify pages emit equivalent structured freshness metadata wasn't checked in this pass and is worth a follow-up `curl` diff against a live `help.qrtub.com` page's `<head>`.
- **FAQ format.** No dedicated FAQ page/format was found on Twilio's docs site itself (the homepage fetch confirmed this explicitly — "No explicit FAQ section; help directed to 'Need some help?' contact section"). So this is **not** a Twilio strength to imitate — it's a wash, not a gap on QRtub's side.

---

## Sources (all fetched live 2026-08-19)

- https://www.twilio.com/docs (homepage/nav structure)
- https://www.twilio.com/docs/sip-trunking/scale-and-limits (and its `.md` raw source)
- https://www.twilio.com/docs/api/errors and https://www.twilio.com/docs/api/errors/21211
- https://www.twilio.com/docs/glossary
- https://www.twilio.com/docs/ai/mcp (and raw `.md`)
- https://www.twilio.com/docs/messaging/quickstart (raw `.md`, word/heading count)
- https://www.twilio.com/en-us/pricing
- https://www.twilio.com/llms.txt (`curl -sI` + content sample)
- https://www.twilio.com/docs/llms.txt (`curl -sI` + content sample)
- https://www.twilio.com/llms-full.txt, https://www.twilio.com/docs/llms-full.txt (both 404, confirmed via `curl -sI`)
- https://www.twilio.com/robots.txt
- Web search: SMS/voice pricing corroboration (Automation Atlas, CostBench — used only to cross-check numbers already seen on the primary pricing page, not as a primary source)
- QRtub-side comparison points: `/workspace/mintlify-docs/help/*.mdx` (word counts, grep for pricing/glossary/FAQ/troubleshoot), `/workspace/mintlify-docs/audit/_raw/external/robots.md`, `/workspace/mintlify-docs/audit/_raw/external/well-known.md`, and a live `curl -sI` against `help.qrtub.com/llms.txt` and `/llms-full.txt`
