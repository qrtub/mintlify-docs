---
title: "External retrieval audit — help.qrtub.com"
description: "Synthesis of per-page HTTP/ratio audits, llms-full.txt, well-known endpoints, robots.txt/noindex behavior, and competitor doc-practice audits, for all numbers see the underlying _raw files."
---

# External Retrieval Audit — help.qrtub.com

Compiled from 19 per-page audits (`audit/_raw/external/*.md`), the `llms-full.txt` audit, the `robots.md` and `well-known.md` findings, and 7 competitor audits (`audit/_raw/competitors/*.md`). All figures below are drawn directly from those files; nothing here is a fresh measurement. Audited 2026-08-19.

**Scope note:** this file covers the *machine-readability layer* — HTTP behavior, HTML:Markdown ratios, boilerplate token cost, `llms.txt`/`.well-known` infrastructure, and indexing signals. Content-accuracy findings surfaced incidentally in the per-page audits (fabricated Media-tracking claims, fabricated integration-partner names, "asset" vs. "Item" terminology drift, etc.) are not restated here — they live in the individual `_raw/external/*.md` files and belong in a content-accuracy synthesis, not this one.

---

## 1. Per-page HTTP/ratio table

All 19 pages returned `HTTP 200` on **both** the default (HTML) request and the `Accept: text/markdown` / `.md` request. Content-Type was `text/html; charset=utf-8` (HTML) and `text/markdown; charset=utf-8` (Markdown) on every single page, with no exceptions. (`X-Robots-Tag: noindex, nofollow` was present on every Markdown response and absent on every HTML response — see §5.)

| Page | HTML bytes | MD bytes | HTML:MD ratio | Est. tokens (MD, chars/4) |
|---|---:|---:|---:|---:|
| help/app-links | 262,938 | 7,107 | 37.0× | ~1,777 |
| help/building-a-page | 256,895 | 7,047 | 36.45× | ~1,757 |
| help/conditional-visibility | 317,717 | 8,751 | 36.3× | ~2,185 |
| help/creating-your-first-link | 216,783 | 1,409 | **153.9×** | ~352 |
| help/custom-fields | 241,108 | 5,035 | 47.9× | ~1,259 |
| help/device-detection | 298,890 | 9,060 | 32.99× | ~2,250–2,265 |
| help/key-concepts | 295,139 | 10,683 | 27.6× | ~2,656 |
| help/media-basics | 276,671 | 7,201 | 38.4× | ~1,790 |
| help/pages-overview | 220,243 | 2,570 | 85.7× | ~640 |
| help/print-batches | 238,058 | 3,841 | 61.98× | ~960 |
| help/print-first-workflow | 240,223 | 5,987 | 40.1× | ~1,497 |
| help/using-fields | 412,897 | 12,460 | 33.14× | ~3,115 |
| industries/arboriculture-tree-management | 257,205 | 11,020 | 23.3× | ~2,748 |
| industries/civil-construction | 293,589 | 7,746 | 37.9× | ~1,933 |
| industries/contract-cleaning | 242,119 | 7,618 | 31.8× | ~1,895 |
| industries/electrical-test-and-tag | 269,069 | 11,817 | **22.77×** (lowest) | ~2,950 |
| industries/local-government-councils | 266,293 | 12,496 | 21.3× (lowest) | ~3,117 |
| integrations/cmms-systems | 252,299 | 4,233 | 59.6× | ~1,058 |
| integrations/safetyculture | 296,494 | 9,411 | 31.5× | ~2,345 |

### Site-wide totals (19 pages)

| Metric | Value |
|---|---:|
| Total HTML bytes | **5,154,630** |
| Total Markdown bytes | **145,492** |
| Weighted (total÷total) HTML:MD ratio | **35.43×** |
| Mean of the 19 per-page ratios | **45.24×** (skewed up by the 153.9× outlier on a very short page) |
| Total estimated tokens (sum of per-page chars/4 estimates) | **~36,292** |
| Average tokens/page | **~1,910** |
| Average HTML bytes/page | ~271,296 |
| Average MD bytes/page | ~7,657 |

**Cross-check:** `llms-full.txt` (the 19 pages concatenated) is 142,968 bytes / ~35,618–35,742 tokens. Subtracting the ~836 tokens of "Documentation Index" banner that appears on every individually-fetched page but is *absent* from `llms-full.txt` (see §2) from the ~36,292-token individual-fetch sum gives ~35,456 — consistent with `llms-full.txt`'s own token estimate. The two independent measurements reconcile.

**Pattern:** ratio is inversely related to page length, not a fixed site constant — the fixed ~250–270KB of Next.js/Mintlify chrome (nav, hydration payload, CSS/JS) is amortized over more or less body content. The shortest page (`creating-your-first-link`, 1,409 MD bytes) has the single highest ratio (153.9×); the two longest pages (`local-government-councils` 12,496 bytes, `using-fields` 12,460 bytes) have the two lowest ratios among their categories (21.3× and 33.14×).

---

## 2. Boilerplate waste — site-wide quantification

Two distinct boilerplate contexts exist and must not be conflated: **(A)** what an agent gets when it fetches one page's Markdown individually, and **(B)** what's actually present in the concatenated `llms-full.txt`. The two differ materially.

### (A) Individual per-page `.md` fetch — banner + footer, confirmed on all 19 pages

| Boilerplate string | Occurrences | Bytes/occurrence (approx, varies by page) | Total bytes (19 pages) | Tokens/occurrence | Total tokens |
|---|---:|---:|---:|---:|---:|
| Top "Documentation Index" banner (3-line blockquote, before the real H1) | 19/19 | ~175–177 | ~3,344 | ~44 | **~836** |
| Bottom footer CTA + contact block (after `***`) | 19/19 | ~149–157 (varies by whether the audit counted the `***` separator/blank lines) | ~2,930 | ~37–39 | **~734** |
| **Combined banner + footer** | 19/19 | ~324–334 | **~6,274** | ~82–83 | **~1,570** |

This is what an agent pays, per page, every time it fetches a page's `.md` individually — confirmed byte-for-byte identical (aside from trivial whitespace-counting differences between audits) across every one of the 19 pages audited: `app-links`, `building-a-page`, `conditional-visibility`, `creating-your-first-link`, `custom-fields`, `device-detection`, `key-concepts`, `media-basics`, `pages-overview`, `print-batches`, `print-first-workflow`, `using-fields`, all 5 `industries/*` pages, and both `integrations/*` pages.

**The boilerplate *share* of a page's tokens is inversely proportional to page length** — the fixed ~82-token tax is punishing on short pages and negligible on long ones:

| Page | Boilerplate share |
|---|---:|
| creating-your-first-link (352 tokens total) | **~24%** (highest) |
| pages-overview (640 tokens) | ~12.7% |
| print-batches (960 tokens) | ~10.2% (~13% incl. "Related" scaffold) |
| cmms-systems (1,058 tokens) | ~9.7–10% (incl. 2 duplicated "Resources" links) |
| custom-fields (1,259 tokens) | 6.6% (9.2% incl. Related) |
| using-fields (3,115 tokens) | 2.7% (6.3% incl. Related) |
| local-government-councils (3,117 tokens) | **~2.65%** (lowest, plus ~4.64% more if the "Ready to Deploy?" scaffold is counted) |
| key-concepts (2,656 tokens) | ~3.05% |

Full per-page figures: app-links 4.6%, building-a-page 4.7–7.3%, conditional-visibility 3.75%, device-detection 3.7%, media-basics 4.6%, print-first-workflow 5.6%, arboriculture 3.0% (5.8% incl. "Core features available" bullets), civil-construction 4.3–6.9%, contract-cleaning 4.3%, electrical-test-and-tag 3.2%, safetyculture 3.5% (4.4% incl. 2 duplicated Resources links).

**Not counted as boilerplate anywhere:** the "## Related" / "## Next Steps" / "## Resources" link-list *pattern* recurs on nearly every page, but its link *targets* are page-specific and genuinely useful, so per-page audits consistently exclude it from the boilerplate byte count — with two named exceptions: `integrations/cmms-systems.md` and `integrations/safetyculture.md` both append the identical two links `[Pages Overview](/help/pages-overview)` and `[Key Concepts](/help/key-concepts)` (78 bytes) after their page-specific resource links — a partial, literal duplication confirmed on at least these 2 of the 19 pages.

### (B) `llms-full.txt` (the concatenated corpus) — only the footer recurs byte-identical

Per the dedicated `llms-full.txt` audit (exact `grep -c`/`grep -Fc` counts, not estimates):

| String | Exact count | Bytes each | Total bytes | Tokens |
|---|---:|---:|---:|---:|
| `**Ready to get started with QRtub?** [See plans and pricing →](https://qrtub.com/pricing)` | 19 | 91 (92 incl. newline) | 1,748 | ~437 |
| `Questions? Email us at [hi@qrtub.com](mailto:hi@qrtub.com)` | 19 | 58 | 1,121 | ~280 |
| `***` (footer-separator instances only, of 31 total `***` lines — the other 12 are internal dividers inside the "Key Concepts" page, not cross-page boilerplate) | 19 | 3 | 76 | ~19 |
| **Full reconstructed footer block** (`***` + CTA + blank lines + Questions line) | 19 | 159/occurrence | **3,021** | **~745–756** |

**3,021 of 142,968 bytes = 2.11% of the entire `llms-full.txt` file.** The top-of-file "Documentation Index" banner (the ~836-token item counted in table A) does **not** appear in `llms-full.txt` at all — Mintlify injects it only on individually-fetched page Markdown, not the concatenated dump. So stripping the footer from `llms-full.txt` would save only ~755 tokens (~2.1%) — not a meaningful reduction; the file is 97.89% unique, substantive content by this measure.

**Structural-but-not-byte-identical templates** (authored redundancy, wording differs each time, so not counted as boilerplate bytes — reported as occurrence counts only):
- `## Ready to Deploy?` heading + `**Core features available:**` lead-in — exactly **5** occurrences, all 5 `/industries/*` pages and only those; bullet content differs per industry.
- `**Note on security:**` paragraph — **3** occurrences (arboriculture, contract-cleaning, local-government-councils); paraphrased each time, not identical.
- `## Related` heading — **9** occurrences (mostly `/help/*` pages); each followed by a different, page-specific link list.

---

## 3. `llms-full.txt` findings

| Metric | Value |
|---|---:|
| Total bytes | **142,968** |
| Total lines | 3,734 |
| Total Unicode characters | 142,470 |
| Estimated tokens (chars/4) | **~35,618** (UTF-8 char count) / ~35,742 (bytes/4) |
| Pages concatenated | **19** (12 `/help/*`, 5 `/industries/*`, 2 `/integrations/*`) |
| Average bytes/page | ~7,525 (~1,880 tokens) |
| Byte-identical cross-page boilerplate | 3,021 bytes = **2.11%** of file |
| Unique per-page content | 139,947 bytes = **97.89%** of file |

Boilerplate breakdown is the table in §2(B) above: only the footer CTA + contact + separator block is byte-identical across all 19 pages (2.11% of the file); the `## Ready to Deploy?`/`**Core features available:**` skeleton (5 pages), `**Note on security:**` paraphrase (3 pages), and `## Related` heading (9 pages) are structural templates with page-specific wording, not counted as duplicate bytes. **Conclusion of the source audit:** stripping the footer would only save ~2.1% of the file — this is a substantive, largely non-redundant corpus, not a padded one.

---

## 4. Well-known endpoints findings

Four endpoints checked live via `curl`; all are Mintlify auto-generated with zero explicit `docs.json` configuration.

| Endpoint | Status | Content | Accuracy | Configurable? |
|---|---|---|---|---|
| `/.well-known/mcp/server-card.json` | 200, `application/json`, `cache-control: public, max-age=86400` | MCP server card; real MCP server confirmed live at `/mcp` (returns the expected MCP-spec error when the required dual `Accept: application/json, text/event-stream` header is omitted — i.e., a genuine server, not a stub) | **Naming defect:** `"description":"Search and retrieve QRtub documentation documentation"` and `serverInfo.name: "QRtub documentation Docs MCP"` — a doubled "documentation documentation," traced to `docs.json`'s top-level `name` field being set to `"QRtub documentation"`, into which Mintlify's template appends the literal word "documentation" again | Indirectly — changing `docs.json.name` to `"QRtub"` would collapse the doubling, but that field also drives the site's browser-tab title, so it's a site-wide naming decision, not a narrow fix |
| `/.well-known/agent-card.json` | 200, `application/json`, `cache-control: public, max-age=86400` | A2A agent card | `"name"` and `"provider.organization"` both echo `"QRtub documentation"` verbatim (same root cause as above, no doubling this time) — reads oddly machine-to-machine but is not technically wrong. The one listed skill is `id: "teralis"`, not `"QRtub"` (see next row) | Same lever as above for the name/org fields |
| `/.well-known/agent-skills/index.json` (+ `/.well-known/agent-skills/teralis/skill.md`) | 200, `application/json` (index) / `text/markdown` (skill file) | One skill: `name: "teralis"`. Frontmatter shows `metadata.mintlify-proj: teralis` — this is Mintlify's **internal project slug**, not sourced from `docs.json` at all (grepped: no "Teralis"/"teralis" string anywhere in `docs.json`). Almost certainly the original Mintlify dashboard project name from before the product was branded QRtub (product is legally owned by Teralis Pty Ltd) | Skill *content* is a largely accurate mirror of `CLAUDE.md` (entity model, Link types, field-binding syntax, no-per-item-Media-tracking correction all check out) with **one inherited accuracy gap**: lists `session.user.id`/`session.user.email`/`session.user.name` fields and omits any `time.*` field, whereas `CLAUDE.md` (the repo's source-of-truth) lists only Item/Tub/Device/**Time** fields (`time.hour`, `time.dayOfWeek`, `time.dayOfMonth`, `time.month`, `time.year`, `time.isWeekend`) for CEL conditions and does not mention a `session` namespace at all. The skill.md faithfully reflects what `help/using-fields.mdx` and `help/conditional-visibility.mdx` currently publish — so the discrepancy originates in those two pages, not in the skill-generation step, but it is now propagating into the AI-agent-facing artifact | **Yes, directly** — a root `skill.md` or `.mintlify/skills/qrtub/SKILL.md` in this repo overrides the auto-generated "teralis"-named one (per Mintlify's own `/docs/ai/skillmd` docs, fetched live for this check) |
| `/.well-known/api-catalog` | **404**, `text/plain;charset=UTF-8`, `cache-control: public, max-age=0, must-revalidate` | Body: `Not Found` | Genuinely absent, not a caching artifact (`must-revalidate, max-age=0` on the 404 itself). No Mintlify documentation mentions this endpoint; QRtub's `docs.json` has no OpenAPI/AsyncAPI spec configured, so there is no underlying API surface for it to describe even if implemented | Not configurable — not a documented Mintlify feature at all |

**Internal inconsistency worth flagging:** the `robots.md` audit's side-finding states the `Link:` response header's `rel="api-catalog"` entry "confirms Mintlify does now serve a `/.well-known/api-catalog` discovery endpoint" — but the dedicated `well-known.md` audit *directly fetched that URL* and got a 404. Every page in this corpus advertises `Link: </.well-known/api-catalog>; rel="api-catalog"` in its response headers, pointing at an endpoint that does not exist. This is a discoverability header advertising a dead link, universal across the site.

**Two concrete repo-side actions flagged by the source audit (not made unilaterally):**
1. Add a custom `skill.md` (or `.mintlify/skills/qrtub/SKILL.md`) naming the skill "QRtub" instead of "teralis," and correct the `session.*`/`time.*` field list once verified against `../qrtub/src/lib/page/context.ts`.
2. Consider whether `docs.json`'s top-level `name` should be `"QRtub"` rather than `"QRtub documentation"` — fixes the MCP server-card doubling, at the cost of also changing the browser-tab title site-wide.

---

## 5. robots.txt / noindex / Mintlify-guidance cross-check — deliberate vs. accidental

### robots.txt — confirmed unconfigured Mintlify default, not a QRtub decision

```
User-agent: *
Content-Signal: ai-train=yes, search=yes, ai-input=yes
Disallow: /cdn-cgi/
Allow: /_next/image
Disallow: /_next/
Sitemap: https://help.qrtub.com/sitemap.xml
```

Verdict, with evidence: **unconfigured default.** No root-level `robots.txt` override file exists in the repo; `grep -in 'seo\|robots\|indexing\|noindex' docs.json` returns zero matches; the `Content-Signal` line matches Mintlify's documented auto-generated default verbatim; `/cdn-cgi/` and `/_next/` are Next.js/Vercel/Cloudflare platform paths, not QRtub-authored rules; nothing disallows any `/help/`, `/industries/`, or `/integrations/` path. **This is itself a finding**: QRtub has taken no explicit AI/crawling policy position at all — contrast with Twilio, which deliberately sets `Content-Signal: ai-train=no, search=yes, ai-input=no` (opting OUT of third-party AI training/RAG input while still allowing search indexing, apparently to steer agents toward its own MCP/Skills channel instead).

### X-Robots-Tag: noindex, nofollow — universal on every `.md` response, absent on every HTML response

Confirmed independently by **all 19 per-page audits** and by `robots.md`'s own separate 3-page spot check (`help/key-concepts`, `help/app-links`, `integrations/safetyculture` — all three consistent, headers identical in shape). Zero exceptions found anywhere in the corpus:

- Every Markdown response (via `Accept: text/markdown` **and** via the literal `.md` URL — confirmed identical) carries `x-robots-tag: noindex, nofollow`.
- Every default HTML response carries **no `X-Robots-Tag` header at all** — i.e., indexable by default, with a `<link rel="canonical">` instead.

**Not documented anywhere in Mintlify's own public docs** (per cross-check against `mintlify-guidance.md`, which is explicit that "this specific claim is not addressed anywhere in Mintlify's published docs, in either direction... None mention an `X-Robots-Tag` HTTP header at all"). This must be reported as an *observed live behavior*, not attributed to any published Mintlify policy.

**Practical effect, repeated across nearly every one of the 19 per-page audits:** the representation built specifically for machine/agent consumption — and actively advertised via `Link: rel="llms-txt"`, `rel="llms-full-txt"`, `rel="mcp-server-card"`, `rel="agent-card"`, `rel="agent-skills"` on every response, plus the in-body "Documentation Index" banner — is simultaneously the one representation told not to be indexed or have its links followed by any crawler that honors `X-Robots-Tag`. The 21–154×-heavier HTML page remains the indexable canonical. This does not block **direct-fetch** agents (a user pasting a URL, a coding agent's own `curl`/`WebFetch`, an MCP tool call — none of these consult `X-Robots-Tag`), but it does mean crawler-driven AI answer engines that respect the header (several document that they do) would index only the noisy HTML, never the clean Markdown twin, even though the site's own `llms.txt`/`Link`-header infrastructure is actively inviting that same class of crawler to explore further. The two signals — `robots.txt`'s permissive site-level `Content-Signal` and the per-resource `X-Robots-Tag: noindex` on `.md` responses — are not strictly contradictory (a crawler can be allowed to fetch a resource and still be told not to index that specific response), but they pull in different directions for exactly the crawler-type agent the `.md`/`llms.txt` setup exists to serve.

### Verdict: deliberate vs. accidental

| Signal | Deliberate or accidental? | Evidence |
|---|---|---|
| `robots.txt` content | **Accidental** (unconfigured platform default) | No override file, no `docs.json` config, content matches documented Mintlify default byte-for-byte |
| `X-Robots-Tag: noindex, nofollow` on `.md` only | **Ambiguous — presumed platform default, not QRtub-specific**, but genuinely undocumented by Mintlify itself | Universal across all 19 pages with zero exceptions; most plausible read is anti-duplicate-content indexing (keep HTML canonical, keep the `.md` twin out of search indexes), but this is inference, not confirmed policy — no Mintlify doc states it either way |
| `Link:` header discovery scaffolding (`llms.txt`, `llms-full.txt`, MCP/agent-card/agent-skills) | **Deliberate, and a genuine strength** | Present identically and correctly on every HTML and Markdown response checked across all 19 pages |
| `/.well-known/api-catalog` advertised but 404 | **Accidental** (broken discovery pointer) | Header claims it exists; direct fetch returns 404 with `must-revalidate` |

---

## 6. Competitor comparison

Only differences plausibly material to QRtub's outcomes, by competitor.

### Asset Panda
- **No public pricing/limits numbers anywhere** — `/pricing` is 100% lead-gen ("Get a personalized quote"), zero plan names, prices, or quotas disclosed. Direct opportunity: any specific QRtub plan quota beats this outright for both self-service and AI-comparison queries.
- `llms.txt` exists (200 OK, `last-modified` current) but is a **Yoast SEO plugin byproduct**, not curated — flat list of ~45 marketing/industry pages, 5 blog posts, 5 press releases, and **zero links to `help.assetpanda.com`** (the actual support docs). Cautionary example: an AI-facing index that omits the real help content is worse than no index.
- Long multi-subtopic pages (`Consumables_and_Inventory_Management.html`, ~2,500+ words covering 5 distinct sub-topics with an anchor-jump TOC) — confirms QRtub's existing single-topic-page discipline is already a relative advantage, not a gap to close.
- No "last updated" stamps on any knowledge-base article checked.

### Flowcode
- Two `llms.txt` files of very different quality: the marketing-domain one is a thin, unfinished-looking placeholder (`x-robots-tag: noindex`, "Not yet launched" blog stub) with zero product-usage content; `help.flowcode.com/llms.txt` is genuinely well-curated (13 collections, ~158 articles, each with a clean per-article `.md` mirror at `<url>.md`, ~18.7KB total). **Lesson: scope the curated index to actual help content, not the marketing domain.**
- **Most transferable finding of the whole competitor set:** a dedicated, dated FAQ article ("Flowcode Scan Data Caps: FAQs") states an exact plan-by-plan numeric table (Basic 500 / Pro 2,000 / Pro+ 6,000 / Growth 10,000 / Enterprise unlimited scans), an explicit effective date ("in effect as of 02/29/2024"), and the precise failure-mode behavior in plain language (code/page keeps working past the cap; only analytics visibility is lost; no overage charge) — reinforced by a second FAQ phrasing of the same fact. QRtub has no plan-quota numbers anywhere in its docs corpus at all (confirmed by grep in the Linear/Twilio comparisons below).
- Visible "last updated" byline dates on individual articles (e.g., "Feb 28, 2024").
- Policy changes get their own dated FAQ artifact rather than a silent pricing-page edit — an explicit, citable "what changed and when" record.

### Linear
- **Page granularity, with exact word counts**: Linear's topic pages run 243–994 words (`priority.md` 243, `start-guide.md` 262, `issue-relations.md` 412, `parent-and-sub-issues.md` 752, `sla.md` 994), one URL per sidebar nav item. QRtub's `/help/*.mdx` pages run 600–1,566+ words with multiple sub-topics folded into one page (`key-concepts.mdx` 1,566 words, `using-fields.mdx` 1,373, `device-detection.mdx` 1,219, `conditional-visibility.mdx` 1,185) — **2–6× longer**, confirmed directly against the source `.mdx` files, not just the rendered Markdown. This is the clearest, most actionable granularity finding across all 7 competitors.
- Pricing page states specific numbers with explicit "Unlimited" where uncapped, never vague (Teams: 2/5/Unlimited/Unlimited across Free/Basic/Business/Enterprise; Issues: 250/Unlimited/Unlimited/Unlimited; file upload 10MB/Unlimited/Unlimited/Unlimited; integrations 15/Unlimited/Unlimited/Unlimited), plus a stated behavioral consequence ("If you have over 250 issues, you will no longer be able to create new issues" after downgrade). QRtub's docs repo has **zero** pricing/limits content anywhere (confirmed by direct search of `docs.json`/`help/`/`industries/`/`integrations/`).
- `llms.txt` (9.7KB) is well-curated and every linked `.md` page checked (`sla.md`, `priority.md`, `issue-relations.md`, `parent-and-sub-issues.md`) carries **zero repeated marketing footer** — direct contrast with QRtub's confirmed 19/19-page identical CTA+footer (see §2/§3). Linear's `llms-full.txt` doesn't actually exist despite a 200 response (it's a fake HTML soft-404 shell) — QRtub's own `llms-full.txt` is confirmed genuine by contrast (§3), so this specific point is not a gap for QRtub.
- Inline per-page FAQ using semantic `<details><summary>` collapsible blocks (`sla.md` has 4 Q&A pairs) that round-trips cleanly into the Markdown export — QRtub currently has no FAQ pattern, inline or standalone, anywhere.

### QR Tiger
- `llms.txt` (~104 lines) is genuinely well-curated: H1 summary, categorized `##` sections (Core Product, 23 QR-type entries, Pricing Plans with numbers inline, Industry Solutions, Features & How-Tos, Integrations, Resources, Company, Mobile Apps), one-line description per link — a reasonable structural template.
- `llms-full.txt` returns HTTP 200 but is a **false positive**: `content-type: text/html`, body is the full Next.js app shell (soft-404 rendered as 200, confirmed because a random garbage path on the same domain correctly 404s). Not a QRtub gap directly (QRtub's own `llms-full.txt` is confirmed real, §3) but a routing check worth keeping in mind if QRtub's hosting ever changes.
- Pricing states specific per-plan numbers (Free: 3 dynamic QR codes/500 scans per code; Regular $7/mo: 12 QR codes/500 API requests/mo/5MB upload; Advanced $16/mo: 200 QR codes/3,000 API requests/mo/bulk batch to 3,000/10MB; Premium $37/mo: 600 QR codes/10,000 API requests/mo/20MB), reserving "unlimited" only for genuinely uncapped items (static QR codes, scans on paid plans, downloads, folders).
- A well-formed troubleshooting index (`/qr-code-not-working`: 25 problems, quick-reference TOC, quick-fix pass, per-problem detail, pro-tips, 9-question FAQ) — QRtub has no equivalent (confirmed via the Twilio comparison below: only 2 grep hits for "troubleshoot" across QRtub's docs, both incidental inside integration pages).
- "Last updated" stamps are inconsistent, not absent — troubleshooting page shows both a publish and update date, API docs show "Last Updated: August 19, 2025," but the FAQ page (which states plan numbers and a 1-year data-retention window) has **no date at all** — the page type most in need of a freshness signal lacks one.
- No glossary (`/glossary`, `/qr-code-glossary` both 404).

### Stripe
- **The sharpest, most portable finding in the whole competitor set:** `docs.stripe.com/rate-limits` gives exact numeric limits for every resource — Global API 100 req/s live / 25 req/s sandbox; individual endpoints 25 req/s default; Payment Intents 1,000 update requests/PaymentIntent/hour; Subscriptions 10 new invoices/subscription/minute + 20/day + 200 quantity updates/subscription/hour; Files API 20 read/s + 20 write/s; Payouts 15 create/s + 30 concurrent/business; Connect account creation 30/s live + 5/s sandbox — plus a worked example for a derived limit ("100 transactions in 30 days → don't exceed 50,000 read requests," floor of 10,000 read requests/month minimum) and the exact `429` response headers (`Stripe-Rate-Limited-Reason` values: `global-rate`, `endpoint-rate`, `global-concurrency`, `endpoint-concurrency`, `resource-specific`) so a caller can identify *which* limit was hit. If QRtub's plan-quota docs are vague or omit failure-mode behavior, this is the concrete model to match.
- **Every page has a parallel clean-Markdown mirror** (`docs.stripe.com/<path>.md`, verified on a dozen+ pages) — `llms.txt` (89,866 bytes, 640 lines, 454 curated links across 26 categories, one-line description each) is just the curated index *into* that mirror, a materially stronger setup than an index file alone.
- `llms.txt` opens with an explicit **LLM steering block** ("Instructions for Large Language Model Agents") naming deprecated patterns to never recommend (Charges API, legacy Card Element, v1 Accounts API, outdated Standard/Express/Custom terms) — directly relevant if QRtub's own terminology drift (e.g., the SafetyCulture→Mitti rename, flagged inconsistently handled across several per-page audits — "Mitti (formerly SafetyCulture)" vs. "Mitti (iAuditor)" used interchangeably on the same page in `civil-construction.md`) risks an AI assistant citing stale names.
- Dedicated `skills.md` + `/.well-known/skills/index.json`, with individual doc pages carrying page-level metadata tagging which skill bundle applies (confirmed in raw HTML: `"skills":["stripe-docs","stripe-best-practices"]`) — a level beyond QRtub's single, generically-named `teralis` skill (§4).
- Standalone glossary page (274 lines, bolded term + 1–2 sentence definition, dozens of terms) — QRtub has none.
- `robots.txt`: `Content-Signal: ai-train=yes, search=yes, ai-input=yes` stated as a deliberate positive stance — same values as QRtub's, but Stripe's is asserted as policy while QRtub's is an unconfigured default (§5).

### Twilio
- **Atomic, stable-URL error/troubleshooting pages** — every REST error code gets its own page (`/docs/api/errors/21211`, ~500–600 words: Description → Possible Causes → Possible Solutions → example table → Related resources), backed by a searchable index with a JSON export. QRtub has no equivalent tier at all: grep for "troubleshoot" across `help/`, `industries/`, `integrations/` returns only **2** hits, both incidental inside `integrations/cmms-systems.mdx` and `integrations/safetyculture.mdx` — no dedicated troubleshooting section exists.
- **Limits live inside the docs themselves, not just the marketing pricing page**: `/docs/sip-trunking/scale-and-limits` states "100 unique SIP trunks," "1 Trunking termination CPS (up to 5 CPS in console)," "Unlimited origination CPS," a 30-day trial window, exhaustible free product units. QRtub's help pages only ever link *out* to `qrtub.com/pricing` with identical generic anchor text ("See plans and pricing →") — confirmed exactly **3** grep hits across `help/*.mdx`, all the same boilerplate link, zero actual plan-quota numbers stated inside the docs corpus itself.
- Two `llms.txt` files of opposite quality from what might be assumed: the root `www.twilio.com/llms.txt` is **poor** — a 2.3MB / ~9,425-line raw crawl dump mixing 2012-era blog posts with current product pages, no curation signal at all; the docs-scoped `www.twilio.com/docs/llms.txt` is **good** — 376KB / ~1,893 lines, organized under real product-taxonomy H2 headings, no marketing/blog noise. Confirms: scope the curated index to docs-only content.
- Deliberate, asymmetric `robots.txt`: `Content-Signal: ai-train=no, search=yes, ai-input=no` — opts OUT of third-party AI training/RAG input while still allowing search indexing, apparently to push agents toward Twilio's own controlled MCP server (`mcp.twilio.com/docs`) and "Twilio Skills" instead. QRtub's `robots.txt` is Mintlify's unconfigured default (`ai-train=yes, search=yes, ai-input=yes`, §5) — QRtub has made no equivalent deliberate choice either way.
- Alphabetically-tabbed glossary (`/docs/glossary`, A–W, one-line definitions + category tags) — grep for "glossary" across QRtub's `help/`, `industries/`, `integrations/` returns **zero** matches.
- Every Twilio doc page carries a `schema.org TechArticle` `dateModified` ISO timestamp in structured data (e.g., `2026-03-09T18:47:22.000Z`) — not necessarily human-visible, but present for search/AI-crawler freshness signaling. Whether QRtub's Mintlify pages emit equivalent structured freshness metadata was not checked in that audit pass.
- No FAQ page/format exists on Twilio's own docs either — a wash, not a gap for QRtub to close.

### Uniqode
- KB taxonomy (`docs.uniqode.com`, Intercom-hosted) has **8 top-level collections**: QR Codes, Cards, Integrations, AI Capabilities, Insights, **Account Management**, Developer, **FAQs**. QRtub's `docs.json` groups (Getting Started, Concepts, Items & Data, Pages, Printing) are all product-mechanics groups — no billing/account-management, analytics, or FAQ home exists for a user with that kind of question.
- Pricing states specific, directly-confirmed numbers ($9/mo entry tier, $49/mo mid-tier, custom-domain add-on "$2,000 per domain per year," Digital Business Card seats "$6 per user per month," 14-day free trial, 30-day money-back guarantee) plus a third-party-corroborated tier ladder (Starter $5/mo: 3 dynamic codes/25,000 scans; Lite $15/mo: 50 codes; Pro $49/mo: 250 codes; Plus $99/mo: 500 codes; analytics retention 30/90/180 days) — numeric quotas per tier, vague only on the top custom-priced tier.
- `llms.txt` (11,208 bytes, 135 lines, confirmed live, `last-modified 2026-07-02`) is well-structured — including a compact glossary-as-data "## Key Concepts" block (Phygital, Smart Rules, Scan Journey Intelligence, Two-Way Contact Sharing, each one-line-defined) directly quotable by an LLM — **but contains zero links into `docs.uniqode.com`**, the actual help center. Same "excludes the real help content" failure mode as Asset Panda's and Flowcode's marketing-domain `llms.txt` — a pattern now confirmed on 3 of 7 competitors, worth explicitly avoiding.
- Separate bespoke long-form AI-facing page, `uniqode.com/about/ai-info` (~3,500–4,000 words), explicitly titled for AI consumption ("structured, canonical information about Uniqode, intended for AI assistants such as ChatGPT, Claude, Perplexity, Gemini, Google AI Overviews"), with sections "Instructions for AI Assistants," "Competitive Context," and a dedicated FAQ block — a more aggressive AI-positioning artifact than a plain `llms.txt`.
- Author + last-updated byline on every KB article ("Written by [Name]" + specific date, e.g., "October 14, 2025") — QRtub has zero author/date stamps anywhere (confirmed by grep across all `help/*.mdx`).
- Per-article TOC + "Related Articles" + "Read next" + a "Did this answer your question?" feedback widget — QRtub's sampled page has only a manual "Next Steps" with 1–2 hand-picked links, no feedback mechanism, no auto-TOC.
- Dedicated FAQ collection as its own top-level KB category — QRtub has none, though `CLAUDE.md` already stages an "FAQ answer" prompting template, confirming the gap is recognized internally but not yet built.

---

## Cross-cutting pattern across all 7 competitors

The single most-repeated failure mode, independently confirmed on **3 separate competitors** (Asset Panda, Flowcode's marketing-domain file, Uniqode) is an `llms.txt` that indexes marketing/blog/press content but contains **zero links to the actual help-center/support documentation** — an AI-facing artifact that looks complete but would leave an AI assistant unable to answer any "how do I actually do X" question from that index alone. If QRtub builds or maintains an `llms.txt`, the one universal lesson is: it must index the real `/help/*`, `/industries/*`, `/integrations/*` content, not just marketing pages.

The single most-repeated *opportunity* is specific, numeric plan quotas (confirmed present at Flowcode, Linear, QR Tiger, Stripe, Uniqode — 5 of 7 competitors) versus QRtub's confirmed-zero plan-quota content anywhere in this docs repo (grep-confirmed against `docs.json`/`help/`/`industries/`/`integrations/` in the Linear and Twilio audits).
