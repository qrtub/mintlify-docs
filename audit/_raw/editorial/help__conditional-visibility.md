# Editorial audit: help/conditional-visibility.mdx

Live: https://help.qrtub.com/help/conditional-visibility
Nav group: Pages (`docs.json` → tab "Docs" → group "Pages": pages-overview, building-a-page, using-fields, **conditional-visibility**, device-detection, app-links)
Siblings skimmed: `help/using-fields.mdx`, `help/device-detection.mdx`

Verification sources: `../qrtub/src/lib/page/bindings.ts`, `destination-resolver.ts`, `section-processors.ts`, `context.ts`, `binding-validator.ts`, `../qrtub/src/lib/types/destination-config.ts`, `../qrtub/src/lib/utils/device-detection.ts`, `../qrtub/src/components/blocks/LandingPageEditor/panels/PropertiesPanel/PropertyForm.tsx`, `../qrtub/src/components/blocks/DestinationRulesEditor/DestinationRulesEditor.tsx`, `../qrtub/BRAND.md`, plus a live `node -e` smoke test against the `cel-js` package actually used in production.

---

## 1. SELF-CONTAINMENT

A cold reader could get the three worked examples in "Common Use Cases" working, but would hit real gaps trying to do anything else:

- **No instructions for where to enter a condition.** The page repeatedly says "Add the condition to each Destination" (line 42, 56, 72) but never says where that field lives. Verified in `PropertyForm.tsx`: it's the **"Show When (CEL Expression)"** field inside the **Visibility** group of a Destination's Properties panel in the Page editor, with helper text "Control when this component is shown. Type to see suggestions." None of that appears on the page.
- **CEL is never defined until the second-to-last section.** "Getting Help" (line 220) is the first place the page says "Conditional visibility uses CEL (Common Expression Language), an industry standard" — after ~190 lines of CEL syntax examples. A reader landing cold has been shown `item.type == "forklift"`-style syntax for the whole page before learning what language it even is.
- **The operator list in the AI prompt template is incomplete and wrong by omission.** Line 90: "Available operators: ==, !=, ||, &&, >, <, >=, <=" — missing `in` (used two examples later, line 104) and missing `!` (used in the Mitti example, line 139) and `size()` (documented on the sibling `using-fields.mdx` but never mentioned here). A reader following only this page's template would under-prompt ChatGPT and get worse expressions than the page's own later examples use.
- **No mention that `time.*` and `request.*` fields exist at all.** Verified in `context.ts`: every page render context always includes `time` (hour, dayOfWeek, dayOfMonth, month, year, isWeekend — UTC) and, when headers are present, `request` (country, city, language, ip, path, referrer). These are listed as `ALWAYS_AVAILABLE_PREFIXES` in `binding-validator.ts` alongside `device`. The page's "Available Fields" section (lines 158–164) lists Item/Custom/Tub/Device/Session fields and stops — no `time`, no `request`. A reader who wants "only show this on weekends" or "only show this to visitors in Australia" has no way to discover these fields exist, on this page or on `using-fields.mdx` (same gap there).
- **No statement of what happens on a bad condition.** Verified live (see §5): an undefined field reference throws inside `cel-js`, is caught by `evaluateCondition()`, and the whole condition silently becomes `false` — the Destination just never appears, with no error shown anywhere in the product's public-facing surface. The page never says this, so a reader debugging "my Destination isn't showing" has no diagnostic model to reach for.
- **No numeric ceilings.** `bindings.ts` defines `CEL_VALIDATION_LIMITS`: max 500 characters, max nesting depth 10, max 20 operators. The page invites combining "multiple conditions" (line 78, 84–86) and shows a 3-clause nested example (the Mitti workaround) without ever stating there's a ceiling.
- **No plan-tier statement.** `BRAND.md` §1.4 lists "Conditional visibility (CEL) | Available (advanced)" with no plan restriction elsewhere in the file. The page never says this explicitly either way — silence here is exactly the gap an AI support agent fills with a guess ("this is probably a Pro feature").

## 2. ANSWER-FIRST

Every H2, quoted verbatim, with a verdict:

- **"## When You Need This"** — opens: *"**You probably don't need conditional visibility if:**"* (bold lead-in, then two bullet lists). Verdict: **answer-first**. No scene-setting; the bullets themselves are the answer to "do I need this."

- **"## Common Use Cases"** — opens: *(nothing — the H2 has zero body text; content jumps straight to "### 1. Equipment Type-Specific Inspections")*. Verdict: **fails answer-first** — there is no 40–60 word answer, or any sentence at all, at the H2 level. A chunker that grabs the H2 span up to the first H3 gets an empty answer.

- **"## Using AI to Generate Conditions"** — opens: *"For more complex conditions, use AI tools like ChatGPT to generate the expressions for you."* Verdict: **directional but too thin** — it is a direct sentence (not preamble), but at 16 words it's well short of a real 40–60 word answer; the actual "how" is entirely delegated to the child H3s.

- **"## Advanced: Device-Specific Destinations"** — opens: *"You can also show/hide Destinations based on what device someone is using."* Verdict: **direct, but context-dependent** — "also" explicitly presumes the reader just read the item-based sections above (see §6 for the chunk-integrity consequence).

- **"## Available Fields"** — opens: *"You have access to:"* then a 5-line bullet list. Verdict: **answer-first** in spirit (the list is the direct answer), though the lead-in clause itself is only 4 words.

- **"## Tips"** — opens directly with *"**Start simple:** Test with one field and one condition first."* — no topic sentence, immediate first tip. Verdict: **no real intro to judge** — acceptable for a tips list, but there's no 40–60 word answer to any implied question because "Tips" doesn't pose one.

- **"## When NOT to Use This"** — opens: *"**Don't use conditional visibility for:**"* Verdict: **answer-first**, but see §3/§6 — this is a near-duplicate of "When You Need This."

- **"## Getting Help"** — opens: *"Conditional visibility uses CEL (Common Expression Language), an industry standard. For complex conditions:"* Verdict: **direct definition**, but structurally misplaced — this is the first and only definition of CEL on the page, ~190 lines after CEL syntax first appears.

- **"## Related"** — link list, no implied question, not applicable.

## 3. ONE QUESTION PER PAGE

This page currently answers at least four distinct questions, two of which duplicate sibling pages nearly verbatim:

1. **"Should I even use conditional visibility?"** — "When You Need This" and "When NOT to Use This" are near word-for-word restatements of each other:
   - Line 11: *"You want different people to see different Destinations (just show all Destinations—people tap what's relevant)"*
   - Line 208: *"Different audiences seeing different content (just show all Destinations—people tap what's relevant)"*
   These are the same guidance, stated twice, ~200 lines apart, with independent wording that will drift out of sync at the next edit.

2. **"How do I write a condition against Item data?"** — the three Common Use Cases (equipment type, tags, test status). This is the page's genuinely unique content.

3. **"How do I get AI to write the CEL expression for me?"** — the prompt template + worked example. Also unique to this page.

4. **"How do I route by device / work around the iOS Safari deep-link block?"** — "Advanced: Device-Specific Destinations," including the full Mitti iOS Safari workaround. **This is a duplicate.** `help/device-detection.mdx` §3 ("iOS Safari Deep Link Workaround (Mitti Example)") carries the identical scenario, the identical two Destinations, the identical two conditions (`device.isDesktop || !device.isIOS || (device.isIOS && device.browser == "safari")` and `device.isIOS && device.browser != "safari"`), down to the same product name and near-identical prose. The "Quick Device Field Reference" table here also duplicates the device field table on `using-fields.mdx` and the fuller one on `device-detection.mdx`.

**Proposed split:**
- **Keep on this page** (unique, condition-writing-specific): the decision framework (merged, not duplicated), the three Item-based use cases, the AI-prompt-generation workflow, and a *new* section on the previously-undocumented `time.*`/`request.*` fields (this page is the natural home since it's the "write a condition" page).
- **Cut from this page, replace with a one-line pointer**: the entire "Advanced: Device-Specific Destinations" section and the "Quick Device Field Reference" table — both already live, in full, on `device-detection.mdx` and `using-fields.mdx` respectively. Link out instead of re-deriving.

The page is not too thin to stand alone — even after removing the duplicated device material it retains a distinct, well-defined job ("how do I write and test a CEL visibility condition") that doesn't belong on any sibling page.

## 4. HEADINGS AS QUESTIONS

- **"When You Need This"** → **"When Do You Need Conditional Visibility?"** — genuinely clearer; the section is a binary decision, and the question form matches how a reader would actually ask it.
- **"When NOT to Use This"** → given the duplication finding above, this shouldn't survive as a separate heading at all; if merged with the above it becomes one section answering **"Should You Use Conditional Visibility?"**
- **"Using AI to Generate Conditions"** → **"How Do I Use AI to Generate a CEL Condition?"** — clearer; matches the literal question a reader has when they hit a condition too complex to write by hand.
- **"Available Fields"** → **"What Fields Can I Use in a Condition?"** — clearer, and disambiguates from `using-fields.mdx`'s own "Available Fields" heading (right now both pages use the identical heading text for different-sized versions of the same list, which is confusing for anyone comparing search results).
- **"Common Use Cases"** — leave as a noun phrase; it's a container for H3 scenarios, not itself an answer to one question.
- **"Tips"**, **"Related"**, **"Getting Help"** — conventional section labels, not natural questions; converting them would feel forced. Leave as-is.
- The H3 scenario titles ("1. Equipment Type-Specific Inspections," "2. Tag-Based Routing," "3. Test Status-Based Routing") are fine as scenario labels — each is immediately followed by a **Scenario:**/**Solution:** pair that already does the answering work a question-form heading would do. Not worth converting.

## 5. EDGE CASES / LIMITS / FAILURE MODES

All verified directly against source, and all currently absent from the page:

- **Undefined/misspelled field → silent `false`, not an error.** Live-tested against the exact library in production:
  ```
  $ node -e "const {evaluate}=require('cel-js'); evaluate('item.nonexistent == \"x\"', {item:{}})"
  → throws: Identifier "nonexistent" not found in context: {"item":{}}
  ```
  `evaluateCondition()` in `bindings.ts` catches exactly this and returns `false` (confirmed by reading the catch block, lines 330–340). Consequence: a typo'd custom field name, a field that was later renamed or deleted, or an invented value (the docs repo's own `CLAUDE.md` notes a past instance of this exact mistake with a fabricated `today` value) all produce a condition that **never fires and shows no error anywhere** — not to the page owner, not to the visitor. The page should say this explicitly; right now a reader debugging "my Destination never shows" has no way to arrive at "check your field name for a typo" as the likely cause.
- **`time.*` fields are UTC, not the visitor's local time.** `destination-resolver.ts`'s `buildTimeInfo()` and the `TimeInfo` interface (`destination-config.ts` lines 91–104) are explicitly documented as UTC. A condition like `time.hour >= 9 && time.hour < 17` is **not** "9am–5pm for the visitor" — it's 9am–5pm UTC, which is evening/night across most of Australia and the Americas. Since `time.*` isn't mentioned on the page at all today, this gotcha isn't even reachable yet, but it must ship alongside any documentation of the field.
- **No full "current date" and no date arithmetic — confirmed by reading the `TimeInfo` interface itself**, which exposes only `hour`, `dayOfWeek`, `dayOfMonth`, `month`, `year`, `isWeekend` as separate integers, no combined date and no comparison helpers. A condition like "hide once the warranty has expired" cannot be built from `time.*` — the page's own use case 3 ("Test Status-Based Routing," `item.testStatus == "expired"`) works around this correctly by using a manually-maintained status field, but the page never explains *why* that indirection is necessary, so a reader might reasonably try `item.expiryDate < time.year` or similar and get silent `false` (per the point above) with no clue why.
- **Expression ceilings.** `CEL_VALIDATION_LIMITS` in `bindings.ts`: 500 character max, 10 max parenthesis/bracket nesting depth, 20 max operator count. Never mentioned, despite the page encouraging exactly the kind of multi-clause combination (AI-generated, nested OR/AND) that could approach these.
- **Plan tier: none found.** `BRAND.md` lists conditional visibility as "Available (advanced)" with no plan gate documented anywhere in the source. The page should say this affirmatively ("available on every plan") rather than leave it silent.
- **All-hidden Page shows nothing — confirmed in `renderer.tsx` line 85: `if (!shouldShow) return null;`.** There is no placeholder, empty state, or fallback message rendered in a hidden section's place. If every Destination's condition evaluates to `false` for a given visitor (e.g., an unanticipated device, or every condition referencing a field that's empty for that item), the Page shows **no Destinations at all** — not an error, not a fallback, just nothing where the buttons would be. The page's "Tips" section gestures at this indirectly ("Consider showing all instead") but never states the actual failure mode or the mitigation (always keep one Destination with no condition, or one whose condition is the logical complement of the others).
- **Supported operator/function set is narrower than general CEL** — confirmed by the comment in `bindings.ts` ("Only using CEL functions that are supported: ==, !=, <, >, <=, >=, in, size(), &&, ||") and by live-testing `!` (negation) works too. The page's own AI-prompt template undersells this (see §1), and never states the ceiling that other CEL features (e.g., regex, `matches()`, string functions like `startsWith`) may not be supported — worth an explicit "supported operators" list rather than reverse-engineering it from scattered examples.

## 6. CHUNK INTEGRITY

Walking each H2 as if it were retrieved alone:

- **"When You Need This"** — mostly self-contained, but references "URL Templates" (line 12, 17) as an alternative without ever explaining what that is on this page. A reader who gets only this chunk doesn't know what to do next with that pointer.
- **"Common Use Cases"** — the H2 itself carries no content (see §2); in isolation it's an empty container. Its three H3 children are each individually self-contained and readable alone.
  - H3 "Test Status-Based Routing" in isolation invites a wrong inference: it shows `item.testStatus == "expired"` with values `"current"`, `"expired"`, `"pending"` and never states, in this chunk, that "expired" must be a value *you* set and update manually — a reader retrieving only this section could reasonably assume QRtub computes "expired" automatically from a date field.
- **"Using AI to Generate Conditions"** — self-contained; the prompt template and worked example are complete without needing anything before or after them.
- **"Advanced: Device-Specific Destinations"** — opens with **"You can *also* show/hide Destinations..."** — "also" is a bare continuity reference to the item-based sections above; retrieved alone, "also" refers to nothing. This section (and its "iOS Safari Workaround" H3) is additionally a near-duplicate of `device-detection.mdx` §3, so a retrieval system indexing both pages will surface the same worked example twice for a query like "iOS Safari deep link workaround."
- **"Available Fields"** — self-contained as a reference list; the pointer to "Using Fields" for the complete reference doesn't block understanding of what's here.
- **"Tips"** — each bullet is an independent, self-contained sentence; no dependency issues.
- **"When NOT to Use This"** — self-contained in isolation, but (per §3) is a near-duplicate of "When You Need This" — a RAG system retrieving both chunks for the same query gets redundant, independently-worded guidance that will silently diverge the next time only one of the two is edited.
- **"Getting Help"** — self-contained; this is actually the one place CEL gets a definition, so in isolation this chunk is more informative about *what CEL is* than any chunk earlier in the page that actually uses CEL syntax.
- **"Related"** — a plain link list; no dependency issues, but also no framing of *why* each link is related beyond the one-line descriptions already present.

---

## Summary

The page's unique, non-duplicated content (the decision framework, the three Item-based use cases, and the AI-prompt workflow) is solid. The defects are: (1) real product mechanics never explained — where to enter a condition, what CEL even is, and the silent-`false` failure mode for bad field references; (2) an entire undocumented field category (`time.*`, `request.*`) that's been live in the render context all along; (3) heavy duplication with two sibling pages (device-detection.mdx, using-fields.mdx) that will drift out of sync; and (4) missing numeric ceilings and plan-tier statement that are exactly the kind of gap an AI support agent will paper over with an invented answer. A substantive rewrite is warranted — see `/workspace/mintlify-docs/audit/proposed/help__conditional-visibility.md`.
