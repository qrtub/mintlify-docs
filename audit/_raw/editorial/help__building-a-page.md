# Editorial Audit — `/help/building-a-page` (Building a Page)

File: `/workspace/mintlify-docs/help/building-a-page.mdx`
Live: https://help.qrtub.com/help/building-a-page
Nav group: **Pages** (`help/pages-overview`, `help/building-a-page`, `help/using-fields`, `help/conditional-visibility`, `help/device-detection`, `help/app-links`)

Verified against `../qrtub/src` (component registry, `LandingPageEditor` reducer/panels/TopBar, `ActionLink.tsx`, `section-processors.ts`, `themes/presets.ts`, `EditTubForm.tsx`, `lib/page/merge.ts`). Almost every factual claim on this page checked out exactly against source — see inline citations below. This is one of the more accurate pages in the corpus; the issues below are about structure, self-containment and a handful of real omissions, not invented capability.

**Verdict: rewrite proposed.** The factual content is sound, but the answer-first failure and
self-containment gaps both land in the same section (Saving), and fixing them properly means
touching the surrounding sections too (undo cap, Page Info, Key Concepts link). Full
replacement draft: `/workspace/mintlify-docs/audit/proposed/help__building-a-page.md`.

---

## 1. SELF-CONTAINMENT

Mostly yes, with three concrete gaps a cold reader would hit:

- **Undefined vocabulary.** The page uses "Tub," "Item," and "Destination" throughout (e.g. "Pages must be switched on for the Tub," "every Item in that Tub renders it with its own data") without defining any of them and without linking to the page that does. `help/key-concepts.mdx` is the page that defines these terms, but it is linked from `help/pages-overview.mdx`'s Related list, not from this page. A reader who lands here first (plausible — it's a top Google/AI-answer result for "how do I edit a QR page") has no path to the vocabulary from this page itself.
- **"Base template" is never defined, only used.** The Saving section says "leaving everything else on the base template" and "updates the base template" — but "base template" is never introduced as a term. The closest thing to a definition is the page's opening sentence, *before any H2*: "You build one layout for a Tub, and every Item in that Tub renders it with its own data." That sentence carries load-bearing meaning for a later section but isn't under any heading (see §6).
- **Page Settings is incompletely described.** Verified in `PageSettingsPanel.tsx`: when nothing is selected, Properties actually shows a **Page Info** group (Page Name, a read-only Version number) above the Theme group the doc describes. The doc's "Theming and layout" section jumps straight to Theme Preset and never mentions Page Name / Version exist. A reader following the doc exactly and looking at their own screen sees a section the page never explains.

Everything else — turning Pages on, opening the editor, the three-tab left panel, adding/arranging sections, the 17 section types and their categories, bindings, conditional visibility, previewing, and the override/save model — is complete enough that a reader could execute the whole workflow using only this page. No missing steps, no "assumed you already did X" gaps in the procedural core.

## 2. ANSWER-FIRST

Checking the literal opening sentence of every H2 against the question its heading implies:

| Heading | Opening sentence(s) | Verdict |
|---|---|---|
| Before you start | "Pages must be switched on for the Tub." | **Direct.** Answers "what do I need before starting" immediately. |
| Opening the editor | "From the Tub's settings, open the **Profile page** tab and choose **Edit profile page**." | **Direct.** No preamble. |
| The layout | "The page sits in the middle. A panel on each side does the work." | **Direct**, if terse — answers "what does the editor look like." |
| Adding and arranging sections | "1. On the **Components** tab, find a section and add it" | **Direct.** Numbered procedure starts immediately. |
| Section types | "Seventeen sections are available, grouped by category in the palette." | **Direct.** Answers "what section types exist" with a number and structure up front. |
| Putting Item data into a section | "Most section settings accept a binding instead of fixed text. Bindings use double curly braces and a namespace:" | **Direct.** |
| Showing a section only sometimes | "Every section has a **Visibility** setting that takes a condition." | **Direct.** |
| Previewing with real data | "The item selector in the top bar switches what the canvas renders against:" | **Direct.** |
| Theming and layout | "With no section selected, **Properties** shows **Page Settings**:" | **Direct**, though see §1 — incomplete (misses Page Info). |
| Saving: the whole Tub, or one Item | "This is the part worth understanding before you save." | **Fails.** Pure scene-setting — tells the reader this section is important but answers nothing. The actual answer to "what happens when I save" doesn't start until the *second* paragraph ("When you are previewing a real Item, the top bar shows an **Override** toggle"). This is the one clear answer-first violation on the page, and it's in the section most likely to be retrieved in isolation by someone asking "why did my edit change every item" or "how do overrides work" — exactly the case where a wasted opening sentence costs the most. |
| Related | (link list) | N/A — not a prose section, exempt from this check. |

Score: 9 of 10 substantive H2s open with a direct answer. One (**Saving**) does not.

## 3. ONE QUESTION PER PAGE

This page is a single coherent task — "build and save a page in the Page Editor" — and every section is a genuine sub-step of that task (turn it on → open the editor → understand the layout → add sections → bind data → add conditions → preview → theme → save). It is not conflating two unrelated questions the way, say, a page mixing "how to do X" with "pricing for X" would.

That said, it is a long page (17 sections, ~160 lines) covering material that also has its own dedicated pages elsewhere in the same nav group:

- **Putting Item data into a section** duplicates the *introduction* to bindings that `help/using-fields.mdx` covers in full (its own "Two Ways to Use Fields" / "URL Templates" sections). This page's version is appropriately short and defers with "see Using Fields for the full reference" — that's the right pattern, not a defect. No change needed here.
- **Showing a section only sometimes** does the same relative to `help/conditional-visibility.mdx` — short intro, one example, explicit "see Conditional Visibility" hand-off. Also correctly scoped.

No split is warranted. The **Saving: the whole Tub, or one Item** section is the best candidate if the page ever needs to shrink — it's the most independently searched sub-topic (override behavior), it's self-contained once its opening sentence is fixed (§2, §6), and there is currently no other page discussing overrides at all. But splitting it off now would leave both halves thinner without a clear retrieval win, since anyone asking about the editor and anyone asking about overrides are both very likely mid-way through "building a page" already. Recommendation: **keep as one page**, just fix the answer-first opening and the two self-containment gaps in §1.

The page is not too thin to stand alone — no merge recommendation.

## 4. HEADINGS AS QUESTIONS

Most headings are already fine as noun phrases because the content right beneath them is unambiguous. Proposing rewrites only where a question form would genuinely resolve ambiguity a support bot might have:

- **"Section types"** → could become **"What sections are available?"** — mild improvement; an AI agent matching a user question ("what components can I add to my page?") against heading text alone would match the question form more directly than the noun phrase. Marginal, take-it-or-leave-it.
- **"Saving: the whole Tub, or one Item"** → already effectively a question in disguise (a false dichotomy framed as a heading) and reads well; a literal question form — **"Does saving change one Item or the whole Tub?"** — is more directly matchable to how a confused user would actually phrase the problem ("why did my change apply to everything"). This is the one heading rewrite I'd actually recommend, since it pairs with fixing the section's answer-first problem (§2).
- **"The layout"** → too vague standalone ("the layout" of what?) but immediately clarified by its own first sentence, so leaving as-is is fine; a heading like "How is the editor laid out?" wouldn't add meaningfully more.
- Everything else ("Before you start," "Opening the editor," "Adding and arranging sections," "Putting Item data into a section," "Showing a section only sometimes," "Previewing with real data," "Theming and layout") already functions as an implicit question and converting to literal question form would not improve clarity — these are left alone deliberately, not overlooked.

## 5. EDGE CASES / LIMITS / FAILURE MODES

The page is unusually good here already — it states several real limits and failure behaviors other pages in this corpus omit:

- ActionLink hides itself and names the missing field when a bound URL can't resolve (verified: `PropertyForm.tsx:379`, `"Link will be hidden - Missing: {fields}"`, and `section-processors.ts` `shouldRenderActionLink`).
- Bindings render as empty rather than erroring when unresolved, and most sections self-hide on empty content.
- Conditional visibility shows live visible/hidden/invalid feedback while typing.
- Undo history is explicitly said to clear when switching Items ("switching to a different Item clears the undo history — finish an edit before you go looking at another Item") — verified exactly in `editorReducer.ts` (`SELECT_ITEM`-equivalent load action resets `history: {past: [], present: action.page, future: []}`).
- The override/save model states its own failure mode clearly: saving with the toggle off on an Item that already has overrides triggers a warning first, and both single-section revert and full-override removal are named as recovery paths.
- Theme preset replacement behavior is stated as a "gotcha" up front ("Choosing a preset replaces the whole theme rather than merging") — verified in `presets.ts` `applyPreset()`, which literally comments "preset replaces current theme."

Gaps that remain (genuine omissions, not invented facts to add — flagging for a human/product-source call before writing prose):

- **Undo history has a hard cap of 50 steps** (`EDITOR_CONSTANTS.MAX_HISTORY_SIZE = 50` in `editorUtils.ts`). The doc mentions undo/redo exist and that switching Items clears them, but never states the depth limit. Worth one clause ("up to 50 steps") since "how far back can I undo" is a natural follow-up question and the current text could imply unlimited undo within a session.
- **No plan/tier gating exists for the Page Editor** (confirmed: no `plan`/`tier`/`Pro`/`Free` string anywhere in `LandingPageEditor/`, and the pricing page scales by QR code volume, not by feature). This is *not* a gap in the sense of missing information the doc should add — there's nothing to state — but per the audit brief's instruction to treat silence on plan tier as a defect: this page's silence is *correct* silence, not an omission, since the feature isn't tier-gated. Noting this explicitly so it isn't misdiagnosed as missing.
- **No stated ceiling on number of sections per page or nesting depth.** Not found in source either (no `MAX_SECTIONS`/`maxDepth` constant) — appears genuinely unlimited, so there is nothing false to correct, but the doc doesn't reassure the reader either way. Low priority; only add if product confirms there truly is no limit worth flagging (e.g. "no fixed limit, but very deep nesting will slow the canvas" — cannot verify a performance ceiling from source, so do not add without confirmation).
- **Page Info (Page Name / Version) fields are entirely undocumented** — see §1. This is the clearest concrete gap: it's not an edge case so much as a whole (small) piece of the visible UI with zero explanation.
- **The Override-OFF behavior is described one level less precisely than the code supports.** The doc says Override OFF "updates the base template, changing the page for *every* Item in the Tub" — true, but overrides are merged **per section, not per Item** (verified: `mergePage()` / `mergeSections()` in `lib/page/merge.ts` does an id-level `$upsert` of override sections onto the base). So an Item that has overridden only its Hero section still picks up a base-template change to, say, its Tags section. The current wording could read as "an Item with any override is now frozen from base changes entirely," which isn't true. Small precision gap, not a fabrication — worth one clause the next time this section is touched.

## 6. CHUNK INTEGRITY

Walking each H2 as if it were the only thing retrieved:

- **Before you start** — Self-contained. Names the exact toggle label and what happens when it's off.
- **Opening the editor** — Self-contained, though it assumes the reader is already inside "the Tub's settings," which is fine as a next-step instruction but would strand a reader who lands on *only* this chunk with no idea what a Tub is (ties to §1's vocabulary gap, not a within-page reference problem).
- **The layout** — Self-contained.
- **Adding and arranging sections** — Self-contained. "Container and Card can hold other sections" references the Section Types section's table by name (implicitly), but Container and Card are common English words the sentence still makes sense without — no real dependency.
- **Section types** — Self-contained.
- **Putting Item data into a section** — Self-contained; correctly re-states the `{{item.name}}` syntax rather than assuming it was seen elsewhere.
- **Showing a section only sometimes** — Self-contained; gives its own example condition.
- **Previewing with real data** — **Depends on prior context.** "The item selector in the top bar" and "The **Preview** toggle" both assume the reader already knows there is a top bar with these controls (introduced, if at all, only implicitly in "Opening the editor" / "The layout," which never actually mention a top bar). In isolation, a reader retrieving just this chunk knows *that* an item selector and Preview toggle exist and what they do, but not *where* they are beyond "the top bar" — acceptable, since "top bar" is stated inline each time rather than "as shown above," but borderline.
- **Theming and layout** — Self-contained.
- **Saving: the whole Tub, or one Item** — **Two real dependency issues.** (1) "This is the part worth understanding before you save" (the opening sentence) refers forward to the section itself, not backward — not a broken reference, but see §2, it's dead weight in isolation since it presupposes the reader already cares/knows this is important, which isn't established without the rest of the page. (2) The section's central term, "base template," is never defined within the section or the page's H2 structure at all — it's only inferable from the pre-H2 intro paragraph ("You build one layout for a Tub, and every Item in that Tub renders it with its own data"). If a retrieval system chunks by H2 and does **not** carry the page's lead paragraph along with every chunk, this section is retrieved with an undefined term at its core. This is the most consequential chunk-integrity finding on the page, because "base template" is exactly the concept someone asks about when confused why their edit did (or didn't) propagate everywhere.
- **Related** — N/A, a link list.

Bottom line: one section (**Previewing with real data**) has a mild, acceptable "top bar" forward/back-reference; one section (**Saving**) has a real defined-term dependency on unheaded intro prose plus a wasted opening sentence — worth fixing together since they're adjacent (§2, §4, §6 all point at the same section).
