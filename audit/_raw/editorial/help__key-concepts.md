# Editorial Audit — /help/key-concepts

**File:** `/workspace/mintlify-docs/help/key-concepts.mdx`
**Live:** https://help.qrtub.com/help/key-concepts
**Nav group:** Help → Concepts (siblings: `help/print-first-workflow`, `help/media-basics`)

---

## 1. SELF-CONTAINMENT

Verdict: **Fails**, and not just on omission — the page states things about Media that are
not true of the shipped product. A cold reader relying only on this page would come away
with an inaccurate model of what QRtub tracks, and would go looking for features that don't
exist.

**Critical factual defect — Media claims contradict the shipped product.** The page's
"Media" H3 (lines 45–65) and "Why track Media separately" list state:

> "**Cost tracking** - A billboard costs $5,000. That's infrastructure worth managing."
> "**Durability** - Metal plaques last 10+ years. Vinyl stickers last 1-3 years."
> "**Replacement** - When Media is damaged, you can replace it with new Media linking to the same Link—so the Item connection is preserved and nothing needs reconfiguring."
> "**Inventory** - Track what's been produced, what's installed, what's in stock."
> "**Production** - Manage Media Batches from different print partners or production runs."

This reads as a list of things QRtub *does*. Its own sibling page, `/help/media-basics.mdx`
(same nav group), states the opposite for everything except batches:

> "There is no record of what an individual QR code is printed *on* — no material type, cost, durability or installation location per piece. These remain planned: Media as a distinct entity, with type and material per item / Media Templates / Media inventory tracking / Replacement workflows / Cost tracking and reporting."

This is also confirmed by this repo's own `CLAUDE.md`, which documents this exact drift as a
past mistake to guard against: *"Pages have previously documented ... Media type tracking. **None of these exist.**"* and *"Per-item Media is not [tracked]. There is no record of what an individual code is printed on — no material type, cost, durability or installation location, and no inventory."* Only **print batches** (production runs, not per-piece cost/durability/inventory) are real, confirmed against source (`/workspace/qrtub/src/app/app/media/page.tsx`, `src/lib/database/server-print-batches.ts`, `src/app/api/print-batches`).

The worked "Example" block (lines 70–91) compounds this — it shows a fabricated per-item
Media record:

```
MEDIA: Metal Plaque #4729
- Type: Stainless steel, engraved
- Cost: $75
- Installed: March 2025, left cab door
- Batch: #47 (500 pieces, PrintCo)
```

`Type`, `Cost`, and `Installed` are presented as if they're stored fields on a Media record.
Only the Batch reference is real. This is precisely the pattern `CLAUDE.md` calls out by
name ("Media type tracking. **None of these exist.**").

**Other missing pieces a cold reader would need:**
- **No plan-tier gating stated anywhere.** Tubs, custom fields, Page Mode, URL Templates,
  conditional visibility — the page never says whether any of these require a paid plan.
- **No mention of what happens to a Link when its Item is deleted.** The sibling page
  `print-first-workflow.mdx` states this clearly ("Deleting an Item does not delete its
  Link — the Link is released back to your unassigned pool"), but `key-concepts.mdx`, which
  is the terminology home page and the one most likely to be retrieved for "what happens to
  my Link if I delete the Item" — says nothing.
- **No mention of what happens when someone scans an unconnected Link.** The
  "Print-Before-Link Workflow" section here describes the workflow's steps but never states
  the outcome of the in-between state, whereas `print-first-workflow.mdx` answers it
  directly ("an unconnected code does not 404... Anyone else gets a neutral branded page
  rather than an error"). A retrieval system that surfaces only this page for a
  print-before-link question will not find that answer.
- **No mention of empty/missing field behavior in URL Templates.** The page correctly warns
  that unencoded special characters break links (verified accurate — matches
  `CLAUDE.md`/`bindings.ts`), but never states what happens when the referenced field is
  empty (per `CLAUDE.md`: "A missing or empty field inserts an empty string").

## 2. ANSWER-FIRST

Every H2, quoted verbatim, with a judgment:

**"## The Three-Entity Model"** (line 8) — opens:
> "Most QR code systems treat everything as one thing. QRtub recognises that physical QR code deployments involve three distinct entities, each with its own lifecycle:"
Judgment: **Partial.** Leads with a competitor-framing sentence ("Most QR code systems...")
before naming the actual answer. Direct version: "QRtub models three distinct entities —
Item, Link, and Media — each with its own independent lifecycle."

**"## Example"** (line 68) — opens with a fenced code block, **zero words of lead-in text.**
Judgment: **Fails.** There is no sentence at all framing what the example demonstrates. A
reader (or an AI agent) arriving at this heading in isolation has no idea what concept is
being illustrated.

**"## Link Modes"** (line 97) — opens:
> "Links can operate in two modes:"
Judgment: **Good.** Six words, directly answers "what are the link modes."

**"## Destinations"** (line 131) — opens:
> "**What they are:** Where users end up when they interact with a Link."
Judgment: **Good.** Direct bolded Q&A, immediate answer.

**"## Tubs"** (line 179) — opens:
> "**What they are:** Category-based workspaces for organising Items."
Judgment: **Good.** Direct.

**"## Print-Before-Link Workflow"** (line 204) — opens:
> "**The traditional problem:**" followed by a 4-item numbered list, then "**The QRtub solution:**" and another 4-item list.
Judgment: **Fails.** This is pure problem/solution scene-setting — the section never states
in one sentence what "print-before-link" *is*; the definition has to be inferred from two
stacked numbered lists (~70 words) before any direct statement appears.

**"## Update Without Reprinting"** (line 222) — opens:
> "**The core benefit:** Change where your QR codes point at any time without touching the physical codes."
Judgment: **Good**, though short (17 words vs. the 40-60 target) — direct, no preamble.

**"## Integration Layer"** (line 236) — opens:
> "**What QRtub is:** A connection layer between your physical items and your digital systems."
Judgment: **Good.** Direct.

**"## Common Questions"** (line 252) — opens directly into the first Q&A pair; this is a
FAQ container rather than a single-answer section, so the "answer-first" test applies
per-question rather than to the H2 as a whole. Each individual Q&A does answer immediately
(e.g. "A: Regular generators hardcode URLs into QR codes...") — see Section 6 for why the
Q&As should be real headings rather than bold text.

**"## Next Steps"** (line 271) — a link list; not a content section, answer-first doesn't
apply.

**Summary:** 2 of 9 content H2s (Three-Entity Model, Print-Before-Link Workflow) open with
preamble instead of a direct answer, and one (Example) opens with no framing sentence at
all.

## 3. ONE QUESTION PER PAGE

**Verdict: this page answers at least six distinct questions**, several of which duplicate
content that already lives — in more depth and, in one case, more *accurately* — on sibling
pages in the same "Concepts" group or elsewhere in the nav:

1. What is the three-entity model (Item/Link/Media)? — core, belongs here.
2. What are Link Modes (Direct vs. Page)? — core, belongs here.
3. What are Destinations, and how do URL Templates work? — the "URL Templates" H3
   (lines 142–176) substantially duplicates `/help/using-fields.mdx` ("Using Fields in
   Pages"), which is the dedicated, deeper reference for field-binding syntax
   (`{{item.field}}`), available fields, and both URL Templates and conditional visibility.
4. What is a Tub? — core, belongs here.
5. What is the print-before-link workflow? — this **fully duplicates**
   `/help/print-first-workflow.mdx`, a sibling page in the same "Concepts" nav group. That
   page covers the same workflow in more depth, with a diagram, and — critically — it
   answers the unconnected-scan edge case that this page omits. `CLAUDE.md` explicitly
   instructs: *"Search for existing information before adding new content. Avoid
   duplication unless strategic."* This is unstrategic duplication: two Concepts-group
   siblings independently explain the same workflow, and the shorter one is missing the
   more important edge-case content.
6. "Update Without Reprinting" — restates the same underlying benefit already covered
   under Link's "Why this matters" bullets (lines 37–41) and again inside the
   Print-Before-Link section. Three separate re-statements of "you can reassign a Link
   instead of reprinting" appear on this one page.
7. "Integration Layer" (what QRtub is/is not) — positioning content, not a "concept you
   need to complete a task." Legitimate to keep briefly (support bots do get asked "does
   QRtub replace my CMMS"), but doesn't need a full H2 with four bullets when a single
   paragraph answers it.
8. A general FAQ catch-all.

**Proposed split:**
- **Keep on this page** (it is legitimately the terminology/glossary primer): Three-Entity
  Model, Example, Link Modes, Destinations (short), Tubs.
- **Cut "Print-Before-Link Workflow" down to a short pointer** (2–3 sentences + link) to
  `/help/print-first-workflow.mdx`, which already owns this topic and does it better,
  including the unconnected-scan behavior this page lacks.
- **Cut "URL Templates" down to a short pointer** to `/help/using-fields.mdx`, which already
  owns the full field-binding reference. Keep only the encoding-safety warning inline since
  it's a genuine gotcha worth surfacing at the point a reader first meets URL Templates.
- **Fold "Update Without Reprinting" into the Link entity's existing bullets** — it's the
  same claim restated as its own H2; no new information is added.
- **Shrink "Integration Layer"** to one short paragraph — keep the verified "what QRtub is
  NOT" bullets since they directly prevent an AI agent from wrongly claiming QRtub replaces
  a customer's CMMS/inspection tool, but the current 4-bullet + "think of it as" framing is
  more than the question needs.

The page is not too thin to stand alone — the opposite problem applies: it should shed
weight to siblings that already own that content, not gain more.

## 4. HEADINGS AS QUESTIONS

Rewrites proposed only where genuinely clearer for retrieval:

| Current heading | Proposed | Rationale |
|---|---|---|
| "The Three-Entity Model" | "What are QRtub's three entities: Item, Link, and Media?" | Names the actual answer terms, matches how a support bot would phrase the incoming question. |
| "Example" | *(no question form — needs a lead-in sentence, not a heading rewrite)* | The heading is fine as a label; the defect is the missing framing sentence (see §2, §6), not the heading itself. |
| "Link Modes" | "What are Direct Mode and Page Mode?" | Names both terms explicitly; "Link Modes" alone doesn't tell a retrieval system what the two modes are called. |
| "Destinations" | "What is a Destination?" | Direct singular-question form matches the section's own "What they are:" opening. |
| "URL Templates" | "How do URL Templates work?" | Clearer intent signal than a bare noun phrase. |
| "Tubs" | "What is a Tub?" | Matches the section's own "What they are:" opening. |
| "Print-Before-Link Workflow" | "What is the print-before-link workflow?" (if kept at all — recommend trimming to a pointer per §3) | — |
| "Update Without Reprinting" | "Can I change where a QR code points without reprinting it?" | This is exactly the question a user or support bot would ask; the current noun phrase requires already knowing the feature name. |
| "Integration Layer" | "Does QRtub replace my other software?" | This is the actual question the section answers (its own bullets are literally "What QRtub is NOT"); the current heading is abstract jargon a reader wouldn't search for. |
| "Common Questions" | *(leave as-is — it's already a FAQ container)* | Fine as a section label; the individual Qs inside should become real headings (see §6), not the H2 itself. |
| "Next Steps" | *(leave as-is)* | Standard nav-section label, not a question. |

## 5. EDGE CASES / LIMITS / FAILURE MODES

Treating absence as a defect, per the brief:

1. **Media overclaim (critical, covered fully in §1).** The page presents per-item cost
   tracking, durability tracking, and inventory as available capabilities. None of these
   exist. An AI support agent asked "can I track how much each plaque cost" would, if it
   retrieved this page, confidently answer yes — which is false. This is the single most
   important defect on the page.
2. **No plan/tier information anywhere on the page.** Not one sentence states whether Tubs,
   custom fields, Page Mode, URL Templates, or conditional visibility require a specific
   plan tier, or what a free/entry tier includes vs. excludes.
3. **No statement of what happens on an unconnected-Link scan.** Covered in §1/§3 — the
   page describes the print-before-link *steps* but not the *behavior* during the gap.
4. **No statement of what happens to a Link when its Item is deleted.** Covered in §1.
5. **No statement of empty/missing-field behavior in URL Templates.** The page states the
   encoding gotcha (verified accurate) but not what happens when the field itself has no
   value.
6. **No caps or ceilings anywhere** — no mention of limits on number of Links, Tubs, custom
   fields per Tub, or Destinations per Page. Contrast with the sibling `media-basics.mdx`,
   which is careful to state boundaries explicitly ("Cost allocation per batch is not
   available," "There is no partner programme yet").
7. **Destinations section doesn't flag that External URLs is presently the only type.**
   The heading "Current Destination Types:" (line 139) implies more exist or are coming,
   but the page never says so one way or the other — leaving an AI agent to guess whether
   asking about a "form Destination" or "payment Destination" means a real feature or not.
8. **The FAQ doesn't cover the two biggest failure-mode questions** a support bot would
   actually receive: "what happens if a code is scanned before it's connected" and "what
   happens to my QR code if I delete the item it was assigned to." Both are answered
   elsewhere in the docs (print-first-workflow.mdx) but not here.

## 6. CHUNK INTEGRITY

Each H2 evaluated as if retrieved completely alone, no surrounding page:

- **"The Three-Entity Model"** — **Self-contained.** Defines all three entities inline via
  H3s with examples. Holds up in isolation.
- **"Example"** — **Fails in isolation**, in two ways. First, structurally: the heading is
  followed immediately by a code fence with no lead-in sentence, so a reader who lands only
  here has no idea what concept the block illustrates (three-entity model? Link Modes?
  something else?). Second, it has a **forward reference**: the block includes `Mode: Page`
  (line 80), but "Link Modes" — the section that defines what Page/Direct mode means — is
  the *next* H2 (line 97), appearing *after* this one. A reader (human or chunked-retrieval
  agent) hitting "Example" in isolation encounters an undefined term.
- **"Link Modes"** — **Self-contained.** Defines both modes with H3s and examples.
- **"Destinations"** — **Partially dependent.** The lines "**In Direct Mode:** The Link has one Destination and immediately redirects." and "**In Page Mode:** The Page displays multiple Destinations..." (lines 135, 137) use "Direct Mode" and "Page Mode" as already-known terms. Retrieved alone, a reader gets the terms used but not defined — they were defined two sections earlier.
- **"Tubs"** — **Self-contained.**
- **"Print-Before-Link Workflow"** — **Self-contained** in the narrow sense that it
  re-explains "Links exist independently from Items" inline rather than assuming the reader
  remembers it from the Link H3 above — no "this" or "the above example" back-references.
- **"Update Without Reprinting"** — **Self-contained.** No back-references.
- **"Integration Layer"** — **Self-contained.**
- **"Common Questions"** — **Structurally weak for chunk retrieval**, though each individual
  Q&A reads fine on its own. The problem is that all five Q&A pairs sit inside one H2 with no
  H3/H4 per question, so a heading-based chunker returns the *entire* FAQ block for any single
  question, and a character-count-based chunker risks cutting an answer mid-sentence. One
  answer — "A: When you replace damaged Media with new Media, both encode the same Link. The
  Item connection stays intact..." — uses "Media" and "Link" as already-defined terms;
  fine within the full page, but if a chunker split mid-FAQ, this Q&A alone still makes sense
  since both terms are used in their plain-English sense.
- **"Next Steps"** — Pure navigation; contains no content of its own, so "isolation" doesn't
  apply in the usual sense, but a retrieval hit landing here returns nothing informative.

**Net: 3 of 9 content sections have a chunk-integrity problem** — "Example" (undefined
forward reference + no framing), "Destinations" (backward reference to undefined terms),
and "Common Questions" (no per-question headings).

---

## Recommendation

A substantive rewrite is warranted — not a tweak. The Media-tracking overclaim is a
correctness defect matching a failure mode this repo's own `CLAUDE.md` explicitly warns
against by name, and the page separately has structural problems (duplicate content across
three areas, a heading-less Example section with a forward reference, non-heading FAQ
entries) that compound the retrieval-quality issues. A proposed replacement has been
written to `/workspace/mintlify-docs/audit/proposed/help__key-concepts.md`.
