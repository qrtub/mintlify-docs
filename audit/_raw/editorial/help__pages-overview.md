# Editorial audit — Pages Overview

**File:** `/workspace/mintlify-docs/help/pages-overview.mdx`
**Live:** https://help.qrtub.com/help/pages-overview
**Nav group:** Help tab → "Pages" (`help/pages-overview`, `help/building-a-page`, `help/using-fields`,
`help/conditional-visibility`, `help/device-detection`, `help/app-links` — per `docs.json`)
**Siblings skimmed for overlap:** `help/building-a-page.mdx`, `help/key-concepts.mdx`,
`help/creating-your-first-link.mdx` (Getting Started group), `help/conditional-visibility.mdx`

---

## 1. Self-containment

**Task implied by the page:** understand what a Page is, and be able to create one.

The conceptual half (what a Page is, Page Mode vs Direct Mode, the benefits) is genuinely
self-contained. The procedural half is not — "Creating a Page" gives four steps but only
step 1 (the Tub toggle) is actually explained on this page:

```
1. In the Tub's settings, turn on the page option (labelled "Show a profile page" in the
   current app)...
2. Create an Item in the Tub
3. Assign a Link to the Item
4. Add Destinations to route people to the systems they need
```

Steps 2–4 name actions with zero mechanics — no menu location, no button name, no field list.
A cold reader cannot complete the task from this page alone. Specific missing pieces:

- **How to create an Item** and **how to generate/assign a Link** — this is the entire
  content of `help/creating-your-first-link.mdx` (Navigate to Links → Generate → Choose Link
  Type → Print or Connect), which this page does not link to anywhere, not even in "Related."
- **How to add a Destination / build the page layout** — this is the entire content of
  `help/building-a-page.mdx` (Page Editor, Components/Data/Structure panels, section types,
  bindings), also absent from "Related."
- **What a "Destination" is.** The term is used 9 times on the page (bullets, table, "Key
  Benefits," "Creating a Page") but never defined. The reader has to infer it from the three
  bullet examples ("Start Inspection" → Mitti, etc.). A cold AI-agent reader retrieving only
  this chunk cannot answer "what is a Destination" from it.
- **What a "Link" is.** Same problem — "Assign a Link to the Item" assumes the reader already
  knows Links exist, have three URL types (random/ID-based/custom), and are generated
  separately from Items. None of that is here or linked.

The frontmatter description promises "how Destinations fit together" — the page doesn't
really deliver on that clause; it uses the term but never explains the fitting-together.

**Verdict: not self-contained for the procedural task.** Fine for the conceptual question
("what is a Page, should I use Page Mode").

---

## 2. Answer-first

**Lead paragraph (before any H2):** "Pages turn a single QR code into a multi-Destination
landing page. Instead of one QR code per system, create one code with multiple options." —
direct, answers "what is this page about" in 24 words. Good.

**H2 "What is a Page?"** — opening sentence: *"When someone scans a QR code linked to a Page,
they see a mobile-friendly page with multiple Destinations:"* (23 words, then a bulleted
example). This is a scenario framing ("When X happens...") rather than a definition
("A Page is..."). It never states the definition directly under its own heading — the actual
definition ("Pages turn a single QR code into a multi-Destination landing page") lives in the
lead paragraph *above* this H2, so a chunk containing only this section's content lacks a
plain declarative definition. **Partial** — informative, but not a direct answer to "what is
a Page" in isolation.

**H2 "Page Mode vs Direct Mode"** — no prose opens the section at all; the very first thing
under the heading is the markdown table. There is no topic sentence before it (e.g. "A Link
runs in one of two modes:"). The table itself does answer the comparison directly (arguably
the most answer-first construct on the page), but a heading with zero lead-in prose means an
AI agent snippet-previewing the section (many show only the first sentence) would surface
nothing but a table with no framing. **Answer-first via structure, but missing a topic
sentence.**

**H2 "Key Benefits"** — no opening sentence either; goes straight to three bold sub-headers
("One Code, Every System", "Audience Routing", "Professional Presentation"), each with its own
1–2 sentence blurb. There's no lead sentence answering "what are the benefits" as a set before
diving into the three. Also skews toward marketing phrasing ("Every scan is a branded
experience") that CLAUDE.md's writing standards ask to avoid ("no empty superlatives").
**Not answer-first — starts with sub-structure, no framing sentence.**

**H2 "Creating a Page"** — opening sentence: *"Pages are switched on per Tub, not per Link:"*
(8 words). This is the best answer-first sentence on the page — it states the single most
important, most-often-misunderstood fact (scope: Tub, not Link) immediately, then the list
follows. **Good.**

**H2 "Related"** — a link list, not a content section; answer-first doesn't apply.

---

## 3. One question per page

This page is doing at least three distinct jobs:

1. **Definitional** — what is a Page (H2 1).
2. **Comparative/decision** — Page Mode vs Direct Mode, when to use each (H2 2 + 3).
3. **Procedural** — how to turn one on (H2 4).

The comparative section (H2 2, "Page Mode vs Direct Mode") is a **near-duplicate** of content
that already lives on `help/key-concepts.mdx` under "## Link Modes," which has its own
"Direct Mode" / "Page Mode" subsections and — this is the striking part — **the same worked
example**:

- `key-concepts.mdx`: *"'Start Inspection' → Mitti (formerly SafetyCulture)" / "'Log
  Maintenance' → Your CMMS" / "'Operator Manual' → PDF documentation" / "'Contact Support' →
  Support form"*
- `pages-overview.mdx`: *"'Start Inspection' → Opens Mitti (formerly SafetyCulture)" / "'Log
  Maintenance' → Opens CMMS system" / "'View Manual' → Opens documentation"*

`key-concepts.mdx` is also the page that `help/creating-your-first-link.mdx` links to for this
exact comparison ("Understand [Direct Mode vs Page Mode](/help/key-concepts#link-modes)") —
i.e., the rest of the docs already treat Key Concepts as canonical for this comparison, while
Pages Overview independently re-explains it. This is exactly the duplication CLAUDE.md's
"Content strategy" section asks to avoid ("Search for existing information before adding new
content. Avoid duplication unless strategic").

**Proposed split:**

- Keep on this page: the definition of a Page (H2 1) and the practical "how do I turn one on"
  procedure (H2 4), tightened and cross-linked to the pages that actually carry the mechanics
  (`creating-your-first-link.mdx`, `building-a-page.mdx`).
- Either **cut** the "Page Mode vs Direct Mode" table and defer entirely to
  `key-concepts.mdx#link-modes`, or keep a *short* version here (since a reader landing on
  "Pages Overview" cold shouldn't have to leave to learn the one comparison this whole page is
  building toward) but stop duplicating the prose/example verbatim — frame it as "the choice
  this page is about," one sentence, table, then point to Key Concepts for the fuller
  treatment (Link + Media lifecycle context).
- "Key Benefits" is thin enough that it could be folded into a trimmed version of the mode
  comparison ("why pick Page Mode") rather than standing as its own H2 — three one-line bold
  bullets don't carry enough distinct content to justify a separate heading from the mode
  choice they're arguing for.

This page is not too thin to stand alone otherwise — merging it entirely into `key-concepts.mdx`
or `building-a-page.mdx` would overload either of those. It should stay a separate page; it
needs de-duplication and a completed procedural link chain, not a merge.

---

## 4. Headings as questions

| Current heading | Keep / rewrite | Rationale |
|---|---|---|
| "What is a Page?" | Keep | Already a clear question, matches the section content. |
| "Page Mode vs Direct Mode" | → "How is Page Mode different from Direct Mode?" | The section *is* answering a comparison question; phrasing it as one gives retrieval systems a better match for "what's the difference between..." queries, which is how support questions on this topic actually arrive. |
| "Key Benefits" | → "Why use Page Mode instead of Direct Mode?" | "Key Benefits" is generic enough to match almost any product page; tying the question to the specific choice this page is about (Page Mode vs Direct Mode) makes it distinctive and answerable in isolation. |
| "Creating a Page" | → "How do I create a Page?" | Gerund-noun heading with a clear procedural answer underneath; question form matches how users actually phrase this ("how do I turn on a page for my items"). |
| "Related" | Keep | Navigational, not a content question. |

---

## 5. Edge cases / limits / failure modes

Gaps found (treated as defects per the audit brief — each is a spot where an AI support agent
would have to guess or invent an answer):

1. **No plan-tier statement.** The page never says whether Page Mode requires a paid plan.
   Checked `src/lib/stripe-plans.ts`: "Drag and drop landing page editor" is listed as a
   **Starter**-tier feature (the lowest paid tier), so Page Mode is not gated to a higher tier
   — but the page doesn't say this, leaving room for an AI agent to guess wrong (e.g., invent
   a "Professional plan required" answer) when asked.

2. **No statement of what happens to existing page content when Page Mode is switched off and
   back on.** The page says "You can switch between modes at any time without reprinting" but
   never addresses the adjacent, obvious follow-up question a reader will have: does turning
   the page off delete the layout/Destinations I built? Per
   `src/lib/types/landing-page-config.ts`, `profilePagesEnabled` is stored as a flag
   independent of the page/destination data, and no delete path was found tied to toggling it
   off in `src/app/api/tubs/[id]/route.ts` — so the answer is very likely "no, it's
   preserved," but the page states neither the question nor the answer, which is precisely
   where an agent would confidently improvise.

3. **"Audience Routing" is described in a way that contradicts the sibling page's own
   clarification, and this page never resolves the ambiguity.** The "Key Benefits" bullet
   says: *"Show different Destinations to different people: Staff see operational tools /
   Customers see support info / Technicians see maintenance access."* Read on its own, this
   sounds like the system automatically shows different Destinations to different people. But
   `help/conditional-visibility.mdx` explicitly says the opposite is true by default: *"You
   probably don't need conditional visibility if: You want different people to see different
   Destinations (just show all Destinations—people tap what's relevant)."* I.e., plain Page
   Mode shows **everyone the same full list** and people self-select; only Conditional
   Visibility actually hides/filters Destinations per viewer. Pages Overview never makes this
   distinction, so a reader (or an AI agent answering from this page alone) could easily tell
   a user "add a Destination for staff and one for customers and they'll each only see
   theirs" — which is false without also adding conditions. This is a real failure mode this
   page sets up and doesn't resolve.

4. **No mention of item-level override mechanics or limits.** The page states "An individual
   Item can override its Tub's setting" as its very last sentence, with no explanation of
   *how* (there's no pointer to where in the Item form this override lives), and no mention of
   whether an override is itself reversible/clearable back to "inherit from Tub" — a
   plausible follow-up question. (Verified in source that per-item override is real:
   `AddEditItemForm.tsx` sets a `destinationType` distinct from the tub default via
   `getEffectiveDestinationType()` in `landing-page-config.ts`.)

5. **No mention of the *other* "override" in the same product area.** `building-a-page.mdx`
   has its own, unrelated "Override" concept (per-Item visual-content override in the Page
   Editor: "Override: ON — saving stores changes for that Item only"). Pages Overview uses
   the word "override" for a completely different mechanism (Page-Mode-vs-Direct-Mode choice
   per item) and never flags that the same word means something else one page over. Not
   necessarily wrong, but a foreseeable source of confusion an AI agent could conflate.

6. **No failure/empty-state behaviour.** What does a scan show if a Tub has Page Mode on but
   the Item has zero Destinations added yet? Not stated, not verified against source for this
   audit — flagging as an open question the page should answer rather than leave silent.

---

## 6. Chunk integrity

Each H2 evaluated as if it were the *only* thing retrieved (no page title, no neighboring
sections):

- **"What is a Page?"** — Mostly holds up: the scenario sentence + three-bullet example is
  understandable without prior context. Weak point: "Destinations" is used but never defined
  in this section (or anywhere on the page) — a reader/agent has to infer the term purely
  from the bulleted examples. No pronoun/"above example" dependency issues.

- **"Page Mode vs Direct Mode"** — The table is self-contained and legible alone (column
  headers carry their own meaning). The trailing sentence "You can switch between modes at
  any time without reprinting" reads fine on its own too. Weak point: nothing in the section
  states *what* is in Page Mode vs Direct Mode (a Link? a Tub? an Item?) — it's implied only
  by the rest of the page. In true isolation, a reader can't tell whether "mode" is a Link
  property, a Tub property, or an Item property (it's actually configured at the Tub level
  with an Item-level override, per the "Creating a Page" section three headings later).

- **"Key Benefits"** — Each of the three bold sub-blurbs is self-contained and doesn't lean on
  "this" or "the above." But the heading itself, "Key Benefits," doesn't say benefits *of
  what* — in isolation (no page title carried into the chunk) a reader can't tell if this is
  about Pages, QRtub generally, or something else. Depends on the page-level title for
  meaning.

- **"Creating a Page"** — Self-contained for what it actually states (the numbered steps
  don't reference "the above" or "this example"). But as covered in §1/§5, steps 2–4 are
  under-specified stubs, so "self-contained" here means "doesn't break if isolated," not
  "sufficient to complete the task."

- **"Related"** — A bare link list; carries no answerable content on its own, so it isn't a
  meaningful retrieval chunk regardless of isolation. Most RAG chunkers either merge this into
  the prior section or drop it; either way it's not something to rely on for standalone
  answers. Also (see §1) it's missing links to `creating-your-first-link.mdx` and
  `building-a-page.mdx`, which are the two pages that actually carry the mechanics this page's
  own procedure section defers to.

---

## Summary of concrete fixes needed

1. Add "Destination" and (briefly) "Link" definitions inline so the page doesn't depend on
   `key-concepts.mdx` for terms it uses constantly.
2. Add `help/creating-your-first-link.mdx` and `help/building-a-page.mdx` to "Related" (and
   ideally inline in the "Creating a Page" steps) — currently entirely unlinked from this
   page despite being the pages that explain steps 2–4.
3. Resolve the "Audience Routing" vs Conditional Visibility ambiguity explicitly (§5.3).
4. State plan availability (Starter tier, confirmed via `stripe-plans.ts`) and the
   toggle-off/on persistence behaviour (§5.1, §5.2) so an AI agent doesn't have to guess.
5. De-duplicate the "Page Mode vs Direct Mode" section against `key-concepts.mdx#link-modes`
   (§3) — shorten and cross-reference rather than re-explaining in full with the same example.
6. Add topic sentences to sections that currently open cold into a table or sub-headers
   ("Page Mode vs Direct Mode," "Key Benefits") so isolated retrieval carries framing.

A full rewrite addressing all of the above has been drafted at
`/workspace/mintlify-docs/audit/proposed/help__pages-overview.md`.
