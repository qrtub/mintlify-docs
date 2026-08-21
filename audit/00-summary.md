---
title: "Executive summary — AI-readiness audit of help.qrtub.com"
description: "Top-ten prioritized fixes, a proposed information architecture, three docs.json description drafts, a false-positive list, and everything the audit could not verify — synthesized from the external, repo, and editorial phase reports."
---

# AI-Readiness Audit — Executive Summary

Synthesizes `01-external.md`, `02-repo.md`, `03-editorial.md`, `retrieval-grid.csv` (+ its summary), `frontmatter-inventory.csv`, `terminology-violations.csv`, `_raw/git-history.md`, and `_raw/mintlify-guidance.md`. Audited 2026-08-19.

**Mechanical fixes already made during this run** (confirmed via `git log --oneline audit/ai-readiness`), so they are not re-listed as findings below:

| Commit | What it did |
|---|---|
| `06a88a9` | Normalised all 38 US→AU/UK spellings identified in the repo audit |
| `ddbbaf9` | Fixed the two broken `index.mdx` Card `href`s (`/industries`, `/integrations`) |
| `f56d90f` | Removed 15 confirmed-inert frontmatter fields (`sidebar_position`/`category`/`slug`) — note its own message says "0 auto-derived descriptions," meaning `index.mdx`'s and `media-basics.mdx`'s bad descriptions (Finding area below) were **not** touched by this commit and are still open |

`20e4168` (URL-template binding fix) and `004873f` (SafetyCulture→Mitti rename) predate this audit (2026-08-17) and are the subject of the coverage-gap and terminology re-checks, not fixes made by this run.

---

## 1. Top Ten Findings

Ordered by impact ÷ effort, highest first.

### 1. `docs.json` has no top-level `description` — `llms.txt` ships with an empty blockquote
**Fix:** add one sentence (see §3 for three drafted options) as `docs.json`'s top-level `description` field.
**Effort: trivial.** One field, verified by Mintlify's own docs (`/docs/ai/llmstxt`) to be the literal source of the blockquote under the H1 in every AI tool's first read of the site. Currently every AI agent reading `/llms.txt` gets a bare title and nothing else. Highest impact-per-character-typed fix in the whole audit.

### 2. `key-concepts.mdx` states fabricated Media capabilities — cost tracking, durability, per-item inventory, "replacement" workflow
**Fix:** delete the "Why track Media separately" bullet list and the fabricated `MEDIA: Metal Plaque #4729` example block (lines ~45–91) from `help/key-concepts.mdx`; keep only the real, shipped fact (print batches are tracked) and point to `help/media-basics.mdx`, which already states the correct planned-vs-shipped line accurately.
**Effort: small.** This is the single most severe defect flagged in the entire editorial phase for this page, it directly contradicts `CLAUDE.md`'s own explicit warning ("Pages have previously documented ... Media type tracking. None of these exist"), and it sits on the page every other help page treats as the canonical entity-definition source — a retrieval agent hitting this page has no way to know the claim is false.

### 3. `key-concepts.mdx` undermines its own canonical terms in the same breath it defines them
**Fix:** rewrite line 186 ("Think of Tubs as: Folders, categories, or asset types—but with superpowers") to drop the banned-synonym framing; remove "simple redirect(s)" at lines 108 and 264 (and the same phrase in `help/pages-overview.mdx:22`) and replace with "Direct Mode."
**Effort: trivial.** Four sentence edits, but this is nav position #3, the page every other page defers to for the Tub and Direct Mode definitions — GLOSSARY.md's explicit "Instead of" banned-synonym list is being used as the definition itself.

### 4. Four page-specific correctness bugs where the docs state the opposite of, or omit, what the app source does
Each independently confirmed against `../qrtub/src/` in the editorial phase, each the single most-severe issue logged for its page:
- **`help/custom-fields.mdx`** — claims allowed-values CSV validation is unconditional; it is skipped entirely when "Allow new values" is on. Fix: state the actual conditional behavior.
- **`help/print-batches.mdx`** — claims exporting always creates a tracked batch ("not a neutral download"); a genuine neutral CSV-only download exists in the app. Fix: document both paths.
- **`help/app-links.mdx`** — conflates two different failure UIs (a styled fallback panel vs. a native browser `alert()`) as one. Fix: describe both, and when each fires.
- **`help/device-detection.mdx`** — documents only the Page-Mode routing path; omits a simpler, already-shipped mechanism (per-Item Conditional Rules). Fix: add the missing mechanism, and state which one a reader should reach for first.
**Effort: small** (one paragraph each, all four already verified against source by the editorial audit — no further research needed, just the rewrite).

### 5. The Device Fields reference table is triplicated, verbatim, across three sibling pages
**Fix:** keep the full table only on `help/using-fields.mdx` (the natural canonical field reference); replace the copies on `help/conditional-visibility.mdx` ("Quick Device Field Reference," lines 167–191) and `help/device-detection.mdx` ("Available Device Information") with a one-line pointer.
**Effort: small.** Confirmed identical field names/types/examples on all three pages by two independent editorial passes. Every future field addition currently means three edits instead of one, and it inflates retrieval-chunk size on two pages for content that isn't their unique job.

### 6. `conditional-visibility.mdx` duplicates its own decision guidance and an entire scenario from `device-detection.mdx`
**Fix:** merge "When You Need This" and "When NOT to Use This" (near word-for-word restatements ~200 lines apart) into one section; cut the "Advanced: Device-Specific Destinations" iOS-Safari-Mitti workaround entirely (identical scenario, identical two conditions, already lives in full on `device-detection.mdx` §3) and replace with a pointer link.
**Effort: small.** Removes real duplication-drift risk (two independently-wordable copies of the same guidance) and shrinks a page that otherwise correctly earns its single-scope status.

### 7. `print-first-workflow.mdx`'s edge-case FAQ belongs on `key-concepts.mdx`, and `key-concepts.mdx`'s full workflow narrative is an unstrategic duplicate of `print-first-workflow.mdx`
**Fix, per the editorial phase's own proposed split:** move "What happens if someone scans a tag early" and "Getting the mistakes back" (reassignment / delete-releases-Link) from `print-first-workflow.mdx` into `key-concepts.mdx`'s existing `## Common Questions` FAQ block, next to its existing Q&A entries; trim `key-concepts.mdx`'s "Print-Before-Link Workflow" H2 down to a 2–3 sentence summary plus a link, since the full step-by-step already lives, better, on `print-first-workflow.mdx`.
**Effort: small-medium.** This is also a direct retrieval fix: three separate retrieval-grid questions about deletion/scan-before-connect behavior surfaced `Key Concepts` as the top hit and graded `partial` specifically because the page has the workflow but not the edge case.

### 8. The entire Industries tab (5 pages) and three Help pages have no link back to the page that defines Tub/Item/Link/Destination/Page
**Fix:** add a `## Related` section linking to `/help/key-concepts` on all five `industries/*.mdx` pages, and add the same link to `help/building-a-page.mdx`, `help/using-fields.mdx`, and `help/app-links.mdx` (all three already have Related sections that omit it).
**Effort: small.** Confirmed as "the single largest reading-order gap in the site" by the repo audit — the Industries tab is directly reachable from search/marketing without ever passing through Help, and every page there uses these terms as established proper nouns with zero definitional path out.

### 9. Two cheap, high-visibility `docs.json`/content gaps that each close a confirmed hallucinate-grade question
- Rename `docs.json`'s top-level `name` from `"QRtub documentation"` to `"QRtub"` — this single field is the root cause of the literal `"...documentation documentation"` string doubling in `/.well-known/mcp/server-card.json`, and the odd `"QRtub documentation"` self-reference in `/.well-known/agent-card.json`; also fixes the browser-tab title.
- Add one explicit sentence to `key-concepts.mdx`'s FAQ stating plainly that there is no per-scan analytics dashboard or scan-count tracking today (only print-batch deployment status is tracked) — directly closes 2–3 retrieval-grid `hallucinate` questions ("Does QRtub track who scans each code?", "Is there an analytics dashboard?") that currently retrieve a page with one dangling, unexplained "scan data" phrase and nothing else.
**Effort: trivial for both.**

### 10. No plan-quota numbers exist anywhere in the docs corpus — the single largest, most-repeated competitor gap
**Fix:** add a **Plan Limits & Quotas** page (see §2) using the real numbers already sitting in `../qrtub/src/lib/stripe-plans.ts` (Starter $5/mo: 100 active links, 1 editor; Professional $25/mo: 1,000 active links, 5 editors, 1 numbered-pattern slot; Scale $90/mo: 10,000 active links, 20 editors, 5 numbered-pattern slots, private-or-public scanning), plus the already-documented-in-source per-request ceilings (100 links/request for Random or Numbered-Auto, 1,000 numbers/request for Numbered-Range).
**Effort: medium-large** — the numbers exist in source and don't need to be invented, but this is a new page, not an edit, and the downgrade/cancellation behavior piece needs a targeted source-code check this audit did not complete (see §5). Ranked here rather than higher only because of that remaining research step; the payoff is the largest in the whole list — 5 of 7 competitors beat QRtub outright on this exact content, and it's the largest single thematic cluster in the retrieval grid's hallucinate bucket (billing/plan/seat questions account for roughly 9 of 23 hallucinate grades).

---

## 2. Proposed Information Architecture

### New pages

**Plan Limits & Quotas** (new page; new "Billing" group, or fold into "Getting Started")
Cover, with exact numbers, not ranges: active-link ceiling per plan tier, editor/seat count per tier, numbered-pattern slot count per tier, per-request generation ceilings (100 for Random/Numbered-Auto, 1,000 for Numbered-Range), and — once verified (§5) — what happens to Links over the new ceiling on downgrade. Model the numeric specificity on the Stripe/Linear/Flowcode competitor pattern documented in `01-external.md` §6: state the number, state the boundary behavior, never say "unlimited" unless it's genuinely uncapped in source.

**Glossary** (new page, top-level or in a new "Reference" group)
One page, terms pulled verbatim from `../qrtub/GLOSSARY.md` — Link, Item, Tub, Page, Destination, Direct Mode, Page Mode, Media, Media Batch (see naming note below), Claim-on-scan, Page Template, Custom Field — each with the glossary's own 1–2 sentence definition and a link to the fuller page. This is the direct fix for the "used-before-defined" pattern flagged repeatedly (index.mdx, creating-your-first-link.mdx, all 5 industry pages) and for zero of QRtub's 7 competitors, 5 have one.

**Troubleshooting index** (new page or small group)
Atomic Q&A entries, one per known failure mode, all already documented individually somewhere in this audit chain: single-brace URL-template bindings not working (recently fixed but a recurring user error), a CEL condition failing silently to `false` on a typo or undefined identifier (no error shown — flagged as "the single most likely support question" on `conditional-visibility.mdx`), an unconnected Link's scan behavior, the iOS Safari deep-link block workaround, an empty/missing URL-template field inserting a blank string, and the custom-fields CSV-validation-skipped-when-"Allow new values" bug (Finding 4 above). Model on Twilio's atomic per-error-code page or QR Tiger's troubleshooting index (`01-external.md` §6) — QRtub currently has zero dedicated troubleshooting content (2 incidental grep hits, both inside integration pages).

**Scan-analytics statement**
Not necessarily a standalone page — a clearly-headed FAQ entry or short section (on `key-concepts.mdx` or a new "Reference" page) stating exactly what is and is not tracked: print-batch deployment status (Draft→Printing→Printed→Deployed, per-code) is tracked; no per-scan count, no scan-timestamp log, no analytics dashboard exists today. State explicitly whether this is Planned per `../qrtub/BRAND.md` or simply not offered — do not leave it ambiguous, since the current single dangling "scan data" phrase is exactly what invites an AI agent to guess.

**Team management** (new page)
Invites (send/resend/revoke), accept/reject via notification bell, roles, bulk-remove, the leave-team flow that forces an ownership transfer if the leaver is the owner. Currently a one-line bullet in a plan feature list is the only mention anywhere.

**Billing & subscription management (in-app)** (new page)
The Stripe customer portal, per-team billing summary for multi-team users, checkout flow, changing plan/interval, viewing past invoices. Today every doc page's only touchpoint is the identical "See plans and pricing →" CTA; nothing explains the in-app billing surface at all.

**Bulk data management** (new page, or a new section on an existing Items & Data page)
Whole-Tub backup/restore (export a Tub as JSON, create a new Tub from a backup), general Item CSV import/export (distinct from the print-batch export already documented), and access-link CSV import with dry-run plus bulk assign/unassign/delete. Explicitly distinguish all three from the print-batch CSV flow already documented on `help/print-batches.mdx`, since the coverage-gap audit found readers currently have no way to tell these apart.

**Smaller additions (not full pages):**
- Name and describe the in-app camera QR scanner and the **Claim-on-scan** mechanic (currently described behaviorally on `print-first-workflow.mdx` but never named — a sitewide grep for "claim" returns zero hits) directly on `print-first-workflow.mdx`.
- One paragraph on the global "Search Everything" top-nav search, on `key-concepts.mdx`.
- One paragraph on the Tub-creation template library ("Choose a template"), on `key-concepts.mdx` or `help/custom-fields.mdx`.
- Account/profile settings (password change, avatar) — lowest priority of the coverage gaps; a short stub is enough.

### Pages to merge / trim (not full page merges — see Finding 5–7 for the specifics)
- Cut the fabricated Media claims and the duplicated three-entity-model recap from `key-concepts.mdx`; keep `media-basics.mdx` as the single accurate source for planned-vs-shipped Media facts.
- Cut the duplicated Device Fields table from `conditional-visibility.mdx` and `device-detection.mdx`; `using-fields.mdx` stays canonical.
- Cut the duplicated iOS-Safari device-routing scenario from `conditional-visibility.mdx`; `device-detection.mdx` stays canonical.
- Move print-first-workflow's edge-case FAQ into key-concepts's FAQ block; trim key-concepts's full workflow narrative to a summary + link.
- `using-fields.mdx`'s "Common Patterns"/example gallery substantially overlaps `conditional-visibility.mdx`'s "Common Use Cases" — keep the decision-oriented cookbook on `conditional-visibility.mdx` only; keep `using-fields.mdx` as pure field reference.

### Pages to split
- None of the 20 pages need a hard split by the editorial phase's own verdicts — every "ONE QUESTION PER PAGE" section that considered splitting concluded either "no split needed" (index, creating-your-first-link, building-a-page, print-batches, safetyculture) or "trim the duplication instead of splitting" (key-concepts, print-first-workflow, media-basics, using-fields, conditional-visibility, device-detection). Treat the merge/trim list above as this audit's actual split-equivalent recommendation — the real fix in every flagged case was de-duplication across existing pages, not further fragmentation.

### Nav / grouping changes
- Rename the `docs.json` "Industries" **group** nested inside the "Industries" **tab** (currently produces "Industries > Industries > Civil Construction" in the breadcrumb) — one-line rename, e.g. "By Industry."
- Give `index.mdx` its own group, or reconsider whether marketing homepage copy belongs ungrouped inside the "Help" tab next to 12 procedural pages at all.
- Consider whether "Industries" (a marketing-verticals tab) should sit as a full peer of "Help" (support docs) at the same tab level — a retrieval agent gets no content-type signal that these are different kinds of pages, which matters more than usual on a corpus already prone to confident hallucination on gaps.
- `help/print-first-workflow.mdx` (Concepts group) and `help/print-batches.mdx` (its own single-page Printing group) are one printing lifecycle split across two nav groups — worth a second look once the merge/trim above is done.
- Resolve the **Print Batch vs. Media Batch** naming conflict as an explicit decision, not a silent docs fix: `GLOSSARY.md` says the canonical term is "Media Batch," but the app's own DB table, service name, and UI all say "Print Batch(es)" — the docs currently match the shipped app, not the glossary. Either update `GLOSSARY.md` to accept "Print Batch" as the aligned term (the app is unlikely to be renamed), or commit to renaming the nav group, page title, and every heading on `help/print-batches.mdx` plus the cross-link on `help/print-first-workflow.mdx:110` and the internal inconsistency on `help/media-basics.mdx` (uses both names within one file). This needs a product decision before either fix, so it is listed here rather than in the Top Ten.

---

## 3. `docs.json` Description — Three Drafted Options

### Option A — Capability-first
> "QRtub turns a printed QR code into a Link you control — print once, then decide (and change) what it opens, whether that's a single URL or a Page with multiple Destinations for different systems and audiences."

Emphasizes the core "print once, update forever" value proposition — the thing that differentiates QRtub from a plain static QR generator — without naming a specific vertical. Optimizes for a general AI assistant asked "what is QRtub" or "how is QRtub different from [competitor]," since it leads with the one claim that's both true (verified against source) and distinctive. Risk: says nothing about who it's for or that this is a help-docs site rather than the marketing site, so an AI summarizing `llms.txt` cold might undersell that this is where the *procedures* live.

### Option B — Industry/vertical-first
> "QRtub is QR-code and Link management for physical assets — equipment, facilities, and field infrastructure — with help guides covering Links, Pages, Tubs, conditional routing, and printing workflows."

Emphasizes the physical-asset/industrial-deployment angle that the Industries tab and `BRAND.md` positioning lean on, and names the actual entity vocabulary (Link/Page/Tub) an AI would need to map a user's question onto this site's terms. Optimizes for B2B/industry-buyer queries and differentiates from generic "make a QR code" tools. Risk: narrower framing risks undersell­ing the general-purpose Page/Destination-routing use cases that aren't industrial (e.g. the "professional item presence" use case), and it's a slightly more editorial claim than Option C since "physical assets" is a framing choice, not a verbatim fact from the docs.

### Option C — Docs-scope, minimal, evergreen
> "Help documentation for QRtub: how to create Links, build Pages, organize Items in Tubs, and manage QR code deployments from printing through installation."

Purely descriptive of what the corpus actually covers, matching Mintlify's own GEO guidance ("write descriptive page titles/descriptions framed as 'what does this page help users do'"). Safest and most evergreen option — it asserts nothing that could go stale as pricing or positioning changes, since it only names entities and actions this repo already documents in full. Optimizes for accurate self-description to any AI reading `llms.txt` cold, with the least risk of the description itself becoming a future accuracy finding. Risk: reads dry and undersells QRtub's actual differentiation — a model asked to compare QR platforms gets less persuasive material to work with than Option A or B would give it.

**Recommendation if one must be picked:** Option A — it is the only one of the three that states a genuine product differentiator rather than either a vertical framing (B, narrower than the product) or a pure table-of-contents (C, no differentiation at all) — while still being fully verified against source and evergreen (no numbers, no dated claims).

---

## 4. Looks Wrong But Is Fine

- **`footer.branding.enabled: false` isn't a documented Mintlify field.** A future reviewer will likely flag this as dead config to delete. Leave it until someone does a live visual check of the rendered footer — if it's actually suppressing a "Powered by Mintlify" mark today, deleting it re-enables that mark; this is a "verify before touching," not a "delete on sight."
- **`integrations/safetyculture.mdx`'s file path/slug still says "safetyculture" even though the product is now branded "Mitti."** Looks like unfinished rename work. It's fine to leave: three external Mitti/`help.mitti.com` backlinks and any existing bookmarks point at the current URL; changing the slug without a `docs.json` redirect would break them for no reader-facing benefit — the page's title and content already say "Mitti" correctly.
- **Nine `.mdx` files still carry inert `sidebar_position`/`category`/`slug`/`industry`/`featured`/`platform` custom keys** (per `frontmatter-inventory.csv`). Looks like leftover cruft demanding cleanup. It's genuinely inert — confirmed against Mintlify's published frontmatter schema — so it's pure cosmetic tidying with zero behavior change; fine to defer indefinitely.
- **All 19 individually-fetched pages carry an identical ~82-token CTA+footer boilerplate.** Looks like wasteful duplication an efficiency-minded reviewer would want stripped via a Mintlify config override. The external audit's own arithmetic says stripping it saves only ~2.1% of `llms-full.txt`'s token cost — not worth the engineering effort for that return.
- **`robots.txt` allows every AI crawler by default (`Content-Signal: ai-train=yes, search=yes, ai-input=yes`).** Looks like an oversight a security-minded reviewer might want locked down. It's Mintlify's unconfigured default, but it's also exactly what Mintlify's own GEO guide recommends ("Allow AI agents in robots.txt") for a help-docs site whose whole purpose is being read by AI assistants — leave it unless a separate business decision is made to withhold docs from AI training (contrast: Twilio and Uniqode both made that call deliberately, QRtub has not, and that's a legitimate choice either way, not a bug).
- **Both `/.well-known/mcp/server-card.json`'s "documentation documentation" doubling and `/.well-known/agent-card.json`'s "QRtub documentation" self-reference look like two separate bugs.** They're the same root cause (`docs.json.name`) — fixing Finding 9 above resolves both in one edit; don't file or fix them separately.
- **Most of the corpus was last touched only 2 days before this audit.** A staleness scan run later might flag files last touched 8–9 days prior as comparatively "old" and worth a fresh look. Per the git-history phase, the entire 9-day spread reflects an in-flight migration (Mitti rebrand + binding-syntax fix), not neglect — this narrow band is not a meaningful staleness signal on its own.

---

## 5. Could Not Verify

- **What happens to Links over a plan's ceiling on downgrade, or to data after a cancelled/lapsed subscription.** The retrieval grid confirms this is asked and hallucinated on (2 separate questions); the coverage-gap and editorial audits located the plan-tier *ceiling* numbers in `../qrtub/src/lib/stripe-plans.ts` but did not trace the actual downgrade/grace-period code path (Stripe webhook handlers or equivalent). Needed before the Plan Limits & Quotas page (§2) can state this safely — flag as an open question rather than guess.
- **Whether `footer.branding.enabled: false` actually suppresses Mintlify's "Powered by Mintlify" mark.** Not a documented field; would need a live look at the rendered footer HTML on help.qrtub.com, which this audit chain did not do.
- **Data residency / hosting region (Australia vs. overseas).** Not addressed anywhere in the docs, `CLAUDE.md`, or any file in this audit chain — and it's outside even the `qrtub` app repo's scope (infrastructure/hosting config, not application source). Would need to be answered directly by the team, not derived from any repo.
- **Whether a "Powered by QRtub" badge exists on public Pages, and whether it's removable.** Confirmed absent from the docs (hallucinate-grade in the retrieval grid) but the coverage-gap sweep did not check the Page-rendering component source specifically for this — status genuinely unknown, not just undocumented.
- **Whether Pages can be password-protected.** Same situation — confirmed undocumented, not confirmed true or false against source.
- **Currency of the competitor figures cited in `01-external.md` §6** (Flowcode's dated scan-cap FAQ, Uniqode's tier ladder, etc.) — these were live-fetched on 2026-08-19 and are explicitly dated by their own sources; this summary cannot attest they're still current at whatever later date this is read.
- **The `/.well-known/api-catalog` `Link:` header vs. 404 contradiction.** Confirmed as a real, universal discrepancy (every response advertises the endpoint, direct fetch 404s), but Mintlify's own published docs don't mention this endpoint at all — resolving whether this is a Mintlify platform bug or something QRtub-specific requires asking Mintlify directly, not further investigation of this repo.
- **A count discrepancy inside `02-repo.md` itself:** its terminology section opens with "30 violations logged" but its closing line says "Full row-by-row detail (34 rows...)." This summary could not re-derive which number is authoritative without re-reading every underlying per-page terminology file individually, so it's flagged here rather than silently resolved one way.
- **Whether the Mintlify "Related pages" add-on (needed only if a future editor wants to switch to the frontmatter `related: []` mechanism) is provisioned on QRtub's plan.** Not checked — irrelevant today since every page uses the manual "## Related" Markdown pattern, which needs no add-on, but would matter if that approach ever changes.
