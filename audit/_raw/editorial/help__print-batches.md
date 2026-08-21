# Editorial audit — /help/print-batches

Source file: `/workspace/mintlify-docs/help/print-batches.mdx`
Live: https://help.qrtub.com/help/print-batches
Nav group: `Printing` (docs.json) — the only page in that group. Siblings checked for overlap:
`help/media-basics.mdx` (Concepts group) and `help/print-first-workflow.mdx` (Concepts group).

---

## 1. SELF-CONTAINMENT

Mostly yes for the core "create and progress a batch" task, with concrete gaps below.

**What's covered adequately:** "Open a Tub, select the Items you want, and choose **Print
list** from the menu. Pick the columns your printer needs — Item fields, link fields, or
both — and export." This matches the actual menu label ("Print List" in
`src/app/app/access-link/page.tsx:1617,1660`) and the real column picker
(`PrintListModal.tsx`, which offers Item fields plus `ACCESS_LINK_KEYS` = Full URL / Short
URL / Active).

**Missing piece #1 — the page's central claim is incomplete.** The page states flatly:
> "The export downloads a CSV **and creates a batch**. This is worth knowing: exporting is
> what marks those links as printed. It is not a neutral download."

This is not true of the actual export dialog. `PrintListModal.tsx` renders **two** footer
buttons when `hideDraftToggle` is false (the case used from the Access Link page):
- A button labeled **"CSV"** with `title="Download CSV without creating a batch"` — a genuine
  neutral download.
- A button labeled **"Create draft batch"** — downloads the same CSV *and* creates a batch.

`access-link/page.tsx` confirms this at the call site: `options.draft === true` means "skip
batch tracking, just download a CSV." So a reader who lands on this page cold and needs to
know "will exporting create a tracked batch or not?" gets a wrong, overconfident answer. This
is the single most important fix this page needs.

**Missing piece #2 — no cap on export size.** `export-print-csv/route.ts` rejects a
selected-ID export over 5000 rows: `if (ids && ids.length > 5000) return errorResponse('Too
many IDs (max 5000)', 400);`. Nothing on the page tells a reader trying to batch a large
site that hand-picking more than 5000 Items in one export will fail. (A filtered/"select all
matching" export that doesn't pass explicit IDs isn't subject to this cap — the page doesn't
distinguish the two paths either.)

**Missing piece #3 — batch deletion is never addressed.** The page has a whole section on
why *links* can't be deleted once printed, but never says whether the *batch record itself*
can be deleted. Source (`server-print-batches.ts`, `deleteBatch`): only `draft` batches can
be hard-deleted; anything `printing`/`printed`/`deployed` throws
`ProtectedBatchError('Cannot delete a batch that is past draft. Archive it instead.')`. This
is the same shape of rule as the link-deletion section and belongs next to it or in
Archiving.

**Missing piece #4 — archiving is stated more narrowly than it behaves.** "Completed batches
can be archived" implies archiving requires reaching some end state. The actual PATCH route
(`/api/print-batches/[id]/route.ts`) applies no status check at all — `updateBatch(id, {
archived: !!body.archived })` runs for a batch in any status, including Draft — and it's
reversible (`media/page.tsx`: `apiClient.printBatches.update(id, { archived: !archived })`,
toast copy "Batch unarchived", plus a "Show archived" view toggle). The page never mentions
un-archiving is possible at all.

**Missing piece #5 — default batch name.** "Batch details" tells the reader they can add "a
name that means something later" but never says a batch is auto-named on creation
(`Print list — {date}` per `createBatchWithCsv`), so a reader doesn't know renaming is
optional cleanup rather than a required step to get a usable batch.

**Missing piece #6 — silent CSV-attach failure.** `createBatchWithCsv`'s own code comment:
"Not transactional: if the CSV upload fails after the batch row exists the batch will be
left with a null `csv_storage_path`." A support agent will get "my batch has no CSV to
download" tickets and the docs currently offer no explanation or next step.

**No plan-tier gap found (this is fine, not a defect):** nothing in `stripe-plans.ts` gates
print batches behind a tier, so the page's silence on plan requirements is accurate — though
an explicit "available on every plan" line would still help an AI agent asked directly.

---

## 2. ANSWER-FIRST

Every H2, with its literal opening sentence(s) and a verdict.

- **Creating a batch** — *"Open a Tub, select the Items you want, and choose **Print list**
  from the menu."* → Answer-first. Direct procedure, no preamble.

- **Finding your batches** — *"Batches live under **Access Media** in the main navigation."*
  → Answer-first.

- **Batch details** — *"Open a batch to add:"* → Answer-first (leads straight into the bullet
  list of fields), though it's a sentence fragment rather than a complete sentence.

- **Status** — *"A batch moves through four stages:"* → Answer-first, leads straight into the
  table.

- **Tracking what is actually installed** — *"A batch being "Printed" does not mean all 500
  stickers are on equipment. Each code inside a batch carries its own deployment status:"* →
  Partial. The first sentence is a caveat/motivating scenario, not the answer to "how do I
  track what's installed" — the actual answer (each code has its own status) only starts in
  sentence two. Short enough that it isn't a real cost, but it is scene-setting before the
  substance, not the direct-answer pattern used elsewhere on this page.

- **The CSV is kept** — *"The exported file stays with the batch, so a reprint uses exactly
  the same list rather than a regenerated one that might have drifted."* → Answer-first.

- **Filtering Items by batch** — *"Back in the Tub, you can filter Items by the batch they
  were printed in — useful when a run needs replacing, or when you want to know what a
  particular delivery covered."* → Answers the question, but opens with **"Back in the
  Tub,"** which presumes the reader just came from the "Creating a batch" section above. See
  §6 — this is a chunk-integrity defect as much as an answer-first one.

- **Why some links cannot be deleted** — *"Once a batch moves past Draft, the links in it are
  protected from deletion."* → Answer-first.

- **Archiving** — *"Completed batches can be archived."* → Answer-first in form, but (per §1)
  factually narrower than the real behavior.

- **What is not tracked** — *"Batches record production runs, not individual pieces of
  media."* → Answer-first.

Verdict: 8 of 10 H2s open with a direct answer; two ("Tracking what is actually installed",
"Filtering Items by batch") open with framing/backward-reference before the answer.

---

## 3. ONE QUESTION PER PAGE

This page is scoped to one *entity* (the print batch) across its whole lifecycle, but that
spans roughly ten distinct sub-questions: create/export, find, name/annotate, status
lifecycle, per-code deployment tracking, CSV retention, filtering Items by batch, link
deletion protection, archiving, and what's out of scope. That's broad for a single retrieval
chunk if judged as "one question," but each H2 is (mostly) independently retrievable — see
§6 — so the practical harm is lower than it would be for ten unrelated topics glued together.

**Recommendation: do not split.** The page earns its breadth because every section is a facet
of the same object's lifecycle (a batch's fields → its states → what's tracked inside it →
how it's cleaned up), which is exactly the shape a reader (or an agent answering "what's the
deal with print batches") wants in one place. Splitting would force a reader chasing "why is
my batch stuck at Printed" to jump between two pages for one mental model.

**What should actually change instead — kill the duplication with `media-basics.mdx`.** That
page's own "Print Batches" H2 (lines 105–118) restates this page almost claim-for-claim:

> "Every print list you export becomes a batch... **Status** - Draft, Printing, Printed,
> Deployed. You can step back one stage while a batch is in progress, but Deployed is final...
> **Filtering** - filter a Tub's items by the batch they were printed in... **Archiving** -
> retire completed batches without deleting them."

This is the same content this page owns, maintained in two places that will drift (the
`CLAUDE.md` in this repo calls out exactly this failure mode — "the copies drifted"). Fix:
shrink `media-basics.mdx`'s "Print Batches" section to 1–2 sentences plus a link to this
page, the way its own "What you can track today" section already does two paragraphs above
it.

---

## 4. HEADINGS AS QUESTIONS

Proposed only where the noun-phrase heading is genuinely ambiguous or loses meaning in
isolation — not a blanket conversion.

- **"Status"** → **"What statuses does a batch move through?"** Worth changing: as a bare
  one-word heading, a chunk retrieved in isolation (e.g. a search snippet titled "Status")
  gives zero indication of *what* has a status. The question form fixes this for free.

- **"Archiving"** → **"How do I archive (or restore) a batch?"** Worth changing: bare noun
  loses the object ("archive what?") outside the page, and folding in "(or restore)" also
  patches the missing un-archive fact from §1.

- **"Batch details"** → **"What information can I record on a batch?"** Mild improvement —
  "details" as a heading doesn't signal that this is an editable list of fields (name, notes,
  tags, photo) versus read-only metadata.

- **"What is not tracked"** → **"What does QRtub not track about print batches?"** Worth
  changing: "not tracked" alone, isolated from the page, could be misread as "not tracked at
  all across QRtub" rather than "not tracked about media/batches specifically."

- Leave as-is: **"Creating a batch"**, **"Finding your batches"**, **"Tracking what is
  actually installed"**, **"The CSV is kept"**, **"Filtering Items by batch"**, **"Why some
  links cannot be deleted"** — these already carry enough of their own subject in the heading
  that a question-form rewrite would add words without adding clarity.

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

Treating every silence below as a defect, per the audit brief:

1. **Neutral-download path never mentioned** — see §1 missing piece #1. The page's own
   framing ("not a neutral download") is the thing that's wrong.
2. **5000-item cap on selected-ID export** — undocumented (`export-print-csv/route.ts`:
   `errorResponse('Too many IDs (max 5000)', 400)`).
3. **Whether a batch itself can be deleted** — undocumented. Only Draft batches can be
   hard-deleted (`ProtectedBatchError('Cannot delete a batch that is past draft. Archive it
   instead.')`), mirroring the link-level rule the page does state.
4. **Archiving is reversible, and available at any status** — undocumented; page's "Completed
   batches can be archived" reads as a stricter rule than the code enforces.
5. **Silent CSV-attach failure** — undocumented failure mode straight from the source's own
   comment ("Not transactional: if the CSV upload fails... the batch will be left with a null
   `csv_storage_path`"). This is precisely the kind of gap an AI support agent will paper over
   with an invented explanation when a user reports a batch with no downloadable file.
6. **Default/auto-generated batch name** — undocumented (`Print list — {date}`).
7. **"Bulk" deployment update is whole-batch-only, not subset-bulk** — the page says "Update
   them one at a time or in bulk" which a reader could take to mean "select several codes and
   bulk-update just those." The real UI (`PrintHistoryPanel.tsx`: `handleMarkAllDeployed`,
   `handleRetireAll`) only offers "mark **all** items in the batch deployed" or "retire
   **all**" — there is no partial-selection bulk action. Minor, but worth a precise word
   swap ("or mark every code in the batch at once") to avoid the wrong mental model.
8. **No plan-tier statement** — not a defect in substance (no gating exists in
   `stripe-plans.ts`), but the page never says so explicitly, leaving an AI agent asked "do I
   need a specific plan for this" with nothing to cite either way.

---

## 6. CHUNK INTEGRITY

Each H2 evaluated as if it were the *only* thing retrieved.

- **Creating a batch** — Stands alone. Uses only product nouns (Tub, Items, Print list) that
  are safe universal assumptions across this docs site, not references to earlier content on
  this page.

- **Finding your batches** — Stands alone.

- **Batch details** — Stands alone ("a batch" is a generic reference, not "the batch above").

- **Status** — Stands alone structurally (self-contained table), but see §4 — the *heading*
  itself doesn't say what has a status, which matters more for a retrieved snippet than for
  the body text.

- **Tracking what is actually installed** — Stands alone. The "500 stickers" / "run of five
  hundred" figures are internal to this section (introduced and paid off within the same
  block), not a dangling reference to a number mentioned elsewhere.

- **The CSV is kept** — Mostly stands alone: the heading itself supplies "CSV," so "the
  exported file" and "the same list" resolve without needing the "Creating a batch" section.
  Low-severity implicit dependency, not a hard failure.

- **Filtering Items by batch** — **Fails in isolation.** Opens with *"Back in the Tub,"* which
  is a literal callback to "Open a Tub, select the Items you want..." in the "Creating a
  batch" section two H2s earlier. A reader (or model) given only this section has no idea
  what "back" refers to. Fix: *"In a Tub's Items view, you can filter Items by the batch they
  were printed in..."* — same meaning, no dependency on prior sections.

- **Why some links cannot be deleted** — Mostly stands alone; leans on "Draft" being
  recognizable as a status name even without having read the Status table first, which is a
  fair bet since it's capitalized and self-explanatory.

- **Archiving** — **Weak in isolation.** "Completed batches can be archived" leaves
  "Completed" undefined within the section — does it mean status = Deployed? The reader has
  to infer, and per §1/§5 the inference would be wrong anyway (no status is actually
  required). Needs the antecedent-free rewrite proposed in §4/§5.

- **What is not tracked** — Stands alone; the link to Media Basics is a real relative link,
  not a vague "see above."

---

## Summary of required actions

1. **Rewrite warranted** — the export/batch-creation claim is materially inaccurate (missing
   the neutral-download path), which alone justifies a substantive edit rather than a
   word-level patch. Combined with the archiving-scope error, the missing deletion/cap/
   default-name facts, and the "Back in the Tub" chunk-integrity break, a full replacement
   draft has been written to
   `/workspace/mintlify-docs/audit/proposed/help__print-batches.md`.
2. **Separate action (not this file):** trim `help/media-basics.mdx`'s duplicated "Print
   Batches" section down to a pointer at this page.
