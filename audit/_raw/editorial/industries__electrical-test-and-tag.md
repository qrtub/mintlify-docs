# Editorial Audit: QRtub for Electrical Test and Tag

**File:** `/workspace/mintlify-docs/industries/electrical-test-and-tag.mdx`
**Live:** https://help.qrtub.com/industries/electrical-test-and-tag
**Nav group:** Industries tab → "Industries" group (`civil-construction`, `contract-cleaning`, `arboriculture-tree-management`, `electrical-test-and-tag`, `local-government-councils`)
**Siblings skimmed for overlap:** `industries/civil-construction.mdx`, `industries/local-government-councils.mdx`

**Bar used for this review, per instructions:** this is a marketing/industry landing page, not a how-to page. It is judged on citability and accuracy of its capability claims — every claim checked against `../qrtub/BRAND.md` ("Claims That Are FALSE"), `../qrtub/GLOSSARY.md`, and the app source in `../qrtub/src` — plus whether it belongs in an AI-facing `llms.txt` index at all. The how-to rubric (self-containment, answer-first, chunk integrity, etc.) does not apply here and was not used.

---

## 1. Overall shape

Structurally this page is one of five near-identical templates (Challenge → two Use Cases → "Professional X Effect" → Bulk Deployment → Real-World Example → Integration → Getting Started → Why Choose → Use Cases list → CTA), applied to the electrical test-and-tag vertical per CLAUDE.md's "change the nouns, not the verbs" instruction for industry pages. The template itself is a reasonable, deliberate pattern (confirmed by `civil-construction.mdx` and `local-government-councils.mdx` sharing the same skeleton almost section-for-section), so structural repetition across the Industries group is expected and not, on its own, a defect of this page. The findings below are specific to the claims this page makes, not to the shared template.

---

## 2. Claim-by-claim accuracy check

Verified against `BRAND.md` §1.4–1.6 and the app source.

### 2.1 A confirmed false/fabricated capability claim: automatic date reformatting

The "Bulk Deployment and Ongoing Management" section states:

> **Compliance deadline tracking:**
> - Item field: `nextTestDue: 2025-08-15`
> - Page shows: "Next test due: 15 Aug 2025"

This describes QRtub taking a stored ISO date (`2025-08-15`) and rendering it back on the public Page in a different, human-readable format (`15 Aug 2025`). No such transformation exists. Verified in the actual Page-rendering components:

- `src/components/page/KeyValue.tsx` and `src/components/page/SpecGrid.tsx` — the only two data-display components in `src/components/page/` that render arbitrary field values — both normalize values with `String(v).trim()` (see `norm()` in each file) and nothing else. There is no date parsing, no `Intl.DateTimeFormat`, no `toLocaleDateString`.
- `src/lib/page/bindings.ts` (the binding-resolution layer) has no date-handling code at all — confirmed by grep, only unrelated matches on the word "validate."
- The one date-formatting utility that does exist in the codebase, `src/lib/utils/au-date.ts` (`isoToLocaleDate`), is explicitly scoped in its own doc-comment to "CSV import/export, grid display" — i.e., the internal dashboard grid, not the public-facing Page. It is not imported anywhere under `src/components/page/` or `src/lib/page/`.

So a field stored as `2025-08-15` renders on the Page exactly as `2025-08-15`, not `15 Aug 2025`. This is the same category of error CLAUDE.md explicitly warns about ("Pages have previously documented a `today` value... automatic URL encoding... None of these exist. All were written from assumption rather than from the code") — it invents a display-formatting capability that isn't implemented. Any reader (human or AI agent) taking this example literally will get a wrong preview of what their Page will actually show, and a support bot asked "will QRtub format my date field nicely" would answer yes, incorrectly, if this page is in its retrieval set.

**Fix:** either show the field rendering literally as stored (`Next test due: 2025-08-15`), or drop the reformatted-output line entirely.

### 2.2 Repeated DD/MM/YYYY examples are not literally false, but are misleading in the same direction

Elsewhere the page shows dates in `DD/MM/YYYY` inside quoted "what the screen shows" examples — "Last tested: 15/02/2025, Next due: 15/08/2025, Status: PASS" (line 51), "PASS - Next due: 20/09/2025" (line 149), "Tested 20/03/2025" (line 156). These are technically achievable if the customer manually types/stores the date in that exact string format (QRtub does render whatever string is stored, verbatim). But combined with §2.1's explicit ISO→formatted example two sections earlier, the cumulative effect across the page is to imply QRtub applies some consistent, automatic date presentation layer. It doesn't — whatever format the field is authored in is what appears, with no reformatting at any layer. Worth a single clarifying sentence somewhere on the page (or in a shared industries-wide note) that field values display exactly as entered.

### 2.3 "Compliance register" framing sits close to BRAND.md's false-claims list

`BRAND.md` §1.3 is explicit that QRtub "IS NOT... A compliance platform" ("It links to compliance tools, doesn't manage compliance") and §1.6 lists as FALSE: "QRtub is asset management software," "QRtub tracks maintenance history (it links to systems that do)."

This page's central marketing conceit is "The Professional Compliance Register Effect" (H2, line 101) and repeatedly calls the thing QRtub provides a "compliance register" / "digital compliance register" (lines 15, 95, 105, 197, 213, 215). Read generously, this is defensible: QRtub Items genuinely are a real, spreadsheet-style register of equipment with custom fields (Equipment ID, Type, Location, Next test due, Serial, etc.), CSV export is a real Available feature, and the page never claims QRtub calculates compliance status, sends overdue alerts, or performs inspection logic itself — the actual test/pass-fail data flow is correctly described as living in "the testing app you use" (lines 58, 85, 145) and as a manually-maintained item field, not an automatically computed one. That's consistent with the CLAUDE.md guidance on "what integration means here."

The risk is for a reader (especially an AI agent summarizing the page) who compresses "Professional Compliance Register Effect" + "audit-ready compliance registers" + "digital compliance register included with our service" into "QRtub is a compliance management/compliance register platform" — which is exactly the sentence BRAND.md's false-claims list forbids. The page is one inference away from the forbidden claim rather than stating it outright. Recommend either (a) adding one explicit sentence disambiguating that the "register" is QRtub's item grid + custom fields, not a compliance-calculation engine, or (b) toning down "compliance register" to "equipment register with test-status fields" in at least the header and the two most citable summary lines (101, 197).

### 2.4 "Real-World Example" headers present hypotheticals as case studies

Line 136, "Real-World Example: Contract Provider Serving Office Building," and its walk-through narrative ("A test and tag contractor services an office building with 300 electrical items...") is an illustrative, generic scenario — no named customer, no real numbers, no verifiable detail. `BRAND.md` §3.2 ("Never") explicitly lists "Invent customer quotes or case studies" as a rule. This isn't a quote, but labeling a made-up scenario "Real-World Example" (rather than "Example Scenario" or "How this plays out") blurs the same line the rule is protecting: a reader — and especially an AI agent retrieving this chunk out of context — has no signal that "Real-World Example" here means "illustrative," not "a real, documented customer." The subjective color inside it compounds this ("Impressed by professional system and easy access," line 165) — an unfalsifiable reaction attributed to a fictional auditor.

This is a site-wide pattern (civil-construction.mdx uses "Real-World Construction Use Cases," local-government-councils.mdx uses "Real-World Example: Council Playground Equipment" with the same structure), so it's not unique to this page, but it's worth flagging here because it directly affects citability: an AI agent should not present this section's contents as evidence of an actual QRtub customer outcome, and nothing in the heading or body warns it against doing so.

### 2.5 Claims verified TRUE / consistent with the source

For balance, the majority of concrete capability claims on this page check out:

- **Link formats** — `qrtub.com/drill-001` (line 183) matches the ID-based format documented in CLAUDE.md's Link URL Structures table.
- **URL Template syntax** — `yourtestapp.com/test?equipmentID={{item.assetID}}&type={{item.equipmentType}}&client={{item.clientName}}` (line 125) uses the correct double-brace, namespaced syntax (`{{item.field}}`) per `src/lib/page/bindings.ts`; this is a real, Available feature.
- **"Test record is saved in the testing app you use"** (lines 58, 85, 145) and **"Update without retagging... without retagging equipment"** (lines 134, 199) — both match the correct "what integration means here" phrasing (no sync/write-back implied) and BRAND.md's TRUE-claims list (Destinations changeable without reprinting).
- **"Manage every item in a spreadsheet-style grid, and export your item data as CSV"** (line 209) — matches BRAND.md Available features ("Spreadsheet-style grid management," "Import/export").
- **"Each client gets their own Tub"** (line 70) and per-client Tub isolation as an organizational pattern — Tubs are genuinely independent workspaces; this is a legitimate usage pattern, not a fabricated feature. It should not be read as a claim of user-level access permissions (BRAND.md lists "Granular permissions" as Planned) — the page doesn't make that claim explicitly, but "isolated equipment register" is a phrase that could be over-read that way. Minor risk, not a hard violation.
- **Tool names** (Test & Tag Manager, PAT software, Mitti (formerly SafetyCulture), CMMS platforms) — presented as external tools QRtub links to, not integrates with in a data-sync sense; consistent with sibling pages' identical convention and with CLAUDE.md's "What integration means here" section.

### 2.6 Minor: section header word choice ("Integration")

Line 169's H2 is "Integration with Test and Tag Software." CLAUDE.md is explicit: "Never write '...integrates with...' in a way that implies data exchange, sync, or write-back." The body text under this header correctly avoids that ("QRtub connects to the tools you already use," "URL Templates auto-populate equipment data") — but the header itself uses the flagged word. This is a template-wide convention (identical header pattern on `civil-construction.mdx`, generic "Integration with Council Systems" on `local-government-councils.mdx`), so it's a shared, low-severity wording choice rather than a page-specific defect — worth fixing across all industry pages together rather than singling this one out.

---

## 3. Overlap / redundancy with sibling pages

`civil-construction.mdx` and `local-government-councils.mdx` share this page's exact section skeleton and several verbatim phrases ("Update without retagging," "URL Templates auto-populate equipment/asset data—configure once, deploy across [X] population," "Zero reprinting," the "Professional X Effect" framing device). This is consistent with CLAUDE.md's explicit industry-page authoring instruction ("Change the nouns, not the verbs — same capabilities, different context") and is not a defect specific to this page. No content unique to electrical test-and-tag is duplicated on a sibling page in a way that would confuse a reader — the redundancy is at the level of narrative device, not overlapping factual claims about the same industry.

---

## 4. Should this page be in `llms.txt`?

**No, not as currently written.**

Reasoning:

1. **Genre mismatch with the rest of the index.** `docs.json`'s `contextual` block enables `chatgpt`, `claude`, `perplexity`, and `mcp` surfaces site-wide, and Mintlify's default `llms.txt` generation pulls every page in `navigation`, including this one, alongside the Help tab's how-to pages. Those how-to pages are held (correctly, per CLAUDE.md) to a strict "verify every capability claim against source" bar with an explicit "state limitations, silence reads as capability" rule. This page is written in a looser, sales-narrative register (subjective reactions, "Professional X Effect" framing, unlabeled hypothetical case studies) that the how-to pages deliberately avoid. Mixing the two registers into one retrieval index means an AI agent cannot distinguish "this is a verified product fact" from "this is illustrative marketing copy" — they arrive with the same formatting and the same apparent authority.

2. **It contains at least one demonstrably false capability claim** (§2.1, the date-reformatting example) that a support bot or coding agent could cite as evidence of real Page-rendering behavior, leading a customer to build a Destination/field expecting output QRtub does not produce.

3. **It contains unlabeled hypothetical case studies** (§2.4) under a heading ("Real-World Example") that reads as a factual attestation. An AI agent summarizing "what results has QRtub delivered for test-and-tag contractors" could easily present this fictional office-building scenario as a real outcome, which is a direct citability failure — the page gives the retrieving agent no signal to treat it otherwise.

4. **Its central framing risks contradicting BRAND.md's own false-claims list** (§2.3) if compressed by an AI into "QRtub is a compliance platform" — exactly the sentence BRAND.md forbids stating.

**Conditional path to inclusion:** if (a) the date-formatting example is corrected or removed, (b) "Real-World Example" headers are relabeled as illustrative (e.g. "Example Scenario") both here and across sibling industry pages, and (c) the "compliance register" language is tightened per §2.3, the remaining content is largely accurate and could reasonably sit in an AI-facing index — industry pages do carry legitimate signal (real tool names, real Link/URL-template mechanics) that a prospect-facing agent might reasonably want to retrieve. Until then, recommend either excluding the Industries tab from `llms.txt` generation entirely (keep it for human/SEO browsing on the marketing surface only) or fixing the three items above first.

---

## 5. Verdict

This page is mostly accurate on its concrete, checkable mechanics (Link formats, URL Template syntax, CSV export, Tub-per-client pattern, "integration" phrasing) but has one confirmed fabricated capability (automatic ISO→human date reformatting on the Page, verified absent from `KeyValue.tsx`, `SpecGrid.tsx`, `bindings.ts`, and `au-date.ts`'s own scope comment), one framing choice ("compliance register") that sits uncomfortably close to BRAND.md's explicit false-claims list without quite crossing it, and one structural habit (unlabeled hypothetical "Real-World Example" scenarios) shared with its siblings that actively undermines citability for an AI agent. None of these are cosmetic — the date-formatting error is a direct, checkable factual error, and the other two are the specific failure modes that make marketing copy unsafe to mix into a technical retrieval index. Recommend fixing all three before this page is treated as a citable source in any AI-facing surface, and recommend the broader Industries tab be reconsidered for `llms.txt` inclusion as a category, not just page-by-page.
