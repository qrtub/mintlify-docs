# Editorial audit: Physical Media Management Basics

- File: `/workspace/mintlify-docs/help/media-basics.mdx`
- Live: https://help.qrtub.com/help/media-basics
- Nav group: **Concepts** (`docs.json`), alongside `help/key-concepts`, `help/print-first-workflow`
- Siblings skimmed: `help/key-concepts.mdx`, `help/print-first-workflow.mdx`, `help/print-batches.mdx`

---

## 1. SELF-CONTAINMENT

The page is mostly conceptual (three-entity model, tracked/not-tracked status) rather than
procedural, so the bar is "can a cold reader understand the concept and get to the one live
action it describes (creating/finding a batch)." It fails the second half.

**Missing pieces:**

- **No path to the actual feature.** "What you can track today" says: *"When you export a
  print list from a Tub, QRtub records it as a batch you can follow in the Media section."*
  It never names where "the Media section" is in the app. The sibling page says the exact nav
  label is **"Access Media"** (`help/print-batches.mdx`: *"Batches live under Access Media in
  the main navigation"* — confirmed against `qrtub/src/components/left-sidebar.tsx` line 131,
  `<span>Access Media</span>`). A reader with only this page cannot find the feature it just
  described.
- **No outbound link to `/help/print-batches` anywhere on the page**, despite that page being
  the only place the actual procedure (button name "Print list", batch detail fields, status
  transitions, deletion protection) lives. The only link in "Next Steps" is to Key Concepts.
- **Internal contradiction about whether a batch guide exists.** "Next Steps" closes with:
  *"Guides for Media Templates, Batch management, replacement workflows and Media Partners
  will follow once those features exist."* This groups "Batch management" in with genuinely
  planned features. But batch management is not planned — it is live (per this page's own
  "What you can track today" and "Print Batches" sections) and already has a dedicated,
  published guide at `/help/print-batches.mdx`. A cold reader who reaches the bottom of the
  page is told, incorrectly, that no such guide exists yet.
- **No statement on plan/tier availability.** Verified against `qrtub/src/lib/stripe-plans.ts`
  and the pricing FAQ (`qrtub/src/app/pricing/pricing-client.tsx`): batch tracking is not
  gated by plan — Starter/Professional/Scale differ only in active-link ceiling (100 / 1,000
  / 10,000), editor count, and numbered-sequence patterns; Business vs Personal differ only in
  GST display ("There is no difference in features or access"). The page says nothing either
  way, which is exactly the silence an AI support agent fills in with a guess ("this is a
  Scale-plan feature").

**Verdict:** Not self-contained. A reader can learn the concept but cannot act on it from this
page alone, and one paragraph actively misinforms them that no batch guide exists.

---

## 2. ANSWER-FIRST

Every H2, quoted verbatim, judged:

- **"Understanding the Three Entities"** — opens *"QRtub recognises three distinct things in
  physical QR deployments:"* → Direct lead into the definition table. Answer-first: **yes**.
- **"Why Track Media Separately?"** — opens *"Physical QR code media is infrastructure with
  real costs and lifecycle needs:"* → Direct answer to the "why" question. **Yes**.
- **"What you can track today"** — opens *"**Print batches.** When you export a print list
  from a Tub, QRtub records it as a batch you can follow in the Media section."* → Bolded
  label plus immediate answer. **Yes** (though see gap in §1 re: "the Media section").
- **"Not tracked yet"** — opens *"There is no record of what an individual QR code is printed
  *on* — no material type, cost, durability or installation location per piece."* → Direct.
  **Yes**.
- **"Media Types"** — opens *"Common physical Media types organisations use:"* → A label
  introducing a list, not an answer, but the heading is a plain noun phrase, not a question, so
  this is acceptable as-is.
- **"Media Templates (Planned Feature)"** — opens *"Future functionality will include reusable
  design templates:"* → States status immediately. **Yes**.
- **"Print Batches"** — opens *"Every print list you export becomes a batch, so you can see
  what was sent to the printer and what has been installed since."* → Direct and clear on its
  own, **yes** — but this is the *second* full answer to the same question "What you can track
  today" already answered above (see §3, §6).
- **"Media Partners (Planned Feature)"** — opens *"Media Partners are the print shops, signage
  companies and engravers who produce physical Media. There is no partner programme yet."* →
  Defines the term and states status in the same breath. **Yes**.
- **"Choosing the Right Media"** — opens *"Consider these factors when selecting Media
  types:"* → A preamble/label, not an answer. The heading implies the question "how do I
  choose the right media," and the section's actual answer (the Environment/Duration/
  Budget/Application checklist) only starts in the next block. **No** — could open with e.g.
  "Match media to the item's environment, expected lifespan, budget, and use case; the
  checklist below covers each factor" before the list.
- **"Next Steps"** — standard closing section, not judged as a content answer.

**Summary:** 7 of 9 content H2s are answer-first. The two misses are "Media Types" (acceptable,
non-question heading) and "Choosing the Right Media" (a genuine miss — heading poses an
implicit question the opening sentence doesn't answer).

---

## 3. ONE QUESTION PER PAGE

This page answers **five distinct questions**, two of which are pure duplicates of content on
sibling pages, and one topic is answered **twice within this same page**:

1. *What is QRtub's three-entity model (Item/Link/Media)?* — Near-verbatim duplicate of
   `help/key-concepts.mdx`'s "The Three-Entity Model" section. Compare this page's "Why Track
   Media Separately?" bullets —
   > `- **Cost** - A metal plaque costs $50. A billboard costs $5,000. These are infrastructure investments.`
   > `- **Durability** - Metal plaques last 10+ years. Weatherproof vinyl lasts 3-5 years.`
   — against Key Concepts' "Why track Media separately" bullets —
   > `- **Cost tracking** - A billboard costs $5,000. That's infrastructure worth managing.`
   > `- **Durability** - Metal plaques last 10+ years. Vinyl stickers last 1-3 years.`
   Same claims, same numbers, reworded. This is the duplication CLAUDE.md's content-strategy
   section warns against ("Search for existing information before adding new content. Avoid
   duplication unless strategic").

2. *What does QRtub track about media today, and what's planned?* — This is the page's actual
   unique, load-bearing content, and matches its placement in the Concepts nav group next to
   Key Concepts and Print-First Workflow.

3. *What are common physical media types, and their typical cost/durability?* — Generic
   industry reference data (sticker prices, plaque lifespan), not a QRtub capability or
   behaviour. Doesn't depend on anything in the QRtub source and won't change when the product
   changes.

4. *How do I choose the right media for my situation?* — A generic decision checklist, same
   category as #3.

**Internal duplication:** Question #2 above is answered **twice** in the same document:
"What you can track today" (lines ~30-37) gives the short version, and "Print Batches" (lines
~105-118) gives a longer version of the identical fact set (batch lifecycle, CSV, deployment
status, filtering, archiving, cost allocation not available). In a chunked-retrieval context, a
query like "does QRtub track print runs" could return both chunks, adding no new information in
the second but consuming the retrieval budget and reading as unedited duplication rather than
elaboration.

**Proposed split:**

- **Keep in `media-basics.mdx`:** questions #1 (trimmed to a short definition + link to Key
  Concepts, not the full duplicated rationale) and #2 (merged into one section, not two).
- **Move out:** questions #3 and #4 (Media Types reference + Choosing the Right Media) belong
  on a separate reference page — call it `/help/media-types` — or folded into
  `help/print-first-workflow.mdx`'s existing "Match the medium to the environment, not the
  budget" bullet, which already gestures at exactly this content in one sentence. Bundling
  generic buying-guide material into a page that otherwise describes QRtub's own tracked/
  untracked data is what makes this page read as two different documents stitched together —
  and risks an AI agent citing "durability: 10+ years" as if it were data QRtub records, when
  the same page states two sections earlier that no such per-piece data is tracked at all.

This audit's proposed rewrite (see below) does not create that second page — out of scope for
a single-page audit — but keeps the Media Types / Choosing content in place, clearly relabelled
as general reference distinct from QRtub-tracked data, and flags the split as a follow-up.

---

## 4. HEADINGS AS QUESTIONS

- **"Understanding the Three Entities"** → *"What are the three entities QRtub tracks?"* —
  clearer; the section literally answers "what is an Item/Link/Media."
- **"Why Track Media Separately?"** — already a question. Keep.
- **"What you can track today"** — already reads as an implicit question; leave, or tighten to
  *"What does QRtub track about media today?"* for parallelism with the next heading.
- **"Not tracked yet"** → *"What isn't tracked yet?"* — parallel construction with the above,
  minor clarity gain.
- **"Media Types"** — leave as a noun phrase. It's a taxonomy/reference list, not an implied
  question; converting it doesn't add clarity.
- **"Media Templates (Planned Feature)"** → *"Are Media Templates available?"* — the section's
  entire content is a direct answer to that exact question ("No — ...").
- **"Print Batches"** → *"How do print batches work?"* — recommend merging this section into
  "What you can track today" rather than retitling it (see §3), but if kept standalone, this is
  the clearer heading.
- **"Media Partners (Planned Feature)"** → *"Are Media Partners available?"* — parallel with
  Media Templates, and matches the section's opening sentence exactly.
- **"Choosing the Right Media"** → *"How do I choose the right media?"* — the heading is
  currently a gerund noun phrase; the content is entirely a decision guide, so the question
  form matches it better and would also force the answer-first fix noted in §2.

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

- **No plan-tier statement.** Confirmed via `qrtub/src/lib/stripe-plans.ts` and the pricing
  FAQ that batch tracking is available on every plan and isn't feature-gated; the only limits
  that touch this workflow are active-link ceilings (100 / 1,000 / 10,000) and editor counts.
  The page states none of this — a gap an AI agent would fill with a guess.
- **Limits that exist on the sibling page but not here.** This page is where "What you can
  track today" and "Print Batches" live, yet it omits limits that `help/print-batches.mdx`
  documents: links in a non-Draft batch are protected from deletion ("Once a batch moves past
  Draft, the links in it are protected from deletion"), and **Deployed is a final status** with
  no way to step back. A reader relying on this page alone will not learn either limit.
- **Contradiction between "Replacement" and "Not tracked yet."** Under "Why Track Media
  Separately?": *"**Replacement** - Damaged Media can be replaced with new Media encoding the
  same Link, so the Item connection survives"* — stated as a present-tense capability. But
  "Not tracked yet" lists *"Replacement workflows"* as planned. Read back to back, one section
  claims replacement works today and the next says it's not built. The page never resolves
  this: the mechanism (reprint the same Link onto new media, swap it in — nothing to
  reconfigure since Media isn't a tracked entity) works today with zero QRtub feature involved;
  what's planned is a *record* that a replacement event happened. As written, the page leaves
  an AI agent to guess whether "replacement" is a feature or not.
- **No workaround pointer.** "Not tracked yet" lists cost/material/durability tracking as
  absent, but doesn't mention the one workaround already documented on the sibling page — the
  batch **notes** field, which `help/print-batches.mdx` says is for "the supplier, the
  material, anything you will want to recall." A reader asking "how do I track material cost
  today, since there's no field for it" gets no answer from this page.
- **"Cost allocation per batch is not available."** — good, explicit limit statement (one of
  the few on the page). Keep this pattern.

---

## 6. CHUNK INTEGRITY

Each H2 evaluated as if retrieved alone, no surrounding page:

- **"Understanding the Three Entities"** — self-contained; table defines all three terms
  without needing prior text. Fine standalone.
- **"Why Track Media Separately?"** — self-contained, no pronoun/backward dependency. Fine.
- **"What you can track today"** — mostly self-contained, but says *"you can follow in the
  Media section"* without ever naming it. A reader with only this chunk doesn't know this
  means the "Access Media" nav item. Minor but real gap for an isolated retrieval.
- **"Not tracked yet"** — self-contained; reads fine alone.
- **"Media Types"** (+ H3 children) — fully self-contained, no dependency on anything else on
  the page.
- **"Media Templates (Planned Feature)"** — self-contained.
- **"Print Batches"** — self-contained in isolation (no "as above" reference), but note this
  chunk and "What you can track today" are near-duplicates of each other (see §3) — a
  retrieval system pulling both for the same query returns redundant, not complementary,
  information.
- **"Media Partners (Planned Feature)"** — self-contained.
- **"Choosing the Right Media"** — self-contained, generic checklist reads fine alone.
- **"Next Steps"** — this is the one chunk that is **actively wrong in isolation**, not just
  missing context: *"Guides for Media Templates, Batch management, replacement workflows and
  Media Partners will follow once those features exist."* Read on its own, this flatly states
  no batch-management guide exists — false, and false regardless of what surrounds it, since
  `/help/print-batches.mdx` is a real, published page.

---

## Summary

The page's unique content (batch tracking status) is sound and answer-first, but the page:

1. Duplicates ~40% of its content from `help/key-concepts.mdx` (three-entity model, "why track
   separately" rationale, near-identical cost/durability figures).
2. Answers its own core question twice within the page ("What you can track today" / "Print
   Batches" sections restate the same facts).
3. Never links to `/help/print-batches.mdx`, the page that actually documents the one live
   feature this page introduces, and closes with a factually wrong claim that no
   batch-management guide exists yet.
4. Bundles in two generic, non-QRtub-specific reference sections (Media Types, Choosing the
   Right Media) that dilute the page's single-question focus and risk being mistaken for
   QRtub-tracked data.
5. Is silent on plan-tier availability and omits limits (deletion protection, Deployed being
   final) that a reader relying on this page alone would need.

A substantive rewrite is warranted. See `/workspace/mintlify-docs/audit/proposed/help__media-basics.md`.
