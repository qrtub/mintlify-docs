# Editorial audit — Creating Your First Link

**File:** `/workspace/mintlify-docs/help/creating-your-first-link.mdx`
**Live:** https://help.qrtub.com/help/creating-your-first-link
**Nav group:** Help → Getting Started (the *only* page in that group)
**Siblings skimmed:** `help/pages-overview.mdx`, `help/key-concepts.mdx`, `help/print-first-workflow.mdx`

Verified against `../qrtub/src/components/blocks/CreateAccessLinkForm/CreateAccessLinkForm.tsx`,
`../qrtub/src/app/app/access-link/page.tsx`, `../qrtub/src/components/left-sidebar.tsx`,
`../qrtub/src/lib/api/error-catalog.ts`, `../qrtub/src/lib/types/link-generation-config.ts`,
`../qrtub/src/app/home-client.tsx`, and `../qrtub/GLOSSARY.md`.

---

## 1. SELF-CONTAINMENT

A cold reader **cannot** complete this task from the page alone, and worse, several of the
concrete instructions it does give don't match what a reader would actually see in the app
today. Specific breaks:

- **Step 1 tells the reader to click something that doesn't exist.** The page says: *"From
  your dashboard, click **Links** in the main navigation."* The live sidebar item
  (`src/components/left-sidebar.tsx` line 135) reads **"Access Link"**, not "Links":
  ```tsx
  <IconLink className="w-5 h-5 shrink-0 flex-none" />
  {!effectiveCollapsed && <span className="text-sm font-medium">Access Link</span>}
  ```
  A reader scanning the sidebar for an item literally labelled "Links" won't find one. (Note:
  `pages-overview.mdx` handles exactly this kind of drift correctly — *"turn on the page option
  (labelled 'Show a profile page' in the current app)"* — this page should follow that same
  pattern instead of asserting a label that isn't there.)

- **Step 2 names a button that doesn't exist.** The page says: *"Click the **Generate Links**
  button."* The actual control (`src/app/app/access-link/page.tsx` line ~1478) is:
  ```tsx
  <span className="text-sm font-semibold text-dash-text">Create Link</span>
  ```
  and it opens a panel titled **"Create Access Link"** (line 426), not a "Generate Links" flow.

- **The "Single Link vs. Bulk Links" choice doesn't exist as described.** The page presents
  these as two alternatives you pick between: *"Single Link - Generate one Link at a time"* /
  *"Bulk Links - Generate multiple Links for professional printing."* In the real form
  (`CreateAccessLinkForm.tsx`) there is one unified form with a **Strategy** selector (Random /
  Numbered / Custom) and, for Random and Numbered-Auto, an optional **"Number of Links"**
  count field (default 1, `min={1} max={100}`). There's no separate bulk entry point — you just
  raise the count. Custom-slug links are always created one at a time (`count: strategy ===
  'custom' ? 1 : count`) — the page never says this, so a reader trying to bulk-create custom
  slugs will be confused when the option silently isn't offered.

- **Step 4's three bullets have zero instructions.** *"Download QR codes for printing,"*
  *"Connect the Link to an Item,"* *"Set up a Page with multiple Destinations"* — none of these
  say where to click. In particular, "Connect the Link to an Item" skips the actual mechanism:
  select the link(s) in the table, then use the row/bulk action that opens an item picker
  (`handleBulkAssignToItem` → `ItemPickerModal`, panel title `Assign N Links to Item`). A cold
  reader has no way to discover this from the doc.

- **"Set up a Page" is presented as a same-page action; it isn't.** Per `pages-overview.mdx`,
  Pages are switched on **per Tub**, not per Link, and require an Item to exist and the Link to
  be assigned to it before any Destination can be added. The bulleted list makes it read like a
  peer action to "download the QR code" — it's actually a multi-step prerequisite chain the
  page doesn't disclose.

- **Missing entirely: you can skip Items altogether.** The create-link form has an optional
  **"Destination URL (optional)"** field (`CreateAccessLinkForm.tsx` line ~584): *"If provided,
  this access link will redirect directly to this URL."* This means a reader can produce a
  fully working redirecting QR code in one step, with no Item and no Page. This is arguably the
  *simplest* path to a working first Link, and the page never mentions it — instead implying
  Item-connection or Page-building are the only next moves.

- **Missing: what a freshly generated, not-yet-connected Link actually does when scanned.**
  Per `print-first-workflow.mdx`, an unconnected Link does not 404 — a team member scanning it
  gets an on-the-spot assign prompt, anyone else gets a neutral branded page. A brand-new user
  who generates a Link and immediately scans it (the obvious first thing to try) has no way to
  know this from this page.

- **Missing: format/limit rules a reader will hit immediately.** None of the following —
  verified in source — appear anywhere on the page:
  - Custom slugs: **3–50 lowercase letters, numbers, hyphens, or underscores**
    (`error-catalog.ts`: `CUSTOM_SLUG_INVALID`).
  - Random links are **case-sensitive** and always live under the `/r/` prefix
    (`qrtub.com/r/aB3xk`), stated directly in the form's own helper text.
  - Numbered/ID-based links: prefix and suffix are optional, digit count is **1–10**
    (`min={1} max={10}`), and there are three generation modes (Auto / Specific / Range) the
    page collapses into one bullet.
  - Per-request ceilings: **100 links** per request for Random or Numbered-Auto, **1,000**
    numbers per request for Numbered-Range (`"Range too large... Maximum 1000 numbers per
    request."`).

**Bottom line:** a reader following this page's literal instructions will fail at Step 1
(wrong nav label), fail at Step 2 (wrong button name, wrong mental model of single/bulk), and
be stuck at Step 4 (no bullet says how to do anything).

---

## 2. ANSWER-FIRST

Quoting the literal opening of every H2:

- **"Step 1: Navigate to Links"** → opens: *"From your dashboard, click **Links** in the main
  navigation."* Answer-first in form (one imperative sentence), but factually wrong (see §1) and
  far short of the 40–60 word target — it gives the click target and nothing else (no mention
  of what "dashboard" means for a reader who just signed up, i.e. app.qrtub.com after login).

- **"Step 2: Generate a New Link"** → opens: *"Click the **Generate Links** button. You can
  create:"* — leads straight into a list rather than a real answer; the button name is wrong
  (see §1); "You can create:" is a sentence fragment, not a 40–60 word answer.

- **"Step 3: Choose Your Link Type"** → has **no prose opener at all**. The heading is followed
  immediately by a bare bulleted list (Random / ID-based / Custom). There is no sentence
  answering "which type should I choose" or even "what is a link type" — the section relies
  entirely on the reader inferring meaning from three one-line bullets.

- **"Step 4: Print or Connect"** → opens: *"Once generated, you can:"* — three words of
  preamble, then a bare list. Not an answer to anything; doesn't even restate the implicit
  question ("what do I do with a Link once it exists?").

- **"Next Steps"** → not a Q&A section, it's a navigation footer (two links out). Fine as-is
  for what it is, but doesn't count toward answer-first scoring.

None of the four procedural H2s meet the 40–60-word direct-answer bar. All four are either a
single terse imperative or a bare list with no framing sentence at all.

---

## 3. ONE QUESTION PER PAGE

The page's implied question is singular and reasonable for a "Getting Started" entry point:
**"How do I create my first Link in QRtub?"** It does not need splitting — the four steps
(navigate → generate → choose type → do something with it) are one continuous task, and the
page already correctly *defers* the two adjacent tasks that would otherwise bloat it:

- Building a full Page with Destinations → deferred to `/help/pages-overview` (correct move).
- Direct Mode vs Page Mode conceptual detail → deferred to `/help/key-concepts#link-modes`
  (correct move).

So: **no split needed.** The opposite problem is closer to the truth — at ~230 words of actual
body content, with three of four steps under-specified to the point of being unusable (§1), the
page is currently too thin to stand alone as a retrieval chunk for the question it claims to
answer. It shouldn't be merged into a sibling (it's the correct canonical landing page for "how
do I make a Link"), it needs to be **fleshed out in place** — same scope, enough detail per step
that each one is independently actionable. The rewrite below does this without absorbing
Pages-Overview or Key-Concepts content.

---

## 4. HEADINGS AS QUESTIONS

Most of the existing headings are procedural imperatives ("Step 1: Navigate to Links"), which
is a defensible pattern for a numbered walkthrough — converting every one to question form would
add noise. A few genuinely read better, and retrieve better, as questions:

- **"Step 3: Choose Your Link Type"** → **"What are the different Link types?"** or **"Random,
  ID-based, or Custom — which Link type should I use?"** This section is pure reference (three
  option descriptions), not a sequential action step, so it's a better candidate for a question
  a reader/AI-agent would literally type ("what link types does qrtub support") than for a
  numbered "Step 3."

- **"Step 4: Print or Connect"** → **"What can I do with a Link once it's created?"** The
  current heading names two of at least four things you can actually do (download QR, connect
  to Item, set up a Page, *or* just set a redirect URL at creation) — a question form invites a
  complete answer rather than the current partial list.

- **"Step 1: Navigate to Links"** and **"Step 2: Generate a New Link"** are fine as imperative
  step headings — they describe a single, ordered action, and forcing them into "How do I
  navigate to Links?" would read as artificial. No change proposed for these two.

---

## 5. EDGE CASES / LIMITS / FAILURE MODES

Treating every absence as a defect, per the audit brief:

- **No plan-tier information at all.** The page never says whether Link creation is gated by
  plan, and no plan-tier cap on Link counts was found anywhere in `../qrtub/src` (pricing tiers
  live externally on qrtub.com/pricing, outside this repo). This is a gap worth flagging to
  product/marketing, but nothing in source contradicts the page's silence here, so no invented
  claim is needed — the rewrite adds a pointer to the pricing page rather than a guess.
- **No per-request ceilings stated** — 100/request for Random and Numbered-Auto, 1,000/request
  for Numbered-Range (see §1). Silence here is exactly the shape of gap that makes a support bot
  invent a number, or worse, tell a user "there is no limit."
- **No custom-slug format rule stated** (3–50 lowercase letters/numbers/hyphens/underscores) —
  a reader who types a slug with a space, capital letter, or symbol gets a rejection the doc
  never prepared them for.
- **No mention that custom slugs can't be bulk-created** — the count field silently disappears
  for that strategy in the app; the doc's "Bulk Links" framing (§1) makes this more confusing,
  not less.
- **No mention of collisions.** Custom slug taken → `CUSTOM_SLUG_IN_USE` / `LINK_URL_IN_USE`;
  a slug that matches an existing Numbered pattern → `CUSTOM_SLUG_RESERVED_BY_PATTERN` ("Choose
  a different custom slug"). None of this appears.
- **No statement of what an unconnected Link does when scanned.** Covered in
  `print-first-workflow.mdx` but not here, even though a first-time reader is more likely to
  scan their brand-new Link *immediately*, before connecting anything, than to read the
  print-first page first.
- **No mention that a Link doesn't strictly need an Item** — the optional Destination URL field
  at creation time is a complete, valid, simpler end state for "my first Link" than anything
  the page currently describes.
- **No mention of QR code file format/size** for the "Download QR codes for printing" bullet —
  not verified either way in the scope of this audit; flagged as an open question rather than
  asserted.

---

## 6. CHUNK INTEGRITY

Testing each H2 as if it were the only text retrieved, no surrounding page:

- **"Step 1: Navigate to Links"** — self-contained as a sentence, but assumes the reader already
  knows what "your dashboard" is and how to reach it (no URL, no mention of needing an account).
  Retrieved alone, a reader with no other context can't act on it.
- **"Step 2: Generate a New Link"** — depends on Step 1 having happened ("Once you're on the
  Links page…" is implied but never stated) — retrieved alone, "Click the Generate Links button"
  has no anchor to *where*.
- **"Step 3: Choose Your Link Type"** — depends on Step 2's "Generate Links" panel being open;
  the three bullets (Random/ID-based/Custom) make sense as pure reference content but the
  heading "Choose Your Link Type" presupposes you're mid-flow somewhere, which isn't true if
  this chunk is retrieved on its own (e.g. someone asking "what link types exist").
  Reference-only content like this survives isolation *better* under a question-form heading
  (§4) with one clarifying sentence than under "Step 3."
- **"Step 4: Print or Connect"** — the phrase **"Once generated"** is a direct backward
  reference to Steps 1–3; retrieved alone, "once generated" refers to nothing. This is the
  clearest antecedent-dependency break on the page.
- **"Next Steps"** — fine in isolation; it's just two outbound links with no dependent pronouns.

---

## Overall verdict

Rewrite warranted. The page's structure (four ordered steps + next-steps footer) is the right
shape for this task, but three of four steps contain instructions that don't match the current
app (wrong nav label, wrong button name, a single/bulk choice that doesn't exist as described),
and every step is under-specified relative to what's needed to actually complete it. A full
replacement draft has been written to
`/workspace/mintlify-docs/audit/proposed/help__creating-your-first-link.md`.
