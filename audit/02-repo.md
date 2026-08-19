---
title: "Repo audit — docs.json, frontmatter, links, terminology, spelling, staleness, coverage"
description: "Synthesis of the docs.json configuration audit, frontmatter inventory, link integrity, terminology violations, spelling normalisation, git-history staleness, and code-vs-docs coverage gaps for the mintlify-docs repo."
---

# Repo Audit — mintlify-docs

Compiled from `audit/_raw/mintlify-guidance.md` (verified against live Mintlify docs, fetched 2026-08-19), `audit/_raw/repo/docsjson.md`, `audit/_raw/repo/links.md`, `audit/_raw/repo/spelling.md`, `audit/_raw/repo/coverage-gaps.md`, `audit/_raw/git-history.md`, `audit/frontmatter-inventory.csv`, and `audit/terminology-violations.csv`. Every concrete finding from those files — every broken link, every terminology violation, every spelling fix — is preserved below; nothing is summarised away. Audited 2026-08-19.

---

## 1. docs.json configuration audit

The file has 11 top-level keys (`$schema`, `theme`, `name`, `redirects`, `colors`, `favicon`, `navigation`, `logo`, `navbar`, `contextual`, `footer`) across 131 lines. Ranked by impact:

1. **Top-level `description` is missing entirely (highest priority).** Per Mintlify's own docs (verified, `/docs/ai/llmstxt`), this field is the source of the blockquote line under the H1 in the auto-generated `/llms.txt`. Its absence means `/llms.txt` currently ships as a bare title with no description line — any AI tool reading `/llms.txt` to judge relevance gets "QRtub documentation" and nothing else. **Fix:** add a one-sentence, evergreen description of what QRtub is and what the docs cover — grounded in what's actually documented, not aspirational features the coverage-gap audit (§7) shows are missing from the docs.

2. **`seo.organization` is unset.** Without it, Mintlify's JSON-LD `Organization` publisher entity is derived from site name/logo/URL alone, rather than naming the actual legal entity (Teralis Pty Ltd, per this repo's `CLAUDE.md`) with `sameAs` links to corroborate it.

3. **`footer.branding.enabled: false` is set but is not a documented Mintlify field.** A grep of both schema-reference pages (`settings-reference`, `settings-structure`) for "branding" found no `footer.branding` sub-field — only `footer.socials` and `footer.links` are documented. This may be a legacy/renamed no-op, or a real field the public schema page hasn't caught up to. **Needs a live check** (inspect the rendered footer HTML on help.qrtub.com for a "Powered by Mintlify" mark) — do not assume the setting is doing what its name implies.

4. **Redundant "Industries" tab/group naming.** `docs.json` nests a group literally named `"Industries"` inside the `"Industries"` tab, producing a duplicated breadcrumb segment ("Industries > Industries > Civil Construction") in both the on-page breadcrumb UI and the JSON-LD `BreadcrumbList`. Compare **Integrations**, which correctly uses a distinct group name (`"Operations"`). One-line fix: rename the inner group (e.g. `"By Industry"`).

5. **The lone `"index"` page has no group and is mis-scoped under the "Help" tab.** `index.mdx` is marketing homepage copy ("Print Once. Update Forever."), not a how-to article, but sits as the sole ungrouped entry inside **Help** next to 12 genuine procedural pages — producing both a content-type signal problem (a retrieval agent sees "Help > Welcome to QRtub" with no group label marking it as different) and a shallower/inconsistent `BreadcrumbList` depth than its siblings. Recommend giving it its own group, or reconsidering whether it belongs in the Help tab's navigation at all.

6. **"Industries" as a full peer tab of marketing pages next to "Help" and "Integrations".** Not a schema violation, but a structural smell: users/agents have no navigational signal that Help (support docs) and Industries (marketing verticals) are different content types at the same tab level — which matters more than usual here because the docs corpus is already prone to confident hallucination on gaps (per the coverage audit), so miscategorising a thin marketing page as authoritative product documentation is a real risk.

7. **`contextual.options` is missing three low-cost, documented, prerequisite-free identifiers.** Current set: `copy`, `view`, `chatgpt`, `claude`, `perplexity`, `mcp`, `cursor`, `vscode` — all verified real and current. Missing and recommended: **`grok`** and **`aistudio`** (same free general-AI-assistant category as the three already enabled — omitting just Grok looks like an oversight rather than a deliberate choice), and **`add-mcp`** (complements the already-enabled `mcp`/`cursor`/`vscode` MCP-installer set). Not recommended without further checking: `assistant` (requires the separate Mintlify AI Assistant add-on to be provisioned), `download-pdf` (Enterprise-gated), `devin`/`devin-desktop`/`devin-mcp` (low-value for QRtub's non-developer audience).

8. **`metadata.timestamp` is unset (defaults `false`).** Mintlify sources this from git commit history automatically (verified, `/docs/organize/pages`) — zero-maintenance in a git-backed repo, and a useful trust/freshness signal on a corpus with currency-sensitive gaps (billing, plans, permissions — see §7). Recommend enabling.

9. **`markdown.instructions` is unset.** This field (verified) appends a custom "Agent Instructions" block to every page's Markdown export and to `llms.txt`/`llms-full.txt`. Zero-infrastructure way to steer AI consumption of the docs — e.g. an instruction not to describe Planned features as available, mirroring the `CLAUDE.md` rule already in force for human writers.

10. **Confirmed non-issues / already-correct:** nav-vs-repo completeness is clean (all 19 `.mdx` files under `help/`, `industries/`, `integrations/` are in `navigation`, every nav entry resolves, no orphans); every page has both `title` and `description` frontmatter; `seo.indexing` is unset but harmless today since there are no un-navigated pages it would exclude.

11. **Minor/non-blocking:** no `navigation.tabs[].icon` set on any tab (cosmetic); `help/print-first-workflow` (Concepts group) and `help/print-batches` (its own single-page Printing group) are printing-lifecycle pages split across two nav groups — may be intentional but worth a second look; **Items & Data** and **Printing** are each single-page groups, fine at current scale but worth revisiting if the nav grows; `footer.socials` and `footer.links` are both unset (low-effort, optional additions, not defects).

**On three commonly-assumed frontmatter fields:** Mintlify's docs confirm `sidebar_position`, `category`, and `slug` are **not** part of the schema — page order/hierarchy comes entirely from the `navigation` array in `docs.json`, and page URLs are just repo file paths. This matches and confirms this repo's own `CLAUDE.md` guidance calling them "inert leftovers." (Their actual presence in specific files is inventoried in §2 below.)

---

## 2. Frontmatter inventory summary

19 of 20 pages have complete, non-duplicated, appropriately-lengthed `title`/`description` frontmatter. No page is missing a description, and no description duplicates its title outright. The issues are narrower and more specific — worst offenders below.

**Worst offenders, quoted:**

1. **`index.mdx`** — description is `"Print Once. Update Forever."` Flagged `description_reads_badly_standalone: yes`. It's a marketing tagline, not a content summary, and as a standalone `llms.txt` line tells a reader or AI nothing about what the page actually covers (product overview? links into help/industries/integrations?). This is the homepage — the single highest-traffic `llms.txt` line on the site.

2. **`help/media-basics.mdx`** — description: `"How to think about the physical material your QR codes are printed on."` Flagged `description_reads_badly_standalone: yes`. Opens with vague framing rather than concrete content; Mintlify's own GEO guidance explicitly warns "vague descriptions don't get cited" by AI answer engines. Should also avoid implying per-item Media tracking that doesn't exist (per `CLAUDE.md` UC-008).

3. **`integrations/safetyculture.mdx`** — flagged `description_duplicates_title: yes`. Description reads: `"Open Mitti inspections straight from a scan, pre-filled with the item's data. Mitti was called SafetyCulture, and iAuditor before that."` The second sentence duplicates the title's parenthetical rename history (`"Mitti (formerly SafetyCulture / iAuditor)"`) — the first sentence alone carries all the real functional content.

4. **Nine files carry confirmed-inert legacy fields** — `sidebar_position`, `category`, and/or `slug`, none of which are part of Mintlify's schema (navigation is driven entirely by `docs.json`): `help/creating-your-first-link.mdx` (slug, sidebar_position, category), `help/key-concepts.mdx` (slug, sidebar_position, category), `help/media-basics.mdx` (slug, sidebar_position, category), `help/pages-overview.mdx` (sidebar_position), `help/using-fields.mdx` (sidebar_position), `help/conditional-visibility.mdx` (sidebar_position), `help/device-detection.mdx` (sidebar_position), `help/app-links.mdx` (sidebar_position), and all 5 `industries/*.mdx` files plus both `integrations/*.mdx` files (slug, sidebar_position — industry pages also carry non-schema `industry` and `featured` custom keys, integration pages carry `platform` and `featured`). These are dead weight, not defects — safe to delete per `CLAUDE.md`, but should not be copied into new pages.

5. **`help/using-fields.mdx`** — description mentions `"user session"` as a data source ("Reference Item data, Tub information, user session, and device details in URLs and conditions"). The documented binding namespaces (`CLAUDE.md`, confirmed against `../qrtub/src/lib/page/bindings.ts`) are item/tub/device/time only — flagged as a content-accuracy question needing verification against source, outside this frontmatter-only audit's scope.

6. **`integrations/cmms-systems.mdx`** — description: `"Connect QR codes to your maintenance system using deep links and URL Templates."` The word "Connect" sits close to language `CLAUDE.md` explicitly warns against ("QRtub does not have API integrations with third-party products... nothing writes data back") — flagged for a content-accuracy re-check.

7. **`industries/local-government-councils.mdx`** — minor naming inconsistency: title says "Local Councils" while the filename/slug say "local-government-councils."

8. **`help/conditional-visibility.mdx`** — description (`"Show or hide Destinations on Pages based on conditions"`) is borderline-close to a plain-language restatement of the title; kept as non-duplicate but flagged as worth a second look.

Full field-by-field data for all 20 pages is in `audit/frontmatter-inventory.csv`.

---

## 3. Link integrity summary

20 in-scope `.mdx` files (12 `help/`, 5 `industries/`, 2 `integrations/`, 1 `index.mdx`) were checked: 117 total links (68 internal page links, 2 internal JSX `href=`, 1 internal image, 25 external, 22 `mailto:`). External links were live-checked with `curl -sI` on 2026-08-19.

### Broken links (2 — both in `index.mdx`, both JSX Card `href`s)

| Source | Target | Why it's broken |
|---|---|---|
| `index.mdx:42` | `href="/industries"` | The "Industries" tab in `docs.json` has no top-level `pages` entry, only a `groups` array. Mintlify does not synthesize a page at the bare tab-root path, and there is no `industries/index.mdx` or redirect rule. 404s. |
| `index.mdx:49` | `href="/integrations"` | Same problem — "Integrations" tab also has only `groups`, no top-level `pages` entry, no index file, no redirect. 404s. |

**Fix:** point these Cards at a real first page in each tab (e.g. `/industries/civil-construction` and `/integrations/safetyculture`), or add an explicit landing page + `docs.json` entry for each tab.

### Anchor-only cross-page links (2 — fragments exist, but flagged as retrieval-opaque)

Both fragments resolve to real headings on their target pages, so neither is a *dead* link — but a fragment-qualified cross-page link is invisible to any retrieval/RAG system indexing by page rather than heading, which sees only "go read that whole page."

| Source | Link text | Target | Target heading confirmed |
|---|---|---|---|
| `help/creating-your-first-link.mdx:39` | "Direct Mode vs Page Mode" | `/help/key-concepts#link-modes` | Yes — `## Link Modes` at `help/key-concepts.mdx:100` |
| `help/using-fields.mdx:329` | "Conditional Visibility" | `/help/conditional-visibility#using-ai-to-generate-conditions` | Yes — `## Using AI to Generate Conditions` at `help/conditional-visibility.mdx:75` |

### External links (all resolve, no 4xx/5xx)

| URL | Status | Note |
|---|---|---|
| `https://qrtub.com/pricing` | 200 OK, no redirect | Linked 12× across the corpus |
| `https://help.mitti.com/en-US/000076/` | 308 → 308 → 200 | Double-redirect chain (drops trailing slash, then drops locale prefix); linking directly to `https://help.mitti.com/000076` would save two hops. Linked from `integrations/safetyculture.mdx:52,127,228` |
| `https://help.mitti.com/en-US/000149/` | 308 → 308 → 200 | Same redirect-chain behaviour. Linked from `integrations/safetyculture.mdx:227` |

### "See here for details" links carrying the only substance (not broken, but a content gap worth flagging alongside link integrity)

Strongest (zero inline substance — the linked page/site is the *only* place the fact lives): `help/app-links.mdx:105` (Mitti setup instructions promised, none given on-page); `integrations/safetyculture.mdx:127` (question item IDs — no instructions anywhere in this doc); `help/using-fields.mdx:329` (the entire "use AI to generate expressions" list item is the link, no technique given inline; also the anchor-only case above).

Medium (some inline content, but the specific thing promised is still deferred entirely): `help/custom-fields.mdx:106-107` (full field reference / page-binding steps); `help/conditional-visibility.mdx:166` ("complete... reference" explicitly deferred after a partial list); `integrations/safetyculture.mdx:52` ("detailed instructions" for finding an entity ID on Mitti's UI).

### Out-of-scope findings surfaced during the whole-repo grep

- `snippets/cta.mdx` and `snippets/snippet-intro.mdx` contain the same pricing/mailto links but are not imported or referenced by any in-scope page or `docs.json` — appear to be unused starter-kit leftovers, flagged for cleanup consideration.
- `help/print-first-workflow.mdx:17` is an image link (`![alt](/images/print-first-plates.jpg)`) — asset confirmed to exist on disk, OK.
- All 22 `mailto:hi@qrtub.com` links are identically spelled across every in-scope page; not classified BROKEN/OK since they aren't page routes, but checked for anomalies (none found).

---

## 4. Terminology violations summary

30 violations logged in `audit/terminology-violations.csv`, cross-checked against `../qrtub/GLOSSARY.md`. Grouped by what's most actionable first.

### Forbidden synonyms for canonical terms (highest priority — these actively contradict the glossary)

- **`help/key-concepts.mdx:186`** — *"Think of Tubs as: Folders, categories, or asset types—but with superpowers."* The glossary's "Instead of" list for **Tub** is exactly Folder / category / group — this sentence presents all three as equivalents on the page every other help page defers to for the Tub definition. This is a reader's first and most authoritative exposure to the term, undermining it directly.
- **"Simple redirect(s)" used 3× for Direct Mode**, the banned synonym per GLOSSARY: `help/pages-overview.mdx:22` (table cell), `help/key-concepts.mdx:108` (inside the `### Direct Mode` section itself — the term is defined and undermined in the same breath), `help/key-concepts.mdx:264` (FAQ section).

### Canonical-term mismatch: "Print Batch" vs. glossary's "Media Batch"

- **`help/print-batches.mdx`** — the entire page (title, H1, every heading, and the `docs.json` nav group name "Printing") calls the feature "Print Batch(es)." GLOSSARY.md's canonical term is **Media Batch** ("A production run of Media items produced together... Instead of: Print run, production run, order"); there is no "Print Batch" entry in the glossary at all. Verified against `../qrtub/src`: the app's own DB table is literally `access_link_print_batches`, so the docs accurately describe today's app UI — but GLOSSARY.md explicitly says not to treat current app copy as the reference. Same class of drift the glossary already calls out for "profile page," just not yet listed there.
- **`help/media-basics.mdx`** — internal inconsistency within one file: line 31 says "Media Batches" (correct), but the heading at line 35 and `## Print Batches` at line 108 rename the same feature "Print Batches" a few lines later.
- **`help/print-first-workflow.mdx:110`** — propagates the mismatch into a cross-link: `[Print Batches](/help/print-batches)`.

### Missing canonical term entirely

- **`help/print-first-workflow.mdx:48-58`** — fully describes the **Claim-on-scan** mechanic ("an unconnected code does not 404... gets an option to assign it there and then, from their phone") but never uses the term. A sitewide grep for "claim" found zero matches anywhere in `help/`, `industries/`, or `integrations/` — the concept is documented, the vocabulary is not.

### Casing-variant inconsistencies (term used correctly capitalized elsewhere in the corpus, lowercased here)

- **Destination**: lowercase every time (5 instances) in `industries/civil-construction.mdx` (lines 51, 84, 92, 171, 177) — the sole outlier file for this term in the whole corpus; also lowercase in `index.mdx:30` ("Pages with multiple destinations," inconsistent with "Link" capitalized in the line above), `help/key-concepts.mdx:42`, and `help/print-first-workflow.mdx:90,102`.
- **Item** (as "per item" vs. glossary-consistent "per Item"): lowercase in `help/app-links.mdx:84`, `industries/electrical-test-and-tag.mdx:130`, `help/media-basics.mdx:47` — contrast with capitalized "per Item" in `integrations/safetyculture.mdx:88,110`, `integrations/cmms-systems.mdx:54`, `help/conditional-visibility.mdx:210`.
- **Page Template** (canonical is both words capitalized): lowercase "template" in `help/key-concepts.mdx:190`, `industries/arboriculture-tree-management.mdx:167`, `industries/contract-cleaning.mdx:131`.

### Verbatim quoted UI labels (not writing errors, but unresolved product/glossary gaps)

- **`help/building-a-page.mdx:18`** — `"open the **Profile page** tab"` and **line 12** — `"labelled **Show a profile page** in the current app"`; same in **`help/pages-overview.mdx:44`**. GLOSSARY bans capitalized "Profile Page" as a product term, but these are quoting the actual current app UI (confirmed in GLOSSARY's own "Not Yet Aligned in the Product" table) — flagged because the reader sees the banned term rendered as a bold label with no caveat that it's a legacy name, not because the docs invented it.

### "Used before defined" — entities used as proper nouns with no definition or forward link

- **`index.mdx`** (whole page, nav position 1 of 20) — uses "Destination" and "Page" as capitalized proper nouns with zero inline explanation and no link to `help/key-concepts.mdx`; its only outbound links are to `/industries` and `/integrations` (which, per §3, are both broken).
- **`help/creating-your-first-link.mdx`** (nav position 2, the literal "Getting Started" entry point) — hands the reader Link, Item, Page, Destination, and Direct/Page Mode before the glossary page that explains any of them, though it does link forward to Key Concepts under "Next Steps."
- **`help/building-a-page.mdx`**, **`help/using-fields.mdx`**, **`help/app-links.mdx`** — each uses Tub/Item/Destination/Link heavily but its Related-links section omits Key Concepts, unlike `conditional-visibility.mdx`, `device-detection.mdx`, `print-batches.mdx`, and `safetyculture.mdx`, which all include it.
- **All 5 industry pages** (`civil-construction`, `contract-cleaning`, `arboriculture-tree-management`, `electrical-test-and-tag`, `local-government-councils`) — none has a Related/Resources section or any link to `help/key-concepts.mdx` or any other definitional page, despite using Tub/Item/Link/Destination/Page throughout as established proper nouns. Flagged as the single largest reading-order gap on the site — the entire Industries tab is reachable directly from search/marketing without ever passing through Help.

### External-site drift (not in mintlify-docs, flagged per the audit's cross-check instruction)

- **qrtub.com homepage** — hero copy uses banned casing `"QRTub"` (capital T); GLOSSARY explicitly bans this. The docs corpus itself has zero mid-sentence casing violations of the brand name (verified by full-corpus grep).
- **qrtub.com homepage** — uses capitalized `"Profile Page"` as an asserted product term (not a quoted UI label), which GLOSSARY forbids — worse than the docs' UI-label-quoting cases above.
- **qrtub.com/pricing** — lowercase "destination" and "landing page editor" instead of canonical "Destination" and "Page Editor"; "landing page editor" also echoes the internal code identifier `LandingPageEditor` that GLOSSARY flags as an internal name that should not leak into user-facing copy.

Full row-by-row detail (34 rows including file/line citations) is in `audit/terminology-violations.csv`.

---

## 5. Spelling normalisation list (full list)

38 US→AU/UK spelling fixes across `help/` (24), `industries/` (12), `integrations/` (1), and `index.mdx` (1). `program` and `judgment`/`judgement` excluded per task brief (both standard/acceptable in AU English); two already-correct AU spellings (`help/app-links.mdx:11` "recognises," `help/app-links.mdx:35` "cancelled") are not defects and are excluded from the count. Two occurrences of the literal field identifier `organizationName` were deliberately left unflagged — only the prose description text beside them is flagged.

| File | Line | Found (US) | Correction (AU/UK) |
|---|---|---|---|
| help/media-basics.mdx | 9 | recognizes | recognises |
| help/media-basics.mdx | 15 | recognizes | recognises |
| help/media-basics.mdx | 55 | organizations | organisations |
| help/media-basics.mdx | 67 | aluminum | aluminium |
| help/media-basics.mdx | 73 | Aluminum | Aluminium |
| help/building-a-page.mdx | 121 | Color | Colour |
| help/pages-overview.mdx | 19 | Behavior | Behaviour |
| help/pages-overview.mdx | 38 | Customize | Customise |
| help/pages-overview.mdx | 38 | colors | colours |
| help/device-detection.mdx | 81 | Optimized | Optimised |
| help/device-detection.mdx | 83 | optimized | optimised |
| help/device-detection.mdx | 91 | optimized | optimised |
| help/device-detection.mdx | 214 | Behavior | Behaviour |
| help/device-detection.mdx | 246 | Optimized | Optimised |
| help/key-concepts.mdx | 13 | recognizes | recognises |
| help/key-concepts.mdx | 60 | recognizes | recognises |
| help/key-concepts.mdx | 170 | personalization | personalisation |
| help/key-concepts.mdx | 184 | organizing | organising |
| help/key-concepts.mdx | 191 | Organized | Organised |
| help/key-concepts.mdx | 212 | finalized | finalised |
| help/key-concepts.mdx | 264 | organized | organised |
| help/conditional-visibility.mdx | 47 | specialized | specialised |
| help/using-fields.mdx | 85 | Organization | Organisation (description column only — leave `organizationName` field identifier unchanged) |
| help/using-fields.mdx | 131 | color | colour (description column only — leave `theme.accent` field identifier unchanged) |
| industries/local-government-councils.mdx | 90 | centers | centres |
| industries/local-government-councils.mdx | 203 | aluminum | aluminium |
| industries/local-government-councils.mdx | 234 | centers | centres |
| industries/civil-construction.mdx | 70 | Utilization | Utilisation |
| industries/civil-construction.mdx | 120 | Utilization | Utilisation |
| industries/civil-construction.mdx | 136 | personalized | personalised |
| industries/contract-cleaning.mdx | 122 | personalized | personalised |
| industries/contract-cleaning.mdx | 139 | weaponize | weaponise |
| industries/arboriculture-tree-management.mdx | 47 | neighborhood | neighbourhood |
| industries/arboriculture-tree-management.mdx | 155 | aluminum | aluminium |
| industries/arboriculture-tree-management.mdx | 183 | honor | honour |
| industries/arboriculture-tree-management.mdx | 185 | neighboring | neighbouring |
| integrations/cmms-systems.mdx | 10 | Computerized | Computerised |
| index.mdx | 15 | finalized | finalised |

**Zero-finding patterns checked and confirmed absent:** `favorite`, `license` (noun), `catalog`, `gray`, `defense`/`offense`, `traveled`/`traveling`, `canceled`/`canceling`, `fulfill`, `meter`/`metre`, `labor`, `flavor`, `humor`, `rumor`, `armor`, `vapor`, `odor`, `tumor`, `harbor`, `mold`, `plow`, `tire`/`tyre`, `curb`/`kerb`, `jewelry`, `enroll`/`enrol`, `skillful`/`willful`, `-yze`/`-yse` verbs. "Practice" (noun) and "check" (verb) were reviewed and are already correct for AU English; "coordination" is an accepted modern AU/UK spelling without a hyphen.

---

## 6. Git history / staleness

| Page | Last commit date | Last commit subject | Days since (as of 2026-08-19) |
|---|---|---|---:|
| help/app-links.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| help/building-a-page.mdx | 2026-08-10 | docs: add Building a Page, Custom Fields and Print Batches | 9 |
| help/conditional-visibility.mdx | 2026-08-17 | docs: SafetyCulture is now Mitti | 2 |
| help/creating-your-first-link.mdx | 2026-08-10 | docs: adopt "Page" terminology, retire the blog, and drop remaining false claims | 9 |
| help/custom-fields.mdx | 2026-08-10 | docs: add Building a Page, Custom Fields and Print Batches | 9 |
| help/device-detection.mdx | 2026-08-17 | docs: SafetyCulture is now Mitti | 2 |
| help/key-concepts.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| help/media-basics.mdx | 2026-08-10 | docs: add Building a Page, Custom Fields and Print Batches | 9 |
| help/pages-overview.mdx | 2026-08-17 | docs: SafetyCulture is now Mitti | 2 |
| help/print-batches.mdx | 2026-08-10 | docs: add Building a Page, Custom Fields and Print Batches | 9 |
| help/print-first-workflow.mdx | 2026-08-11 | docs: call the plates photo anodised aluminium, and correct the brief | 8 |
| help/using-fields.mdx | 2026-08-17 | docs: SafetyCulture is now Mitti | 2 |
| index.mdx | 2026-08-10 | docs: adopt "Page" terminology, retire the blog, and drop remaining false claims | 9 |
| industries/arboriculture-tree-management.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| industries/civil-construction.mdx | 2026-08-17 | docs: SafetyCulture is now Mitti | 2 |
| industries/contract-cleaning.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| industries/electrical-test-and-tag.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| industries/local-government-councils.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| integrations/cmms-systems.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |
| integrations/safetyculture.mdx | 2026-08-17 | Fix URL-template bindings: single braces never worked | 2 |

**Every page in the corpus was touched within the last 9 days**, and 13 of 20 within the last 2 days (the 17-18 Aug 2026 edit flurry: the binding-syntax fix and the SafetyCulture→Mitti rename). **This makes most classic "staleness" findings currently moot.** There is no page in this corpus that has gone stale by neglect — the entire site is either mid-migration (Mitti rebrand) or freshly corrected (binding syntax), which is a materially different risk profile from "nobody has looked at this in a year." The active risk right now is *incompleteness of an in-flight edit*, not staleness: the coverage-gap audit (§7) independently confirms no remaining single-brace binding examples were missed by the 17 Aug fix (`20e4168`), and the two remaining `item.{fieldName}` occurrences are prose about the naming convention, not URL examples the fix would have needed to touch.

**No page stands out as genuinely oldest-touched** — the full spread is 9 days (2026-08-10) to 2 days (2026-08-17/18), which is a narrow band by any normal documentation-staleness standard. If forced to name the "oldest" tier, it's the five files last touched 2026-08-10 (`help/building-a-page.mdx`, `help/creating-your-first-link.mdx`, `help/custom-fields.mdx`, `help/media-basics.mdx`, `help/print-batches.mdx`, plus `index.mdx`) — but "9 days old" is not a staleness finding worth acting on by itself. `help/print-first-workflow.mdx` (2026-08-11, 8 days) sits just inside that same original batch. The practical takeaway: staleness-by-git-date is not where this repo's real risk is; the terminology (§4), coverage-gap (§7), and content-accuracy flags raised elsewhere in this audit are the load-bearing findings, not last-touched dates.

---

## 7. Coverage gaps (full list)

Checked by reading every API route under `qrtub/src/app/api/`, every page under `qrtub/src/app/app/`, the "Key Features" section of `qrtub/CLAUDE.md`, `qrtub/src/lib/stripe-plans.ts`, and the settings screens, cross-referenced against the full text of all 20 docs pages. "Partially-documented" means the feature gets a passing mention a reader couldn't act on.

### Undocumented — no page or section covers this at all

1. **Team management** (invites, roles, removal, leave/transfer-ownership) — `src/app/app/team/page.tsx`, the full `team-users` API surface, `teams/[id]/transfer-ownership`. Only appearance anywhere in docs: a one-line bullet in Professional/Scale plan feature lists. A substantial multi-screen feature (send/resend/revoke invites, accept/reject via notification bell, bulk-remove, the guarded "leave team" flow that forces an ownership transfer first if the leaver is the owner) with zero help-page coverage.

2. **Billing & subscription management (in-app)** — the Stripe customer portal, per-team billing summary across multiple teams (`MyTeamsSubscriptionsSection`), checkout flow. Every doc page's only touchpoint is the boilerplate "See plans and pricing →" CTA to the external marketing site; nothing explains opening the customer portal, changing plan/interval, viewing a past invoice, or what "per-team" billing means for a multi-team user.

3. **Numbered-pattern reservation UI** (prefix/suffix/digits, auto/specific/range modes, live conflict checking, 1000-numbers-per-request cap) — partially-documented. `creating-your-first-link.mdx` mentions only "ID-based... Sequential or branded patterns" as one bullet; no page explains claiming a numbered range, conflict behaviour, the 1000-number cap, or the plan-tier limit on how many patterns a team may hold (1 on Professional, 5 on Scale — itself only stated on the external pricing page).

4. **Per-Tub auto-link generation mode** (random / item-ID-mask / none) and the Item-ID mask builder — the setting that decides whether creating an Item mints a link automatically and what its slug looks like. Distinct from, and prior to, the manual "Generate Links" flow already documented. No page mentions it exists.

5. **Whole-Tub backup/restore** (export a Tub as JSON, create a new Tub from a backup) — reachable from Tub Settings → Admin. Nothing in `help/` distinguishes this from the CSV item export/import below; a reader would not know it exists.

6. **General Item CSV import/export** (outside the print-batch flow) — partially-documented. `custom-fields.mdx` mentions in passing that validation applies "during CSV import," but no page walks through the actual workflow (column mapping, upsert behaviour, what a full-backup export contains vs. the print-list export already documented in `print-batches.mdx`).

7. **Access-link CSV import (with dry-run) and bulk assign/unassign/delete** — distinct from the print-batch CSV; this is importing/bulk-editing Links themselves (e.g. migrating an existing numbering scheme). No page mentions it.

8. **Global "Search Everything"** — team-scoped debounced search across Tubs, Items, Access Links, and profile pages from the top-nav search box. Not mentioned anywhere.

9. **In-app camera QR scanner** ("Scan" button, top navbar) — partially-documented. `print-first-workflow.mdx` narrates the *behaviour* ("gets an option to assign it there and then, from their phone" — this is the same Claim-on-scan mechanic flagged terminology-missing in §4) but never names the feature, says where to find it, or mentions the automatic reference-photo capture.

10. **Account/profile settings** (password change, avatar upload) — no help page covers account settings at all.

11. **Tub-creation template library** ("Choose a template") — a searchable gallery of pre-built field configurations offered at Tub-creation time. `key-concepts.mdx` uses "Heavy Equipment"/"Meeting Rooms"/"Fire Safety Equipment" purely as illustrative example Tubs — it never says these are actual one-click starter templates a user can pick.

### Not customer-facing (flagged for completeness, no doc action expected)

12. **Waitlist system** — a signup-gating mechanism for when public registration is off (`NEXT_PUBLIC_ENABLE_SIGNUP=false`), not a feature an active customer uses.

13. **`/api/templates`** (label/print template activate/deactivate) — distinct from both #11 and the Page Editor. No UI component calls these routes at all (`apiClient.templates` unreferenced anywhere in the UI layer) — reads as backend-only or unwired. Flagged so it isn't mistaken for a doc gap; recommend re-checking before writing anything since it may simply be unfinished.

### Confirmed well-covered (checked specifically, not gaps)

- `help/print-batches.mdx` — batch creation, the four-stage status lifecycle (Draft → Printing → Printed → Deployed), per-code deployment status, retained CSV, batch filtering, deletion restrictions, and **archiving** (explicitly covered — "Completed batches can be archived. They stay available for reference but drop out of the default view.").
- Page Editor section count — `building-a-page.mdx` states "Seventeen sections are available"; counted against the actual component manifests: exactly 17, no divergence.
- Conditional Visibility, Device Detection, Using Fields (bindings), App Links & Fallback URLs, Custom Fields, Pages Overview, and the core Item/Link/Media three-entity model, including the item-level page Override toggle — all thoroughly and accurately documented, matching the corresponding service/table implementations exactly.

### Divergences beyond the 17 Aug 2026 binding-syntax fix

None found. The specific claims most likely to drift from code (section count, archiving, URL-encoding behaviour, deletion/release-on-delete policy, batch status transitions) were re-checked and all matched source. No remaining single-brace binding instances were missed by commit `20e4168` — the two remaining `item.{fieldName}` occurrences are prose describing the naming convention, not URL examples.
