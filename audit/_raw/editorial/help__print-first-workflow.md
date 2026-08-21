# Editorial audit — The Print-First Workflow

- File: `/workspace/mintlify-docs/help/print-first-workflow.mdx`
- Live: https://help.qrtub.com/help/print-first-workflow
- Nav group: **Concepts** (`docs.json`), siblings: `help/key-concepts`, `help/media-basics`
- Verified against: `../qrtub/src/lib/database/server-access-urls.ts`, `server-print-batches.ts`,
  `lib/types/link-generation-config.ts`, `components/blocks/UnallocatedLinkPage/UnallocatedLinkPage.tsx`,
  `components/blocks/CreateAccessLinkForm/CreateAccessLinkForm.tsx`, `lib/stripe-plans.ts`,
  `app/[id]/page.tsx`, `app/api/access-links/{generate,export-print-csv}/route.ts`, `../qrtub/GLOSSARY.md`,
  `../qrtub/CLAUDE.md`

---

## 1. SELF-CONTAINMENT

The task the page describes ("order tags in bulk, apply as gear arrives, connect when ready") is
conceptually complete, but a reader who cannot follow a link is missing the pieces needed to
actually *do* it:

- **No UI path for step 1.** "**1. Generate the Links first.** Create as many as you need — a
  hundred, a thousand — before any Items exist." is the entire instruction. It never says where
  in the app this happens. Compare the sibling procedural page `help/creating-your-first-link.mdx`,
  which spells it out ("From your dashboard, click **Links**... Click **Generate Links**...
  **Bulk Links** - Generate multiple Links for professional printing") — none of that is here, and
  it isn't even linked inline in this section (only in the bottom "Related" list, after the reader
  has already left the topic).

- **No UI path for step 2, and a real inconsistency with the page it points to.** "**2. Send the
  batch to be produced.** Export the list and give it to whoever makes your tags." Export *how*?
  The only sibling that documents exporting a batch is `help/print-batches.mdx`, and its entire
  documented procedure is: "Open a **Tub**, select the **Items** you want, and choose **Print
  list** from the menu." That requires Items to already exist and be selectable in a Tub — which
  directly contradicts this page's premise that step 1 happens "before any Items exist." Verified
  in source: `app/api/access-links/export-print-csv/route.ts` accepts an account-level `teamId`
  and an `assigned` filter (i.e., it *can* export pure, item-less Links), and the account-level
  Links page (`app/app/access-link/page.tsx`) calls this same endpoint — so the capability
  exists, but neither this page nor `print-batches.mdx` documents that path. A reader following
  only these two pages would hit a wall trying to export a batch of freshly generated, unassigned
  Links.

- **No mention of any ceiling.** The page implies unlimited scale ("Create as many as you need
  — a hundred, a thousand"). Two real limits are never mentioned:
  - Link **generation** is capped at 1,000 per request — confirmed in
    `server-access-urls.ts`: `const count = Math.min(options.count || 1, 1000); // Max 1000 per
    request`, and echoed in the UI (`CreateAccessLinkForm.tsx`: `"Range too large... Maximum 1000
    numbers per request."`). For "a thousand" this is exactly the ceiling; for anything larger
    the reader needs to know to repeat the request, which nothing here says.
  - Every plan caps **total active Links** — `lib/stripe-plans.ts`: Starter "Up to 100 active
    links", Professional "Up to 1,000 active links", Scale "Up to 10,000 active links". Since
    `is_active` defaults to `true` on creation regardless of whether a Link is assigned to an
    Item (`server-access-urls.ts` lines 365/433/491/534/613: `is_active: options.isActive ??
    true`), print-first Links sitting unassigned in the pool count against this cap. A reader on
    Starter who takes "create as many as you need — a hundred, a thousand" at face value could
    order 1,000 plates and then discover their plan only supports 100 active Links.

- **No caution about the URL-template example.** The final section's code block
  (`https://example.com/assets/{{item.serial_number}}`) is exactly the pattern flagged in
  `CLAUDE.md` and documented on the sibling page `help/key-concepts.mdx`: "Values are inserted
  **exactly as stored**. QRtub does not URL-encode them, so a field containing a space, `&`, `?`
  or `#` will produce a broken link." This page repeats the same template mechanism but omits
  the warning entirely — a reader who binds a field with a space in it (e.g. a free-text
  location) gets a broken link with no explanation on this page of why.

- **Terminology drift from the glossary.** `GLOSSARY.md`'s canonical term for this exact
  workflow is **"Print-before-link"** ("Workflow: print Links on QR codes first, connect to
  Items later"), and the sibling `help/key-concepts.mdx` uses it consistently (`## Print-Before-
  Link Workflow`, "This is the 'print-before-link' workflow"). This page never once uses that
  term — its title and every reference call it "print-first." A reader (or an AI agent) who has
  learned the product's own vocabulary from `key-concepts.mdx` and then searches for
  "print-before-link" won't obviously connect it to this page's title.

**Verdict:** Not self-contained for execution — it explains the *why* and *what* well, but a
reader with no other page open cannot find the buttons, doesn't know the ceilings, and would hit
an undocumented gap trying to export a batch of pure Links.

---

## 2. ANSWER-FIRST

Quoting the actual opening sentence(s) of every H2:

| H2 | Opening text | Verdict |
|---|---|---|
| Why the usual order does not survive contact with a site | "Durable identification is made in runs. Photo anodised aluminium plates are laid up and cut as a sheet. Engraved tags are set up once and produced as a batch. Even ordinary UV-resistant vinyl comes with minimum quantities and a lead time." | **Preamble, not answer.** These three sentences are scene-setting about how manufacturing works. The actual answer to "why doesn't the usual order survive" — "you physically cannot produce one tag on demand at any sensible cost" — doesn't land until the third paragraph, ~90 words in. |
| How it works | "**1. Generate the Links first.** Create as many as you need — a hundred, a thousand — before any Items exist. Each one is a real, resolvable URL from the moment it is created." | **Answer-first.** Goes straight into step 1 with a concrete claim, no throat-clearing. ~28 words. |
| What happens if someone scans a tag early | "This is the question that usually decides whether the workflow is practical, and the answer matters: **an unconnected code does not 404.**" | **Mostly answer-first**, but padded. The clause "This is the question that usually decides whether the workflow is practical, and the answer matters" (18 words) is commentary-about-the-question before the actual answer ("an unconnected code does not 404," 6 words). Cut the first clause and it's a perfect 40-60-word open once the two follow-on bullets are counted. |
| Getting the mistakes back | "Tags get put on the wrong machine. Gear gets sold. A Link can be reassigned to a different Item at any point, so a mistake costs an edit rather than a reprint." | **Borderline.** Two short problem-statements (8 words) precede the actual mechanism. It's tight enough (33 words total) that it reads fine, but the heading asks "how do I get the mistake back" and the answer only starts at sentence 3. |
| Things worth doing while you are at it | "**Print the link in text as well as the code.** Codes get scratched, painted over, caked in mud, or scanned in bad light on a cracked screen." | **Answer-first per bullet.** This H2 isn't a single question — it's a list of independent tips, and each bolded lead-in *is* the answer to its own implicit sub-question ("what's worth doing #1"). Judged as a list, this works; judged as a single 40-60-word H2 answer, there isn't one because there are four separate answers. |
| Where the codes point | "A Link can go straight to a single destination, or open a Page with several options. Either way you set it up once for the whole category rather than per tag, using a template that fills in each Item's own data:" | **Answer-first.** Directly answers "where do the codes point" in the first sentence, ~40 words to the colon. Good. |

**Summary:** 3 of 6 substantive H2s open with real preamble or throat-clearing before the answer
(the "why" section worst offender at ~90 words of setup); the "how it works" and "where the
codes point" sections are genuinely answer-first.

---

## 3. ONE QUESTION PER PAGE

This page answers **at least five distinct questions**, not one:

1. Why doesn't order-record-then-print work at scale? (conceptual/motivational)
2. How do I run the print-first process end to end? (procedural, 4 steps)
3. What happens if a tag is scanned before it's connected? (behavioural/edge-case FAQ)
4. How do I fix a mislabeled tag, or retire one? (behavioural/edge-case FAQ — reassignment +
   delete-releases-link)
5. What's good practice when ordering a run? (a 4-item tips list: readable text, matching tag
   number to slug, matching media to environment, over-ordering)
6. How do Links resolve to different data per tag (URL templates)? (a distinct technical
   mechanism, already covered in more depth on `help/key-concepts.mdx` under "URL Templates" and
   on `help/using-fields.mdx`)

This matters for retrieval: an AI agent asked narrowly "what happens if a tag gets scanned before
it's assigned in QRtub" would retrieve this entire page — including the manufacturing-run
motivational essay and the URL-template code sample — to answer a question that only needs two
paragraphs.

**Proposed split:**

- **Keep on this page** (rename target: align title with the glossary's "Print-before-link,"
  see §1): the motivational "why" section + the 4-step "How it works" procedure. This is the
  coherent core concept.
- **Move "What happens if someone scans a tag early" and "Getting the mistakes back"** into
  `help/key-concepts.mdx`, which already has a `## Print-Before-Link Workflow` section and a
  `## Common Questions` FAQ block — these two sections are natural additions there, next to the
  existing "Q: What's the difference between a Link and a QR code?" style entries, rather than a
  second, differently-named page.
- **Move "Things worth doing while you are at it"** into `help/print-batches.mdx` (the
  Printing-group page about running a print batch) as a "Before you export a batch" or "Tips"
  section — it's about ordering/labelling decisions at print time, not about the print-first
  *concept*.
- **Drop "Where the codes point"** from this page entirely, or reduce it to one linking sentence
  — it duplicates the "URL Templates" section already on `help/key-concepts.mdx` (near-identical
  claim: "Values are inserted exactly as stored... does not URL-encode them") and is the actual
  subject of `help/using-fields.mdx`. Right now three pages explain the same template mechanism
  at three different depths with no single source of truth.

This page is not "too thin" — the reverse problem (too many questions bundled) is the one to fix.

---

## 4. HEADINGS AS QUESTIONS

| Current heading | Implicit question | Propose rewrite? |
|---|---|---|
| Why the usual order does not survive contact with a site | "Why can't I just print a tag whenever I need one?" | **Yes** — rewrite to `## Why can't a single tag be printed on demand?` The current heading is vivid but is a *statement*, and doesn't surface for a query phrased as a question, which is exactly how a support bot or a user would phrase it. |
| How it works | "How does the print-first workflow work?" | **No** — "How it works" is already unambiguous in context (the page is *about* the print-first workflow) and a full question form ("How does the print-first workflow work?") is only marginally clearer. Leave as-is. |
| What happens if someone scans a tag early | (already a question) | **No** — already phrased as the implicit question almost verbatim; just needs a `?`. Minor: `## What happens if someone scans a tag early?` |
| Getting the mistakes back | "What happens if a tag ends up on the wrong asset, or the asset is retired?" | **Yes** — "Getting the mistakes back" is a vague, idiomatic noun phrase that doesn't hint at *what* mistake or *how*. Rewrite to `## What if a tag ends up on the wrong item, or the item is gone?` — this also matches the two concrete scenarios the section actually covers (misassignment, deletion). |
| Things worth doing while you are at it | — | **No** — this is a list of independent tips, not one question; forcing it into "What should I do before ordering a print run?" would flatten four distinct pieces of advice into one heading that doesn't help a reader (or retriever) find any specific one of them. If kept as one page, better to give each tip its own H3 (they're already bolded as if they were) so each is independently addressable. |
| Where the codes point | "Where does a Link point, and can one template cover a whole batch?" | **Marginal — recommend dropping the section instead (see §3)** rather than rewriting the heading, since the content duplicates other pages. If retained, `## Where does a Link point?` is a cleaner question-form than the current noun phrase. |

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

Concrete gaps found (each verified against source):

- **No generation ceiling stated.** Confirmed hard cap: `Math.min(options.count || 1, 1000)` in
  `server-access-urls.ts` — every generate request is capped at 1,000 Links, enforced
  server-side and surfaced client-side ("Maximum 1000 numbers per request"). The page's own
  example number ("a hundred, a thousand") sits right at this ceiling and never says so, or that
  a bigger run means multiple requests.
- **No plan/tier ceiling stated.** `lib/stripe-plans.ts` sells "Up to 100 active links" (Starter),
  "Up to 1,000" (Professional), "Up to 10,000" (Scale) — a directly relevant fact for a page whose
  entire pitch is "generate a hundred, a thousand" Links up front. Not mentioned anywhere on the
  page, nor is there a pointer to `qrtub.com/pricing` for this specific detail (the page's closing
  CTA links to pricing, but generically, not in the context of "here's the ceiling this workflow
  runs into").
- **No mention of the export/print-batch gap for pure Links.** See §1 — the only documented
  export procedure (`print-batches.mdx`) requires selecting Items in a Tub, which is impossible
  for Links generated before Items exist. This is the single biggest edge case the page is silent
  on, precisely because it's the hinge point of the entire workflow it's describing.
- **No URL-encoding caveat on the template example**, despite the sibling page carrying it
  verbatim for the identical mechanism (`key-concepts.mdx`: "Values are inserted **exactly as
  stored**... a field containing a space, `&`, `?` or `#` will produce a broken link"). This
  page's own example field, `item.serial_number`, is a plausible source of exactly that failure
  if a serial number ever contains a space or slash.
- **"A single action" is asserted, not qualified.** "Create the Item and connect it to the tag
  already on it... it is a single action" — creating an Item still means filling in whatever
  fields the Tub requires; "single action" describes the *connect* step, not the full Item-creation
  effort. Not factually wrong, but a reader could take it to mean zero data entry.
- **No failure mode for a mis-scanned or damaged code** (can't scan at all, code printed wrong,
  wrong tag applied to the wrong batch of gear) — only the "wrong machine" and "asset sold/retired"
  cases are covered. Nothing about what happens if the printer's CSV drifts from what's actually on
  the plates, which is exactly the scenario `help/print-batches.mdx` was written to solve ("The
  exported file stays with the batch, so a reprint uses exactly the same list") — worth at least a
  cross-reference at the point of the "mistakes" section, not just in "Related."
- **No mention of Direct vs Page mode limits.** "Where the codes point" says "A Link can go
  straight to a single destination, or open a Page with several options" but doesn't say a Link
  can only be in one mode/point to one Item at a time (this exists as an explicit FAQ answer on
  `key-concepts.mdx`: "Can one Link connect to multiple Items? A: No.") — reasonable to omit here
  if the section is cut per §3, but if kept, worth stating since a reader could otherwise assume a
  single print-first Link could later fan out to several Items.

---

## 6. CHUNK INTEGRITY

Testing each H2 in isolation, as if only that section were retrieved:

- **"Why the usual order does not survive contact with a site"** — Mostly self-contained, but
  the image caption depends on the image: "Above: a run of photo anodised aluminium plates being
  cut." **"Above"** refers to the `![...]` image immediately preceding it in the full page; in an
  isolated text chunk (most chunkers strip or separate image alt-text from surrounding prose) this
  reads as a dangling reference to nothing. The final sentence, "Print-first accepts both realities
  instead of fighting them," also depends on "both realities" defined two paragraphs earlier (the
  batch-manufacturing reality and the gear-arrives-over-weeks reality) — in isolation, "both"
  has no antecedent.
- **"How it works"** — Self-contained. Each numbered step is independently readable and doesn't
  lean on prior sections. Good chunk.
- **"What happens if someone scans a tag early"** — Self-contained. Opens by restating its own
  question, defines "unconnected code," and both bullets make sense without anything above.
  Minor: "So a tag applied on Monday and connected on Friday is not a dead code in the meantime"
  invents a Monday/Friday example that isn't grounded elsewhere, but doesn't depend on prior text
  to parse — fine as a standalone illustration.
- **"Getting the mistakes back"** — Mostly self-contained, but "The same applies at the end of an
  asset's life" implicitly assumes the reader already has "a mistake costs an edit rather than a
  reprint" in mind from the paragraph above — in isolation it reads fine, but "the same" is doing
  a small amount of unexplained cross-paragraph work. Passable.
- **"Things worth doing while you are at it"** — Each bolded tip is independently sensible in
  isolation. The heading itself ("while you are at it") implies "while doing [something described
  earlier]" — in true isolation, a reader doesn't know *what* they're "at" (ordering a print run).
  Minor context loss, but the tips themselves don't require it to be useful.
- **"Where the codes point"** — Self-contained as written, though it silently assumes the reader
  already knows what "a Link" and "an Item" are (no definition on this page) — acceptable only
  because this page's audience already read `key-concepts.mdx` first, which this page never
  states as a prerequisite.

---

## Additional cross-cutting notes (outside the six categories)

- **Redundancy with `help/key-concepts.mdx`:** the "How it works" 4-step list here
  (Generate → Send to produce → Apply as gear arrives → Connect) is near-identical in structure
  and content to `key-concepts.mdx`'s own `## Print-Before-Link Workflow` section ("1. Generate
  Links... 2. Professional printing... 3. Field deployment... 4. Connect later"). Two pages
  currently teach the same 4-step process under two different names ("print-first" vs.
  "print-before-link"). Consolidating per the §3 split would remove this duplication.
- **Terminology:** this page never uses "print-before-link," the glossary's canonical term for
  this exact workflow (`GLOSSARY.md`). Recommend at minimum a one-line alias note near the top so
  a reader arriving from `key-concepts.mdx`'s vocabulary recognizes it's the same thing.
