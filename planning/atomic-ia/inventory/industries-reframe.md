# Atomic IA Inventory — Industries Tab Naming/Reframing

Domain: the **Industries** tab (`help.qrtub.com`, `docs.json` tab `"Industries"`), currently
one group ("Industries") holding five vertical landing pages: `civil-construction`,
`contract-cleaning`, `arboriculture-tree-management`, `electrical-test-and-tag`,
`local-government-councils`.

This is not a content-atomization pass like the other inventory files in this directory —
the deliverable the product owner asked for is a naming/reframing decision for the tab
itself, plus a call on whether that decision should ship together with or separately from
the accuracy fixes already found on these pages. The table below still uses the shared
inventory schema so it composes with the other domain files, but most rows are structural
(naming options, tab/group shape) rather than page content.

Sources checked:
- `/workspace/qrtub/BRAND.md` — Part 1 (product definition, "industry-agnostic" claim),
  Part 4 (Target Audiences — both are horizontal personas, *not* named verticals), Part 6
  ("Early Industries": Marine, Construction, Equipment hire, Lifesaving)
- `/workspace/qrtub/GLOSSARY.md` — canonical terms (Link/Destination/Item/Page/Media); no
  "industry" concept anywhere in it
- `/workspace/mintlify-docs/industries/*.mdx` — all five pages read in full or by section
- `/workspace/mintlify-docs/docs.json` — nav structure (tab → group → pages)
- `/workspace/mintlify-docs/CLAUDE.md` — site structure, "Industry pages" writing pattern,
  explicit warning that "Industry pages have historically been the worst offenders for
  overclaiming"
- `grep -ri "industry|vertical" /workspace/qrtub/src` — **no hits that are the concept
  "industry."** (The two file-name hits, `StructurePanel.tsx` etc., matched CSS `vertical`
  properties, not a product notion of industry/vertical.) Confirms "industry" is a purely
  editorial/docs-side grouping with zero representation in the product's data model —
  renaming or restructuring this tab has no code, schema, or feature-flag dependency.
- `/workspace/mintlify-docs/planning/atomic-ia/inventory/integrations.md` — the sibling
  inventory that already confirmed QRtub has no native third-party API integrations; every
  "integration" claim on the Industries pages is the same underlying mechanism (a URL,
  optionally templated with `{{item.field}}`, optionally a deep link with a fallback)

## What the pages actually are today

All five pages follow one template (confirmed by heading-level grep): "The Challenge" →
a "One Code, [X] Effect" section → an "Integration with [X] Software" section → "Getting
Started" → "Why [X] Choose QRtub" → "Use Cases" → "Ready to Deploy?" CTA. Word counts run
1,006–1,630 words each — 1–3× Linear's observed 200–1,000-word band for a single-concept
page, but these aren't single-concept pages; they're single-vertical landing pages carrying
6–8 sub-topics each, which is a different genre than the atomic help pages elsewhere in this
redesign. That's expected for a marketing/positioning surface and is not itself a defect.

The register, however, is a defect relative to `BRAND.md` §2 (voice) and is the real reason
a rename was floated. Confirmed phrases actually in these files: "The Restaurant Kitchen
Effect," "guerrilla marketing," "**QRtub Pages weaponize it**," "Here's the genius," "The
Urban Forest Dashboard Effect," "The Professional Council Effect." BRAND.md's own tone
calibration example labels almost exactly this register — enthusiastic, metaphor-heavy,
oversold — as the "too casual/too corporate" failure case to avoid, and lists "Quietly
confident" / "Doesn't need to shout" as a personality trait. None of the five pages match
that voice.

Also confirmed independently (consistent with the "4 of 5 pages" figure from the prior
audit): fabricated or overreaching capability claims beyond the known Mitti-naming and
integration-mechanism issues already tracked in `integrations.md`:
- `electrical-test-and-tag.mdx`: "Register updates automatically" and "premium reporting,
  analytics, alerts" as an "upsell opportunity" — QRtub has no register that updates from
  scan/test data (nothing writes back into QRtub, per `CLAUDE.md`) and no analytics/alerts
  feature (BRAND.md 1.6 lists analytics dashboards as a false claim; alerts don't exist at
  all, planned or otherwise).
- `arboriculture-tree-management.mdx`: "Reports auto-route to correct council team with tree
  ID and location pre-filled" — implies workflow routing logic that doesn't exist; the
  accurate claim is that a URL Template pre-fills a form, not that QRtub routes anything.
- `contract-cleaning.mdx` and others: "Integration with Cleaning/Council/Construction
  Software" sections list named third-party products (Swept, CleanTelligent, Aspire, Jobber,
  UpKeep, Fiix) with no hedge that these are just URL/deep-link targets, not integrations —
  same category of issue `integrations.md` already flagged for Mitti, just unaddressed here.
- Every "Real-World Example" / "Real-World [Industry] Use Cases" heading introduces a
  generic, unattributed hypothetical ("A civil contractor manages 150+ pieces...") under a
  label that reads as a documented case study. Not technically an invented customer quote
  (BRAND.md 3.2's specific prohibition), but the heading implies real-world sourcing the
  content doesn't have.

## Inventory

| Concept | Definition | Proposed nav category | Proposed page/title | Proposed slug | Atomic or merge |
|---|---|---|---|---|---|
| Tab name: "Industries" (status quo) | Current tab label; frames the section as "pick your vertical" sales-funnel navigation | — (baseline, not proposed) | — | — | N/A — this is the option being reconsidered, not a page |
| Tab name option: "Applications" | Rename to a broader, less-vertical term implying "where this fits," not "which industry you're in" | Top-level tab | — (tab label only) | — | N/A — structural naming decision, see tradeoffs below |
| Tab name option: "Use Cases" | Rename reusing QRtub's own existing internal vocabulary (BRAND/CLAUDE's UC-001…UC-008 "use cases library") | Top-level tab | — (tab label only) | — | N/A — structural naming decision; **recommended**, see below |
| Tab name option: "Industry Guides" | Keep the vertical framing but recast register from landing-page pitch to reference-guide, matching the "Integrations" tab's guide tone | Top-level tab | — (tab label only) | — | N/A — structural naming decision, see tradeoffs below |
| Group shape: "By Industry" under "Use Cases" | If the tab becomes "Use Cases," nest the five vertical pages under a "By Industry" group rather than flattening them, leaving room for a future horizontal "By Workflow" group (UC-001…UC-008 style) without a name collision | Use Cases > By Industry | (group, not a page) | — | N/A — nav structure recommendation |
| Civil Construction page | Equipment fleets, one code per machine, Mitti/CMMS/manual destinations, ownership-mix scenarios | Use Cases > By Industry | Construction Equipment | `use-cases/construction` | Standalone — already carries genuinely distinct scenarios (fleet, multi-site, mixed ownership); needs a tone/accuracy edit, not a split |
| Contract Cleaning page | Facility QR codes serving staff, clients and public simultaneously; heaviest "salesy" offender in the set | Use Cases > By Industry | Contract Cleaning | `use-cases/contract-cleaning` | Standalone content-wise, but requires the heaviest rewrite — strip "weaponize"/"guerrilla marketing"/"genius" language entirely, this isn't a trim, it's a re-draft of most sections |
| Arboriculture & Tree Management page | Tree population inspection/works-history codes, public education angle, storm-response reporting | Use Cases > By Industry | Arboriculture & Tree Management | `use-cases/arboriculture` | Standalone — fix the fabricated "auto-route" claim and "Dashboard Effect" heading, otherwise structurally fine |
| Electrical Test and Tag page | Two audiences (in-house vs. contract provider) sharing one compliance-register pattern | Use Cases > By Industry | Electrical Test and Tag | `use-cases/electrical-test-and-tag` | Standalone — has the worst fabrications (auto-updating register, analytics/alerts upsell); needs the most factual correction of the five |
| Local Councils page | Multi-department council assets, contractor coordination, public issue reporting | Use Cases > By Industry | Local Government & Councils | `use-cases/local-councils` | Standalone — same "Real-World Example" framing risk and unhedged software-name list as the others |
| `industry:` frontmatter field | Present on all five pages (`industry: "Construction"`, etc.) but read by nothing — Mintlify navigation comes from `docs.json`, and `grep` across `src/` finds no product concept of "industry" at all | (cross-cutting, all 5 pages) | — | — | Merge/remove — dead metadata per `CLAUDE.md`'s note that `sidebar_position`/`category`/`slug` frontmatter are inert leftovers; `industry` belongs on that list too and should either be deleted or actually wired to something (e.g., a future filter UI) before being kept |
| "Real-World Example" heading pattern | A generic, unattributed hypothetical scenario presented under a heading that implies a documented case study | (cross-cutting, all 5 pages) | — | — | Merge — a global find/replace to something like "Example scenario" or "Typical setup," not a separate page |
| BRAND.md industry list vs. docs industry list mismatch | BRAND.md Part 6 "Early Industries" names Marine, Construction, Equipment hire, Lifesaving; the docs tab names Civil Construction, Contract Cleaning, Arboriculture, Electrical Test & Tag, Local Councils — only "Construction" overlaps | (cross-cutting — flags a content-strategy gap, not a page) | — | — | N/A — not a page; flagged in Open Questions for the drafting phase |
| Rename vs. accuracy-fix sequencing | Whether the tab rename/reframe ships as its own change or bundled with the fabrication fixes already found on 4 of 5 pages | (process decision, not a page) | — | — | N/A — see recommendation below: **do together**, not sequentially |

## Naming alternatives: tradeoffs

**"Applications"**
- Optimizes for: a neutral, non-sales-funnel word; doesn't ask the reader to self-select a
  vertical before they've decided whether to trust the product.
- Risks: collides in meaning with the adjacent **Integrations** tab, which is literally about
  connecting to other applications (Mitti, CMMS platforms). A reader scanning the top nav
  could reasonably expect "Applications" to be about connecting apps, not about verticals.
  It's also a flat, bureaucratic word in plain English (a permit application, a job
  application) that doesn't evoke "which kind of team is this for" the way "Industries" does.

**"Use Cases"** — recommended
- Optimizes for: reuses vocabulary QRtub already has internally (BRAND.md/CLAUDE.md's
  UC-001…UC-008 "use cases library"), so this isn't a new term being introduced, it's an
  existing internal frame becoming public-facing. It also matches the actual shape of the
  content better than "Industries" does — every page already organizes itself around
  "Two Use Cases, Same Solution," "Real-World Example," multi-audience scenarios — the
  pages are already use-case narratives wearing an industry-vertical label. Reframing the
  tab name to match the content's real shape, rather than the other way around, is less
  rewriting than any other option, and it slightly softens the sales-funnel feel of
  "pick your industry" without losing vertical discoverability if paired with a "By
  Industry" group underneath (see the nav-shape row above).
- Risks: ambiguity against the *other* meaning of "use case" already in the brand vocabulary
  — the horizontal, cross-industry mechanisms (multi-system integration, audience routing,
  vendor-lock-in prevention) that BRAND/CLAUDE already number UC-001 through UC-008. If those
  eight ever get their own docs pages, "Use Cases" will have two competing referents unless
  the nav explicitly splits "By Industry" from something like "By Workflow." Mitigated by
  the group-shape recommendation above, but only if that split is made explicit from day
  one, not backfilled once collision occurs.

**"Industry Guides"** — something else entirely
- Optimizes for: keeps the vertical discoverability intact (still says "industry," still one
  page per named vertical, zero navigation regression for anyone with the current tab
  bookmarked) while recasting the register — "guide" signals documentation, not a landing
  page pitch, matching how the sibling "Integrations" tab already reads (precise, honest
  about mechanism, per `CLAUDE.md`'s own page-type table).
- Risks: the weakest option against the deeper positioning tension — BRAND.md states
  outright that "QRtub is industry-agnostic," yet grouping content by five named verticals
  at all (regardless of what the group is called) implies the opposite to a reader skimming
  the top nav. "Industry Guides" doesn't fix that; it just makes the pages read less like a
  pitch once you're inside one. It's the safest, lowest-effort option and the least
  effective at solving the actual problem the product owner raised.

## Recommendation

Rename the tab to **"Use Cases,"** with the five current pages nested under a **"By
Industry"** group (leaving room for a future "By Workflow" group so the two senses of "use
case" never collide in the nav). This matches the content's actual shape better than any
alternative, costs nothing in navigation continuity if slugs redirect, and reuses a term the
brand docs already treat as canonical rather than inventing a new one.

**Do the rename together with the accuracy fixes, not before or after them.** The rename's
entire purpose is to move these five pages from a sales-pitch register to something closer
to the site's documentation voice — but the specific sentences that need factual correction
(the fabricated auto-updating register, the phantom analytics/alerts upsell, the unhedged
"auto-route" claim, the unhedged software-integration lists) are the same sentences carrying
the worst of the "weaponize"/"guerrilla marketing" tone. Fixing tone and fixing fact are the
same edit on the same lines in the same five files. Doing the rename first would ship "Use
Cases" pages that still read as oversold sales copy and still contain false capability
claims — defeating the reframe's purpose. Doing the accuracy fixes first and the rename
later means touching all five files twice. One combined per-page pass (rename the tab in
`docs.json`, retitle, de-fabricate, de-tone) is a single small change per page, five total,
matching how the rest of this redesign is being scoped.
