# Editorial Audit

One page audited per file in `audit/_raw/editorial/`, in `docs.json` navigation order. Each section reproduces that page's full editorial assessment (self-containment, answer-first, one-question-per-page, headings-as-questions, edge cases, and chunk integrity for `/help/*` pages; the citability/accuracy bar for `/industries/*` marketing pages), followed by a pointer to its proposed rewrite draft where one exists.

## Summary

| Page | Needs rewrite | Most severe issue |
|---|---|---|
| [index](#homepage-index) | Yes | No link to the actual first-action page, and core vocabulary (Link/Page/Destination) is never defined |
| [help/creating-your-first-link](#creating-your-first-link) | Yes | Literal instructions don't match the live app — wrong nav label ("Links" vs. "Access Link") and wrong button name ("Generate Links" vs. "Create Link") |
| [help/key-concepts](#key-concepts) | Yes | Media section claims cost/durability/inventory tracking that doesn't exist — contradicts CLAUDE.md and the sibling media-basics page |
| [help/print-first-workflow](#the-print-first-workflow) | Yes | No UI path for either step, and the only documented export procedure contradicts this page's own print-before-Items premise |
| [help/media-basics](#physical-media-management-basics) | Yes | States no batch-management guide exists yet, even though /help/print-batches is a live, published page |
| [help/custom-fields](#custom-fields) | Yes | Allowed-values CSV validation is claimed unconditional but is skipped entirely when "Allow new values" is on — the opposite of what's stated |
| [help/pages-overview](#pages-overview) | Yes | Procedural steps 2–4 have zero mechanics, and "Audience Routing" implies per-viewer content targeting that doesn't exist without Conditional Visibility |
| [help/building-a-page](#building-a-page) | Yes | The "Saving" section's answer-first opener fails on the exact question readers ask ("why did my edit change every item"), and "base template" is never defined |
| [help/using-fields](#using-fields-in-pages) | Yes | Two entire field categories (Time, Request) are missing, despite a sibling page's promise that this is "the full reference" |
| [help/conditional-visibility](#conditional-visibility) | Yes | No mention that a bad or mistyped condition fails silently (evaluates to false, no error shown) — the single most likely support question |
| [help/device-detection](#device-detection-routing) | Yes | An entire simpler, shipped mechanism (per-Item Conditional Rules) is missing — the page documents only the more complex Page-Mode path |
| [help/app-links](#app-links-fallback-urls) | Yes | Two different failure UIs (a styled panel vs. a native browser alert()) are treated as one, misleading readers about what customers actually see |
| [help/print-batches](#print-batches) | Yes | States exporting always creates a batch ("not a neutral download"), but a genuine neutral CSV-only download option exists in the app |
| [industries/civil-construction](#qrtub-for-civil-construction) | No | "Different Information for Different People" implies automatic role-based content targeting the app does not do |
| [industries/contract-cleaning](#qrtub-for-contract-cleaning) | No | Named third-party integrations (Swept, CleanTelligent, Aspire, Jobber) are stated with unverifiable, Mitti-level confidence |
| [industries/arboriculture-tree-management](#qrtub-for-arboriculture-tree-management) | No | A named integration partner, "CONFIRM (Abacus)", appears to be a fabricated vendor attribution; "Arborcheck" is miscategorised as inspection software |
| [industries/electrical-test-and-tag](#qrtub-for-electrical-test-and-tag) | No | Confirmed fabricated capability: automatic ISO-to-human date reformatting on the Page that the code does not implement |
| [industries/local-government-councils](#qrtub-for-local-government-councils) | No | Claims per-department Tub access control ("each sees only their assets") that doesn't exist — matches a BRAND.md-forbidden claim category |
| [integrations/safetyculture](#mitti-formerly-safetyculture-iauditor) | Yes | The page's central "configure once, deploy to 500 items" pitch never states what happens when a bound field is empty (the destination is silently dropped) |
| [integrations/cmms-systems](#cmms-systems-integration) | Yes | No in-app procedure at all — the page never says where in the QRtub UI to actually enter these URLs |

## Homepage (index)

**Source page:** `index` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/index.mdx`
**Live:** https://help.qrtub.com/index
**Nav placement:** `docs.json` — Help tab, top-level page (`"pages": ["index"]`), sitting above every group (Getting Started, Concepts, Items & Data, Pages, Printing). It is the only page in the Help tab with no group, i.e. the tab's landing page.

**Siblings skimmed for overlap:** `help/creating-your-first-link.mdx`, `help/key-concepts.mdx`, `industries/civil-construction.mdx`, `integrations/safetyculture.mdx`.

---

### 1. SELF-CONTAINMENT

A cold reader landing on this page, unable to follow any link, **cannot complete the implied task** ("start using QRtub") because:

- **No path to the actual first action.** The page never links to `/help/creating-your-first-link` — the page that literally contains Step 1–4 for generating a Link. The homepage's own "Get Started" section (line 26) lists features but gives no instructions and no link to the how-to page. A reader stuck on this chunk alone has no next click that leads to actually doing anything in the product.
- **No signup or login URL.** The page mentions "Deploy QR codes with professional management from day one" but the only outbound link is `https://qrtub.com/pricing`. There's no `app.qrtub.com/login` or signup URL in the body content itself (those only exist in `docs.json` navbar, which isn't part of the Markdown chunk an AI agent would retrieve).
- **Core vocabulary is used but never defined.** "Page" appears once, uncapitalized-context-dependent ("One QR code with a **Page** serves them all," line 18) with no definition. "Destination" — the term for where a Page's buttons route to — is never used at all on this page, even though the whole value proposition ("update Destinations whenever requirements change," conceptually) depends on it. A reader (or an AI agent synthesizing an answer from just this page) has no way to know what a Link, Page, Item, Tub, or Destination actually is — those definitions live only in `help/key-concepts.mdx`, which this page never links to.
- **No plan/pricing specifics.** "See plans and pricing →" is a bare link with zero inline numbers. The actual tiers (`Starter`/`Professional`/`Scale`) and their ceilings (see §5) are invisible to a reader who can't follow the link.

**Concrete missing pieces:** a link to `/help/creating-your-first-link`, a one-line definition of Link/Page/Destination (or a link to `/help/key-concepts`), and at least the plan-tier ceiling numbers or a pointer to them.

---

### 2. ANSWER-FIRST

Every H2's opening sentence(s), quoted verbatim, judged against the question implied by its heading:

#### H2: "Why Choose QRtub?"
No paragraph directly under the H2 — it drops straight into an H3 ("Print Before You're Ready"). Judging by that first H3's opening sentence:
> "Professional printing requires bulk orders and a lead time. Don't wait until everything is finalised."
This is **pain/scene-setting, not an answer**. The actual answer — "Generate Links, print QR codes, and connect them when convenient" — is the *third* sentence. A retrieval system truncating to the first sentence gets the problem statement, not the capability.

#### H3: "One Code, Multiple Systems"
> "Normally QR Codes only do one thing for one audience. One QRtub code can do the job of many."
Same pattern: opens with the competitor/default-behavior framing before the QRtub-specific answer. Two sentences of setup before the point lands.

#### H3: "Future Proof"
> "Switch software, add new systems, fix broken links—all without touching your physical QR codes."
This one **is** answer-first — it states the capability immediately, no preamble. Good example on the page.

#### H3: "Built for Physical Deployments"
> "Marine operations, construction equipment, rental fleets, lifesaving stations — QRtub handles real-world deployments where QR codes need to last months or years."
Answer-first — leads with the concrete industries, then the claim. Good.

#### H2: "Get Started"
> "Core features available today:" (immediately followed by a bullet list, no sentence answer at all)
This is a **label, not an answer**. The heading implies "how do I get started?" but the section is a feature inventory with no steps and no first action. It answers "what exists" not "how do I begin."

#### H2: "Find out more"
No text at all — the H2 is followed immediately by two `<Card>` components with one-line descriptions inside them ("How your industry uses QRtub." / "How to use QRtub with your favourite tools."). There is no sentence-level answer under the H2 itself; the section *is* the two card blurbs. In isolation (see §6) this section conveys almost nothing.

**Summary:** 2 of 6 headings (Future Proof, Built for Physical Deployments) are genuinely answer-first. The rest open with preamble, an unanswered label, or nothing at all.

---

### 3. ONE QUESTION PER PAGE

This page mixes three distinct jobs in one chunk:
1. **Marketing pitch** ("Why Choose QRtub?") — why the product exists, competitive framing.
2. **Feature inventory** ("Get Started") — a bullet list of what's available, with no procedure.
3. **Navigation hub** ("Find out more") — two cards pointing to Industries and Integrations tabs.

For a homepage, mixing pitch + feature list + hub navigation is the expected shape (it's the tab's landing page, not a task page), so **no structural split is needed** — this isn't the same failure mode as a how-to page answering two unrelated procedures.

The real problem is **redundancy with sibling pages**, not multiple questions on one page:
- "Print Before You're Ready" (index.mdx) restates, almost beat-for-beat, `help/key-concepts.mdx`'s "Print-Before-Link Workflow" section (*"You don't have all your Item details finalised yet... Generate Links... Professional printing... Field deployment"*) and `industries/civil-construction.mdx`'s "Print Before Deployment" section (*"Professional weatherproof QR codes require bulk printing and lead time..."*).
- "Future Proof" (index.mdx) is a shorter restatement of `key-concepts.mdx`'s "Update Without Reprinting" section and `civil-construction.mdx`'s "Update Without Reprinting" section — three pages now carry the same "switch vendors, don't reprint" pitch in near-identical language.

**Recommendation:** not a split, but a **trim**. The homepage's "Why Choose QRtub?" H3s should stay short (one sentence each, as a teaser) and defer full explanation to `/help/key-concepts`, rather than re-arguing the case at similar length. Currently the homepage argument is *nearly as long* as the concept page's argument, which means an AI agent asked "why print before linking?" has three near-duplicate sources to reconcile instead of one canonical one.

**Is the page too thin to stand alone?** No — as the tab landing page it earns its place structurally. But its actual informational payload (once you strip preamble and duplication) is thin relative to its heading count; see the proposed rewrite for a denser version.

---

### 4. HEADINGS AS QUESTIONS

| Current heading | Genuinely clearer as a question? | Proposed rewrite |
|---|---|---|
| "Why Choose QRtub?" | Already a question — fine as-is. | — |
| "Print Before You're Ready" (H3) | Yes — this is exactly the FAQ a prospect searches for. | "Can I print QR codes before my systems are ready?" |
| "One Code, Multiple Systems" (H3) | Yes — matches a real query shape. | "Can one QR code link to multiple systems?" |
| "Future Proof" (H3) | Yes — "Future Proof" is vague marketing-speak; the question form is more specific and matches what someone actually searches. | "What happens if I switch vendors or systems later?" |
| "Built for Physical Deployments" (H3) | Marginal — it's a positioning statement more than an answerable question. Leave as a noun phrase, but consider: | "Which industries is QRtub built for?" (optional, weaker gain than the others) |
| "Get Started" | No — "Get Started" is a standard, well-understood section label; converting it to a question ("How do I get started?") doesn't add clarity, and per house style (matches pattern in `creating-your-first-link.mdx`, `key-concepts.mdx`) numbered "Step N" headings are the convention for procedures, not this summary list. | — |
| "Find out more" | No — this is a navigation label, not a question a reader is asking. | — |

---

### 5. EDGE CASES / LIMITS / FAILURE MODES

This page states **zero** limits, ceilings, or plan-tier gating. Given the house rule in `CLAUDE.md` ("State limitations explicitly; silence reads as capability" and the audit brief's own framing — this is exactly where an AI support agent invents a wrong answer), these are defects:

- **No plan tiers or ceilings are named**, even though they exist and are concrete (`../qrtub/src/lib/stripe-plans.ts`):
  - Starter ($5/mo): up to 100 active links, 1 editor
  - Professional ($25/mo): up to 1,000 active links, 5 editors, 1 numbered sequence pattern
  - Scale ($90/mo): up to 10,000 active links, 20 editors, 5 numbered sequence patterns, private-or-public scanning
  An AI agent asked "how many QR codes can I deploy on the Starter plan?" has nothing on this page (or anywhere else in the skimmed set) to answer from, and would either say "unlimited" (wrong) or fabricate a number.
- **No statement of what's NOT available.** The bullet list ("Bulk Link generation," "Pages with multiple destinations," "Print-before-link workflow," "URL Templates for bulk deployment") is presented as the complete feature set with no "not yet" callout. Per `../qrtub/BRAND.md` §1.4, API access, cross-account transfer/sharing, granular permissions, and payment Destinations are Planned, not available. A support bot retrieving only this page has no signal that these don't exist yet, and nothing stops it from answering "yes" to "does QRtub have an API?"
- **No mention of what happens when a URL Template field is empty or contains special characters.** This is a real, documented failure mode (`key-concepts.mdx`: *"QRtub does not URL-encode them, so a field containing a space, `&`, `?` or `#` will produce a broken link"*) but the homepage's bullet "URL Templates for bulk deployment" gives no hint this footgun exists. Since the homepage doesn't link to `key-concepts.mdx` at all, a reader stopping here has no way to discover it.
- **No mention of Direct Mode vs. Page Mode.** "One QR code can do more, linking to multiple systems" (line 8) is true only in Page Mode — Direct Mode Links go to exactly one Destination. The page states the multi-destination benefit as if it's the default/only behavior, with no caveat that a Link must be in Page Mode and carry a Page for this to work.
- **No mention of `qrtub.com/r/...` vs custom/ID-based Link formats**, unlike `creating-your-first-link.mdx` which explains the three types. A reader can't tell from this page alone what a "Link" looks like as a URL.

---

### 6. CHUNK INTEGRITY

Testing each H2 (and its H3 children) as if retrieved in isolation, no surrounding page:

#### Intro paragraphs (before any H2)
Reads fine standalone as a value-prop paragraph, though it assumes the reader already knows what a "Destination" is conceptually ("Update Destinations whenever requirements change") without ever using that word — it says "Connect them during installation... Update Destinations whenever requirements change" (line 8), which is the *only* place anything Destination-like is implied, and even there the word "Destinations" is used **without ever having been introduced or defined** anywhere on this page. Isolated, a reader has no idea what a "Destination" is.

#### H2: "Why Choose QRtub?"
- Depends on undefined terms: "Link" (never defined, only exemplified via "Generate Links"), "Page" (used once, undefined: *"One QR code with a Page serves them all"*, line 18).
- The four H3s are otherwise self-contained sentences that don't refer back to "the above example" or "this" — no anaphoric dependency problems within the section itself. This is a genuine strength: each H3 could be lifted individually without breaking grammatically.
- However, "Built for Physical Deployments" (*"QRtub handles real-world deployments where QR codes need to last months or years"*) implicitly assumes the reader already knows *why* longevity matters (durability of physical media, no reprinting) — that context lives in `key-concepts.mdx`'s "Media" section, not here.

#### H2: "Get Started"
Standalone, the bullet list ("Bulk Link generation," "Pages with multiple destinations," "Print-before-link workflow," "URL Templates for bulk deployment") is a list of product-internal nouns with zero explanation. Someone who has never seen "URL Templates" or "Print-before-link workflow" gets a term, not an answer. This section's usefulness in isolation is close to zero for anyone but an existing user — it functions as a recap, not a stand-alone explainer.

#### H2: "Find out more"
Worst case for chunk integrity. Retrieved in isolation, this section is:
```
<Columns cols={2}>
  <Card title="Industries" icon="building" href="/industries/civil-construction">
    How your industry uses QRtub.
  </Card>
  <Card title="Integrations" icon="plug" href="/integrations/safetyculture">
    How to use QRtub with your favourite tools.
  </Card>
</Columns>
```
No prose at all — if an AI agent chunks by H2 and returns just this section's text, the retrievable content is two six-word blurbs pointing at single example pages (civil-construction, safetyculture) that don't represent the full Industries/Integrations tabs (the nav has 5 industry pages and 2 integration pages; the cards only surface one each, so the section is also **under-representative of its own target tabs**, not just underspecified).

---

### Overall assessment

The page does not make any false capability claims (a real positive — every bullet under "Get Started" matches `BRAND.md`'s "Available" feature list and the app's actual behaviour). Its defects are structural: undefined vocabulary the rest of the docs rely on, no link to the actual first-action page, no plan/limit numbers, no "not yet available" signal, and one section ("Find out more") that carries no retrievable information on its own. These are exactly the failure modes the audit brief calls out, and they justify a rewrite — see `/workspace/mintlify-docs/audit/proposed/index.md`.


**Proposed rewrite:** `audit/proposed/index.md`

---

## Creating Your First Link

**Source page:** `help/creating-your-first-link` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/creating-your-first-link.mdx`
**Live:** https://help.qrtub.com/help/creating-your-first-link
**Nav group:** Help → Getting Started (the *only* page in that group)
**Siblings skimmed:** `help/pages-overview.mdx`, `help/key-concepts.mdx`, `help/print-first-workflow.mdx`

Verified against `../qrtub/src/components/blocks/CreateAccessLinkForm/CreateAccessLinkForm.tsx`,
`../qrtub/src/app/app/access-link/page.tsx`, `../qrtub/src/components/left-sidebar.tsx`,
`../qrtub/src/lib/api/error-catalog.ts`, `../qrtub/src/lib/types/link-generation-config.ts`,
`../qrtub/src/app/home-client.tsx`, and `../qrtub/GLOSSARY.md`.

---

### 1. SELF-CONTAINMENT

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

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

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

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Overall verdict

Rewrite warranted. The page's structure (four ordered steps + next-steps footer) is the right
shape for this task, but three of four steps contain instructions that don't match the current
app (wrong nav label, wrong button name, a single/bulk choice that doesn't exist as described),
and every step is under-specified relative to what's needed to actually complete it. A full
replacement draft has been written to
`/workspace/mintlify-docs/audit/proposed/help__creating-your-first-link.md`.


**Proposed rewrite:** `audit/proposed/help__creating-your-first-link.md`

---

## Key Concepts

**Source page:** `help/key-concepts` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/key-concepts.mdx`
**Live:** https://help.qrtub.com/help/key-concepts
**Nav group:** Help → Concepts (siblings: `help/print-first-workflow`, `help/media-basics`)

---

### 1. SELF-CONTAINMENT

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

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

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

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Recommendation

A substantive rewrite is warranted — not a tweak. The Media-tracking overclaim is a
correctness defect matching a failure mode this repo's own `CLAUDE.md` explicitly warns
against by name, and the page separately has structural problems (duplicate content across
three areas, a heading-less Example section with a forward reference, non-heading FAQ
entries) that compound the retrieval-quality issues. A proposed replacement has been
written to `/workspace/mintlify-docs/audit/proposed/help__key-concepts.md`.


**Proposed rewrite:** `audit/proposed/help__key-concepts.md`

---

## The Print-First Workflow

**Source page:** `help/print-first-workflow` &nbsp;|&nbsp; **Needs rewrite:** Yes

- File: `/workspace/mintlify-docs/help/print-first-workflow.mdx`
- Live: https://help.qrtub.com/help/print-first-workflow
- Nav group: **Concepts** (`docs.json`), siblings: `help/key-concepts`, `help/media-basics`
- Verified against: `../qrtub/src/lib/database/server-access-urls.ts`, `server-print-batches.ts`,
  `lib/types/link-generation-config.ts`, `components/blocks/UnallocatedLinkPage/UnallocatedLinkPage.tsx`,
  `components/blocks/CreateAccessLinkForm/CreateAccessLinkForm.tsx`, `lib/stripe-plans.ts`,
  `app/[id]/page.tsx`, `app/api/access-links/{generate,export-print-csv}/route.ts`, `../qrtub/GLOSSARY.md`,
  `../qrtub/CLAUDE.md`

---

### 1. SELF-CONTAINMENT

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

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

| Current heading | Implicit question | Propose rewrite? |
|---|---|---|
| Why the usual order does not survive contact with a site | "Why can't I just print a tag whenever I need one?" | **Yes** — rewrite to `## Why can't a single tag be printed on demand?` The current heading is vivid but is a *statement*, and doesn't surface for a query phrased as a question, which is exactly how a support bot or a user would phrase it. |
| How it works | "How does the print-first workflow work?" | **No** — "How it works" is already unambiguous in context (the page is *about* the print-first workflow) and a full question form ("How does the print-first workflow work?") is only marginally clearer. Leave as-is. |
| What happens if someone scans a tag early | (already a question) | **No** — already phrased as the implicit question almost verbatim; just needs a `?`. Minor: `## What happens if someone scans a tag early?` |
| Getting the mistakes back | "What happens if a tag ends up on the wrong asset, or the asset is retired?" | **Yes** — "Getting the mistakes back" is a vague, idiomatic noun phrase that doesn't hint at *what* mistake or *how*. Rewrite to `## What if a tag ends up on the wrong item, or the item is gone?` — this also matches the two concrete scenarios the section actually covers (misassignment, deletion). |
| Things worth doing while you are at it | — | **No** — this is a list of independent tips, not one question; forcing it into "What should I do before ordering a print run?" would flatten four distinct pieces of advice into one heading that doesn't help a reader (or retriever) find any specific one of them. If kept as one page, better to give each tip its own H3 (they're already bolded as if they were) so each is independently addressable. |
| Where the codes point | "Where does a Link point, and can one template cover a whole batch?" | **Marginal — recommend dropping the section instead (see §3)** rather than rewriting the heading, since the content duplicates other pages. If retained, `## Where does a Link point?` is a cleaner question-form than the current noun phrase. |

---

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Additional cross-cutting notes (outside the six categories)

- **Redundancy with `help/key-concepts.mdx`:** the "How it works" 4-step list here
  (Generate → Send to produce → Apply as gear arrives → Connect) is near-identical in structure
  and content to `key-concepts.mdx`'s own `## Print-Before-Link Workflow` section ("1. Generate
  Links... 2. Professional printing... 3. Field deployment... 4. Connect later"). Two pages
  currently teach the same 4-step process under two different names ("print-first" vs.
  "print-before-link"). Consolidating per the §3 split would remove this duplication.
- **Terminology:** this page never uses "print-before-link," the glossary's canonical term for
  this exact workflow (`GLOSSARY.md`). Recommend at minimum a one-line alias note near the top so
  a reader arriving from `key-concepts.mdx`'s vocabulary recognizes it's the same thing.


**Proposed rewrite:** `audit/proposed/help__print-first-workflow.md`

---

## Physical Media Management Basics

**Source page:** `help/media-basics` &nbsp;|&nbsp; **Needs rewrite:** Yes

- File: `/workspace/mintlify-docs/help/media-basics.mdx`
- Live: https://help.qrtub.com/help/media-basics
- Nav group: **Concepts** (`docs.json`), alongside `help/key-concepts`, `help/print-first-workflow`
- Siblings skimmed: `help/key-concepts.mdx`, `help/print-first-workflow.mdx`, `help/print-batches.mdx`

---

### 1. SELF-CONTAINMENT

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

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

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

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Summary

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


**Proposed rewrite:** `audit/proposed/help__media-basics.md`

---

## Custom Fields

**Source page:** `help/custom-fields` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/custom-fields.mdx`
**Live:** https://help.qrtub.com/help/custom-fields
**Nav group:** Help → Items & Data (the *only* page in that group)
**Siblings skimmed for overlap:** `help/using-fields.mdx` (Pages group), `help/building-a-page.mdx` (Pages group)

---

### 1. Self-containment

Mostly yes, with two concrete holes that would strand a cold reader or send them to the wrong tab.

**Gap A — "the destination URL field" is used before it's defined, and the page's own instructions for where to configure it are wrong for that one field.**

The "Where fields are configured" section tells the reader, as the very first instruction on the page: "Open the Tub, go to its settings, and choose the **Fields** tab." The "Defaults" section then says:

> "The **destination URL** field's default behaves slightly differently: it is stamped onto the Item when the Item is created."

Two problems verified against `../qrtub/src`:

- The page never explains what "the destination URL field" *is*. Nothing earlier on the page mentions Destinations, Direct Mode, or Pages — a reader who landed on this page cold (not "Key Concepts") has no way to know this is the field that decides what a scan resolves to.
- It is **not configured on the Fields tab** at all. `FieldConfigurationManagerCompact.tsx` explicitly hides it: `const isManagedExternally = (fieldName: string) => fieldName === 'destination_url';`. The actual control is a "Default destination" box on a different part of Tub settings (`EditTubForm.tsx`, the pass-through/Destination configuration, shown when profile pages are off) — not the Fields tab the page just told the reader to open.

A reader following this page's own first instruction to find and set that default will not find it there. Fix: either drop the destination-URL aside from this page (it's arguably out of scope for "Custom Fields") or explicitly say which tab it lives on and link to it.

**Gap B — Field type table names an option that doesn't exist under that name in the product.**

The Field Types table lists:

```
| **object** | Structured data |
```

The actual field-type dropdown in `FieldConfigurationManagerCompact.tsx` (`FieldTypeSelect`) offers exactly six options, verbatim: `Text`, `Number`, `Date`, `Yes/No`, `List`, **`UUID`**. There is no "Structured data" label anywhere in that UI. A reader (or a support bot paraphrasing this page) who goes looking for "object / structured data" in the Fields tab will see "UUID" and reasonably conclude the docs are describing something else, or that the feature has changed. This is a naming mismatch a cold reader cannot resolve from the page alone.

**Gap C — two behaviors a reader would need mid-task are not stated anywhere on the page:**

- **What happens to an Item's existing value when a custom field is deleted?** Verified in `thing-serializer.ts`: deleting a field only removes it from `fieldConfig.fields` (`deleteFieldFromConfig`); the underlying value is **not purged** from `metadata.fields`. The serializer has dedicated "orphan data" handling — *"This preserves data even if config is out of sync"* — and will still surface the old value, now keyed by its raw nanoid (e.g. `Xk9mPq7z`) instead of a human label, since `idToKeyMap` no longer has an entry for the deleted field. A reader deleting a field to "clean up" has no way to know from this page that the data survives (and resurfaces under an opaque key) rather than being removed.
- **What happens to existing values when a field's type is changed after Items already have data?** No validation, migration, or warning was found in `FieldConfigurationManagerCompact.tsx` gating a type change on existing data — the type selector is freely editable regardless of whether the field is already populated. The page doesn't mention this at all, and a reader has no way to know whether QRtub will convert, ignore, or corrupt existing values.

Otherwise, the page is a reasonably complete standalone reference for the mechanics it does cover (where to configure fields, what core vs. custom means, key rules, defaults, references, required-field enforcement) — someone who landed here cold could complete "define a custom field with defaults and validation" without following a link, apart from the two gaps above.

---

### 2. Answer-first

Checked every H2's opening sentence(s) against the question implied by its heading.

| Heading | Opening sentence(s) | Verdict |
|---|---|---|
| Where fields are configured | "Open the Tub, go to its settings, and choose the **Fields** tab." | Direct instruction, no preamble. Good, though at 11 words it's terse rather than a 40–60 word answer — there simply isn't more to say for this heading, so this is fine as-is. |
| Core fields and custom fields | "Four fields are always present and cannot be removed or renamed: **name**, **item_id**, **description** and **tags**." | Direct answer to "what's the difference between core and custom fields," leads with the concrete list. Good. |
| Field keys | "Every field has a key, used in bindings like `{{item.serial_number}}` and in conditions." | Answers *why keys matter* but the heading implies "what are the rules for keys" — that's answered by the bullet list right after, not the opening sentence. Borderline: acceptable, but the first sentence is scene-setting for the *bullets*, not itself the answer. |
| Field types | *(no opening sentence — goes directly to the table)* | Not answer-first in the sense of a sentence, but not preamble either — the table itself is the direct answer. Missing is a one-line frame ("QRtub has six field types:") that would help a chunk-only retrieval fit the type table to the question "what field types does QRtub support." |
| Allowed values | "A field can carry a fixed list of allowed values, which turns it into a picker rather than a free-text box." | Direct, concrete. Good. |
| Defaults | "A field can have a Tub-level default, applied when an Item is created with that field left blank." | Direct. Good. |
| Reference fields | "A field can point at another record instead of holding a value of its own." | Direct. Good. |
| Required fields and validation | "Any field can be marked required. Required fields are enforced when Items are created or edited, including during CSV import — a row missing a required value is rejected while the rest of the file still imports." | Direct, ~45 words, answers the heading precisely. Good — but see §5, this claim is incomplete without the allowed-values caveat that lives two sections earlier. |
| How fields get used elsewhere | "Once a field exists, it is available in three places:" | Direct lead-in to the list that follows; functions as a correct answer-first opener. Good. |
| Related | *(link list, no prose)* | N/A — not a content section. |

No section opens with marketing preamble or throat-clearing. This page's answer-first discipline is good; the main defect is content-completeness (§1, §5), not structure.

---

### 3. One question per page

This page is a single coherent question — "how do I define and configure the fields on a Tub's Items?" — covering one task (schema authoring) end to end: where to configure, core vs. custom, keys, types, allowed values, defaults, references, required/validation, and where fields get consumed. I would **not** split it; the sections are tightly related sub-steps of one configuration task, not distinct tasks bolted together.

One borderline candidate for a future split: **Reference fields** (lines 72–87) is a self-contained sub-feature (linking an Item to a team member / another Item / a Tub) that could stand alone as "Linking Items to Other Records," especially since it's the one part of this page that models relationships rather than plain data. At its current length (~16 lines, one table) it's too thin to justify a standalone page today — recommend leaving it merged, but flagging it as the first thing to peel off if the page grows further (e.g. if reference-field behavior on deletion of the referenced record, or bulk-reference CSV import, gets documented in more depth later).

The page is not too thin to stand alone — no merge recommendation.

---

### 4. Headings as questions

Most headings are fine as noun phrases because they already read as the natural label for a reference table/section (e.g. "Field types," "Allowed values") rather than hiding an implicit question. Proposing rewrites only where a question form is genuinely clearer:

- **"Where fields are configured"** → keep as-is; already reads as an implicit question and a QA-style rewrite ("Where do I configure fields?") adds nothing.
- **"Core fields and custom fields"** → **"Which fields are built in, and which can I add?"** This heading currently reads as a topic label; the section is actually answering a comparison question (what's fixed vs. what's mine), and the question form signals that up front.
- **"Field keys"** → **"What are the rules for a field's key?"** The current heading doesn't signal that the section is a validation-rules reference; a reader scanning headings for "why did my field key get rejected" would more easily match a question form.
- **"Field types"** → keep as-is (a table of options, not really an implicit question).
- **"Allowed values"** → keep as-is (topic label matches a reference table).
- **"Defaults"** → **"What happens if an Item is created with a field left blank?"** — sharper than the generic noun, and matches exactly what the section answers.
- **"Reference fields"** → **"How do I link a field to another record?"** — the current noun phrase doesn't signal "this is how you model relationships," which the first sentence of the section actually says.
- **"Required fields and validation"** → keep as-is; already close to a question in spirit and the compound noun phrase reads fine.
- **"How fields get used elsewhere"** → already phrased as an implicit question ("how... get used"); keep as-is.

---

### 5. Edge cases / limits / failure modes

This is the weakest dimension of the page. Concrete gaps, each checked against source:

1. **Allowed-values import validation is stated as unconditional but is actually conditional — and the condition is the opposite of what's implied.** The page says: *"Fields with allowed values are also validated on import, so a typo in a status column is caught rather than silently creating a new status."* Verified in `csv-row-validation.ts`:
   ```
   // 3. Allowed values (enum) — only when the field forbids new values.
   for (const [key, def] of Object.entries(fieldConfig.fields)) {
     if (!def.enabled || def.allowNewValues === true) continue;
   ```
   This check is **skipped entirely** when "Allow new values" is on for that field — which the page itself documents two sections earlier as a normal, common setting. So for any field with "Allow new values" on, a typo in a CSV status column is *not* caught — it silently registers as a new allowed value, exactly the failure mode the sentence claims doesn't happen. This is the single highest-risk gap on the page for an AI support agent: asked "will a typo in my CSV get caught," the correct answer is "only if the field doesn't allow new values," and nothing on the page says that.

2. **No statement of what happens to an Item's data when a custom field is deleted.** See §1 Gap C. Verified: data is not purged, it becomes an "orphan" surfaced under its raw nanoid key. This is exactly the kind of question ("if I delete a field, do I lose the data?") a support bot would otherwise have to guess at, and either wrong answer (yes/no) is plausible-sounding without the source.

3. **No statement of what happens when a field's type is changed after Items already hold values in it.** No conversion/validation logic found gating this in `FieldConfigurationManagerCompact.tsx`; the page is silent on it entirely.

4. **No stated ceiling on the number of custom fields a Tub can have.** Searched `field-config.ts`, `server-field-config.ts`, and `stripe-plans.ts` — no cap, no plan-tier gating of field count found anywhere. Given the CLAUDE.md instruction to treat silent absence as a defect, the page should say explicitly that there's no limit, rather than leaving a reader (or an AI agent) to wonder or invent a number.

5. **No plan-tier gating mentioned, and none appears to exist.** Custom Fields is not referenced in `stripe-plans.ts` as a gated feature. Worth a one-line explicit confirmation ("available on every plan") so an AI agent doesn't infer a tier restriction from its absence, consistent with how other QRtub docs handle unlimited/ungated capabilities.

6. **Field type table names a type ("object" / "Structured data") whose actual UI label ("UUID") and apparent purpose (see §1 Gap B) diverge from the description given.** This is as much an edge-case/accuracy gap as a self-containment one: nothing on the page explains what "object" fields are actually used for, and the label mismatch means a reader can't map the docs to the product with confidence.

7. **Required-field enforcement on CSV *update* rows is narrower than the page implies.** The page says "Required fields are enforced when Items are created or edited, including during CSV import." Verified in `csv-row-validation.ts`: on an update row, a required field is only checked "when it's present in the row" (`isUpdate && !(key in item)` → skipped) — so a partial-update CSV that omits a required column is *not* rejected for that column. The current wording ("enforced... during CSV import") reads as unconditional and doesn't distinguish create-import from update-import.

---

### 6. Chunk integrity

Evaluated each H2 as if retrieved alone, no surrounding page.

- **Where fields are configured** — Stands alone fine. One self-contained instruction.
- **Core fields and custom fields** — Stands alone fine. "them" in the second sentence refers back to the four fields named in the same section's first sentence — no cross-section dependency.
- **Field keys** — Stands alone fine.
- **Field types** — Stands alone as a table; no prose to depend on anything else. (Its only defect is the object/UUID mismatch, not isolation.)
- **Allowed values** — Stands alone; "the list" and "this" both resolve within the section's own preceding sentence. Fine in isolation, **but** its accuracy is coupled to information that actually lives in the "Required fields and validation" section (the import-validation caveat, per §5.1) — a reader who retrieves *only* this section would come away believing typos are always caught, which is wrong once "Allow new values" is on. This is a genuine chunk-integrity failure: the two sections make claims that only cohere when read together, and neither one contains the caveat that would make it independently correct.
- **Defaults** — **Fails in isolation.** "The destination URL field's default behaves slightly differently" assumes the reader already knows what "the destination URL field" is and where it lives — neither is established anywhere on this page (see §1 Gap A). A reader retrieving just this section has no way to resolve what's being referred to, and would also be misled about where to configure it (this page's own "Where fields are configured" section, if also retrieved, points to the wrong tab for this specific field).
- **Reference fields** — Stands alone fine; the table is self-explanatory and doesn't depend on prior sections.
- **Required fields and validation** — Stands alone fine in the sense of no pronoun/"above" dependency, but see the Allowed-values note above: its own claim is subtly incomplete without content from the earlier "Allowed values" section.
- **How fields get used elsewhere** — Stands alone fine; the three bullets are self-explanatory and it correctly links out (rather than assuming) for depth.

---

### Cross-page note (context, not a defect of this page)

`help/using-fields.mdx` (Pages group) lists a "Standard Item Fields" table with 14 entries — `item.id`, `item.status`, `item.serial_number`, `item.location`, `item.owner`, `item.type`, `item.subtype`, `item.created_at`, `item.updated_at`, etc. — presented as always-present standard fields. This directly conflicts with `custom-fields.mdx`'s own, source-verified claim that only **four** fields (`name`, `item_id`, `description`, `tags`) are core/always-present (confirmed in `field-defaults.ts`: `CORE_FIELDS = ['name', 'item_id', 'description', 'tags']`). Everything else `using-fields.mdx` lists as "standard" (status, serial_number, location, owner, type, subtype, etc.) is actually a **disableable, renamable, optional** `DEFAULT_FIELD_DEFINITIONS` entry — several of them (`equipment_manager`, `notes`, `manufactured_date`, `parent_item`) even ship `enabled: false` by default. This is flagged separately for whoever audits `using-fields.mdx`, but it's relevant here because it directly undermines a reader's ability to trust `custom-fields.mdx`'s "four core fields" claim if they cross-reference the sibling page afterward — the two pages currently tell a materially different story about what's built-in vs. optional.

---

### Verdict

A proposed rewrite is warranted and has been drafted to `/workspace/mintlify-docs/audit/proposed/help__custom-fields.md`. Rationale: the gaps found are not cosmetic — they touch self-containment (destination-URL default sent to the wrong tab, undefined term), a factual contradiction with real support-ticket consequences (allowed-values-on-import caveat), and two "what happens when things go wrong" edge cases with verified-but-undocumented answers (field deletion, field type change) that are exactly the kind of thing an AI support agent would otherwise fabricate.


**Proposed rewrite:** `audit/proposed/help__custom-fields.md`

---

## Pages Overview

**Source page:** `help/pages-overview` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/pages-overview.mdx`
**Live:** https://help.qrtub.com/help/pages-overview
**Nav group:** Help tab → "Pages" (`help/pages-overview`, `help/building-a-page`, `help/using-fields`,
`help/conditional-visibility`, `help/device-detection`, `help/app-links` — per `docs.json`)
**Siblings skimmed for overlap:** `help/building-a-page.mdx`, `help/key-concepts.mdx`,
`help/creating-your-first-link.mdx` (Getting Started group), `help/conditional-visibility.mdx`

---

### 1. Self-containment

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

### 2. Answer-first

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

### 3. One question per page

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

### 4. Headings as questions

| Current heading | Keep / rewrite | Rationale |
|---|---|---|
| "What is a Page?" | Keep | Already a clear question, matches the section content. |
| "Page Mode vs Direct Mode" | → "How is Page Mode different from Direct Mode?" | The section *is* answering a comparison question; phrasing it as one gives retrieval systems a better match for "what's the difference between..." queries, which is how support questions on this topic actually arrive. |
| "Key Benefits" | → "Why use Page Mode instead of Direct Mode?" | "Key Benefits" is generic enough to match almost any product page; tying the question to the specific choice this page is about (Page Mode vs Direct Mode) makes it distinctive and answerable in isolation. |
| "Creating a Page" | → "How do I create a Page?" | Gerund-noun heading with a clear procedural answer underneath; question form matches how users actually phrase this ("how do I turn on a page for my items"). |
| "Related" | Keep | Navigational, not a content question. |

---

### 5. Edge cases / limits / failure modes

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

### 6. Chunk integrity

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

### Summary of concrete fixes needed

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


**Proposed rewrite:** `audit/proposed/help__pages-overview.md`

---

## Building a Page

**Source page:** `help/building-a-page` &nbsp;|&nbsp; **Needs rewrite:** Yes

File: `/workspace/mintlify-docs/help/building-a-page.mdx`
Live: https://help.qrtub.com/help/building-a-page
Nav group: **Pages** (`help/pages-overview`, `help/building-a-page`, `help/using-fields`, `help/conditional-visibility`, `help/device-detection`, `help/app-links`)

Verified against `../qrtub/src` (component registry, `LandingPageEditor` reducer/panels/TopBar, `ActionLink.tsx`, `section-processors.ts`, `themes/presets.ts`, `EditTubForm.tsx`, `lib/page/merge.ts`). Almost every factual claim on this page checked out exactly against source — see inline citations below. This is one of the more accurate pages in the corpus; the issues below are about structure, self-containment and a handful of real omissions, not invented capability.

**Verdict: rewrite proposed.** The factual content is sound, but the answer-first failure and
self-containment gaps both land in the same section (Saving), and fixing them properly means
touching the surrounding sections too (undo cap, Page Info, Key Concepts link). Full
replacement draft: `/workspace/mintlify-docs/audit/proposed/help__building-a-page.md`.

---

### 1. SELF-CONTAINMENT

Mostly yes, with three concrete gaps a cold reader would hit:

- **Undefined vocabulary.** The page uses "Tub," "Item," and "Destination" throughout (e.g. "Pages must be switched on for the Tub," "every Item in that Tub renders it with its own data") without defining any of them and without linking to the page that does. `help/key-concepts.mdx` is the page that defines these terms, but it is linked from `help/pages-overview.mdx`'s Related list, not from this page. A reader who lands here first (plausible — it's a top Google/AI-answer result for "how do I edit a QR page") has no path to the vocabulary from this page itself.
- **"Base template" is never defined, only used.** The Saving section says "leaving everything else on the base template" and "updates the base template" — but "base template" is never introduced as a term. The closest thing to a definition is the page's opening sentence, *before any H2*: "You build one layout for a Tub, and every Item in that Tub renders it with its own data." That sentence carries load-bearing meaning for a later section but isn't under any heading (see §6).
- **Page Settings is incompletely described.** Verified in `PageSettingsPanel.tsx`: when nothing is selected, Properties actually shows a **Page Info** group (Page Name, a read-only Version number) above the Theme group the doc describes. The doc's "Theming and layout" section jumps straight to Theme Preset and never mentions Page Name / Version exist. A reader following the doc exactly and looking at their own screen sees a section the page never explains.

Everything else — turning Pages on, opening the editor, the three-tab left panel, adding/arranging sections, the 17 section types and their categories, bindings, conditional visibility, previewing, and the override/save model — is complete enough that a reader could execute the whole workflow using only this page. No missing steps, no "assumed you already did X" gaps in the procedural core.

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

This page is a single coherent task — "build and save a page in the Page Editor" — and every section is a genuine sub-step of that task (turn it on → open the editor → understand the layout → add sections → bind data → add conditions → preview → theme → save). It is not conflating two unrelated questions the way, say, a page mixing "how to do X" with "pricing for X" would.

That said, it is a long page (17 sections, ~160 lines) covering material that also has its own dedicated pages elsewhere in the same nav group:

- **Putting Item data into a section** duplicates the *introduction* to bindings that `help/using-fields.mdx` covers in full (its own "Two Ways to Use Fields" / "URL Templates" sections). This page's version is appropriately short and defers with "see Using Fields for the full reference" — that's the right pattern, not a defect. No change needed here.
- **Showing a section only sometimes** does the same relative to `help/conditional-visibility.mdx` — short intro, one example, explicit "see Conditional Visibility" hand-off. Also correctly scoped.

No split is warranted. The **Saving: the whole Tub, or one Item** section is the best candidate if the page ever needs to shrink — it's the most independently searched sub-topic (override behavior), it's self-contained once its opening sentence is fixed (§2, §6), and there is currently no other page discussing overrides at all. But splitting it off now would leave both halves thinner without a clear retrieval win, since anyone asking about the editor and anyone asking about overrides are both very likely mid-way through "building a page" already. Recommendation: **keep as one page**, just fix the answer-first opening and the two self-containment gaps in §1.

The page is not too thin to stand alone — no merge recommendation.

### 4. HEADINGS AS QUESTIONS

Most headings are already fine as noun phrases because the content right beneath them is unambiguous. Proposing rewrites only where a question form would genuinely resolve ambiguity a support bot might have:

- **"Section types"** → could become **"What sections are available?"** — mild improvement; an AI agent matching a user question ("what components can I add to my page?") against heading text alone would match the question form more directly than the noun phrase. Marginal, take-it-or-leave-it.
- **"Saving: the whole Tub, or one Item"** → already effectively a question in disguise (a false dichotomy framed as a heading) and reads well; a literal question form — **"Does saving change one Item or the whole Tub?"** — is more directly matchable to how a confused user would actually phrase the problem ("why did my change apply to everything"). This is the one heading rewrite I'd actually recommend, since it pairs with fixing the section's answer-first problem (§2).
- **"The layout"** → too vague standalone ("the layout" of what?) but immediately clarified by its own first sentence, so leaving as-is is fine; a heading like "How is the editor laid out?" wouldn't add meaningfully more.
- Everything else ("Before you start," "Opening the editor," "Adding and arranging sections," "Putting Item data into a section," "Showing a section only sometimes," "Previewing with real data," "Theming and layout") already functions as an implicit question and converting to literal question form would not improve clarity — these are left alone deliberately, not overlooked.

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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


**Proposed rewrite:** `audit/proposed/help__building-a-page.md`

---

## Using Fields in Pages

**Source page:** `help/using-fields` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/using-fields.mdx`
**Live:** https://help.qrtub.com/help/using-fields
**Nav group:** Help → Pages (siblings: `help/pages-overview`, `help/building-a-page`,
`help/conditional-visibility`, `help/device-detection`)

Verified against `/workspace/qrtub/src/lib/page/bindings.ts`, `context.ts`,
`destination-resolver.ts`, `public-link-page.tsx`, `/workspace/qrtub/src/lib/editor/property-schemas.ts`,
`/workspace/qrtub/src/lib/types/field-config.ts`, `/workspace/qrtub/src/lib/types/destination-config.ts`,
`/workspace/qrtub/src/lib/stripe-plans.ts`, and the `things` table migration
(`supabase/migrations/20250718000002_complete_schema.sql`).

---

### 1. SELF-CONTAINMENT

**Verdict: Mostly — a reader can write a working `{{item.field}}` URL template or a
condition from this page alone — but several real, verifiable gaps would produce wrong
answers or broken Destinations, and one core term is used throughout without ever being
defined.**

**"Destination" is never defined on this page.** The very first H3 says "Use `{{field.name}}`
syntax to insert field values into **Destination URLs**" (line 12), and the word
"Destination(s)" recurs in nearly every example and pattern (`## Common Patterns` alone uses
it 12+ times: "Destination: 'Forklift Inspection'", etc.) — but the page never states what a
Destination *is*. A cold reader who hasn't already read `/help/pages-overview` has no way to
know whether "Destination" means a button, a redirect rule, or something else. This is a
page-wide dependency, not a one-off.

**Missing field categories that are real and in production use.** The `## Available Fields`
section documents Item, Tub, Session, Device, and Theme fields — but omits two entire
categories that exist in the same context object and are resolved the same way:

- **Time fields** (`time.hour`, `time.dayOfWeek`, `time.dayOfMonth`, `time.month`,
  `time.year`, `time.isWeekend`) — defined in `TIME_PROPERTIES`
  (`qrtub/src/lib/editor/property-schemas.ts:233-264`), exposed in the visual editor's field
  picker, and *always* added to the destination-resolution context: `buildDestinationContext`
  in `destination-resolver.ts` unconditionally sets `context.time = buildTimeInfo()` (line 284
  — "Always add time info").
- **Request fields** (`request.country`, `request.city`, `request.language`, `request.ip`,
  `request.path`, `request.referrer`, `request.timestamp`) — defined in `REQUEST_PROPERTIES`
  (`property-schemas.ts:267-303`) and added to the same context whenever headers are available
  (`destination-resolver.ts:280`).

This isn't a hypothetical omission — the sibling page `/help/building-a-page.mdx` explicitly
sends readers here for it: *"Beyond Item and Tub fields, you can bind device and time
information — see [Using Fields](/help/using-fields) for the full reference."* (line 81-82).
That promise is broken; time information is not in this "full reference" at all, and an AI
agent asked "can I route by country" or "can I restrict a Destination to business hours"
would, working from this page alone, wrongly say no.

**The "Standard Item Fields" table doesn't distinguish guaranteed fields from optional ones.**
Per `qrtub/src/lib/types/field-config.ts`:
- `CORE_FIELDS = ['name', 'item_id', 'description', 'tags']` (line 337) — fixed DB columns,
  always present, cannot be disabled.
- `SYSTEM_FIELDS = ['id', 'tub_id', 'created_at', 'updated_at', 'image', 'destination_config']`
  (line 314) — also always present.
- Everything else in the page's table — `item_number`, `type`, `subtype`, `status`,
  `serial_number`, `location`, `owner` — comes from `DEFAULT_FIELD_DEFINITIONS`, a *library* of
  optional fields. `getStandardFieldConfig()` (line 427), the function backing a
  "start from scratch" Tub, enables **only the four core fields**: *"A 'start from scratch'
  tub holds ONLY the four core fields — nothing else... they're offered on demand from the
  global library instead"* (lines 428-432). A Tub that hasn't added the Status field has no
  `item.status` to reference — the page never says this, so a reader has no reason to check
  before building a condition on `item.status == "operational"` (the page's own leading
  example).

**The page also omits `item.item_id`, a field it implies doesn't exist.** `item_id` is one of
the four `CORE_FIELDS` — always present, cannot be disabled (`field-config.ts:337`) — and
`binding-translator.ts`'s `isCoreField()` check means `{{item.item_id}}` passes straight
through untranslated, so it resolves like any other core-field binding. It is the "identifier
users lead with" per the field's own default ordering (`DEFAULT_FIELD_ORDER`, line 271-272,
puts `item_id` right after `name`). Neither this page's table nor the app's own field picker
(`ITEM_PROPERTIES` in `property-schemas.ts`) list it — a real, always-available field is
invisible to a reader relying on either surface.

**`item.id` is documented with a misleading example.** The table gives `item.id` → `"item-456"`
as the example value. In the schema, `things.id` is `UUID PRIMARY KEY DEFAULT gen_random_uuid()`
(`supabase/migrations/20250718000002_complete_schema.sql:22`), and
`ThingSerializer.deserialize` sets `id: dbThing.id` directly (`thing-serializer.ts:111`) — so
the real value looks like `"3f8a91c2-4b77-4e1a-9c30-eb15b7a2f001"`, not a short friendly string.
A reader building `https://yourapp.com/equipment/{{item.id}}` (the page's own "Custom
Application" example, line 175) will get a UUID in that URL, not `item-456`.

### 2. ANSWER-FIRST

Quoting the first line of body content under every H2 (the page nests almost all content one
level down in H3s, so where the H2 itself carries no text, the immediate next line is quoted
and marked as such):

**"## Two Ways to Use Fields"** (line 8) — no text of its own; the next line is H3
`### 1. URL Templates (Double Curly Braces)` (line 10), which opens: *"Use `{{field.name}}`
syntax to insert field values into Destination URLs."*
Judgment: **The H2 itself fails** — zero framing sentence before the reader is inside
subsection #1. The H3 opener is fine on its own (11 words, direct), but a reader who wants
"what are the two ways" gets no single answer at the H2 level; they must read both H3 headers
to construct it themselves.

**"## Available Fields"** (line 35) — no text of its own; next line is H3 `### Item Fields`
(line 37): *"Items have **standard fields** (built-in) and **custom fields** (you define
them)."*
Judgment: **Same pattern** — H2 has no lead sentence stating what categories exist or how many
(five documented, two more real-but-missing per §1). The reader has to scroll the whole
section to discover the field-category list.

**"## URL Template Examples"** (line 133) — no text of its own; next line is H3
`### Basic Field Insertion` (line 135), followed immediately by bold label + fenced code, no
prose at all in this H2's span.
Judgment: **Fails** — pure example gallery, no answer sentence anywhere.

**"## Conditional Visibility Examples"** (line 178) — same structure: H3 `### Item Status`
(line 180) straight into bold label + code, no prose.
Judgment: **Fails** — same as above.

**"## Common Patterns"** (line 252) — no text of its own; H3
`### Pattern 1: Equipment-Specific Inspections` (line 254) opens: *"Different equipment types
route to different inspection templates."*
Judgment: **Partial** — the H3 opener is a reasonable 8-word direct statement, but again the
H2 itself has no summary sentence, and the pattern relies on the still-undefined term
"Destination" (see §1, §6).

**"## Important Notes"** (line 303) — opens directly into a bold subhead, `**Field Names:**`,
then a bullet list starting *"Use exact field names: `item.item_number` NOT `item.number`"*.
Judgment: **Good enough** — this section is reference-style (a list of gotchas), and each
bullet is self-contained and direct; no scene-setting preamble to complain about.

**"## Getting Help"** (line 324) — opens: *"For complex field usage:"* followed by a numbered
list.
Judgment: **Good** — five words of framing, then direct action items. No preamble problem.

**"## Related"** (line 333) — a link list, not a content section; answer-first doesn't apply.

**Summary:** the page has essentially no prose at the H2 level anywhere except the single
opening paragraph before all headings. Every content H2 immediately drops into an H3 or a
labeled code block. For a retrieval system chunking on H2, four of six content sections
("Two Ways to Use Fields," "Available Fields," "URL Template Examples," "Conditional
Visibility Examples") return zero direct-answer text before the reader hits either a
sub-heading or a bare code block.

### 3. ONE QUESTION PER PAGE

**This page is doing two jobs that should be separated: (1) the field reference (what fields
exist, their syntax, their failure modes) and (2) a routing-recipes cookbook — and the second
job substantially duplicates the sibling page `/help/conditional-visibility.mdx`, which has
its own, near-identical worked examples.**

Concretely:
- This page's `## Common Patterns` → `### Pattern 1: Equipment-Specific Inspections`
  ("Different equipment types route to different inspection templates... Destination:
  'Forklift Inspection'... Condition: `item.type == "forklift"`") is the same scenario as
  `conditional-visibility.mdx`'s `## Common Use Cases` → `### 1. Equipment Type-Specific
  Inspections` ("You manage forklifts and cranes... Condition for 'Forklift Inspection'
  Destination: `item.type == "forklift"`").
- This page's `### Pattern 3: Status-Dependent Actions` overlaps
  `conditional-visibility.mdx`'s `### 3. Test Status-Based Routing` — both teach
  status-field-based show/hide with the same `item.status`/`item.testStatus` shape.
- This page's entire `## Conditional Visibility Examples` gallery (Item Status, Tags,
  Equipment Type, Combining Multiple Conditions, Device-Based Conditions) covers the same
  ground `conditional-visibility.mdx` already owns end-to-end, including its own device
  section (`## Advanced: Device-Specific Destinations`) and its own "Using AI to Generate
  Conditions" workflow.

CLAUDE.md is explicit about this: *"Search for existing information before adding new
content. Avoid duplication unless strategic."* Two Pages-group siblings independently
teaching "show the forklift inspection only for forklifts" is unstrategic duplication, and
`conditional-visibility.mdx` itself already points back here calling this page *"the complete
field reference"* (line 165, 230) — meaning the two pages *should* be dividing labor
(reference vs. decision-guidance/cookbook), but both currently carry the same cookbook.

**Proposed split:**
- **Keep on this page**, because nothing else owns it: the binding syntax (`## Two Ways to
  Use Fields`, tightened), the complete `## Available Fields` reference (with Time and
  Request added — see §1/§5), and the failure-mode/limits material from `## Important Notes`.
- **Cut `## URL Template Examples`, `## Conditional Visibility Examples`, and
  `## Common Patterns` down to a small, non-overlapping set** — one or two examples per field
  category is enough to show the syntax; the "which pattern for which business problem"
  cookbook role belongs to `conditional-visibility.mdx`, which already has it (and already has
  the "when you need this / when you don't" decision framing this page lacks entirely).

The page is not too thin to stand alone — the opposite risk applies here: it should shed the
duplicated cookbook weight and keep its unique reference content.

### 4. HEADINGS AS QUESTIONS

Proposed only where the question form is a genuinely clearer retrieval target than the
current noun phrase:

| Current heading | Proposed | Rationale |
|---|---|---|
| "Two Ways to Use Fields" | "How do I reference a field?" | Matches how a user or support bot would actually phrase the question; "two ways" requires already knowing there are two. |
| "Available Fields" | "What fields are available?" | Direct question match for the page's core reference purpose. |
| "Item Fields" (H3) | "What Item fields can I use?" | A chunker splitting on H3 benefits from the question form when this table is retrieved alone. |
| "Tub Fields" (H3) | "What Tub fields can I use?" | Same reasoning. |
| "Session Fields" (H3) | "What session/user fields are available?" | Same reasoning, and see §5 — this heading should also carry the "usually null" caveat. |
| "Device Fields" (H3) | *(leave as-is)* | Already cross-referenced in depth by `/help/device-detection`; noun form is fine here since the H3 body opens with a direct definition. |
| "Theme Fields" (H3) | *(leave as-is)* | Same — short, unambiguous category label. |
| "URL Template Examples" / "Conditional Visibility Examples" / "Common Patterns" | *(leave as labels, but see §3 — recommend trimming instead of renaming)* | These are example galleries, not single answerable questions; converting to question form doesn't fix the actual problem, which is duplicated content. |
| "Important Notes" | "What should I watch out for when using fields?" | More specific than a generic label; matches what a reader is actually trying to find (gotchas). |
| "Getting Help" / "Related" | *(leave as-is)* | Standard navigational sections. |

### 5. EDGE CASES / LIMITS / FAILURE MODES

Treating absence as a defect:

1. **Two entire field categories are missing** (Time, Request) — covered fully in §1. An AI
   agent would incorrectly deny that business-hours or country-based routing is possible.
2. **No CEL expression limits are documented anywhere on this page or on
   `conditional-visibility.mdx`.** `qrtub/src/lib/page/bindings.ts` enforces real ceilings via
   `CEL_VALIDATION_LIMITS` (lines 223-227): max expression length **500 characters**, max
   nesting depth **10**, max operator count **20** — checked by `validateBindingExpression`,
   which is wired into the actual condition-editing UI
   (`DestinationRulesEditor.tsx:78`, `PropertyForm.tsx:288`). The page's own
   `## Combining Multiple Conditions` section teaches readers to build compound expressions
   like `(item.type == "crane" || item.type == "forklift") && item.status == "operational"`
   with no indication that stacking several more of these will eventually be rejected, or what
   the error looks like.
3. **The "Missing Fields" guidance is incomplete for the case that matters most.** The page
   states: *"If a field is empty/null, URL templates insert empty string."* (line 315). That
   describes the low-level per-token substitution in `resolveBindings`, but it is not what a
   reader observes for an actual Destination. `destination-resolver.ts`'s `resolveDestination()`
   calls `resolveBindingsForUrl`, and when any binding in a rule's URL is unresolved
   (missing/null/empty, or an object/array value), the code does:
   ```
   if (result.unresolvedPaths.length > 0) {
     bindingErrors.push(`Rule ${i}: Skipped - unresolved: ...`);
     continue;   // the ENTIRE rule is skipped
   }
   ```
   (lines 58-61) — falling through to the next rule or `defaultLink`, not delivering a URL
   with a blank segment. A reader troubleshooting "why didn't my Destination fire, or why did
   the wrong one fire" would be misled by the current wording into looking for a broken link
   with an empty query param, when the actual symptom is a *different rule (or none) taking
   over entirely*.
4. **The array-handling advice doesn't say how, and may not be possible at all.**
   `## Important Notes` says: *"Cannot directly insert arrays in URL templates (convert to
   string first)."* (line 322) — but no mechanism for "converting to string first" is
   documented anywhere in the docs, and the source suggests there isn't one exposed to users:
   `resolveBindingsForUrl`'s own comment states *"An object/array binding would stringify to
   '[object Object]' / 'a,b' — a broken URL that would otherwise count as 'resolved'. Treat it
   as unresolved instead."* (bindings.ts:416-419) — i.e. the code deliberately treats any
   array/object binding as a failed (skipped) binding rather than emitting a joined string.
   "Convert to string first" implies a capability that doesn't appear to exist; this should
   either be corrected to "arrays cannot be inserted into URL templates" or the actual
   conversion mechanism (if one exists elsewhere, e.g. a template helper) should be named.
5. **No plan-tier statement.** Silent on whether field binding, URL Templates, or conditional
   visibility require a paid plan. Verified against `qrtub/src/lib/stripe-plans.ts`: the
   $5/month Starter plan already includes *"Drag and drop landing page editor"* with no
   separate mention of field binding or conditional visibility as a gated add-on anywhere in
   the plan feature lists — so as far as the source shows, this is available on every plan.
   That should be stated explicitly (a one-line "not gated by plan"), rather than left for an
   AI agent to guess.
6. **Session fields never state when `session.user` is actually populated.** The page's own
   hedge — *"Access information about the logged-in user (if authenticated)"* — is present but
   thin. Verified in `public-link-page.tsx:236-250`: the session passed into the page context
   comes from `supabase.auth.getUser()` on the **anonymous public scan request** — for the
   overwhelming majority of real-world scans (a member of the public scanning an item's QR
   code), there is no signed-in session, so `session.user` is `null` and every session-based
   condition or binding is dead weight. It's only non-null when the person viewing the page at
   that moment happens to be signed in (e.g., a team member checking their own item). This is
   exactly the kind of caveat a support agent needs and currently has to infer.

### 6. CHUNK INTEGRITY

Each H2 evaluated as if it were the only thing retrieved:

- **"Two Ways to Use Fields"** — **Mostly self-contained**: both numbered H3s carry their own
  syntax and example. Weak point: uses "Destinations" and "Destination URLs" without defining
  the term (see §1) — a reader with zero prior context knows the mechanics but not what
  they're mechanics *for*.
- **"Available Fields"** — **Self-contained** for the categories it covers (each H3 carries a
  full table + example); the defect here is completeness (§1), not isolation.
- **"URL Template Examples"** — **Fails in isolation.** Retrieved alone, this section is nothing
  but bare code blocks (`https://api.example.com/assets/{{item.serial_number}}`, etc.) with no
  reminder of what the `{{ }}` syntax means or that it only works inside a Destination URL —
  that explanation lives entirely in the earlier "Two Ways to Use Fields" section. A chunk
  retrieved here alone teaches nothing about the mechanism, only shows it in use.
- **"Conditional Visibility Examples"** — **Fails in isolation** for the same reason: no
  reminder of what a "condition" is, where it's entered, or what evaluating `true` vs. `false`
  does to a Destination. Pure boolean-expression snippets with a category label.
- **"Common Patterns"** — **Fails in isolation**, and more severely: each pattern is written as
  `**Destination: "Forklift Inspection"**` / `- URL: ...` / `- Condition: ...` bullet fragments
  that assume the reader already has both (a) the `{{ }}`/CEL vocabulary from earlier sections
  and (b) the concept of a "Destination" as a named, orderable thing on a Page — neither of
  which is established inside this section, and (b) is never established anywhere on this
  page (§1).
- **"Important Notes"** — **Self-contained.** Each bullet (field-name case sensitivity, URL
  encoding, missing fields, arrays) reads fine as an independent fact, though see §5 for
  factual completeness issues within it.
- **"Getting Help"** — **Self-contained**; item 1 points to an anchor on
  `conditional-visibility.mdx` as a "for more" pointer, which is fine (external enrichment, not
  a dependency).
- **"Related"** — Pure link list; no content to assess for isolation.

**Net: 3 of 6 content H2s ("URL Template Examples," "Conditional Visibility Examples,"
"Common Patterns") depend on vocabulary and concepts established only in earlier sections of
the same page, and all three inherit the page-wide undefined-"Destination" problem from §1.**

---

### Recommendation

A substantive rewrite is warranted, not a tweak. Two categories of real, verified fields
(Time, Request) are missing from what a sibling page calls "the full reference"; the
"Standard Item Fields" table doesn't distinguish the four always-present core fields from
optional library fields that a given Tub may not have enabled, and omits the one field
(`item.item_id`) that actually is always present; the stated failure mode for a missing field
in a URL template describes the wrong layer of the system (per-token substitution rather than
the rule-skipping behavior a reader will actually observe); no CEL complexity limits are
documented anywhere; "Destination" is used dozens of times and defined zero times; and roughly
half the page duplicates worked examples that `/help/conditional-visibility.mdx` already owns
in more depth. A proposed replacement has been written to
`/workspace/mintlify-docs/audit/proposed/help__using-fields.md`.


**Proposed rewrite:** `audit/proposed/help__using-fields.md`

---

## Conditional Visibility

**Source page:** `help/conditional-visibility` &nbsp;|&nbsp; **Needs rewrite:** Yes

Live: https://help.qrtub.com/help/conditional-visibility
Nav group: Pages (`docs.json` → tab "Docs" → group "Pages": pages-overview, building-a-page, using-fields, **conditional-visibility**, device-detection, app-links)
Siblings skimmed: `help/using-fields.mdx`, `help/device-detection.mdx`

Verification sources: `../qrtub/src/lib/page/bindings.ts`, `destination-resolver.ts`, `section-processors.ts`, `context.ts`, `binding-validator.ts`, `../qrtub/src/lib/types/destination-config.ts`, `../qrtub/src/lib/utils/device-detection.ts`, `../qrtub/src/components/blocks/LandingPageEditor/panels/PropertiesPanel/PropertyForm.tsx`, `../qrtub/src/components/blocks/DestinationRulesEditor/DestinationRulesEditor.tsx`, `../qrtub/BRAND.md`, plus a live `node -e` smoke test against the `cel-js` package actually used in production.

---

### 1. SELF-CONTAINMENT

A cold reader could get the three worked examples in "Common Use Cases" working, but would hit real gaps trying to do anything else:

- **No instructions for where to enter a condition.** The page repeatedly says "Add the condition to each Destination" (line 42, 56, 72) but never says where that field lives. Verified in `PropertyForm.tsx`: it's the **"Show When (CEL Expression)"** field inside the **Visibility** group of a Destination's Properties panel in the Page editor, with helper text "Control when this component is shown. Type to see suggestions." None of that appears on the page.
- **CEL is never defined until the second-to-last section.** "Getting Help" (line 220) is the first place the page says "Conditional visibility uses CEL (Common Expression Language), an industry standard" — after ~190 lines of CEL syntax examples. A reader landing cold has been shown `item.type == "forklift"`-style syntax for the whole page before learning what language it even is.
- **The operator list in the AI prompt template is incomplete and wrong by omission.** Line 90: "Available operators: ==, !=, ||, &&, >, <, >=, <=" — missing `in` (used two examples later, line 104) and missing `!` (used in the Mitti example, line 139) and `size()` (documented on the sibling `using-fields.mdx` but never mentioned here). A reader following only this page's template would under-prompt ChatGPT and get worse expressions than the page's own later examples use.
- **No mention that `time.*` and `request.*` fields exist at all.** Verified in `context.ts`: every page render context always includes `time` (hour, dayOfWeek, dayOfMonth, month, year, isWeekend — UTC) and, when headers are present, `request` (country, city, language, ip, path, referrer). These are listed as `ALWAYS_AVAILABLE_PREFIXES` in `binding-validator.ts` alongside `device`. The page's "Available Fields" section (lines 158–164) lists Item/Custom/Tub/Device/Session fields and stops — no `time`, no `request`. A reader who wants "only show this on weekends" or "only show this to visitors in Australia" has no way to discover these fields exist, on this page or on `using-fields.mdx` (same gap there).
- **No statement of what happens on a bad condition.** Verified live (see §5): an undefined field reference throws inside `cel-js`, is caught by `evaluateCondition()`, and the whole condition silently becomes `false` — the Destination just never appears, with no error shown anywhere in the product's public-facing surface. The page never says this, so a reader debugging "my Destination isn't showing" has no diagnostic model to reach for.
- **No numeric ceilings.** `bindings.ts` defines `CEL_VALIDATION_LIMITS`: max 500 characters, max nesting depth 10, max 20 operators. The page invites combining "multiple conditions" (line 78, 84–86) and shows a 3-clause nested example (the Mitti workaround) without ever stating there's a ceiling.
- **No plan-tier statement.** `BRAND.md` §1.4 lists "Conditional visibility (CEL) | Available (advanced)" with no plan restriction elsewhere in the file. The page never says this explicitly either way — silence here is exactly the gap an AI support agent fills with a guess ("this is probably a Pro feature").

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

- **"When You Need This"** → **"When Do You Need Conditional Visibility?"** — genuinely clearer; the section is a binary decision, and the question form matches how a reader would actually ask it.
- **"When NOT to Use This"** → given the duplication finding above, this shouldn't survive as a separate heading at all; if merged with the above it becomes one section answering **"Should You Use Conditional Visibility?"**
- **"Using AI to Generate Conditions"** → **"How Do I Use AI to Generate a CEL Condition?"** — clearer; matches the literal question a reader has when they hit a condition too complex to write by hand.
- **"Available Fields"** → **"What Fields Can I Use in a Condition?"** — clearer, and disambiguates from `using-fields.mdx`'s own "Available Fields" heading (right now both pages use the identical heading text for different-sized versions of the same list, which is confusing for anyone comparing search results).
- **"Common Use Cases"** — leave as a noun phrase; it's a container for H3 scenarios, not itself an answer to one question.
- **"Tips"**, **"Related"**, **"Getting Help"** — conventional section labels, not natural questions; converting them would feel forced. Leave as-is.
- The H3 scenario titles ("1. Equipment Type-Specific Inspections," "2. Tag-Based Routing," "3. Test Status-Based Routing") are fine as scenario labels — each is immediately followed by a **Scenario:**/**Solution:** pair that already does the answering work a question-form heading would do. Not worth converting.

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Summary

The page's unique, non-duplicated content (the decision framework, the three Item-based use cases, and the AI-prompt workflow) is solid. The defects are: (1) real product mechanics never explained — where to enter a condition, what CEL even is, and the silent-`false` failure mode for bad field references; (2) an entire undocumented field category (`time.*`, `request.*`) that's been live in the render context all along; (3) heavy duplication with two sibling pages (device-detection.mdx, using-fields.mdx) that will drift out of sync; and (4) missing numeric ceilings and plan-tier statement that are exactly the kind of gap an AI support agent will paper over with an invented answer. A substantive rewrite is warranted — see `/workspace/mintlify-docs/audit/proposed/help__conditional-visibility.md`.


**Proposed rewrite:** `audit/proposed/help__conditional-visibility.md`

---

## Device Detection & Routing

**Source page:** `help/device-detection` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/device-detection.mdx`
**Live:** https://help.qrtub.com/help/device-detection
**Nav group:** Pages (`help/pages-overview`, `help/building-a-page`, `help/using-fields`,
`help/conditional-visibility`, `help/device-detection`, `help/app-links`)
**Siblings skimmed:** `help/conditional-visibility.mdx`, `help/using-fields.mdx`,
`help/building-a-page.mdx`, `help/pages-overview.mdx`, `help/app-links.mdx`
**Source verified against:** `qrtub/src/lib/utils/device-detection.ts`,
`qrtub/src/lib/page/destination-resolver.ts`, `qrtub/src/lib/page/bindings.ts`,
`qrtub/src/components/blocks/DestinationConfiguration/DestinationConfiguration.tsx`,
`qrtub/src/components/blocks/DestinationRulesEditor/DestinationRulesEditor.tsx`,
`qrtub/src/components/blocks/AddEditItemForm/AddEditItemForm.tsx`,
`qrtub/src/lib/types/landing-page-config.ts`, `qrtub/src/lib/stripe-plans.ts`

### Headline finding

The page documents only one of the two shipped mechanisms for routing by device, and it is
arguably the more complicated one for most of its own use cases. There is a second, simpler,
fully shipped mechanism — **per-Item "Conditional Rules" on a single Destination** (a checkbox
labelled "Enable conditional routing", a "When… Then…" rule list evaluated first-match-wins,
an "Else" connector between rules, and a "Default URL" fallback, all inside the Item's
"Destination Link" radio option, on the Item's **Destination** tab, in
`AddEditItemForm`) — that the page never mentions. It requires no
Page at all (works in Direct Mode) and is a closer match to nearly every scenario the page
walks through (mobile app vs. web app, app-store routing, the Mitti iOS Safari workaround),
which are all "one silent redirect that differs by device," not "show the visitor two buttons
and let them pick." This is detailed under Self-Containment and Edge Cases below, and is why a
rewrite is proposed.

---

### 1. SELF-CONTAINMENT

A reader landing here cold, unable to follow a link, could correctly write CEL device
conditions (the field reference is accurate and complete) but **could not actually complete
either of the two real setup paths**, for these concrete reasons:

- **The page never names the UI control it tells you to use.** Every use case says "Set
  condition: `device.isMobile`" or "Condition: `device.isIOS`" as if "condition" were a
  labelled field. The actual control, per `help/building-a-page.mdx`, is: *"Every section has
  a **Visibility** setting that takes a condition"* (line 89). A cold reader has no way to know
  they're looking for a field called **Visibility** in the page editor's Properties panel — the
  word "Visibility" does not appear anywhere in `device-detection.mdx`.

- **Missing prerequisite: Pages must be switched on for the Tub.** Every "Setup" block in this
  page assumes multiple Destinations exist on a Page, but per `help/building-a-page.mdx`:
  *"Pages must be switched on for the Tub. Open the Tub's settings and turn on the page option
  (labelled **Show a profile page** in the current app). If it is off, scans go straight to a
  single Destination and there is no page to edit."* This page never says that, so a reader
  starting from "Create a 'Mobile App' Destination" has nowhere to click — there is no page
  to add a second Destination to yet.

- **An entire, simpler, shipped mechanism is missing.** Verified in
  `DestinationConfiguration.tsx` and `DestinationRulesEditor.tsx`: on an Item's own
  **Destination** tab (verified label in `AddEditItemForm.tsx`'s `tabs` array: `{ id:
  'destination', label: 'Destination' }`), with the **Destination Link** radio option selected
  (Direct Mode — no Page required), there is a **"Conditional Rules"** editor:
  a checkbox "Enable conditional routing," an "Add Rule" button that adds a `When [condition]
  → Then [redirect to URL]` pair (each rule can also carry its own app-link Fallback URL/Message
  and an optional Label), rules connected by an "Else" divider and badged "First match wins,"
  and a "Default URL (fallback when no rules match)" field below. This is the mechanism a
  single QR code would actually use to *silently* redirect a mobile visitor to the app and a
  desktop visitor to the web app — no visible second button, no Page needed. The current page's
  own use cases ("Mobile App vs Web App," "Platform-Specific App Downloads," the Mitti Safari
  workaround) all describe exactly this shape of problem, yet the page only ever tells the
  reader to build it with two visible Page Destinations plus per-Destination Visibility. That's
  a valid alternative (see next point) but presenting it as the *only* path is a real gap.

- **No mention of the practical trade-off between the two mechanisms.** Page-mode Destinations
  are Tub-wide — per `help/building-a-page.mdx`, "You build one layout for a Tub, and every Item
  in that Tub renders it with its own data" — so one Visibility condition covers every Item.
  The Conditional Rules editor, by contrast, is **per-Item** (it only appears in
  `AddEditItemForm`, not in any Tub-level template or bulk editor) — verified by grepping
  `DestinationConfiguration`/`DestinationRulesEditor` usage across the whole `src/components`
  tree; the only import site is `AddEditItemForm.tsx`. A reader with hundreds of items who wants
  every one of them to silently redirect by device would need to configure Conditional Rules on
  each Item individually, whereas the same outcome via a Page Visibility condition is set once.
  This is exactly the kind of ceiling an AI support agent needs and currently has no way to
  state correctly.

- **No plan-tier statement.** Checked `qrtub/src/lib/stripe-plans.ts`: device fields,
  conditional visibility, and the landing-page editor are not gated by plan at all — Starter
  already includes "Drag and drop landing page editor," and neither `DestinationConfiguration`
  nor `DestinationRulesEditor` reference plan/feature-flag checks anywhere. The page never
  says this is available on every plan, so an AI agent has nothing to cite when a user asks "do
  I need to upgrade for this?" and may guess wrong in either direction.

**Verdict:** Not self-contained. The field reference alone (device.type/os/browser + flags) is
accurate and complete, but the actual "how do I make this happen" instructions are incomplete
in a way that would leave a cold reader stuck at the first step.

---

### 2. ANSWER-FIRST

Quoting the literal opening text of every H2, in document order:

- **"## Why Device Detection Matters"** opens: *"Different devices need different
  experiences:"* — a one-line lead-in to a bullet list, not itself an answer to "why does this
  matter." No standalone 40-60 word answer exists; you must read the four bullets plus the
  closing line to get the actual point. **Preamble, not answer-first.**

- **"## Common Use Cases"** has **zero lead sentence** — the H2 goes directly to `### 1. Mobile
  App vs Web App`. There is nothing here that answers any question at the H2 level at all.
  **Empty opener** — worse than preamble.

- **"## Available Device Information"** also has **zero lead sentence** — goes straight to
  `### Device Type` / `**Field:** \`device.type\``. Same defect as above.

- **"## How to Set Up Device Routing"** opens directly with the numbered list: *"1. **Create
  your Destinations** - One for each device scenario"*. This is reasonably answer-first for a
  procedural heading (the reader gets the actionable step immediately) but it is three short
  fragments, not a 40-60 word framing of what the procedure assumes (a Page must already exist,
  Destinations means Page sections) — so it answers "what do I do" but not "what does this
  presuppose."

- **"## Combining Device Detection with Item Data"** opens: *"You can combine device detection
  with Item fields for powerful routing:"* — this is the one H2 on the page that genuinely
  answers its own implied question ("can I combine these?") in a single direct sentence, though
  at 11 words it's well under the 40-60 word target and gives no mechanism detail before the
  example.

- **"## Important Notes"** opens directly into a bolded sub-label: *"**User-Agent
  Detection:**\nDevice detection uses browser User-Agent strings. These are generally reliable
  but:"* — no topic sentence for the H2 itself; the heading is a grab-bag (see Heading and Chunk
  Integrity sections below) so there is no single question it could answer-first.

- **"## Testing Your Setup"** opens: *"Before deploying QR codes with device routing:"* —
  preamble before the numbered list, not an answer.

- **"## When NOT to Use Device Detection"** opens: *"**Don't use device detection for:**"* —
  functionally this IS the answer (a direct bulleted list of the "don't" cases immediately),
  so this is the second-best-performing H2 on the page for answer-first structure, just not
  written as flowing prose.

- **"## Related"** — a links list, not subject to answer-first (as expected for a Related
  section).

**Verdict:** 1 of 8 content H2s (arguably 2, counting "When NOT to Use") opens with something
that functions as a direct answer; the rest open with scene-setting fragments or, in two cases,
nothing at all before dropping into subheadings. This will read fine to a human scanning
top-to-bottom, but a chunk-retrieval system handing an LLM just the H2 heading + its first
sentence gets little to nothing to work with for "Common Use Cases" and "Available Device
Information" specifically.

---

### 3. ONE QUESTION PER PAGE

This page is not thin — if anything it's carrying more than one distinct kind of content:

1. **A reference table** (device.type / device.os / device.browser, values, convenience flags,
   examples) under "## Available Device Information."
2. **A procedural task** ("how do I make different destinations show for different devices").
3. **A judgment/decision question** ("should I even use this feature").
4. **A testing checklist.**

The reference table (#1) is a genuine, near-verbatim triplicate across three sibling pages:

- `help/using-fields.mdx` has a complete **Device Fields** table (lines 107-122) with the exact
  same fields, types, and example values, plus its own pointer: *"See Device Detection for
  detailed device routing examples."*
- `help/conditional-visibility.mdx` has its own **"### Quick Device Field Reference"**
  (lines 167-191) — restating `device.type`/`device.os`/`device.browser` and every convenience
  flag again, with nearly identical example snippets (`device.type == "mobile"`,
  `device.os == "ios"`, `device.browser == "safari"`), immediately after telling the reader
  *"See [Using Fields](/help/using-fields) for complete field reference..."* — i.e., it points
  to the canonical source and then restates the table anyway.
- `device-detection.mdx` restates it a third time under "## Available Device Information,"
  including the same three example blocks per field.

This directly contradicts the project's own stated content strategy (`mintlify-docs/CLAUDE.md`):
*"Search for existing information before adding new content. Avoid duplication unless
strategic."* `using-fields.mdx` is clearly positioned as the canonical field reference (it also
covers Item/Tub/Session/Theme fields in the same table format) — the other two pages copying
its Device Fields section is unstrategic duplication, not intentional redundancy.

**Proposed split:**
- Keep `using-fields.mdx` as the single canonical source for field names/types/values (it
  already is, and already covers every field family, not just device).
- Shrink `device-detection.mdx`'s "## Available Device Information" to a two-or-three line
  summary (just enough to make the page's own examples legible without a click) plus a link to
  `using-fields.mdx#device-fields` for the full table — matching the "one source, referenced —
  not duplicated" principle `CLAUDE.md` states for code facts, applied here to field references.
  Do not delete it outright: the answer-first and chunk-integrity checks above depend on some
  device-field context surviving in this page for standalone retrieval, so trim rather than
  cut.
- Shrink `conditional-visibility.mdx`'s "### Quick Device Field Reference" the same way (out of
  scope for this single-page audit, but noted since it's the same defect against the same
  canonical source).

The procedural, judgment, and testing content (#2-#4) are legitimately one task ("route by
device") and its natural sub-questions (how, when to, how to verify) — they don't need
splitting into separate pages. The page is not too thin to stand alone.

**Verdict:** Split the reference table out (de-duplicate against `using-fields.mdx`); keep the
procedural/decision/testing content together as currently scoped.

---

### 4. HEADINGS AS QUESTIONS

Only proposing rewrites where the question form is a genuine retrieval/clarity improvement —
several headings are already effectively questions and are left alone.

| Current heading | Verdict |
|---|---|
| Why Device Detection Matters | Keep — already frames a rationale; a question form ("Why does device detection matter?") is not clearer. |
| **Common Use Cases** | Rewrite → **"When would I use device-based routing?"** — the section is entirely decision-support (five scenarios), and a question heading signals that intent for retrieval better than a generic label. |
| **Available Device Information** | Rewrite → **"What device information can I use in conditions?"** — directly matches the content, and pairs naturally with the `using-fields.mdx` pointer proposed above. |
| **Device Type** / **Operating System** / **Browser** (H3s) | Rewrite → **"What values does `device.type` return?"** / **"What values does `device.os` return?"** / **"What values does `device.browser` return?"** — each section is answering exactly this question and nothing else; the question form is unambiguous where "Device Type" alone could be mistaken for a settings/config topic. |
| How to Set Up Device Routing | Minor tighten → **"How do I set up device-based routing?"** — already question-shaped ("How to..."), low-priority polish only. |
| Combining Device Detection with Item Data | Rewrite → **"Can I combine device detection with Item fields?"** — matches the section's own opening sentence ("You can combine...") and is a genuinely common query shape. |
| **Important Notes** | This is the worst-performing heading on the page for retrieval — it is a grab-bag covering four unrelated points (UA reliability, security posture, fallback defaults, "always have an escape hatch"). Split into named questions instead of one vague H2: **"How reliable is User-Agent detection?"**, **"Can I use device detection to restrict access?"**, **"What happens if QRtub can't detect the device?"**, **"Should every Page always have a fallback Destination?"** A vague heading here is precisely where an AI agent retrieving "Important Notes" alone has no idea what's inside without reading the whole section — the heading itself carries none of the section's four topics. |
| Testing Your Setup | Rewrite → **"How do I test device routing before deploying?"** — the section is a checklist answering exactly this. |
| When NOT to Use Device Detection | Keep — already effectively a question in imperative form; no clearer question-form rewrite available. |
| Related | N/A — not subject to this heuristic. |

---

### 5. EDGE CASES / LIMITS / FAILURE MODES

**What the page already states correctly** (verified against `device-detection.ts`):
- User-Agent can be spoofed or masked by privacy browsers (page: *"Users can modify their
  User-Agent (rare)"* / *"Some privacy browsers mask device info"*) — accurate framing.
- "New devices/browsers might be detected as `'unknown'`" — accurate; `parseUserAgent` defaults
  `os`/`browser` to `'unknown'` when no regex matches.
- Fallback behaviour when detection fails — *"Type: `desktop`, OS: `unknown`, Browser:
  `unknown`"* — verified exactly against both `getDeviceInfoFromNavigator`'s no-`window` branch
  and `parseUserAgent`'s default `type = 'desktop'`.
- "Not for Security" warning — correct and appropriately blunt; nothing in
  `destination-resolver.ts` or `bindings.ts` treats device fields as an auth mechanism.
- "Always Provide Options" (a fallback Destination with no condition) — good, matches the
  `defaultLink`/"Default URL" fallback pattern that actually exists in the resolver.

**Gaps — stated with no evidence, or missing outright:**

1. **No plan-tier statement**, as detailed in Self-Containment §1. Verified not gated in
   `stripe-plans.ts` — worth stating explicitly ("available on every QRtub plan") precisely
   because *silence* here is what causes an AI support agent to invent a tier requirement.

2. **The entire per-Item "Conditional Rules" mechanism is absent** — see Self-Containment §1.
   This is the single biggest capability gap on the page.

3. **Silent-false condition typos are not mentioned.** `bindings.ts`'s `evaluateCondition` is
   the same evaluator behind every condition on this page (Page Visibility and Conditional
   Rules alike), and per this repo's own `qrtub/CLAUDE.md`: *"An undefined identifier makes the
   whole condition evaluate to `false` silently... An invented value ... produces a rule that
   never fires and no error message."* A typo like `device.isMoblie` will not error anywhere in
   the UI the reader is likely to notice quickly — the Destination just never matches. The page
   never warns that a bad device condition fails **silently**, which is exactly the class of bug
   an AI support agent would otherwise have to guess at ("did you spell the field right?" is not
   something it can suggest confidently without this being documented).

4. **Tablet misdetection on iPad is a real, silent gotcha the regex itself predicts** — verified
   directly in `device-detection.ts`: `isTabletUA = /ipad|android(?!.*mobile)|tablet/` depends on
   the literal substring `"ipad"` appearing in the User-Agent. iPads running Safari with
   "Request Desktop Website" (the platform default in recent iPadOS versions) send a
   Mac-style User-Agent with no `"ipad"` token, so such a device is classified `type: 'desktop'`,
   `os: 'macos'` — not `tablet`/`ios`. The page's "Tablet-Optimised Experience" use case and its
   "Fallback Behaviour" note both talk about *undetectable* devices, but never about a device
   that *is* detected, just detected as the wrong bucket. This is worth a one-line caveat.

5. **Bots, crawlers, and link-preview unfurlers are not mentioned as a source of `'unknown'`/
   `'desktop'` results.** When Slack, iMessage, or a search-engine crawler fetches the link
   server-side to build a preview, its User-Agent won't match any of the mobile/tablet/OS/
   browser regexes, so it resolves to the same "desktop, unknown OS, unknown browser" bucket as
   genuine undetectable traffic. This is a common real-world support question ("why did the
   Slack preview show the desktop version?") that the current "Fallback Behaviour" note doesn't
   anticipate.

6. **No mention that device context is re-evaluated on every scan, not cached per QR code or
   per visitor.** Minor, but worth one sentence so a reader doesn't wonder whether switching
   phones changes the outcome (it does, immediately — verified: `getDeviceInfoFromHeaders` reads
   the request's own `user-agent` header fresh on every hit in `destination-resolver.ts`).

---

### 6. CHUNK INTEGRITY

Testing each H2 (and, where a chunker would split at H3, each H3) in isolation, no surrounding
page:

- **"## Why Device Detection Matters"** — self-contained. No dangling references.
- **"## Common Use Cases"** (H2 alone, no children) — **broken in isolation**: there is no text
  at all between the heading and the first `### 1.` subheading, so a chunk containing only this
  H2's "own" text would be empty. If a chunker instead retrieves individual `###` items:
  - `### 1. Mobile App vs Web App` — self-contained.
  - `### 2. Platform-Specific App Downloads` — self-contained.
  - `### 3. iOS Safari Deep Link Workaround (Mitti Example)` — self-contained; the one internal
    cross-reference ("the [Fallback URL feature](/help/app-links) handles that automatically")
    is an explicit link, not a bare "see above," so it survives isolation fine.
  - `### 4. Tablet-Optimised Experience` — self-contained.
  - `### 5. Browser-Specific Features` — self-contained.
- **"## Available Device Information"** (H2 alone) — same defect as "Common Use Cases": zero
  lead text, straight into `### Device Type`. Each child H3 (`Device Type`, `Operating System`,
  `Browser`) is, individually, fully self-contained (Field / Values / Convenience flags /
  Examples, no forward or backward references).
- **"## How to Set Up Device Routing"** — self-contained; the ASCII "Example Page" tree doesn't
  reference anything defined earlier in the document.
- **"## Combining Device Detection with Item Data"** — self-contained.
- **"## Important Notes"** — the *content* is self-contained sentence-by-sentence (no "as
  discussed above" or "this" with an unclear antecedent), but the *heading* gives zero signal
  about what's inside (see Heading section) — a retrieval system matching on heading text alone
  would never surface this section for "what happens if a phone can't be detected" or "is this
  secure," even though both answers live here.
- **"## Testing Your Setup"** — self-contained; closing line ("Scan the same QR code from each
  device...") doesn't depend on anything earlier.
- **"## When NOT to Use Device Detection"** — self-contained.
- **"## Related"** — a links list; not applicable to this check.

**Verdict:** No pronoun/"as above"-style breakage anywhere on the page — that's a genuine
strength. The only chunk-integrity defects are the two H2s ("Common Use Cases," "Available
Device Information") that carry no text of their own before dropping into subheadings, which
matters only if a chunker ever retrieves an H2 span without its children.

---

### Summary of required changes

1. Add the missing per-Item **Conditional Rules** mechanism (checkbox, rule list, Else/first-
   match-wins, Default URL) as a documented path — verified in
   `DestinationRulesEditor.tsx`/`DestinationConfiguration.tsx`.
2. Name the actual UI control ("Visibility") wherever the page currently says "condition:".
3. State the Page-mode prerequisite ("Pages must be switched on for the Tub").
4. State the Tub-wide vs. per-Item scope difference between the two mechanisms.
5. State plan availability explicitly (not gated, verified against `stripe-plans.ts`).
6. Add the four missing edge cases: silent-false condition typos, iPad-as-desktop
   misclassification, bot/crawler unfurler traffic, per-scan (not cached) evaluation.
7. Trim "## Available Device Information" against the `using-fields.mdx` canonical table.
8. Rewrite the flagged headings, especially splitting "Important Notes" into named questions.
9. Give every H2 (especially "Common Use Cases" and "Available Device Information") a real
   lead sentence before its children.

Given the scope of #1-#4 in particular (a missing shipped mechanism, not a wording nit), a full
replacement draft has been written to
`/workspace/mintlify-docs/audit/proposed/help__device-detection.md`.


**Proposed rewrite:** `audit/proposed/help__device-detection.md`

---

## App Links & Fallback URLs

**Source page:** `help/app-links` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/help/app-links.mdx`
**Live:** https://help.qrtub.com/help/app-links
**Nav group:** Pages (`help/pages-overview`, `help/building-a-page`, `help/using-fields`, `help/conditional-visibility`, `help/device-detection`, `help/app-links`)
**Siblings skimmed:** `help/device-detection.mdx`, `help/conditional-visibility.mdx`, `help/key-concepts.mdx`, `integrations/safetyculture.mdx`

Source verified against: `qrtub/src/lib/hooks/useAppLinkHandler.ts`, `qrtub/src/lib/utils/app-link-open.ts`, `qrtub/src/lib/utils/url-helpers.ts`, `qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`, `qrtub/src/components/blocks/AppLinkFallbackWarning/AppLinkFallbackWarning.tsx`, `qrtub/src/components/page/AppLink.tsx`, `qrtub/src/components/page/{Button,Link,ActionLink}.tsx`, `qrtub/src/lib/page/destination-resolver.ts`, `qrtub/src/lib/page/public-link-resolution.tsx`, `qrtub/src/lib/page/bindings.ts`.

---

### 1. Self-containment

A cold reader landing on this page (no link-following) can mostly configure a fallback URL, but three things would leave them either stuck or wrong:

- **Undefined vocabulary.** The page uses "Destination editor," "Link settings," "Item editor," and "Item" throughout (e.g. "In the Destination editor:" line 43; "In the Link settings" in the table at line 92) without ever defining what a Destination, Link, Item, or Page is. `help/key-concepts.mdx` defines all four (`## The Three-Entity Model`, `## Destinations`) but is only reachable via the "Related" list at the very bottom (line 138), after the reader has already been asked to act on these terms. A reader who has never opened `key-concepts` cannot locate "the Destination editor" from this page alone.
- **The two different failure experiences are never distinguished — this is the biggest gap.** App links render through two different code paths depending on how the Destination is used, and they produce visibly different results for the exact same Fallback Message setting:
  - **Single-Destination Item (Direct Mode)**: the scan server-redirects to `AppLinkOpener` (`qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`), a full-page component. It shows a spinner ("Opening app..."), and on timeout a branded card with the fallback message (or a default "App not available" heading + explanatory copy) plus **"Try Again" and "Go Back" buttons**.
  - **A Destination used as a button/link on a Page** (`ActionLink`, `Button`, `Link` in `qrtub/src/components/page/`, all wrapping `AppLink.tsx` → `useAppLinkHandler.ts`): on timeout, `useAppLinkHandler.ts` line 36 runs `if (fallbackMessage) alert(fallbackMessage);` — a plain **native browser `alert()` popup**, not a styled panel, and there is no "Try Again"/"Go Back" affordance at all.

  The page's own framing — "When someone without the app scans, they see your message rather than a confusing blank screen" (line 63) — is true for the Direct-Mode/single-Destination case and materially misleading for the Page/multi-Destination case, where "your message" is a bare OS `alert()` dialog. A support agent answering "what will my customer see if they don't have the app" cannot give a correct answer from this page alone without knowing which mode the reader is using.
- **Binding-failure behaviour is unstated and would produce a wrong answer if asked.** Section 6 ("Using Bindings in Fallback URLs") promises "Configure both once. Deploy to 500 items. Each scan routes to the right app screen (or web equivalent) for that specific item" (line 83). Verified in `qrtub/src/lib/page/bindings.ts` (`resolveBindingsForUrl`, lines 404-429): if a bound field evaluates to `undefined`, `null`, or `""` for a given item, the **entire URL** (not just that segment) is marked unresolved. For the primary Destination URL this means the rule is skipped entirely (`destination-resolver.ts` lines 58-61); for a Fallback URL, `resolveFallbackUrl` (`destination-resolver.ts` lines 107-111) discards the whole fallback and falls through to the next level (Link → Item) or the default "App not available" message. So an item missing the bound field does **not** get a URL with a blank placeholder — it silently gets no fallback (or no destination) at all. The page's "no per-item configuration" claim is true only when every item has that field populated; it does not say what happens when one doesn't.

### 2. Answer-first (every H2, quoted verbatim)

| H2 | Opening sentence(s) as written | Verdict |
|---|---|---|
| What Are App Links? | "App links are URLs that use a custom scheme instead of `https://`. When a device recognises the scheme, it opens the corresponding app." | **Answer-first.** Direct, ~27 words, no preamble. |
| The Problem: App Not Installed | "When someone scans a QR code and taps a Destination with an app link:" *(followed immediately by a two-item bullet list)* | **Preamble.** The heading poses a problem; the opening line is a colon-terminated fragment that sets up a list rather than stating in prose what the problem actually is. A retrieval snippet that stops at the first sentence learns nothing. |
| How QRtub Handles App Links | "QRtub detects when a Destination URL is an app link (any non-`https://` scheme). At scan time:" *(then a 4-step numbered list)* | **Partial.** First sentence is a real, direct answer (detection mechanism). The second sentence is again a colon fragment leading into a list rather than a self-contained 40-60 word answer — a snippet retrieved as "first sentence only" undersells the mechanism (misses the timer/fallback behaviour entirely). |
| Setting a Fallback URL | "When you add or edit a Destination with an app link URL, QRtub automatically shows a fallback configuration panel." | **Mostly answer-first** but thin (19 words) — describes when the UI appears, not yet how to set the value; the actual "how" is in the numbered list that follows. |
| Setting a Custom Fallback Message | "If there's no web equivalent to fall back to, set a **Fallback Message** instead of a URL." | **Answer-first.** Direct, 17 words. |
| Using Bindings in Fallback URLs | "Fallback URLs support `{{item.field}}` bindings — the same way Destination URLs can include Item data automatically." | **Answer-first.** Direct. |
| Where Fallback Settings Live | "Fallback URL and Fallback Message can be set at three levels:" *(then a 3-row table)* | **Borderline.** Correctly answers "where" but as a colon fragment into a table rather than a sentence; acceptable because the table itself is the answer, but there's no prose fallback if a retrieval system strips tables. |
| Common Examples | *(no lead sentence — jumps straight to `### Mitti (iAuditor)`)* | **N/A / preamble by omission.** This H2 has zero prose of its own; it's a pure grouping header for three H3s. Not wrong, but it means the H2 itself answers nothing if retrieved alone. |
| App Links vs Device Detection | "These two features are complementary, not alternatives:" *(then a 2-row comparison table)* | **Answer-first**, if terse (8 words) — it directly resolves the "vs" framing in the heading before the table elaborates. |

### 3. One question per page

This page is scoped to a single coherent task — "how do I make an app-link Destination survive the app-not-installed case" — and covers it end to end (URL fallback, message fallback, bindings, the three-level hierarchy, and how it interacts with device detection). That's an appropriate single-page scope; **no split is recommended**.

It is not too thin to stand alone either — at ~140 lines with multiple worked examples it's a legitimate standalone chunk, not a stub that should be folded into `key-concepts.mdx` or `device-detection.mdx`.

The one internal redundancy worth naming: `integrations/safetyculture.mdx` has its own section "## App Not Installed? Set a Fallback URL" (lines 129-150) that re-explains the same 2.5-second-timeout mechanism and links back here with "See [App Links & Fallback URLs](/help/app-links) for full details." That's a defensible product-specific worked example, not true duplication — it stays.

### 4. Headings as questions

| Current heading | Proposed question form | Why (or why not) |
|---|---|---|
| What Are App Links? | *(keep as-is)* | Already effectively a question. |
| The Problem: App Not Installed | **What happens when the app isn't installed?** | Converts a labelled-problem heading into the actual implicit question, and matches how a support agent or searcher would phrase it. |
| How QRtub Handles App Links | **What happens, step by step, when someone taps an app link?** | Slightly clearer that this section is the mechanism/timeline, not a settings guide. |
| Setting a Fallback URL | **How do I set a Fallback URL?** | Matches "how do I..." phrasing readers/agents actually search. |
| Setting a Custom Fallback Message | **How do I set a custom Fallback Message?** | Same reasoning. |
| Using Bindings in Fallback URLs | **Can Fallback URLs use field bindings like `{{item.field}}`?** | The current noun phrase reads as a feature list; the question form matches how someone would ask before finding this section. |
| Where Fallback Settings Live | **Where can I set Fallback URL and Fallback Message?** | Minimal change, clarifies it answers "where," not "what are the settings." |
| Common Examples | *(leave as noun phrase)* | This is a worked-examples index, not an implicit question — forcing a question form here would be artificial. |
| App Links vs Device Detection | **When should I use App Links instead of Device Detection?** | The "vs" framing reads as a comparison-table heading; the question form signals it also tells you which to reach for. |

### 5. Edge cases / limits / failure modes

Explicit gaps found (each verified against source, not asserted from behavior guesses):

1. **No mention that a Fallback Message renders as a native browser `alert()`, not a styled page, when the Destination is used as a Page button/link** (`useAppLinkHandler.ts` line 36: `if (fallbackMessage) alert(fallbackMessage);`). This is a real UX detail a reader configuring a Page full of Destinations would want to know before writing message copy (an `alert()` has no styling, no line breaks beyond `\n`, and blocks the page until dismissed).
2. **No mention of the "Try Again" / "Go Back" recovery UI** shown on the branded failure screen for the single-Destination/Direct-Mode path (`AppLinkOpener.tsx` lines 43-58). The page describes only the message/URL outcome, not what the screen looks like or that it offers retry.
3. **Binding-resolution failure is undocumented** (see §1 above, `bindings.ts` `resolveBindingsForUrl`): an empty/missing bound field doesn't degrade to a URL with a blank segment — it silently drops the entire fallback (or destination rule) and falls through to the next level or the default message. Nothing on the page — or in "Using Bindings in Fallback URLs" specifically, where this matters most — says what happens when the field is empty for some items.
4. **The "flaky scan" timer-suspension nuance is unstated.** `app-link-open.ts` deliberately does *not* fire the fallback if the tab was hidden or the timer was suspended for far longer than the timeout (mobile browsers pause JS timers while the native app is foregrounded) — this is exactly the mechanism that prevents a returning user from being wrongly bounced to the fallback. The page's step 4 ("If the page is still visible after 2.5 seconds — the app wasn't installed") is *directionally* correct but omits the actual safeguard, which matters if a reader is troubleshooting "the fallback fired even though the app opened."
5. **No plan/tier statement.** The page never says whether App Links / Fallback URLs require a specific QRtub plan. Checked `qrtub/src/lib/stripe-plans.ts` and found no gating tied to app-link or fallback fields — this appears to be a base capability of Destinations, not a premium feature — but the page should say so explicitly rather than leaving the reader to infer it, since silence on tier is exactly where an AI agent guesses.
6. **`tel:` links are also "app links" by this mechanism** (`isAppLink()` in `url-helpers.ts` classifies anything that isn't `http:`/`https:`, which includes `tel:`) but the page's three examples (lines 13-15) are all custom-scheme mobile apps. A reader with a `tel:` Destination wondering "does the 2.5-second fallback timer apply to phone-call links too?" gets no answer either way.
7. **No stated ceiling on URL length**, even though the sibling `integrations/safetyculture.mdx` page states "Very long deep links may not work consistently" and "keep it under ~2000 characters" in its own troubleshooting section. This page, which is the canonical fallback/app-link reference, states no such limit.
8. **No mention of what a desktop scan/click does.** Device-detection.mdx explicitly recommends combining App Links with Device Detection to route app-store downloads by OS, implying a plain app-link Destination with no device condition will also fire (and presumably fail) on desktop — but app-links.mdx never states what a desktop visitor sees when they hit a bare app-link Destination with no Device Detection layered on.

### 6. Chunk integrity (each H2 read in isolation)

- **What Are App Links?** — Self-contained. Fine on its own.
- **The Problem: App Not Installed** — Self-contained; doesn't depend on prior prose, though it does presuppose the reader already knows what a "Destination" is (see §1).
- **How QRtub Handles App Links** — Self-contained mechanism description; fine alone.
- **Setting a Fallback URL** — Self-contained procedurally, but assumes "the Destination editor" is a known, locatable UI surface (see §1 gap).
- **Setting a Custom Fallback Message** — Self-contained.
- **Using Bindings in Fallback URLs** — Self-contained; the example is fully inline (app link + fallback URL both shown), doesn't lean on the earlier Mitti example.
- **Where Fallback Settings Live** — Self-contained table; reads fine alone.
- **Common Examples** — The parent H2 has no prose, so isolating it alone yields *nothing* — only the H3 children below it carry content. If a retrieval system chunks by H2 and swallows H3s underneath, this is fine; if it treats "Common Examples" as its own chunk boundary excluding children, the H2 in isolation is empty.
- **App Links vs Device Detection** — Mostly self-contained, but the comparison table's first row reads **"App not installed | Fallback URL (this page)"** (line 128) and the closing paragraph says "For Mitti specifically..." — the phrase **"(this page)"** is a deictic reference that breaks if this section is retrieved in isolation from the page title/URL (a chatbot quoting just this table would show "(this page)" with no antecedent). Should read "Fallback URL — see above" or name the mechanism directly instead of pointing at the container document.
- **Related** — Standard footer link list; not a content section, no isolation issue.

---

#### Summary

The page is accurate everywhere it commits to a specific, checkable claim (the 2.5-second timeout, the binding syntax, the three-level fallback priority order, the Mitti example) — no factual errors found in what it *does* say. The defects are almost entirely omissions: two different failure UIs treated as one, a binding-failure mode that silently produces "no fallback" rather than "url with blank," an undefined-terms self-containment gap shared with the rest of the Pages group, and a couple of preamble/colon-fragment H2 openers. These are substantive enough (particularly #2 in self-containment and #1/#3 in edge cases) to warrant a rewrite rather than a line edit — see `audit/proposed/help__app-links.md`.


**Proposed rewrite:** `audit/proposed/help__app-links.md`

---

## Print Batches

**Source page:** `help/print-batches` &nbsp;|&nbsp; **Needs rewrite:** Yes

Source file: `/workspace/mintlify-docs/help/print-batches.mdx`
Live: https://help.qrtub.com/help/print-batches
Nav group: `Printing` (docs.json) — the only page in that group. Siblings checked for overlap:
`help/media-basics.mdx` (Concepts group) and `help/print-first-workflow.mdx` (Concepts group).

---

### 1. SELF-CONTAINMENT

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

### 2. ANSWER-FIRST

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

### 3. ONE QUESTION PER PAGE

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

### 4. HEADINGS AS QUESTIONS

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

### 5. EDGE CASES / LIMITS / FAILURE MODES

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

### 6. CHUNK INTEGRITY

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

### Summary of required actions

1. **Rewrite warranted** — the export/batch-creation claim is materially inaccurate (missing
   the neutral-download path), which alone justifies a substantive edit rather than a
   word-level patch. Combined with the archiving-scope error, the missing deletion/cap/
   default-name facts, and the "Back in the Tub" chunk-integrity break, a full replacement
   draft has been written to
   `/workspace/mintlify-docs/audit/proposed/help__print-batches.md`.
2. **Separate action (not this file):** trim `help/media-basics.mdx`'s duplicated "Print
   Batches" section down to a pointer at this page.


**Proposed rewrite:** `audit/proposed/help__print-batches.md`

---

## QRtub for Civil Construction

**Source page:** `industries/civil-construction` &nbsp;|&nbsp; **Needs rewrite:** No

**File:** `/workspace/mintlify-docs/industries/civil-construction.mdx`
**Live:** https://help.qrtub.com/industries/civil-construction
**Nav group:** Industries tab → "Industries" group (siblings: `industries/contract-cleaning`,
`industries/arboriculture-tree-management`, `industries/electrical-test-and-tag`,
`industries/local-government-councils`)

**Scope note:** per instructions, this page is audited against a *different* bar than the
how-to pages in this repo's editorial audit — not SELF-CONTAINMENT/ANSWER-FIRST/CHUNK-INTEGRITY
criteria, but **citability and accuracy of marketing claims**: is every capability claim true,
against `/workspace/qrtub/BRAND.md` §1.6 ("Claims That Are FALSE") and the app source, and is
the page safe for an AI agent to cite verbatim out of its original marketing context.

---

### 1. CAPABILITY CLAIM LEDGER

Every substantive capability claim on the page, checked against `BRAND.md` §1.4/1.5/1.6, the
sibling pages, and app source where needed.

| Claim (location) | Verdict | Basis |
|---|---|---|
| "Every excavator, loader, or crane gets a single QR code linking to all required systems" (L25) | **True** | Matches BRAND §1.5 ("One QR code can connect to multiple Destinations via a Page") |
| "Mitti (formerly SafetyCulture) inspections for daily pre-start checks" (L26) | **True** | Matches `integrations/safetyculture.mdx` |
| "Maintenance logs / Operator manuals / Service history" as Destinations (L27–29) | **True, correctly scoped** | Framed as "linking to," never "tracking" — matches CLAUDE.md's rule that QRtub does not track maintenance itself |
| "Order stickers before equipment arrives... No dependency on perfect timing" (L34–38) | **True** | Matches UC-004 and `help/print-batches.mdx` (Draft status exists before printing) |
| "Switch inspection apps without reprinting... Connect to different systems on different sites" (L44–47) | **True** | Matches UC-003 |
| "they update 150 destinations in QRtub. Zero reprinting. Zero downtime." (L82) | **True** | Matches UC-003 mechanism — Destinations are what changes, not physical codes |
| "URL Templates auto-populate equipment IDs from QRtub Item fields" (L106) | **True** | Matches CLAUDE.md field-binding section and `integrations/safetyculture.mdx`'s own worked examples |
| "Route operators directly to the right asset in your maintenance system" (L110) | **True** | A Destination URL/deep link with a `{{item.field}}` binding, same mechanism as Mitti section |
| Final feature list: bulk Links, Pages, Mitti/CMMS integration, URL Templates (L174–177) | **True — all "Available"** | Cross-checked against BRAND §1.4; nothing here is drawn from a "Planned" row |
| "Different Information for Different People" section (L51–71) | **Overstated — see §4** | Implies role-based targeting; the actual mechanism is self-selection from one shared Page |
| "Weatherproof codes lasting years" under "Designed for Physical Deployments" (L143) | **Ambiguous — see §4** | Durability is a property of customer-sourced Media, not something QRtub the software "handles" |
| "Order Professional Codes... Order in bulk for cost efficiency" (Step 2, L127–128) | **Ambiguous, not false** | QRtub generates the print-ready export (`help/print-batches.mdx`); it does not manufacture or ship physical stickers/plaques itself. The page never says QRtub does, but it also never clarifies the reader sources printing from their own vendor |

**Net:** no claim on this page states or implies a feature from BRAND.md's "Planned" list
(API access, analytics dashboards, cross-account transfer, granular permissions, payment
Destinations, full Media inventory/Batch/Template management) as available today. That is the
single most important thing this page gets right, and it is worth stating plainly: **this page
does not commit the specific errors CLAUDE.md calls out industry pages for historically.**

### 2. BRAND.MD §1.6 "CLAIMS THAT ARE FALSE" — DIRECT CHECK

Checked each of the twelve listed false claims against this page's text:

| False claim (BRAND.md §1.6) | Present on this page? |
|---|---|
| QRtub is asset management software | No — never stated; "asset" is used only as the customer's own domain word for equipment (explicitly sanctioned by CLAUDE.md's industry-page guidance), never as a claim about what QRtub *is* |
| QRtub replaces SafetyCulture / maintenance systems / any software | No — every mention frames Mitti/CMMS as something the code "links to" or "opens," never "replaces" |
| QRtub provides inspection capabilities | No |
| QRtub tracks maintenance history | No — "Maintenance logs in your CMMS platform" (L27) correctly attributes the tracking to the external CMMS |
| QRtub is a payment platform | No — Payment Destinations aren't mentioned at all |
| QRtub has an API | No |
| QRtub provides analytics dashboards | No — "Utilisation tracking" and "Cost allocation" (L68–69) are listed as what *external* fleet-management tools provide, under "Site managers see," not attributed to QRtub itself (though see §4 for the chunk-isolation risk this creates) |
| QRtub supports cross-account transfer | No |
| QRtub has granular permissions/sharing between orgs | No |
| Full Media inventory management is available | No — page never uses "Media" as a tracked entity; it only talks about physical stickers/plaques in generic terms |
| Media Batch management is available | No |
| Media Templates are available | No |

**Verdict: clean.** This page does not make any of the twelve specifically-flagged false claims.
That distinguishes it from at least one sibling page in this repo's own editorial audit
(`help__key-concepts.md`, which found active Media-tracking overclaims). The defects on this
page are of a different, subtler kind — see §4.

### 3. TERMINOLOGY CONSISTENCY

Checked against `/workspace/qrtub/GLOSSARY.md` and the two integration pages this page draws
its named tools from.

- **Product-name and domain casing**: "QRtub" and "qrtub.com" are used correctly throughout
  (checked every occurrence at L2, L8, L20, L106, L125, L181) — no `QRTub`/`QR Tub` typos, no
  uppercase domain.
- **No deprecated "Profile Page"** — the term never appears; "Pages" is used correctly as the
  generic capitalised entity (L134, L173, L175).
- **"Asset" vs "Item"** — GLOSSARY.md §"What NOT to Call Things" maps `Asset → Item` for
  QRtub's own data model, but CLAUDE.md's industry-page guidance explicitly allows "a
  council's 'assets'" as legitimate customer-domain vocabulary. This page's uses ("One Code
  Per Asset," L24; "the right asset in your maintenance system," L110) are all customer-domain
  usage, not a claim about QRtub's own entity model — **not a violation**.
- **Mitti naming is inconsistent within this single page.** L26 calls it "Mitti (formerly
  SafetyCulture)"; L105, forty lines later, calls the same product "Mitti (iAuditor)" with no
  mention of SafetyCulture at all. The canonical source, `integrations/safetyculture.mdx`,
  titles it "Mitti (formerly SafetyCulture / iAuditor)" — the full three-name chain, every
  time. Splitting the qualifier across two different partial forms in one page is a minor but
  real citability defect: a chunked retrieval that surfaces only the L105 sentence ("Mitti
  (iAuditor) - Pre-start inspections...") gives an agent no path back to the far more
  commonly-searched name "SafetyCulture," since that word never appears near it. Recommend
  using the full "Mitti (formerly SafetyCulture)" form consistently, matching the L26 usage.

### 4. AI-RETRIEVAL CITABILITY RISK

The core question for this bar: if a support bot or search agent retrieves a chunk of this
page **in isolation**, without the rest of the article's framing, does it produce a false or
misleading answer? Three findings, ranked by severity.

#### 4.1 "Different Information for Different People" implies capability the app doesn't have (most severe)

The section (L51–71) reads:

> "**Operators see:** Pre-start inspection checklist / Daily check-in procedures / Emergency contacts / Operating instructions
>
> **Maintenance teams see:** Service history and logs / Maintenance schedules / Parts documentation / Technical specifications
>
> **Site managers see:** Compliance documentation / Certification status / Utilisation tracking / Cost allocation
>
> Same QR code. Relevant information for each role."

This phrasing describes automatic, role-differentiated content delivery — as if scanning the
same code as "an operator" surfaces different information than scanning it as "a site
manager." Checked against the actual mechanism (`../qrtub/src/lib/page/bindings.ts`,
`../qrtub/src/lib/page/context.ts`, and CLAUDE.md's own Conditional Visibility section):
conditional visibility (CEL) can branch on Item fields, Tub fields, device fields, and time
fields — **and** (confirmed in `bindings.ts`, not documented in CLAUDE.md's own conditional
visibility section) a `session.user` binding exists for a specific logged-in QRtub team
member's ID. None of these constitute a "role" abstraction. There is no way for QRtub to know
that the person currently scanning is "an operator" versus "a site manager" without either (a)
the visitor self-selecting from a shared list of Destinations, or (b) someone hand-writing a
CEL condition against one specific user's account ID — which does not scale to an unbounded
population of operators.

The honest version of this exact feature is stated correctly by three of this page's own
siblings. `contract-cleaning.mdx` (L27): *"Everyone who scans sees the SAME branded Page with
ALL the buttons. Each person taps what's relevant to them."* `local-government-councils.mdx`
(L33) uses identical framing. Those pages are explicit that this is self-selection from one
shared list, not targeted delivery.

**Why this matters for chunked retrieval specifically:** a support bot asked "does QRtub show
operators and managers different information automatically?" that retrieves only this H3 in
isolation has strong textual grounds to answer "yes" — which is false. Retrieved alongside the
rest of the page it is more likely (though not certain, since nothing later in the page
explicitly corrects the framing) to be read as a "different people tap different buttons"
story. The fact that this page is the *one* sibling in the group that doesn't state the
self-selection mechanism plainly is the defect — not that the underlying feature (multi-
Destination Pages) is unavailable.

**Recommended fix:** rewrite to match the accurate framing already used by the sibling pages
— e.g., "Every code opens one Page listing everything: pre-start checklist, service logs,
compliance status. Operators tap what they need; maintenance and management do the same" —
and drop the "X see: / Y see:" heading structure, which is what invites the role-targeting
misread.

#### 4.2 "Utilisation tracking" / "Cost allocation" bullets are one chunk-boundary away from a false capability claim

Still inside §4.1's section, the "Site managers see:" bullet list includes "Utilisation
tracking" and "Cost allocation" with no qualifier that these live in external fleet-management
software. The qualifying context — "QRtub connects your physical equipment to the digital
tools you already use" — appears in a different H2 ("Integration with Construction Software,"
L100) roughly 50 lines later, under its own "Fleet Management" H3 (L117–120), which *does*
correctly attribute "Utilisation tracking systems" and "Cost allocation platforms" to external
tools. A retrieval system that returns only the L51–71 chunk for a query like "does QRtub do
cost allocation for equipment" has no attribution info in front of it, and BRAND.md §1.3 is
explicit that QRtub "IS NOT" asset management software. Recommend adding the words "in your
fleet system" or similar directly inside the L51–71 bullets themselves, rather than relying on
a separate section to carry the qualifier — the qualifier needs to travel with the claim, not
sit in a heading 50 lines away.

#### 4.3 "Real-World Construction Use Cases" heading over-promises for unattributed hypothetical scenarios

Section (L73–98) is titled "Real-World Construction Use Cases" and opens each subsection with
specific, concrete numbers presented as fact: "A civil contractor manages 150+ pieces of heavy
machinery..." (L76), "You order 200 equipment QR codes for a new project. Machinery arrives
over 3 months..." (L40). These are unattributed (no named customer, no quote), so they don't
violate BRAND.md §3.2's literal rule against inventing "customer quotes or case studies" — but
the heading itself asserts these are "real-world," when they read as illustrative composites,
not documented customer stories. An AI agent that quotes "QRtub has a civil contractor customer
managing 150+ pieces of machinery across multiple sites" as a verified fact, sourced to a
heading that says "Real-World," would be overstating what the page actually supports.
Recommend renaming to something like "Example Scenarios" or "How This Plays Out," which invites
the same content without implying documented provenance.

### 5. REDUNDANCY WITH SIBLING INDUSTRY PAGES

Per `audit/01-external.md` (§1, §2), all five `/industries/*` pages — including this one —
share a byte-identical `## Ready to Deploy?` + `**Core features available:**` template
skeleton, with only the bullet content varying. This page contributes 7,746 MD bytes (~1,933
estimated tokens) to the 19-page `llms-full.txt` corpus (`audit/01-external.md` §1).

Beyond that confirmed byte-identical skeleton, this page's core explanatory sections — "One
Code Per Asset" (UC-001), "Print Before Deployment" (UC-004), "Update Without Reprinting"
(UC-003) — restate, in different words but the same substance, exactly the use-case library
entries CLAUDE.md documents once for reuse across every industry page. `contract-cleaning.mdx`
restates the identical three points ("Bulk Deployment Across Facilities," "Real-World Example:
Public Restroom QR Code" mirrors "Update without reprinting"). `local-government-councils.mdx`
does the same. None of the three industry pages point back to a single canonical explanation
of *how* the underlying mechanism works (that lives in `help/print-first-workflow.mdx` and
`help/key-concepts.mdx`); each re-derives it independently for its own vertical.

For a retrieval system, this means a generic question like "can I switch software without
reprinting my QR codes" can surface three-to-five near-duplicate industry-flavored answers,
none more authoritative than another, none linking to the dedicated `help/*` page that actually
owns the mechanism in depth. This is a corpus-dilution problem more than a per-page defect —
noted here because it directly bears on §6's llms.txt recommendation.

Distinguishing note: unlike `contract-cleaning.mdx` and `local-government-councils.mdx`, this
page does **not** use the "public scans the code and treats visible operational buttons as a
marketing signal" narrative device (their "Note on security" / "Restaurant Kitchen Effect" /
"Professional Council Effect" framing). That specific redundancy is confined to those two
pages and does not extend to this one — worth noting so it isn't over-generalized.

### 6. SHOULD THIS PAGE BE IN LLMS.TXT?

**Verdict: yes, keep it in — but only once §4.1 and §4.2 are fixed. As currently worded, the
page should not stay in an index an AI agent treats as ground truth.**

Reasoning:

- The premise that a technical-docs `llms.txt`/`llms-full.txt` should exclude marketing content
  *by genre* doesn't hold up on its own — per `audit/01-external.md`, `llms-full.txt` is
  "97.89% unique, substantive content," and industry-fit questions ("does this work for
  construction equipment," "what does this connect to for civil contractors") are real
  questions a prospect or support bot legitimately asks that no `/help/*` page answers. Genre
  alone isn't disqualifying; **accuracy is the correct bar**, which matches how this audit was
  scoped.
- Measured against that bar, this page is mostly clean: §1 and §2 above find no BRAND.md
  "FALSE" claims, no Planned feature presented as available, and every "Available" feature
  claimed in the closing checklist is genuinely available. That is a materially better result
  than at least one `/help/*` page already audited in this project (`help__key-concepts.md`,
  which found an active Media-tracking overclaim).
- However, §4.1 is a real, specific defect: the "Different Information for Different People"
  section describes automatic role-based targeting that the app does not do, in a page whose
  own sibling pages get the identical underlying feature right. A support bot citing this
  section alone would give a wrong answer about a core mechanism (Pages/Destinations) that
  this same corpus documents correctly elsewhere. That inconsistency — right answer in three
  sibling pages, wrong framing in this one — is exactly the kind of drift an `llms.txt`
  corpus should not be shipping.
- §4.2's un-anchored "utilisation tracking / cost allocation" bullets compound the same risk:
  they are one chunk-boundary away from contradicting BRAND.md's explicit "QRtub IS NOT asset
  management software" line.

**Concrete recommendation:** fix §4.1 (rewrite to the self-selection framing already used
correctly by `contract-cleaning.mdx` and `local-government-councils.mdx`) and §4.2 (move the
"in your fleet system" qualifier into the bullet list itself) before this page's next
regeneration into `llms-full.txt`. Once fixed, there is no accuracy-based reason to exclude it.
The redundancy noted in §5 is a lower-priority, corpus-wide cleanup (deduplicating the
"Ready to Deploy?" skeleton and pointing industry pages back to the canonical `help/*`
explanations) that applies to all five industry pages equally, not a reason to single out this
one for removal.

---

### Summary of findings, ranked by severity

1. **(Moderate-high, citability)** "Different Information for Different People" (L51–71)
   implies role-based automatic content targeting the app does not have; the honest
   self-selection framing is stated correctly by two sibling pages this page fails to match.
2. **(Moderate, citability)** "Utilisation tracking" / "Cost allocation" bullets (L68–69) carry
   no in-line qualifier that these are external-tool destinations, one chunk-boundary away from
   contradicting BRAND.md's "QRtub is NOT asset management software."
3. **(Minor, terminology)** "Mitti (formerly SafetyCulture)" (L26) vs. "Mitti (iAuditor)"
   (L105) — same product, two different partial names within one page.
4. **(Minor, wording)** "Weatherproof codes lasting years" (L143) attributes a physical-media
   durability property to the software platform rather than to customer-sourced Media.
5. **(Minor, framing)** "Real-World Construction Use Cases" (L73) heading asserts documented
   provenance for what read as unattributed illustrative scenarios.
6. **(Corpus-level, not page-specific)** Structural redundancy with sibling industry pages —
   confirmed byte-identical "Ready to Deploy?" skeleton (`audit/01-external.md`) plus
   independently-restated explanations of the same three use-cases (UC-001/003/004) across
   at least three of the five industry pages.

No BRAND.md §1.6 "Claims That Are FALSE" violation was found on this page (§2) — this page's
problems are about *how* true claims are framed for isolated, chunked consumption, not about
stating untrue ones.


**Proposed rewrite:** none drafted — this page was audited on the citability/accuracy bar (marketing/industry page) and its findings call for targeted fixes rather than a full replacement draft.

---

## QRtub for Contract Cleaning

**Source page:** `industries/contract-cleaning` &nbsp;|&nbsp; **Needs rewrite:** No

**File:** `/workspace/mintlify-docs/industries/contract-cleaning.mdx`
**Live:** https://help.qrtub.com/industries/contract-cleaning
**Nav placement:** `docs.json` — **Industries** tab, single ungrouped "Industries" group, 5 pages: `civil-construction`, **`contract-cleaning`**, `arboriculture-tree-management`, `electrical-test-and-tag`, `local-government-councils`. No parent "Getting Started"/"Concepts" scaffolding the way the Help tab has — the Industries tab is flat, five peer marketing pages, nothing else.

**Siblings skimmed:** `industries/civil-construction.mdx`, `industries/arboriculture-tree-management.mdx`, `integrations/safetyculture.mdx`, `integrations/cmms-systems.mdx`, `help/key-concepts.mdx`.

**Verified against:** `/workspace/qrtub/BRAND.md` (§1.5 "Claims That Are TRUE," §1.6 "Claims That Are FALSE," §2–3 voice/tone rules); `/workspace/qrtub/src/lib/page/bindings.ts`, `destination-bindings.ts` (URL Templates); `/workspace/qrtub/src/lib/database/server-page-templates.ts`, `/workspace/qrtub/src/app/api/page-templates/route.ts`, `/workspace/qrtub/src/lib/page/get-page.tsx` (tub-level Page templates and how they propagate to Items); `/workspace/qrtub/src/lib/templates/` directory listing (no per-vendor template exists for Swept, CleanTelligent, Aspire, or Jobber; one exists for SafetyCulture/Mitti — `safetyculture-asset.ts`).

**Note on scope:** this page is a marketing/industry-fit landing page, not a how-to page, so it is audited against a different bar than the sibling `help/*` audits in this directory (which use SELF-CONTAINMENT / ANSWER-FIRST / etc.). The bar here is **citability and factual accuracy of its claims** — is every capability claim actually true of the product — plus whether this content type belongs in an AI-agent-facing `llms.txt` index at all. The numbered sections below are the marketing-page-appropriate equivalent of that shared audit format.

---

### 1. CLAIM-BY-CLAIM ACCURACY

Going through every concrete (checkable) capability claim on the page:

| Claim (as written) | Verdict | Basis |
|---|---|---|
| "One professional QR code per facility—everyone sees the same page, each chooses what's relevant." | **True** | Matches the Link/Page model in `help/key-concepts.mdx` (Page Mode: one Link, multiple Destinations as buttons). |
| "The buttons are visible, but they're still protected... [Mitti/Swept] requires login credentials." | **True, consistent with sibling page** | `integrations/safetyculture.mdx` confirms QRtub only builds a deep link/URL to Mitti — "No data is exchanged between the two systems; QRtub builds the URL and Mitti handles the rest." QRtub itself never claims to gate or authenticate — correctly attributes access control to the destination app, not to QRtub. This also correctly avoids the BRAND §1.6 false claim "QRtub provides inspection capabilities." |
| URL Templates: `yourform.com/inspection?facility={{item.facilityID}}&name={{item.facilityName}}` — "no per-facility configuration needed" | **True** | Confirmed against `bindings.ts` / `destination-bindings.ts` — `{{item.field}}` placeholder syntax is real and resolves per-Item at scan time. |
| "Switch inspection apps across all 80 facilities with one Destination update. Zero reprinting." | **True in mechanism, but see §4 for the specific number** | Confirmed against `server-page-templates.ts` / `get-page.tsx`: a Page template is stored once per Tub and inherited by every Item in that Tub (`template?.page_data`), so editing one Destination in the shared template does propagate to every Item that hasn't overridden it. The claim is architecturally correct — the only issue is the inconsistent facility count across the page (100 / 80 / "10, 50, or 100+"), addressed below. |
| "Order QR codes - Vinyl stickers, laminated cards, or metal plaques" | **True** | Matches `help/key-concepts.mdx` Media examples (vinyl sticker, metal plaque as named examples). |
| "Create Tub - Define facility fields (name, client, contact, schedule)" | **True, correct terminology** | Matches `key-concepts.mdx` Tub definition ("category-based workspaces... custom fields"). |
| "Inspection platforms: Mitti (formerly SafetyCulture...), Swept, CleanTelligent" / "Operations: Aspire, Jobber" | **Unverifiable / overstated as stated** | See §4 — flagged as a citability risk, not a BRAND-false claim, because the underlying mechanism (paste any URL into a Destination) is real, but naming four specific named products with no supporting integration guide, deep-link scheme, or template implies a level of tested support the codebase doesn't back up. |
| "Payment"/"API"/"analytics dashboard"/"cross-account transfer" | **Not claimed anywhere on this page** | Checked page text against BRAND §1.6's full false-claims list — none of the seven listed false claims appear on this page, explicitly or implicitly. This is a genuinely clean result. |

**Bottom line:** every claim that is checkable against the codebase is either confirmed true or is a real feature described accurately (URL Templates, tub-level Page templates, Media types, login-gated third-party apps). No BRAND §1.6 false claim is made. The one factual defect is internal (the facility-count inconsistency, §4) rather than a mismatch with the product.

---

### 2. BRAND / VOICE COMPLIANCE

**FALSE-claims check (BRAND.md §1.6):** none of the seven prohibited claims ("asset management software," "replaces SafetyCulture/maintenance systems," "provides inspection capabilities," "tracks maintenance history," "payment platform," "has an API," "analytics dashboards," "cross-account transfer," "granular permissions") are made, explicit or implied. The page is careful to attribute inspections/damage-reporting/logging to the *destination* app (Mitti, Swept, the client's CMMS), not to QRtub itself — this is the correct framing per BRAND §2.2's "Honest" attribute ("QRtub connects to your maintenance system—it doesn't replace it").

**Tone calibration (BRAND §2.3) — two phrases run hotter than the documented "Just right" register:**
- *"Here's the genius:"* and *"QRtub Pages weaponise it"* — BRAND's own "Too corporate" / "Just right" examples model a calm, confident register ("QRtub lets you change what happens when someone scans, without reprinting"), not self-congratulatory or aggressive language. "Genius" and "weaponise" read closer to hype copy than the "quietly confident... doesn't need to shout" personality BRAND §2.1 defines. Not a factual problem, but a voice-consistency one worth a copy pass.
- *"This happens more than you think."* and *"Result: One curious restroom scan becomes a qualified sales lead"* present an anecdotal, unfalsifiable scenario with the same declarative confidence as a verified feature claim. BRAND §3.1 requires "Keep technical accuracy — don't invent features," and this isn't a feature claim, but the same discipline arguably should extend to outcome claims: nothing on the page or in the codebase substantiates a lead-conversion rate, so stating the outcome as a near-certainty ("this happens," not "this can happen") blurs the line between illustrative scenario and factual claim in a way a citing AI agent won't distinguish.

Terminology capitalization (Tub, Link, Page, Destination, Item) is used correctly and consistently throughout, matching `help/key-concepts.mdx`'s canonical glossary — no drift found here.

---

### 3. CROSS-PAGE REDUNDANCY (siblings)

`industries/arboriculture-tree-management.mdx` (skimmed in full for its first two sections) uses the **identical structural template**: same H2 order start ("The Challenge" → "One Code, One Page, All Audiences See Everything"), the same bolded framing device ("Every X gets a single QR code. Everyone who scans sees the SAME Page with ALL the buttons. Each person taps what's relevant to them."), and the same "the public sees the operational buttons and it's social proof of professionalism" argument, just re-skinned for trees instead of facilities. `civil-construction.mdx` shares the "Update Without Reprinting" / no-vendor-lock-in argument and the "print before deployment" framing near-verbatim in spirit if not in wording.

This is a **template skeleton problem across all 5 industry pages**, not specific to this one page in isolation, but it directly affects this page's citability: an AI agent asked "why should a facilities company choose QRtub over a generic inspection link" and given both `contract-cleaning.mdx` and `arboriculture-tree-management.mdx` as candidate sources would retrieve two pages making the *same* underlying argument with different nouns substituted in — redundant signal, not complementary information. (The existing retrieval-focused audit at `audit/_raw/external/industries__contract-cleaning.md` independently found that ~48% of this page's own body prose repeats one single insight three times within the page itself — "One Code, One Page," "The Restaurant Kitchen Effect," and "Real-World Example: Public Restroom QR Code" all make the identical "visible buttons impress the public" point. That is a within-page finding from the retrieval audit; it compounds the across-page redundancy noted here.)

The named-vendor integration claim also creates a **naming inconsistency with sibling integration pages**: `integrations/safetyculture.mdx` is titled and framed around "Mitti (formerly SafetyCulture / iAuditor)" with a full rename history, deep-link scheme (`iauditor://`), Template ID instructions, and an explicit statement of the integration mechanism ("No data is exchanged between the two systems"). This page name-drops "Mitti" correctly and consistently with that sibling page. But it also names **Swept, CleanTelligent, Aspire, and Jobber** with the same confident framing ("QRtub connects to the tools you already use") and no equivalent sibling integration page, deep-link documentation, or codebase template exists for any of them (`/workspace/qrtub/src/lib/templates/` contains only a `safetyculture-asset.ts` vendor-specific template — nothing for the other four names). See §4.

---

### 4. CITABILITY RISKS FOR AI AGENTS

Specific problems that would produce a wrong or overconfident answer if an AI agent cited this page verbatim:

1. **Unverified specific vendor names presented with the same confidence as the one verified vendor.** "Inspection platforms: Mitti..., Swept, CleanTelligent" and "Operations: Aspire, Jobber" sit in the same bullet-list register as Mitti, which *does* have a dedicated, source-verified integration guide. Nothing in the docs or codebase confirms QRtub has been tested against Swept, CleanTelligent, Aspire, or Jobber specifically — the underlying truth is simply "any Destination can be an external URL" (true, per BRAND §1.5), which works for literally any web-based tool, named or not. An AI agent fielding "does QRtub integrate with Jobber?" would likely answer "yes" based on this page's phrasing, when the accurate answer is closer to "you can point a Destination at any Jobber URL you have — QRtub doesn't have a dedicated Jobber integration the way it does for Mitti." This is a materially different answer for a prospect evaluating a purchase.
2. **Internal numeric inconsistency undermines any specific number an agent might quote.** The page states three different facility counts for what reads as the same illustrative scenario: "Deploy to 100 facilities" (Bulk Deployment section), "across all 80 facilities" (next paragraph, same section), and "Deploy to 10, 50, or 100+ facilities" (Why Cleaning Companies Choose QRtub, near the end). An agent asked "how many facilities does the QRtub example cover?" has no single correct number to extract from this page.
3. **Zero outbound links.** This page contains no `## Related` section and no in-body links to `/help/pages-overview`, `/help/creating-your-first-link`, `/help/custom-fields`, or even `/integrations/safetyculture` — the exact integration it names in prose. For a multi-hop retrieval agent trying to go from "why QRtub for cleaning" to "how do I actually configure this," this page is a dead end; every other candidate next step must be reconstructed from the nav rather than followed from the page itself.
4. **Anecdotal outcome stated as near-certain fact** (see §2) — "This happens more than you think" / "becomes a qualified sales lead" — is the kind of line an AI agent answering "will this generate leads for my cleaning company" would likely lift and restate as a product guarantee, when it's illustrative marketing framing with no supporting data anywhere in the corpus.

---

### 5. SHOULD THIS PAGE BE IN `llms.txt`? — **No, not in its current form or placement.**

Mintlify auto-generates `llms.txt`/`llms-full.txt` from every page in `docs.json`'s navigation with no manual curation lever, so today this page (and all 4 sibling industry pages) is included by default — confirmed in `audit/_raw/external/llms-full.md` (all 5 `/industries/*` pages present, 5 of 19 total pages in the corpus). My opinion is that this is the wrong default for this content type, for reasons distinct from — and in addition to — the specific defects above:

- **Content-type mismatch with the rest of the index.** `llms.txt` on a help/docs subdomain (`help.qrtub.com`) is the artifact a support bot, coding agent, or ChatGPT/Claude/Perplexity plugin reaches for when a user asks a product/how-to question. Sales/positioning copy — "guerrilla marketing," "the restaurant kitchen effect," "weaponise it" — answers a different question ("is this product a fit for my industry / why should I buy it") that belongs in front of a *prospect*, not mixed into the same retrieval corpus an agent uses to answer "how do I configure a fallback URL." Concatenated into `llms-full.txt`, 5 of 19 pages (a meaningful fraction of the token budget) are marketing narrative rather than product mechanics, and — per the existing retrieval audit — a large share of each industry page's own prose is repetitive framing rather than new information. That dilutes an already-scarce context budget for an agent doing broad retrieval over the whole site.
- **This specific page's citability defects would get amplified, not caught, by AI citation.** The named-vendor overstatement (§4.1) and the facility-count inconsistency (§4.2) are the kind of thing a human skimming the marketing page shrugs off as "obviously illustrative," but an AI agent extracting a specific factual answer ("does it work with Jobber," "how many locations can I roll this out to") has no such judgment and would likely repeat the page's confident phrasing as fact to a real prospect — which is a worse outcome for a support/sales bot than for a human reader landing on the page directly.
- **The page is a dead end for the multi-hop retrieval pattern `llms.txt` is meant to support.** Zero outbound links (§4.3) means an agent that reaches this page via `llms.txt` search has no forward path to the actual configuration docs — undermining the entire point of a navigable documentation index.

**What I'd recommend instead of removing the page from the web entirely:** keep it live and crawlable on `help.qrtub.com` for human visitors and normal search/SEO/GEO purposes (it's reasonable marketing content, and industry-fit pages are legitimate GEO targets for "QR code system for cleaning companies" style queries) — but exclude the Industries tab from the AI-agent-facing `llms.txt`/`llms-full.txt` index via a custom `llms.txt` (Mintlify supports a hand-authored override per the `docs.json`/`markdown.instructions` mechanism referenced in `audit/_raw/raw_ai_llmstxt.md`), so agents doing product-support retrieval aren't handed sales copy — and marketing/GEO discovery for prospects happens through normal web search and the qrtub.com marketing site rather than through the technical-docs index. If the Industries tab must stay in `llms.txt`, this specific page should first have the vendor-integration claim softened to match what's actually verifiable (Mitti only, or explicit "bring your own URL" framing for the rest), the facility count made consistent, and at minimum one outbound link added (to `/integrations/safetyculture` and `/help/creating-your-first-link`) so it isn't a retrieval dead end.

---

#### Summary

No BRAND-prohibited false claims found, and every checkable capability claim (URL Templates, tub-level Page-template propagation, Media types, login-gated third-party apps) verifies true against the codebase — the page is more accurate than it might first appear given its hype-heavy tone. The real problems are: named third-party integrations (Swept, CleanTelligent, Aspire, Jobber) stated with unverifiable, Mitti-level confidence; a self-contradicting facility count (100 / 80 / 10-50-100+) within one page; anecdotal outcome claims stated as near-fact; zero outbound links; and heavy template/content redundancy against sibling industry pages and within the page itself. Given those citability risks and the content-type mismatch with a technical documentation index, **this page (and likely the whole Industries tab) should not be part of the AI-agent-facing `llms.txt`/`llms-full.txt` in its current form** — it should stay published for human/SEO/GEO purposes, but be excluded from the machine-facing index until the specific accuracy issues above are fixed and it carries real outbound links into the rest of the docs.


**Proposed rewrite:** none drafted — this page was audited on the citability/accuracy bar (marketing/industry page) and its findings call for targeted fixes rather than a full replacement draft.

---

## QRtub for Arboriculture & Tree Management

**Source page:** `industries/arboriculture-tree-management` &nbsp;|&nbsp; **Needs rewrite:** No

**File:** `/workspace/mintlify-docs/industries/arboriculture-tree-management.mdx`
**Live:** https://help.qrtub.com/industries/arboriculture-tree-management
**Nav group:** Industries → Industries (siblings: `industries/civil-construction`,
`industries/contract-cleaning`, `industries/electrical-test-and-tag`,
`industries/local-government-councils`)
**Page type:** Vertical marketing / landing page, not a how-to page. Assessed below against
citability-and-accuracy criteria (is every capability claim true, per `../qrtub/BRAND.md`
§1.5–1.6 and the app source), not the how-to rubric (self-containment, answer-first, chunk
integrity) used for `/help/*` pages in this same audit.

---

### 1. CLAIM-BY-CLAIM ACCURACY AUDIT

Every distinct capability claim on the page, checked against `../qrtub/BRAND.md` §1.4–1.6,
`CLAUDE.md`, and (where named) the app source.

| Claim (paraphrased, with line) | Verdict | Basis |
|---|---|---|
| "One professional QR code per tree" / bulk Link generation for tree populations (lines 8, 74–83, Getting Started #1) | **True** | Bulk Link generation is `Available` per BRAND.md §1.4. |
| URL Templates auto-fill `{{item.assetID}}`, `{{item.species}}`, `{{item.location}}` (lines 79–83) | **True, syntax correct** | Matches the documented double-brace `{{item.field}}` binding syntax (`CLAUDE.md` → `../qrtub/src/lib/page/bindings.ts`). URL Templates are `Available` per BRAND.md. |
| "Update the Destination... Physical tags stay unchanged" (line 85, and repeated in "50-year asset lifecycle") | **True** | Matches the core, verified "update without reprinting" mechanic. |
| Everyone scanning sees the same Page with all Destinations, public taps what's relevant (lines 26–41) | **True** | Matches Page Mode / Destinations model (`Available`). |
| "**Note on security:** ...they reach your inspection app which requires login credentials. The Page shows what's available; your existing systems control who can actually access them" (line 41) | **True and well-hedged** | Correctly attributes access control to the external system, not QRtub — the one place on this page that gets the mechanism right. |
| "Log Maintenance" / "View Inspection History" Destinations shown to council staff (lines 34–35, 96–99, 106–107) | **True as Destinations, but see §3** | A Destination is just a link/button (per `../qrtub/GLOSSARY.md`: "Destination... an external URL"). Fine as long as the reader understands the *history itself* lives in the external system QRtub links to, not in QRtub. This page never states that explicitly — see §3. |
| "Arborists... Log findings and risk rating" (line 94) | **Unverifiable / risky as worded** | See §3 — reads as QRtub doing the logging when read out of surrounding context. |
| "Report Tree Issue" as a "public reporting" capability (lines 36, 155) | **Technically true, imprecisely worded** | A Destination *can* be a link to a form (per GLOSSARY.md), so "public reporting" is achievable — but only as a link the council configures to their own intake system/email/form. The page never says this is "just a Destination you point somewhere," so it reads as a built-in reporting/ticketing feature. Worth one clarifying sentence. |
| "Order tree tags — Weatherproof aluminium tags, stainless steel plaques, or durable vinyl labels" (line 153) | **True but requires care** | This describes the customer's own procurement action (buying physical media from a supplier), not a QRtub fulfillment service. Matches "Basic Media tracking" (`Available`) but reads adjacent to Media Templates / Media Batch management, which are `Planned` per BRAND.md §1.4. The page doesn't claim QRtub supplies or designs the physical tag, so this clears the bar, but it sits one sentence away from overclaiming — the identical pattern appears verbatim across all four sibling industry pages. |
| "QRtub connects to the tools arborists and councils already use" + named platform list (lines 141–146) | **Overclaims the mechanism — see §2** | `CLAUDE.md` is explicit: *"QRtub does not have API integrations with third-party products. It builds a URL that opens them... Never write 'integrates with X' in a way that implies data exchange, sync, or write-back."* This page never states the actual mechanism (deep link / URL only, no data exchange) anywhere, unlike the dedicated `integrations/safetyculture.mdx` page, which states outright: *"No data is exchanged between the two systems; QRtub builds the URL and Mitti handles the rest."* A reader (or an AI agent) relying on this page alone has no way to know that "connects to" means "opens a URL," not "syncs with." |
| Named platforms: Arborcheck, TreePlotter, CONFIRM (Abacus), TreeWorks, Mitti (line 143) | **One fabricated attribution, one category error — see §2** | See §2 for the detailed check. |
| "QRtub is currently in BETA" | **N/A — correctly absent** | Per `CLAUDE.md`, the word "BETA" was retired site-wide in August 2026. This page correctly does not use it. |

### 2. THIRD-PARTY PRODUCT VERIFICATION

The brief calls for checking whether every capability claim is actually true. Because this
page names five specific external products as tree-management software QRtub "connects to,"
those names were checked (web search, since they're outside the app repo):

- **"CONFIRM (Abacus)"** — **The parenthetical vendor attribution appears to be wrong.**
  Confirm is a real asset-management platform used by UK councils for infrastructure
  including street trees — but it is not made by a company called Abacus. It was developed
  under Yotta and is now sold under Siemens (`assetmanagement.siemens.com/products/confirm`).
  No connection between "Confirm" and "Abacus" turned up in any source checked. This reads
  like an invented or confused vendor credit sitting inside a claim about what QRtub
  "connects to" — exactly the kind of detail an AI agent would repeat verbatim if it cited
  this page, and exactly the kind of detail a reader at the actual council using Confirm
  would immediately flag as wrong.
- **"Arborcheck"** — **Real, but not the kind of product this list implies.** Arborcheck is a
  chlorophyll-fluorescence—based tree health *diagnostic instrument* (a physiological stress
  measurement system built on research from Bartlett Tree Research Laboratories), not a
  records/inspection software platform with forms or a mobile app that a QR-code deep link
  would open. Listing it alongside TreePlotter, CONFIRM, TreeWorks and Mitti under "Tree
  inspection platforms" that URL Templates "auto-populate... pre-fill inspection forms" for
  is a category error: there is no indication Arborcheck has a URL-addressable
  form/app to pre-fill. The same "Arborcheck (trees)" credit is repeated in
  `industries/local-government-councils.mdx` (line 191), so the error is duplicated, not
  isolated to this page.
- **TreePlotter, TreeWorks** — Real products (PlanIT Geo's TreePlotter; The Kenerson Group's
  TreeWorks, an ArcGIS Pro extension for urban forestry). Plausibly real software with field
  data collection, though this audit did not confirm either supports URL query-parameter
  pre-fill specifically — the mechanism this page's central claim depends on.
- **Mitti (formerly SafetyCulture)** — **Correct**, and the naming matches the site's own
  canonical reference (`integrations/safetyculture.mdx`): "iAuditor → SafetyCulture → Mitti
  (August 2026)."

**Net:** of five named tree-software integrations, one carries a vendor attribution that
doesn't check out (CONFIRM/Abacus) and one is miscategorized as inspection-form software when
it's actually a hardware diagnostic system (Arborcheck). For a page whose entire "Integration"
section exists to be citable ("what does QRtub work with for tree inspections"), getting two
of five names wrong is a real accuracy problem, not a style nit.

### 3. INSPECTION-LOGGING WORDING — READS AS A CLAIM QRTUB DOESN'T MAKE ELSEWHERE

BRAND.md §1.6 lists as explicitly **FALSE**: *"QRtub provides inspection capabilities"* and
*"QRtub tracks maintenance history (it links to systems that do)."*

This page's "Real-World Example" section (lines 91–94) reads:

> **Arborists** (during scheduled inspection):
> - Tap "Start Tree Health Assessment"
> - Complete inspection using pre-filled tree data
> - Log findings and risk rating

Read after the earlier "Note on security" paragraph (line 41, which does establish that
tapping a button "reaches your inspection app"), a careful reader can infer that
"complete inspection" and "log findings" happen in the *external* app, not in QRtub. But nothing in this specific section restates that — there's no "...in your inspection app" or
equivalent qualifier attached to the action itself. Compare the sibling
`industries/electrical-test-and-tag.mdx`, which is careful every time this exact narrative
beat recurs: *"Complete test, apply new tag. Test record is saved in the testing app you
use."* That qualifying clause is missing from the parallel arboriculture beat and from the
"Park Tree" example (lines 91–99) alike.

This matters specifically for AI-agent retrieval: if a chunker or a support bot surfaces only
this bullet list (a very plausible retrieval unit — it's a tight, self-contained-looking
block), the natural paraphrase is "QRtub lets arborists log inspection findings and risk
ratings," which is exactly the false claim BRAND.md prohibits. The fix is a one-clause
addition, not a rewrite — add the same disclaimer sibling pages already use.

### 4. OVERLAP / REDUNDANCY WITH SIBLING PAGES — AND A DIRECT CONTRADICTION

Per `CLAUDE.md`: *"Search for existing information before adding new content. Avoid
duplication unless strategic."* Industry pages are explicitly meant to share a template
("change the nouns, not the verbs") — but this page's tree content specifically duplicates,
and in one place contradicts, `industries/local-government-councils.mdx`:

- **Structural duplication is total.** Every major heading here — "One Code, One Page, All
  Audiences See Everything," the "Note on security" paragraph (byte-identical apart from the
  quoted button name), "The [X] Effect," "Bulk Deployment," "Real-World Example," "Public
  Education," "Integration with [X] Software," "Getting Started," "Why [X] Choose QRtub,"
  "Use Cases," "Ready to Deploy?" — reappears in the same order in
  `local-government-councils.mdx`, which *also* covers street trees, heritage trees, and park
  trees as one of its explicit asset categories (its own text: *"parks, playgrounds,
  buildings, equipment, trees, signs, infrastructure"*; *"Street trees: Tag urban forest
  population. Arborists perform assessments. Public learns about species, reports
  concerns."*, line 234). A support bot asked "does QRtub work for council street trees" has
  two nearly-equally-relevant pages to choose from, with no cross-link between them.
- **The two pages give contradictory answers to the same question.** This page's
  Integration section lists five tree-inspection platforms: *"Arborcheck, TreePlotter, CONFIRM
  (Abacus), TreeWorks, Mitti (formerly SafetyCulture)"* (line 143). The council page's
  Integration section, addressing the identical use case (council-managed trees), lists only
  one: *"Arborcheck (trees)"* (line 191), alongside a totally different asset-management
  vendor list (TechnologyOne, Assetic, Conquest, Molo, Pathway) that doesn't appear on this
  page at all. An AI agent asked "what tree-inspection software does QRtub connect to" will
  give a different answer depending on which of these two pages it retrieves — a direct,
  checkable inconsistency between two live, currently-published pages.
- **Neither page links to the other.** Given the topical overlap, a reader landing on either
  page has no signal that a second, overlapping page exists.

### 5. VOICE / FRAMING RISK — NARRATED "THOUGHTS" READ CLOSE TO INVENTED TESTIMONIALS

BRAND.md §3.2 ("Never"): *"Invent customer quotes or case studies."* This page (and its
siblings) repeatedly narrates an invented persona's internal thought in quotation marks as if
recorded:

> "They think: 'The council actually manages these trees professionally. I can report issues
> directly. And I learned something about this tree.'" (line 68)

This isn't attributed to a named customer, so it doesn't cross into a fabricated case study
in the strict sense BRAND.md is guarding against — but structurally it's doing the same
rhetorical job (a quoted reaction presented as representative and specific), and an AI agent
summarizing the page could easily present it as "users report that..." if it doesn't parse
the hypothetical framing correctly. Low severity, but worth flagging since it recurs multiple
times across the page (lines 68, 108) and the pattern is shared with every sibling industry
page.

### 6. SHOULD THIS PAGE BE IN llms.txt / llms-full.txt?

**No — not as currently written.** Reasoning:

1. **It fails the one bar that matters most for an AI-consumed index: every claim must be
   citable and true.** This audit found one fabricated/unverifiable vendor attribution
   (CONFIRM/Abacus), one category error presenting a diagnostic instrument as inspection
   software (Arborcheck), an integration-mechanism claim ("connects to") that omits the
   "URL-only, no data exchange" caveat the site states clearly elsewhere, and inspection/
   logging language that reads as a capability claim BRAND.md explicitly lists as false when
   read out of surrounding context — exactly the retrieval mode `llms.txt` exists to serve.
   A support bot or research agent treats every sentence in an `llms.txt`-indexed page as
   equally authoritative; this page hands it two wrong facts and one exploitable ambiguity.
2. **It duplicates, and in one place contradicts, a sibling page that is also indexed.**
   `local-government-councils.mdx` covers council-managed trees directly and is also in
   `llms.txt`. Having both in the same AI-facing index means the index itself contains
   conflicting answers to "what tree software does QRtub connect to" — a self-inflicted
   consistency problem that costs tokens and invites a wrong citation regardless of which
   page the agent happens to retrieve.
3. **The page is marketing rhetoric, not documentation, and reads as such even mid-page.**
   Lines like "the operational buttons they won't use demonstrate professional tree
   stewardship" and "This builds community trust" are persuasion copy aimed at a prospective
   customer deciding whether to buy QRtub — not information a support agent would ever need
   to answer a user's product question, and not something a coding agent integrating QRtub
   needs either. Mixing this register into a technical index degrades signal-to-noise for
   every other page in the same file, and risks an agent citing sales narrative
   ("demonstrates professional urban forest management") as if it were a documented product
   behavior.

**Conditional path to "yes":** if the `/industries/*` pages stay in the index at all (a
separate, defensible decision — they do answer "does QRtub work for my industry," a real
question), this specific page should not go back in until: the two false/miscategorized
vendor names are fixed or removed, the integration section states the actual mechanism (URL
deep link, no sync — matching `integrations/safetyculture.mdx`'s own wording), the
inspection-logging bullets get the same disclaiming clause the electrical-test-and-tag page
already uses, and the overlap with `local-government-councils.mdx` is resolved (either merge
the tree content into one canonical page with a cross-link, or make the two pages' vendor
lists consistent). Until then, the safer default for an AI-facing index is to exclude it and
keep the marketing tab as a human-only surface, or ship a stripped verified-facts-only
version specifically for `llms.txt` that omits the persuasion framing and the unverified
vendor list.

---

### Recommendation

Do not add this page to `llms.txt` in its current form. Fix, in order of severity: (1) the
CONFIRM/Abacus attribution and the Arborcheck category error — both are checkable-wrong facts
sitting in a "what does QRtub connect to" claim, the single highest-risk sentence type for an
AI index; (2) state the integration mechanism explicitly (URL/deep-link only, no data
exchange) at least once on the page, matching `integrations/safetyculture.mdx`; (3) add the
"...in your inspection app" qualifier to the inspection-logging bullets, matching
`industries/electrical-test-and-tag.mdx`; (4) resolve the tree-content duplication and vendor-
list contradiction with `industries/local-government-councils.mdx`, either by cross-linking
or by consolidating. None of this requires a structural rewrite — the page's shape (problem →
one-code-all-audiences → bulk deployment → integration → getting started → use cases) is
sound and consistent with its siblings; the defects are specific, fixable factual claims, not
the page's approach.


**Proposed rewrite:** none drafted — this page was audited on the citability/accuracy bar (marketing/industry page) and its findings call for targeted fixes rather than a full replacement draft.

---

## QRtub for Electrical Test and Tag

**Source page:** `industries/electrical-test-and-tag` &nbsp;|&nbsp; **Needs rewrite:** No

**File:** `/workspace/mintlify-docs/industries/electrical-test-and-tag.mdx`
**Live:** https://help.qrtub.com/industries/electrical-test-and-tag
**Nav group:** Industries tab → "Industries" group (`civil-construction`, `contract-cleaning`, `arboriculture-tree-management`, `electrical-test-and-tag`, `local-government-councils`)
**Siblings skimmed for overlap:** `industries/civil-construction.mdx`, `industries/local-government-councils.mdx`

**Bar used for this review, per instructions:** this is a marketing/industry landing page, not a how-to page. It is judged on citability and accuracy of its capability claims — every claim checked against `../qrtub/BRAND.md` ("Claims That Are FALSE"), `../qrtub/GLOSSARY.md`, and the app source in `../qrtub/src` — plus whether it belongs in an AI-facing `llms.txt` index at all. The how-to rubric (self-containment, answer-first, chunk integrity, etc.) does not apply here and was not used.

---

### 1. Overall shape

Structurally this page is one of five near-identical templates (Challenge → two Use Cases → "Professional X Effect" → Bulk Deployment → Real-World Example → Integration → Getting Started → Why Choose → Use Cases list → CTA), applied to the electrical test-and-tag vertical per CLAUDE.md's "change the nouns, not the verbs" instruction for industry pages. The template itself is a reasonable, deliberate pattern (confirmed by `civil-construction.mdx` and `local-government-councils.mdx` sharing the same skeleton almost section-for-section), so structural repetition across the Industries group is expected and not, on its own, a defect of this page. The findings below are specific to the claims this page makes, not to the shared template.

---

### 2. Claim-by-claim accuracy check

Verified against `BRAND.md` §1.4–1.6 and the app source.

#### 2.1 A confirmed false/fabricated capability claim: automatic date reformatting

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

#### 2.2 Repeated DD/MM/YYYY examples are not literally false, but are misleading in the same direction

Elsewhere the page shows dates in `DD/MM/YYYY` inside quoted "what the screen shows" examples — "Last tested: 15/02/2025, Next due: 15/08/2025, Status: PASS" (line 51), "PASS - Next due: 20/09/2025" (line 149), "Tested 20/03/2025" (line 156). These are technically achievable if the customer manually types/stores the date in that exact string format (QRtub does render whatever string is stored, verbatim). But combined with §2.1's explicit ISO→formatted example two sections earlier, the cumulative effect across the page is to imply QRtub applies some consistent, automatic date presentation layer. It doesn't — whatever format the field is authored in is what appears, with no reformatting at any layer. Worth a single clarifying sentence somewhere on the page (or in a shared industries-wide note) that field values display exactly as entered.

#### 2.3 "Compliance register" framing sits close to BRAND.md's false-claims list

`BRAND.md` §1.3 is explicit that QRtub "IS NOT... A compliance platform" ("It links to compliance tools, doesn't manage compliance") and §1.6 lists as FALSE: "QRtub is asset management software," "QRtub tracks maintenance history (it links to systems that do)."

This page's central marketing conceit is "The Professional Compliance Register Effect" (H2, line 101) and repeatedly calls the thing QRtub provides a "compliance register" / "digital compliance register" (lines 15, 95, 105, 197, 213, 215). Read generously, this is defensible: QRtub Items genuinely are a real, spreadsheet-style register of equipment with custom fields (Equipment ID, Type, Location, Next test due, Serial, etc.), CSV export is a real Available feature, and the page never claims QRtub calculates compliance status, sends overdue alerts, or performs inspection logic itself — the actual test/pass-fail data flow is correctly described as living in "the testing app you use" (lines 58, 85, 145) and as a manually-maintained item field, not an automatically computed one. That's consistent with the CLAUDE.md guidance on "what integration means here."

The risk is for a reader (especially an AI agent summarizing the page) who compresses "Professional Compliance Register Effect" + "audit-ready compliance registers" + "digital compliance register included with our service" into "QRtub is a compliance management/compliance register platform" — which is exactly the sentence BRAND.md's false-claims list forbids. The page is one inference away from the forbidden claim rather than stating it outright. Recommend either (a) adding one explicit sentence disambiguating that the "register" is QRtub's item grid + custom fields, not a compliance-calculation engine, or (b) toning down "compliance register" to "equipment register with test-status fields" in at least the header and the two most citable summary lines (101, 197).

#### 2.4 "Real-World Example" headers present hypotheticals as case studies

Line 136, "Real-World Example: Contract Provider Serving Office Building," and its walk-through narrative ("A test and tag contractor services an office building with 300 electrical items...") is an illustrative, generic scenario — no named customer, no real numbers, no verifiable detail. `BRAND.md` §3.2 ("Never") explicitly lists "Invent customer quotes or case studies" as a rule. This isn't a quote, but labeling a made-up scenario "Real-World Example" (rather than "Example Scenario" or "How this plays out") blurs the same line the rule is protecting: a reader — and especially an AI agent retrieving this chunk out of context — has no signal that "Real-World Example" here means "illustrative," not "a real, documented customer." The subjective color inside it compounds this ("Impressed by professional system and easy access," line 165) — an unfalsifiable reaction attributed to a fictional auditor.

This is a site-wide pattern (civil-construction.mdx uses "Real-World Construction Use Cases," local-government-councils.mdx uses "Real-World Example: Council Playground Equipment" with the same structure), so it's not unique to this page, but it's worth flagging here because it directly affects citability: an AI agent should not present this section's contents as evidence of an actual QRtub customer outcome, and nothing in the heading or body warns it against doing so.

#### 2.5 Claims verified TRUE / consistent with the source

For balance, the majority of concrete capability claims on this page check out:

- **Link formats** — `qrtub.com/drill-001` (line 183) matches the ID-based format documented in CLAUDE.md's Link URL Structures table.
- **URL Template syntax** — `yourtestapp.com/test?equipmentID={{item.assetID}}&type={{item.equipmentType}}&client={{item.clientName}}` (line 125) uses the correct double-brace, namespaced syntax (`{{item.field}}`) per `src/lib/page/bindings.ts`; this is a real, Available feature.
- **"Test record is saved in the testing app you use"** (lines 58, 85, 145) and **"Update without retagging... without retagging equipment"** (lines 134, 199) — both match the correct "what integration means here" phrasing (no sync/write-back implied) and BRAND.md's TRUE-claims list (Destinations changeable without reprinting).
- **"Manage every item in a spreadsheet-style grid, and export your item data as CSV"** (line 209) — matches BRAND.md Available features ("Spreadsheet-style grid management," "Import/export").
- **"Each client gets their own Tub"** (line 70) and per-client Tub isolation as an organizational pattern — Tubs are genuinely independent workspaces; this is a legitimate usage pattern, not a fabricated feature. It should not be read as a claim of user-level access permissions (BRAND.md lists "Granular permissions" as Planned) — the page doesn't make that claim explicitly, but "isolated equipment register" is a phrase that could be over-read that way. Minor risk, not a hard violation.
- **Tool names** (Test & Tag Manager, PAT software, Mitti (formerly SafetyCulture), CMMS platforms) — presented as external tools QRtub links to, not integrates with in a data-sync sense; consistent with sibling pages' identical convention and with CLAUDE.md's "What integration means here" section.

#### 2.6 Minor: section header word choice ("Integration")

Line 169's H2 is "Integration with Test and Tag Software." CLAUDE.md is explicit: "Never write '...integrates with...' in a way that implies data exchange, sync, or write-back." The body text under this header correctly avoids that ("QRtub connects to the tools you already use," "URL Templates auto-populate equipment data") — but the header itself uses the flagged word. This is a template-wide convention (identical header pattern on `civil-construction.mdx`, generic "Integration with Council Systems" on `local-government-councils.mdx`), so it's a shared, low-severity wording choice rather than a page-specific defect — worth fixing across all industry pages together rather than singling this one out.

---

### 3. Overlap / redundancy with sibling pages

`civil-construction.mdx` and `local-government-councils.mdx` share this page's exact section skeleton and several verbatim phrases ("Update without retagging," "URL Templates auto-populate equipment/asset data—configure once, deploy across [X] population," "Zero reprinting," the "Professional X Effect" framing device). This is consistent with CLAUDE.md's explicit industry-page authoring instruction ("Change the nouns, not the verbs — same capabilities, different context") and is not a defect specific to this page. No content unique to electrical test-and-tag is duplicated on a sibling page in a way that would confuse a reader — the redundancy is at the level of narrative device, not overlapping factual claims about the same industry.

---

### 4. Should this page be in `llms.txt`?

**No, not as currently written.**

Reasoning:

1. **Genre mismatch with the rest of the index.** `docs.json`'s `contextual` block enables `chatgpt`, `claude`, `perplexity`, and `mcp` surfaces site-wide, and Mintlify's default `llms.txt` generation pulls every page in `navigation`, including this one, alongside the Help tab's how-to pages. Those how-to pages are held (correctly, per CLAUDE.md) to a strict "verify every capability claim against source" bar with an explicit "state limitations, silence reads as capability" rule. This page is written in a looser, sales-narrative register (subjective reactions, "Professional X Effect" framing, unlabeled hypothetical case studies) that the how-to pages deliberately avoid. Mixing the two registers into one retrieval index means an AI agent cannot distinguish "this is a verified product fact" from "this is illustrative marketing copy" — they arrive with the same formatting and the same apparent authority.

2. **It contains at least one demonstrably false capability claim** (§2.1, the date-reformatting example) that a support bot or coding agent could cite as evidence of real Page-rendering behavior, leading a customer to build a Destination/field expecting output QRtub does not produce.

3. **It contains unlabeled hypothetical case studies** (§2.4) under a heading ("Real-World Example") that reads as a factual attestation. An AI agent summarizing "what results has QRtub delivered for test-and-tag contractors" could easily present this fictional office-building scenario as a real outcome, which is a direct citability failure — the page gives the retrieving agent no signal to treat it otherwise.

4. **Its central framing risks contradicting BRAND.md's own false-claims list** (§2.3) if compressed by an AI into "QRtub is a compliance platform" — exactly the sentence BRAND.md forbids stating.

**Conditional path to inclusion:** if (a) the date-formatting example is corrected or removed, (b) "Real-World Example" headers are relabeled as illustrative (e.g. "Example Scenario") both here and across sibling industry pages, and (c) the "compliance register" language is tightened per §2.3, the remaining content is largely accurate and could reasonably sit in an AI-facing index — industry pages do carry legitimate signal (real tool names, real Link/URL-template mechanics) that a prospect-facing agent might reasonably want to retrieve. Until then, recommend either excluding the Industries tab from `llms.txt` generation entirely (keep it for human/SEO browsing on the marketing surface only) or fixing the three items above first.

---

### 5. Verdict

This page is mostly accurate on its concrete, checkable mechanics (Link formats, URL Template syntax, CSV export, Tub-per-client pattern, "integration" phrasing) but has one confirmed fabricated capability (automatic ISO→human date reformatting on the Page, verified absent from `KeyValue.tsx`, `SpecGrid.tsx`, `bindings.ts`, and `au-date.ts`'s own scope comment), one framing choice ("compliance register") that sits uncomfortably close to BRAND.md's explicit false-claims list without quite crossing it, and one structural habit (unlabeled hypothetical "Real-World Example" scenarios) shared with its siblings that actively undermines citability for an AI agent. None of these are cosmetic — the date-formatting error is a direct, checkable factual error, and the other two are the specific failure modes that make marketing copy unsafe to mix into a technical retrieval index. Recommend fixing all three before this page is treated as a citable source in any AI-facing surface, and recommend the broader Industries tab be reconsidered for `llms.txt` inclusion as a category, not just page-by-page.


**Proposed rewrite:** none drafted — this page was audited on the citability/accuracy bar (marketing/industry page) and its findings call for targeted fixes rather than a full replacement draft.

---

## QRtub for Local Government Councils

**Source page:** `industries/local-government-councils` &nbsp;|&nbsp; **Needs rewrite:** No

**Page:** QRtub for Local Councils
**Live:** https://help.qrtub.com/industries/local-government-councils
**File:** `/workspace/mintlify-docs/industries/local-government-councils.mdx`
**Bar applied:** citability/accuracy for a marketing/industry landing page (per task brief), not the how-to bar used for `/help/*` pages.
**Cross-checked against:** `../qrtub/BRAND.md` (Feature Status, Claims That Are TRUE/FALSE), `../qrtub/GLOSSARY.md`, `../qrtub/src` (bindings, database schema, team/tub permission model), sibling pages in the same `docs.json` nav group (`industries/civil-construction.mdx`, `industries/electrical-test-and-tag.mdx`, `industries/contract-cleaning.mdx`, `industries/arboriculture-tree-management.mdx`).

---

### 1. Page Overview

Frontmatter is complete (`title`, `description`, plus non-standard `industry`/`featured` fields used by the industries nav, which is fine — CLAUDE.md only mandates `title`/`description`). No internal links anywhere in the body (only the standard footer CTA to `qrtub.com/pricing` and `mailto:hi@qrtub.com`) — the page never links to any `/help/*` page even though it describes URL Templates, contractor access, and Tub/department structure in depth. This is a missed cross-link opportunity for a chunk-based retriever, though it matches the pattern on every other industries page (none of the five link internally either).

The page sits in `docs.json` under `tab: "Industries" → group: "Industries"`, alongside `civil-construction`, `contract-cleaning`, `arboriculture-tree-management`, and `electrical-test-and-tag`. Per the external retrieval audit (`audit/_raw/external/llms-full.md`), this page is one of 19 pages Mintlify auto-concatenates into `llms-full.txt` and auto-lists in `llms.txt` — there is no custom `llms.txt` in this repo curating that list down, so every page in `docs.json`'s nav, marketing or technical, is exposed identically to AI retrieval.

### 2. Sibling Page Overlap & Redundancy Context

All five industry pages (confirmed by diffing headings against `civil-construction.mdx`, `electrical-test-and-tag.mdx`, `contract-cleaning.mdx`, `arboriculture-tree-management.mdx`) share one template almost verbatim, with only the nouns swapped:

```
### The Challenge
### One Code[...], All Audiences See Everything
### The [Industry Noun] Effect          <!-- "Restaurant Kitchen", "Urban Forest Dashboard", "Professional Council" -->
### Bulk Deployment Across [Population]
### Real-World Example: [Scenario]
### Integration with [Industry] Software/Systems
### Getting Started
### Why [Industry] Choose QRtub
### Ready to Deploy?
**Core features available:**
```

This is confirmed structurally (not byte-identical) by the external audit's finding that `## Ready to Deploy?` + `**Core features available:**` appears on exactly 5 pages, all industries pages, and that the "Note on security" paragraph (buttons visible but still gated behind login) appears on exactly 3 pages including this one (arboriculture, contract-cleaning, local-government-councils).

Practical implication for retrieval: an AI agent that has already ingested one industry page has effectively already seen this page's rhetorical shell — the "public sees operational buttons and infers professionalism" argument, the "print before systems are ready" argument, the "update destinations without retagging" argument. The only genuinely new information this specific page contributes is the concrete list of council-specific nouns (asset types, department names, named software: TechnologyOne, Assetic, Conquest, Molo, Pathway, Mitti, Proludic, Arborcheck) and the two claims flagged in §3.4/§3.5 below. That is a thin unique-information yield for a page of this length (258 lines) — most of the page is the shared rhetorical template restated in council vocabulary.

### 3. Claim-by-Claim Accuracy Audit

#### 3.1 Verified accurate

- **URL Templates syntax** (line 119): `yourinspectionapp.com/asset?id={{item.assetID}}&type={{item.assetType}}&location={{item.location}}&department={{item.department}}` — correct double-brace, `item.`-namespaced syntax per `../qrtub/src/lib/page/bindings.ts` and CLAUDE.md. No claim of automatic URL-encoding is made, which is correct (there is none).
- **"No per-asset configuration needed"** for URL Templates — accurate; this is exactly what URL Templates do (bind once, resolve per Item).
- **"Update without retagging"** claims throughout — accurate; Links/Destinations are decoupled from physical Media per the entity model, and this is a BRAND.md "Claims That Are TRUE" item.
- **"No contractor logins to provision in QRtub"** — accurate. Destinations are plain URLs; QRtub has no per-viewer authentication layer of its own, so this claim holds.
- **"Findings are saved in the inspection app you use"** (line 134) and **"Record work performed in your maintenance system"** (line 139) — these correctly attribute storage to the third-party system, not QRtub. This is exactly the phrasing CLAUDE.md asks for ("opens the inspection... pre-filled", not "syncs").
- **"Basic Media tracking"**-adjacent language: the page only ever describes ordering/tagging physical Media in general terms (materials, weatherproofing) and never claims batch tracking, cost tracking, or Media inventory — so it does not trip the BRAND.md false claim "Full Media inventory management is available."
- **No BETA language** — the page never describes QRtub as in beta, consistent with the August 2026 retirement of that word (per CLAUDE.md), even though `../qrtub/BRAND.md` itself still says "QRtub is currently in BETA" under Claims That Are TRUE (1.5) and titles its feature table "1.4 Feature Status (BETA)". That is a drift between the two source-of-truth files, not a defect in this page — flagged here only so it isn't mistaken for something this page got right by accident.

#### 3.2 "Each sees only their assets" — unverified / likely overclaim

Line 111: *"Same platform. Different departments. Each sees only their assets. Unified access in the field."* This, plus the "Ready to Deploy" bullet *"Department-isolated Tubs with cross-visibility options"* (line 246), reads as a claim that QRtub enforces per-department access control — i.e., a Parks Tub is actually walled off from a Facilities Tub for a given staff member, with an option to grant cross-Tub visibility where wanted.

Checked against the app source:
- Team membership (`../qrtub/src/lib/database/generated-types.ts`, `teams`/team-user tables) carries a `role` field, but that role is scoped to the **team/account**, not to an individual Tub. `../qrtub/src/app/app/team/page.tsx` and `../qrtub/src/app/features/page.tsx` ("Invite team members to collaborate on your Tubs. Control access with role-based permissions.") describe account-level roles, not per-Tub restriction.
- A `shared_tubs` table exists in the database schema (`permission_level`, `share_team_id`, `tub_id`) that would be the natural mechanism for "cross-visibility options" — but it is referenced **nowhere else in `src/`** outside the generated Supabase types. No route, hook, or component reads or writes it. It is schema scaffolding with no shipped feature behind it.
- `../qrtub/BRAND.md` §1.4 lists "Granular permissions" and "Cross-account transfer & sharing" as **Planned**, and §1.6 "Claims That Are FALSE" explicitly lists "QRtub has granular permissions/sharing between organizations (planned)."

**Conclusion:** "Each sees only their assets" and "cross-visibility options" describe a per-Tub access-control feature that does not appear to be built. Separate departments can get separate Tubs (true — Tubs are just workspaces), but nothing in the code enforces that a Parks officer's login can't also see the Facilities Tub, and no cross-visibility toggle exists to grant it deliberately either. This is the page's clearest instance of the BRAND.md-forbidden claim category ("granular permissions... planned"), phrased in industry-specific language rather than QRtub's own terms, which is likely why it wasn't caught by a literal glossary/terminology scan.

#### 3.3 "View Inspection History" / "Access complete inspection records" — ambiguous attribution

The page is generally careful to say a Destination "opens" or "saves in" the third-party app (see §3.1). But the "View Inspection History" button (appears 4 times: lines 39, 53, 146, 152) and "Access complete inspection records" (line 153, Auditor persona) are never explicitly attributed to an external system the way "Log Maintenance" and "Playground Safety Inspection" are. A reader — human or AI agent — could reasonably conclude QRtub itself holds an inspection history log that Destinations merely surface, rather than that button being one more link-out to whatever inspection platform the council already uses.

That distinction matters because `../qrtub/BRAND.md` §1.6 explicitly lists as FALSE: *"QRtub tracks maintenance history (it links to systems that do)."* The page never says this outright, but it also never says the opposite for this specific button, unlike every sibling button. Recommend making the external attribution explicit here too (e.g., "opens your inspection app's history view for this asset") so an AI agent answering "does QRtub keep an audit trail?" can't reasonably cite this page for "yes."

#### 3.4 Named third-party systems — real companies, unverifiable partnership claims

Line 190: *"Asset management: TechnologyOne, Assetic, Conquest, Molo, Pathway"*; line 191: *"Inspections: Mitti (formerly SafetyCulture), Proludic (playgrounds), Arborcheck (trees)"*. These are real, named enterprise vendors used by Australian/NZ councils. Per CLAUDE.md's own guidance ("What 'integration' means here" — QRtub has no API integrations; it builds a URL that opens them) naming a specific vendor is only accurate if the intent is "you can build a URL Template to this vendor's product," which is true of any URL-accepting system and isn't unique to these five names. The page doesn't claim sync or write-back for any of them (good), but presenting them under a heading literally titled "Integration with Council Systems" — with the lead sentence "QRtub connects to the systems councils already use" — invites an AI agent or reader to treat this as a verified, tested partner list rather than a set of illustrative examples. Given TechnologyOne, Assetic, Conquest, Molo and Pathway are real, identifiable companies, an inaccurate implied-partnership claim carries more real-world risk than a generic "CMMS platforms" reference (which the sibling `civil-construction.mdx` page uses instead: "UpKeep, Fiix, Maintenance Connection" under the same pattern — so this is a corpus-wide pattern, not unique to this page, but worth flagging here since the task is a per-page audit).

Recommend either: (a) softening the heading/lead sentence to make explicit these are examples of systems a URL Template can point to, not tested integrations, or (b) confirming with product/BRAND owners that these five names are deliberately chosen because customers have actually configured URL Templates against them, in which case the current wording is fine and this is a non-issue.

#### 3.5 Everything else in "Why Councils Choose QRtub" / "Use Cases" / "Getting Started"

These sections restate claims already covered above (public accountability narrative, contractor coordination, field accessibility, audit access) without introducing new capability claims. No additional BRAND.md violations found in these sections beyond the ones already listed in 3.2–3.4.

### 4. Internal Redundancy (within-page duplication)

Beyond the cross-page template overlap in §2, this page repeats its own central scenario almost verbatim **twice within itself**. Compare:

> Lines 49–56 ("Public accountability in action"):
> *"A parent at a playground scans the QR code on the equipment, sees: Playground equipment details and age rating / 'Playground Safety Inspection' button (council uses professional inspection systems!) / 'Log Maintenance' button (they track maintenance properly!) / 'View Inspection History' (transparency!) / 'Report Issue' button ← They tap this to report a damaged swing"*

> Lines 141–148 ("Real-World Example" → Parent/ratepayer):
> *"Scan out of curiosity or to report issue / See 'Installed: 2018, Last inspected: 15/02/2025' / See 'Playground Safety Inspection' (council has professional systems!) / See 'Log Maintenance' (they track upkeep!) / See 'View Inspection History' (transparency!) / Trust council's playground management / Tap 'Report Issue' if swing is broken"*

This is the same persona, same asset type (playground/swing), same five bullet beats, restated with cosmetic wording changes roughly 90 lines apart. Combined with the "Professional Council Effect" section (lines 58–76) making the identical "operational visibility signals professionalism to ratepayers" argument a third time in different words, the same rhetorical point is made three separate times in one 258-line page.

For human readability this reads as padding — a reader skimming for "what does this actually do" has to wade through the same anecdote twice. For AI-agent retrieval specifically, this is worse than ordinary repetition: a chunk-based retriever (splitting on `##`/`###`) will very likely return both the "One Code Per Asset" chunk and the "Real-World Example" chunk for the same query (e.g., "how does a resident report a broken playground swing"), consuming two chunks' worth of context budget to deliver one idea, and increasing the odds a summarizing agent presents the same anecdote twice in one answer as if they were two different examples.

**Recommendation:** cut the "Public accountability in action" bullet list (lines 47–56) entirely and let the later "Real-World Example" section carry the one full worked scenario — or vice versa — rather than running both.

### 5. AI-Retrieval / Chunking Assessment

- Fifteen `##` H2 sections, reasonably sized (mostly under 30 lines each) — good chunk granularity if a Mintlify/naive chunker splits on H2.
- Several structural sub-groupings rely on bold text as pseudo-headers (`**Parks & Reserves Tub:**`, `**Contractor** (during maintenance):`) rather than real headings. A chunker that only recognizes `#`/`##` will fold five distinct department blocks (Parks & Reserves, Buildings & Facilities, Fleet & Equipment, Urban Forest, Infrastructure) into one "Multi-Department, Unified Access" chunk — acceptable size-wise, but means a query about one specific department (e.g., "does this work for street trees") retrieves the whole multi-department block rather than a tight, tree-specific chunk.
- No internal links to `/help/*` pages that explain the mechanics referenced here (URL Templates, Tubs, Destinations) in more procedural depth — an agent that retrieves this page for "how do I set up department Tubs" has nowhere to route the user for the step-by-step version.
- The repeated three-times narrative (§4) and the five-page shared template (§2) both dilute the signal-to-noise ratio of any retrieval corpus this page is part of.

### 6. Human Readability Notes

- Voice mostly matches BRAND.md's "quietly confident" register in the mechanical sections (Bulk Deployment, Getting Started, Integration), but the "Professional Council Effect" / "public accountability in action" sections lean into a more oversold, exclamation-mark-heavy style ("(council uses professional inspection systems!)", "(they track upkeep!)", "(transparency!)") that reads closer to BRAND.md's own "too casual" example than its "just right" calibration. Recommend trimming the parentheticals and exclamation marks — the underlying point (visible operational buttons read as professionalism) can be made once, plainly, without needing three repetitions and inline cheerleading.
- Terminology: "asset"/"asset management" is used throughout in place of QRtub's canonical "Item" — this is explicitly sanctioned by CLAUDE.md's industry-page guidance ("a council's 'assets'" is named as the correct customer-domain term to use), so this is **not** a glossary violation, just noting it was checked.
- Tub/Page/Destination/Link capitalization is consistent with GLOSSARY.md throughout (no "Profile Page," no "Asset" as the QRtub entity name, no "Access Link").

### 7. llms.txt Inclusion — Verdict

**No — this page should not be in the `llms.txt` / `llms-full.txt` index as currently written**, though not for the blanket reason "it's marketing, marketing doesn't belong in docs." The reasoning is narrower and more actionable:

1. **It contains at least one claim (§3.2) that matches BRAND.md's own "Claims That Are FALSE" category** — a specific, checkable capability claim (per-department Tub access control with a cross-visibility option) that the source code does not support. An AI support agent or ChatGPT/Perplexity answering "can we restrict a department's Tub from other departments?" by citing this page would give a council prospect a wrong, sales-relevant answer. That is exactly the failure mode a technical citability index (as opposed to an SEO landing page) needs to avoid, and it's a materially different risk than the same wording sitting on the human-browsed marketing site, where a salesperson can correct it in conversation.
2. **One ambiguous attribution (§3.3)** compounds this: "View Inspection History" reads as if QRtub itself holds the record, contradicting another explicit BRAND.md false-claim entry ("QRtub tracks maintenance history").
3. **Low unique-information density relative to its length** (§2, §4): three internal restatements of the same anecdote plus a five-page shared rhetorical template mean an agent spends a disproportionate share of retrieved context on repeated persuasion copy rather than new facts, compared to the `/help/*` pages in the same index, which are dense, single-purpose, and mechanically verified line-by-line per CLAUDE.md's own checklist.
4. Practically, Mintlify's default `llms.txt`/`llms-full.txt` generation (confirmed via `audit/_raw/external/llms-full.md` and the Mintlify guidance doc) lists every page in `docs.json`'s nav with no way to exclude one page short of a custom `llms.txt` file, which this repo does not yet have. So the real decision isn't "add a noindex flag to this one file" — it's whether to introduce a custom `llms.txt` that curates `/help/*` and `/integrations/*` in (mechanically verified, narrow-claim content) while leaving `/industries/*` to be discovered the normal way (site nav, sitemap, Google/GEO) rather than through the same index a support bot uses to answer "can QRtub do X."

**Bottom line:** fix §3.2 and §3.3 (they're the actual accuracy defects), trim the internal repetition (§4), and only then is this page fit to sit in the same index as `/help/*` content. As currently written, it should not be treated as an equally-citable source in `llms.txt`.

### 8. Summary of Recommended Fixes

1. **(High)** Rewrite or remove "Each sees only their assets" (line 111) and "Department-isolated Tubs with cross-visibility options" (line 246) — no per-Tub access-control or cross-visibility feature exists in the app. If department separation is only ever "put them in different Tubs, anyone on the team can still see every Tub," say that.
2. **(Medium)** Clarify that "View Inspection History" / "Access complete inspection records" open the council's own inspection platform, not a QRtub-native log — match the explicit-attribution treatment already given to "Log Maintenance" and "Findings are saved in the inspection app you use."
3. **(Medium)** Soften "Integration with Council Systems" heading/lead sentence, or confirm the five named asset-management vendors (TechnologyOne, Assetic, Conquest, Molo, Pathway) are deliberately chosen as verified customer configurations rather than illustrative examples.
4. **(Medium)** Cut the duplicate playground/parent anecdote — keep either the "Public accountability in action" bullets (lines 47–56) or the "Real-World Example" persona table (lines 126–157), not both.
5. **(Low)** Add at least one or two internal links to relevant `/help/*` pages (e.g., URL Templates, Tubs) so an agent that lands here for the pitch has somewhere to send the reader for mechanics.
6. **(Process)** Once 1–4 are fixed, this page is reasonable to keep in `llms.txt`; until then, recommend a custom `llms.txt` that excludes `/industries/*` from the AI-citability index while leaving it fully live and indexable on the normal site.


**Proposed rewrite:** none drafted — this page was audited on the citability/accuracy bar (marketing/industry page) and its findings call for targeted fixes rather than a full replacement draft.

---

## Mitti (formerly SafetyCulture / iAuditor)

**Source page:** `integrations/safetyculture` &nbsp;|&nbsp; **Needs rewrite:** Yes

**File:** `/workspace/mintlify-docs/integrations/safetyculture.mdx`
**Live:** https://help.qrtub.com/integrations/safetyculture
**Nav group:** Integrations tab → "Operations" (`docs.json` lines 79-90: `integrations/safetyculture`, `integrations/cmms-systems`)
**Siblings skimmed:** `integrations/cmms-systems.mdx`, `help/app-links.mdx`, `help/key-concepts.mdx`

Source verified against: `qrtub/src/lib/page/bindings.ts` (`resolveBindings`, `resolveBindingsForUrl`), `qrtub/src/lib/page/destination-resolver.ts`, `qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`, `qrtub/src/lib/stripe-plans.ts`, `qrtub/src/lib/templates/safetyculture-asset.ts`, and the already-verified content of `help/app-links.mdx`.

---

### 1. SELF-CONTAINMENT

A cold reader who lands on this page cold, unable to follow any link, can get through the **basic** "start an inspection" case but stalls or is left guessing on several other parts of the page:

- **Undefined core vocabulary.** The page instructs "In your Item's Page, add a new Destination:" (Basic Setup → Mobile App Deep Link) and later "Configure the Destination once with a template placeholder. Each Item in your Tub can have a different `templateID` field value" (Using QRtub URL Templates) without ever defining Item, Page, Destination, or Tub. `help/key-concepts.mdx` defines all of these, but it is linked only once, in the **Resources** section at the very bottom of the page — after the reader has already been asked to act on all four terms. A reader who has never opened Key Concepts cannot locate "the Destination" UI from this page alone.
- **Template ID discovery is fully outsourced.** "Getting Your Template ID" gives three generic steps ("Open the Mitti web app / Navigate to your inspection template / Copy the Template ID from the URL") and then says "See [Mitti's guide on getting entity IDs](https://help.mitti.com/en-US/000076/) for detailed instructions." That's adequate for the basic case, but:
- **Question item IDs — required for the entire "Advanced: Pre-Fill" section — are never explained on this page at all.** The page states twice that you need "the specific **question item ID** from your Mitti template (not arbitrary parameter names)" and twice more links out to the same external Mitti guide ("Pre-Fill Multiple Questions" note; "Data not pre-filling" troubleshooting bullet). A reader who cannot follow links has no way to complete the single task this section promises — pre-filling inspection answers — because the one piece of information the whole section depends on (where a question item ID comes from) is never shown, only pointed at.
- **No definition of "URL Template" as a QRtub feature.** "Using QRtub URL Templates" and "Using Bindings in Fallback URLs"-style content assume familiarity with `{{item.field}}` binding syntax. The mechanics (double braces, exact-as-stored insertion, no URL-encoding) are stated correctly and completely *for this specific case* — that part is genuinely self-contained — but the general feature is defined only in `help/key-concepts.mdx § URL Templates`, unlinked from anywhere except the bottom Resources list.
- **No path back to "how do I even add a Destination."** The page assumes the reader already has a Tub with Items and knows how to open an Item's Page and add a Destination rule. `help/pages-overview.mdx` covers this, but again only surfaces in the final Resources list, not inline where the instruction first appears ("In your Item's Page, add a new Destination:").
- **A Mitti-side dependency is left completely unstated.** The page never says whether Template pre-fill, question-item IDs, or Asset Profile links require a specific Mitti/SafetyCulture *subscription tier* on the reader's own Mitti account (SafetyCulture has historically gated its Assets module and custom question configuration behind paid plans). This isn't verifiable from the QRtub source since it concerns a third party's plans, but the page's silence here is exactly the kind of gap that produces a wrong AI-agent answer if a reader asks "why can't I see my asset ID in Mitti to set up the destination" — the correct diagnosis might be "your Mitti plan" and the page gives no hint to check that.
- **No QRtub plan/tier statement.** Checked `qrtub/src/lib/stripe-plans.ts` — no gating is tied to Destinations, deep links, or URL Templates. This looks like a base capability available on every QRtub plan, but the page never says so; a reader (or an AI support agent) has no way to confirm this without independently checking, and could easily invent a wrong "this needs the Professional plan" answer.

**Concrete missing pieces:** inline links (not just bottom-of-page) to `/help/key-concepts` and `/help/pages-overview` at first use of Item/Page/Destination/Tub/URL Template; an explicit statement that question item IDs cannot be found within QRtub and must come from Mitti's own guide; an explicit "this works on every QRtub plan" statement or equivalent.

---

### 2. ANSWER-FIRST

Every H2 (and H3s that carry the section's only content), opening sentence(s) quoted verbatim, word count included where relevant to judge whether a first-sentence-only retrieval would be useful:

| Heading | Opening as written | Words | Verdict |
|---|---|---|---|
| ## If you know it as SafetyCulture or iAuditor | "Same product, three names. It launched as **iAuditor**, was renamed **SafetyCulture**, and became **Mitti** in August 2026. SafetyCulture remains the company; Mitti is the platform." | 25 | **Answer-first.** Direct, complete thought, no throat-clearing. Short of the 40-60 target but nothing is lost if truncated here. |
| ## Overview | "Mitti (formerly SafetyCulture) is a mobile inspection platform. Used with QRtub, you can:" *(then a 4-item bullet list)* | 14 (before the colon) | **Partial.** The definition sentence is answer-first (8 words) but the section's real payload — what the integration lets you do — sits entirely in the bulleted list after a colon fragment. A retrieval system stopping at sentence one learns only "Mitti is a mobile inspection platform," not what the integration does. |
| ## Integration Method | "QRtub connects to Mitti using **deep links** — URLs that open Mitti directly at a specific inspection, template or report. No data is exchanged between the two systems; QRtub builds the URL and Mitti handles the rest." | 36 | **Answer-first.** Complete, direct, close to the target length, correctly states the no-data-exchange fact up front. Best-opening H2 on the page. |
| ## Getting Your Template ID | "Before setting up deep links, you'll need your Mitti Template ID:" *(then a 4-step numbered list)* | 12 | **Preamble.** States a prerequisite, not an answer — the heading implies "how do I get it," and the opener just announces that you need it, then defers to a list. |
| ## Basic Setup: Start Inspection | *(no prose — drops straight into `### Mobile App Deep Link`)* | 0 | **N/A / preamble by omission.** The H2 itself answers nothing if retrieved alone. |
| &nbsp;&nbsp;### Mobile App Deep Link | "In your Item's Page, add a new Destination:" *(then a Name/URL field pair and a code example)* | 8 | **Preamble/label**, not a sentence-level answer. |
| &nbsp;&nbsp;### Web App Deep Link | "Alternatively, use a web app deep link for users who prefer desktop/browser access:" | 12 | **Answer-first** for its short scope — states the alternative and who it's for. |
| &nbsp;&nbsp;### Using QRtub URL Templates | "Use QRtub's URL Template feature to automatically insert template IDs from your Item data:" | 14 | **Answer-first**, direct, though again colon-terminated into an example. |
| ## Advanced: Pre-Fill Inspection Questions | "Mitti allows you to pre-fill specific inspection questions using question item IDs." | 12 | **Answer-first**, direct statement of the capability. |
| &nbsp;&nbsp;### Mobile App Pre-Fill Format | *(no prose — opens directly with a fenced code block)* | 0 | **N/A / preamble by omission.** |
| &nbsp;&nbsp;### Pre-Fill Multiple Questions | "Use `&` to pre-fill multiple questions:" | 6 | **Answer-first** but minimal — acceptable given the section is inherently a syntax note. |
| ## App Not Installed? Set a Fallback URL | "\`iauditor://\` deep links only work if Mitti is installed on the device. If someone scans and doesn't have the app, the link does nothing." | 24 | **Answer-first.** Direct statement of the failure condition — also the one H2 on the page already phrased as a question, and it delivers on that phrasing. |
| ## Additional Deep Link Options | "Beyond starting new inspections, you can create Destinations for other Mitti actions:" | 12 | **Borderline/label.** States the topic but the real content (which actions, which URLs) is entirely in the H3s below; colon fragment into a list. |
| &nbsp;&nbsp;### View Inspection Report / Edit Existing Inspection / Open Asset Profile / View Document/File | Each opens directly with **Mobile:**/**Web:** code lines — no lead sentence at all | 0 | **N/A / preamble by omission**, four times over. Explanatory prose (where present, e.g. "Store the latest inspection ID in each Item's `inspectionID` field...") comes *after* the code, not before it. |
| ## Use Cases | "**Equipment Inspections**" *(bold sub-label, then a bullet list of examples — no sentence at all)* | 0 | **N/A.** Pure catalog section; there's no implied question to answer, so this is a structural observation rather than a defect. |
| ## Troubleshooting | "**Mobile app doesn't open:**" *(bold sub-label, then a bullet list — no lead sentence)* | 0 | **Acceptable pattern despite 0-word opener.** Each bold sub-label is itself a mini-question ("Mobile app doesn't open," "Data not pre-filling," "Users need access") immediately followed by direct bullets — this is a reasonable FAQ-style structure even without a framing sentence for the H2 as a whole. |
| ## Resources | *(pure link list, no prose)* | 0 | **N/A.** Link index; not expected to answer anything. |

**Summary:** 6 of 17 heading-levels evaluated open with a genuine direct-answer sentence (If you know it as..., Integration Method, Web App Deep Link, Using QRtub URL Templates, Advanced: Pre-Fill Inspection Questions, App Not Installed?...). None hit the 40-60 word target — most are considerably shorter, which is fine when the sentence is still complete, but seven sections (Basic Setup: Start Inspection, Mobile App Deep Link, Mobile App Pre-Fill Format, and all four "Additional Deep Link Options" H3s) have **zero lead sentence** and open directly on a label or code block, meaning a chunk-retrieval system grabbing "the section" gets a fragment with no framing prose at all.

---

### 3. ONE QUESTION PER PAGE

The page's core scope — "how do I wire a QRtub Destination to open Mitti at the right screen with the right data" — is a single, appropriately-scoped task, and it's covered end to end in one place the same way the sibling `integrations/cmms-systems.mdx` covers its own single scope. **No structural split of the core content is recommended.**

Two things are still worth naming:

- **The rename explainer ("If you know it as SafetyCulture or iAuditor") is a genuinely separate question** — "is this the same product as SafetyCulture/iAuditor, and do my existing setups still work?" — bundled into what is otherwise a setup guide. It is not duplicated anywhere else in the docs (checked: `grep` across every `.mdx` file for the rename language returns only this page), so there's no cross-page redundancy to fix, but if rename confusion turns out to be a common support question independent of Mitti setup, it's a natural candidate to promote to its own short FAQ entry (or a note in `help/key-concepts.mdx`) that this page links to, rather than being the sole place the explanation lives.
- **The page is not too thin to stand alone** — at ~200 lines with multiple worked examples (start inspection, pre-fill, fallback, four "additional" deep link types, use cases, troubleshooting) it's a substantial, legitimate single chunk. It should not be merged into `cmms-systems.mdx` or any other page.

---

### 4. HEADINGS AS QUESTIONS

| Current heading | Proposed question form | Why (or why not) |
|---|---|---|
| If you know it as SafetyCulture or iAuditor | **Is Mitti the same as SafetyCulture or iAuditor?** | The current heading is a conditional clause; the question form matches how a confused reader (or a search query) would actually phrase it. |
| Overview | **What can I do by connecting QRtub to Mitti?** | "Overview" is a content-free label; the question form tells a retrieval system and a scanning reader exactly what's inside. |
| Integration Method | **How does QRtub connect to Mitti?** | Minor clarity gain — "Method" is already fairly precise, but the question form matches "how does this actually work" search intent, and reinforces the important no-data-exchange fact that follows. |
| Getting Your Template ID | **How do I find my Mitti Template ID?** | Direct match to the implied task; "Getting" reads slightly like a gerund label. |
| Basic Setup: Start Inspection | **How do I set up a Destination that starts a new Mitti inspection?** | Converts a colon-separated label into the actual question the section answers. |
| Advanced: Pre-Fill Inspection Questions | **How do I pre-fill Mitti inspection answers with Item data?** | Same reasoning — "Advanced:" is a difficulty label, not a question; readers search by task, not by difficulty tier. |
| App Not Installed? Set a Fallback URL | *(keep as-is)* | Already a question and already answers it directly — the strongest heading on the page. |
| Additional Deep Link Options | **What other Mitti destinations can a QR code link to?** | "Additional...Options" is a catalog label; the question form matches how someone would search ("can I link to an asset profile instead of an inspection"). |
| Use Cases | *(leave as noun phrase)* | This is a scenario catalog, not an implicit question — forcing a question form here would be artificial, matching the sibling audit's treatment of the same pattern. |
| Troubleshooting | *(leave as noun phrase)* | Conventional, well-understood heading; the bold sub-labels underneath already function as the questions. |
| Resources | *(leave as noun phrase)* | Pure link list — no question to convert. |

---

### 5. EDGE CASES / LIMITS / FAILURE MODES

Concrete, source-verified gaps — each is a place an AI support agent would have to guess:

1. **Binding-failure behavior is completely unstated, and it's the single biggest gap on the page.** The page's central pitch, repeated three times almost verbatim — "Configure the Destination once with a template placeholder... One Destination configuration serves all equipment types" (Using QRtub URL Templates); "Configure this URL once. Deploy to 500 pieces of equipment. Each scan substitutes that Item's own asset ID" (Pre-Fill Multiple Questions); "Configure both once. Deploy to 500 items" (implied via the fallback section) — never states what happens for the one Item in that batch of 500 whose `templateID` or `assetID` field is empty. Verified directly in `qrtub/src/lib/page/bindings.ts` (`resolveBindingsForUrl`, lines 404-430) and `qrtub/src/lib/page/destination-resolver.ts` (lines 55-61, 107-110): if a bound field evaluates to `undefined`, `null`, or `""`, the **entire URL** is marked unresolved, the whole rule is logged as `Skipped - unresolved`, and destination-resolver falls through to the next rule (or shows nothing / the default "App not available" state) rather than producing a URL with a blank segment. The Troubleshooting section's "Data not pre-filling → Ensure Item fields in QRtub contain data" bullet hints at the *symptom* but never explains the *mechanism* — that a missing field doesn't degrade gracefully to a partial link, it silently drops the whole Destination.
2. **No plan/tier statement for QRtub itself.** Checked `qrtub/src/lib/stripe-plans.ts` — no feature gating exists for Destinations, deep links, or URL Templates. This is very likely a base capability on every plan, but the page never says so explicitly, leaving the door open for an invented "this is a premium feature" answer.
3. **No caveat about Mitti/SafetyCulture-side plan requirements.** Not verifiable from the QRtub source (third-party product), but the page's silence here is a defect in the same spirit as #2 — a reader whose Template ID or question item ID lookup fails inside Mitti has no signal from this page that the cause might be their Mitti subscription tier rather than anything QRtub-side.
4. **The iOS-Chrome deep-link failure mode is documented on a *different* page but omitted here, on the actual Mitti page.** `help/app-links.mdx` states explicitly: "For Mitti specifically: iOS Safari works with `iauditor://` deep links, but Chrome on iOS blocks them." That sentence names Mitti by name and is exactly the kind of platform-specific gotcha a reader configuring *this* integration needs — but it does not appear anywhere on `integrations/safetyculture.mdx`, and the only link from this page to `app-links.mdx` is a generic "See App Links & Fallback URLs for full details on how fallbacks work," giving no hint that Mitti-specific browser behavior is the reason to click through.
5. **The "keep it under ~2000 characters" limit's origin is unstated.** Troubleshooting says "Verify deep link isn't too long (keep it under ~2000 characters)" and the Pre-Fill section separately warns "Very long deep links may not work consistently." No constant matching this figure was found in `qrtub/src/` (only an unrelated 500-character CEL *expression* length cap in `bindings.ts`, which governs conditional-visibility rules, not destination URLs). As written, a reader can't tell whether ~2000 characters is a QRtub-enforced ceiling (it isn't, as far as the source shows) or a mobile OS/browser URL-length convention — the vagueness itself is the defect, since "may not work consistently" gives no actionable threshold or explanation of whose limit it is.
6. **The Fallback UX difference (styled panel vs. native browser `alert()`) is inherited from the duplicated fallback section but not disclosed here either.** This page's own "What happens at scan time" bullets ("Mitti installed → app opens directly..." / "Mitti not installed → after 2.5 seconds, QRtub redirects to the web version") describe only one of two real code paths. Confirmed via `qrtub/src/components/blocks/AppLinkOpener/AppLinkOpener.tsx`: a single-Destination Item redirect shows a full styled page with "Try Again"/"Go Back" buttons, but a Destination used as a button on a multi-Destination Page follows a different component path that (per the already-verified `help/app-links.mdx` audit) surfaces the Fallback Message via a plain `alert()` instead. Since this page's own "Use Cases → Multi-Audience Routing" section explicitly recommends multi-Destination Pages, this gap directly affects the scenario the page itself promotes.
7. **A possible content inconsistency worth flagging for verification against Mitti's own docs:** "View Inspection Report" lists **Mobile:** `iauditor://audit/<inspection_id>` and **Web:** `https://app.mitti.com/report/audit/<inspection_id>`. "Edit Existing Inspection," directly below it, lists the *identical* **Mobile:** `iauditor://audit/<inspection_id>` but a *different* **Web:** `https://app.mitti.com/inspection/<inspection_id>`. If the mobile deep link genuinely opens the same editable view for both "viewing" and "editing" an inspection, the page should say so (view/edit are the same action on mobile); if it's a copy-paste artifact, the mobile URL for one of the two is wrong. Either way, a reader retrieving "Edit Existing Inspection" in isolation has no way to know why its mobile link matches "View Inspection Report" and its web link doesn't.
8. **No word/character ceiling stated for Item field values used in bindings** (e.g., how long can `templateID` or `assetID` be before it contributes to the "~2000 character" deep link problem above) — related to #5, the page has no guidance on keeping field values short, only a vague warning about the resulting URL.

---

### 6. CHUNK INTEGRITY

Each H2 (and any H3 carrying unique content) evaluated as if it were the only thing retrieved:

- **If you know it as SafetyCulture or iAuditor** — Self-contained. No dependency on surrounding text.
- **Overview** — Self-contained.
- **Integration Method** — Self-contained.
- **Getting Your Template ID** — Self-contained; the example Template ID is clearly marked "Example:" so it reads correctly without the rest of the page.
- **Basic Setup: Start Inspection** (and its H3s) — **Depends on prior content.** The H3s "Mobile App Deep Link" and "Web App Deep Link" use `<template_id>` as a bare placeholder with no inline explanation — a reader who retrieves only this H2 (without "Getting Your Template ID" just above it) has no way to know what `<template_id>` refers to or where to get one. This is the page's clearest chunk-integrity failure: the section is not self-sufficient without its predecessor.
  - "Using QRtub URL Templates" H3 is somewhat better off — it explains its own placeholder inline ("{{item.templateID}}... Each Item in your Tub can have a different `templateID` field value") — but still assumes "URL Template," "Destination," "Item," and "Tub" are already known terms (see §1).
- **Advanced: Pre-Fill Inspection Questions** — Better than the section above: its own example includes an inline "Where:" glossary ("`template_fcbc86fd41a74180921347e4be53bdf2` is your Mitti template ID / `8f2f287e-be6e-470c-a2e2-a0fd8ab966ae` is the question item ID in your template / `{{item.assetID}}` is the QRtub field binding") that re-establishes context locally. This is a good pattern and makes the section largely self-sufficient even in isolation — it doesn't lean on "the above example" the way "Basic Setup" implicitly does.
  - "Pre-Fill Multiple Questions" H3 reuses the same good "Where:"-style habit is absent here, but the example is short enough (two bindings, both already-familiar `assetID`/`location` pattern) that it reads fine alone.
- **App Not Installed? Set a Fallback URL** — Self-contained; restates the `iauditor://` example inline rather than pointing back to "Basic Setup."
- **Additional Deep Link Options** — Each H3 (View Inspection Report, Edit Existing Inspection, Open Asset Profile, View Document/File) pairs its own Mobile/Web URLs, so none of them require the reader to have read "Basic Setup" first — good isolation, aside from the View/Edit inconsistency noted in §5.7.
- **Use Cases** — Self-contained; a generic scenario list that doesn't reference any other section.
- **Troubleshooting** — Mostly self-contained. One soft dependency: "the entity ID is correct" (under "Web link doesn't work") uses "entity ID" as an umbrella term for template/inspection/asset IDs that is only ever named as a phrase in the linked-out Mitti guide title ("Get Mitti entity IDs") — the term itself is never defined anywhere in this page's own prose.
- **Resources** — Self-contained by nature (a link list needs no surrounding context).


**Proposed rewrite:** `audit/proposed/integrations__safetyculture.md`

---

## CMMS Systems Integration

**Source page:** `integrations/cmms-systems` &nbsp;|&nbsp; **Needs rewrite:** Yes

- File: `/workspace/mintlify-docs/integrations/cmms-systems.mdx`
- Live: https://help.qrtub.com/integrations/cmms-systems
- Nav group: Integrations → Operations (sibling: `integrations/safetyculture.mdx`)
- Verified against: `/workspace/qrtub/src/lib/page/bindings.ts`, `/workspace/qrtub/src/lib/page/destination-resolver.ts`, `/workspace/qrtub/src/lib/types/destination-config.ts`, `/workspace/qrtub/src/components/blocks/DestinationPreview/DestinationPreview.tsx`, and sibling docs `help/app-links.mdx`, `help/pages-overview.mdx`, `help/key-concepts.mdx`, `integrations/safetyculture.mdx`

---

### 1. Self-containment

A cold reader cannot complete this page's implied task ("connect QRtub to my CMMS") using only this page. Specific missing pieces:

- **No UI procedure at all.** The page never says *where* in the QRtub app to enter these URLs. Compare the sibling page, which is explicit: "In your Item's Page, add a new Destination: **Destination Name:** Start Inspection, **Destination URL:** `iauditor://...`" (`integrations/safetyculture.mdx`, "Basic Setup: Start Inspection"). This page's "Example Setup: UpKeep" section jumps straight to three bare URL blocks under a "Create Page Destinations" H3 with zero instruction on how to create one.
- **Page Mode is never mentioned as a prerequisite.** The "Multi-System Page" section and the whole premise of multiple Destinations depend on the Tub having its page option turned on (`help/pages-overview.mdx`: "Pages are switched on per Tub, not per Link... In the Tub's settings, turn on the page option"). A reader landing cold on this CMMS page, follows the "Multi-System Page" example, and gets nothing, because their Tub is still in Direct Mode — the page gives no indication this setting exists.
- **No mention of the app-link fallback mechanism.** "Option 1: Direct Deep Links" presents `yourCMMS://asset/[ASSET_ID]` with no warning about what happens if the app isn't installed. `help/app-links.mdx` documents QRtub's automatic 2.5-second fallback timer and the Fallback URL/Fallback Message configuration — none of that is referenced here, even though Option 1 is exactly the deep-link scenario that mechanism exists for. A reader who wires up a CMMS deep link per this page, without reading `app-links.mdx` (not linked from this page at all), will ship links that show a blank screen for anyone without the CMMS app installed.
- **No URL-encoding caveat**, despite the whole page being built around inserting Item field values into URLs. `help/key-concepts.mdx` and `integrations/safetyculture.mdx` both explicitly warn: "QRtub inserts field values exactly as stored and never URL-encodes them. A value containing a space, `&` or `#` will break the deep link." The CMMS page's own examples (`PUMP-042`, `{{item.assetID}}`) invite exactly this failure mode with zero warning.
- **Inconsistent field name between sections** — see §6 below. A reader can't tell whether the field should be called `cmmsAssetID` or `assetID`.
- **Resources section doesn't link `help/app-links`** even though deep links are Option 1 of this very page — a genuine gap versus the equivalent Mitti page, which links it prominently.

### 2. Answer-first

Quoting the actual opening text of every H2 (and the H3s where the H2 has no lead-in of its own):

- **## Overview** — opens: *"CMMS platforms track maintenance schedules, work orders, and asset history. Integrating with QRtub enables:"* then a bullet list. This is background-first, not answer-first — it defines what a CMMS is generically before saying what *this integration* does, and never states in one place what QRtub's role is (build a URL; no data exchange). Partial credit: the bullets are concrete, but there's no single 40–60 word direct answer.
- **## Integration Approaches** — has **no text at all** before its first H3. The H2 chunk, if retrieved alone, starts with `### Option 1: Direct Deep Links (If Supported)` and its lead sentence *"Some CMMS platforms provide deep link URLs that open the mobile app:"* — reasonable as an H3 opener, but the H2 itself never states the actual answer ("there are three ways to connect: deep links, web URLs, or URL Templates").
- **## Example Setup: UpKeep** — no H2-level lead sentence; falls straight into `### Create Page Destinations` and three bare code blocks with bold labels. No sentence tells the reader what field UpKeep expects, what `assetID` must contain, or that this is illustrative rather than a verified UpKeep API contract.
- **## Multi-System Page** — opens: *"Combine CMMS with other systems:"* — a four-word fragment, not a sentence, let alone a 40–60 word answer. It gives no mechanism (that this needs Page Mode, that each numbered item is a separate Destination).
- **## Common CMMS Platforms** — opens directly with the table, no lead sentence. The one piece of framing text — *"**Note:** Deep link availability changes. Check your CMMS documentation or contact support."* — appears **after** the table, so a reader (or an AI agent) skimming just the table sees unqualified ✅/❌ claims before the caveat that they may be wrong.
- **## Best Practices** — opens straight into `**Field Mapping**` bold-lead-in, no H2 sentence framing what "best practices" means here or why these three matter.
- **## Troubleshooting** — opens directly with `**"Page not found" errors:**` and a bullet list — no framing sentence describing the two failure modes it covers before listing them.
- **## Resources** — a bare link list, fine as-is; this heading doesn't imply a question that needs a direct-answer opener.

Verdict: **zero of the seven content H2s** open with a direct 40–60-word answer to their heading's implied question. Every one leads with either a generic-definition sentence, a sentence fragment, or no lead sentence before subheadings/lists/tables.

### 3. One question per page

This page is answering at least four distinct questions, bundled:

1. **"How do I connect QRtub to a generic/unknown CMMS?"** — Integration Approaches (deep link / web URL / URL Template), the real, vendor-agnostic core of the page.
2. **"How do I set this up for UpKeep specifically?"** — Example Setup: UpKeep. This is a single-vendor deep-dive sitting inside a page whose title and framing are vendor-agnostic ("CMMS Systems Integration," "Common CMMS Platforms" table lists five vendors). Nothing explains why UpKeep gets a worked example and Fiix/eMaint/Hippo/Maintenance Connection don't.
3. **"How do I combine CMMS with other systems on one code?"** — Multi-System Page. This duplicates content that already lives on `help/key-concepts.mdx` (UC-001, the "Excavator" example with "Start Inspection → Mitti / Log Maintenance → CMMS") almost verbatim, and on `integrations/safetyculture.mdx` ("Multi-Audience Routing" under Use Cases). It adds no CMMS-specific information.
4. **"Which CMMS platforms are known to work, and how do I test/troubleshoot a rollout?"** — Common CMMS Platforms table, Best Practices, Troubleshooting.

**Proposed split:**

- **Keep on this page** (rename conceptually to "Connecting QRtub to a CMMS"): the three integration approaches, the field-mapping/testing best practices, and troubleshooting — these are all one coherent task ("wire up my CMMS, whichever one it is, and get it working reliably").
- **Cut "Multi-System Page" entirely** and replace with a one-line pointer: "To show a CMMS Destination alongside other systems (inspections, manuals) on one code, see [Pages Overview](/help/pages-overview) and [Key Concepts](/help/key-concepts)." This section is pure duplication today and adds retrieval noise — an AI agent asked "how do I combine CMMS with SafetyCulture" would get a thinner, staler answer from this page than from key-concepts.mdx.
- **Cut "Example Setup: UpKeep"** as a dedicated H2, or fold its three URL patterns into the Option 3 explanation as one labeled example ("e.g., for UpKeep: ..."), clearly hedged as illustrative and not vendor-verified. A single-vendor deep dive without an equivalent for the other four vendors in the platform table reads as incomplete rather than as a deliberate choice.
- If UpKeep genuinely deserves its own worked example (parity with the Mitti/SafetyCulture page), it should be a **separate page** (`integrations/cmms-upkeep.mdx` or similar), following the same shape as `integrations/safetyculture.mdx` — named product, verified deep link scheme, its own troubleshooting. As written, it's too thin (3 URLs, no confirmation these are real UpKeep endpoints) to justify a standalone page today, and too vendor-specific to sit inside a vendor-agnostic guide.

This page is not too thin to stand alone once the above is trimmed — the three-approaches + best-practices + troubleshooting core is a legitimate, complete single-question chunk.

### 4. Headings as questions

| Current heading | Keep or rewrite? | Proposed question form |
|---|---|---|
| Overview | Keep | — (not a question-shaped section; fine as a noun heading) |
| Integration Approaches | Rewrite | **"How do I connect QRtub to my CMMS?"** — the section answers exactly this, and the noun phrase gives no hint that three concrete options follow |
| Option 1: Direct Deep Links (If Supported) | Keep | Already effectively a labeled option, not a question |
| Option 2: Web-Based Access | Keep | Same |
| Option 3: URL Templates | Keep | Same |
| Example Setup: UpKeep | Rewrite (if kept) | **"What does a UpKeep setup look like?"** — minor clarity gain, and signals "example," not "instructions" |
| Multi-System Page | Rewrite/cut | If kept as a pointer only: **"Can I show CMMS alongside other systems on one code?"** |
| Common CMMS Platforms | Rewrite | **"Which CMMS platforms support deep links?"** — the table is answering a support question ("does my CMMS work with this?"), and the current noun phrase reads as a marketing list rather than a direct capability answer |
| Best Practices | Rewrite | **"How do I test a CMMS rollout before deploying it at scale?"** — this is what the section actually contains (field mapping + a pre-deployment test checklist), and "Best Practices" is generic enough to be meaningless to a retrieval system matching a specific question |
| Troubleshooting | Keep | Already implicitly a question-answering heading; "Troubleshooting" is a well-understood convention, converting it to "What do I do if the CMMS link doesn't work?" would be a lateral move, not a clarity gain |
| Resources | Keep | Not a question-shaped section |

### 5. Edge cases / limits / failure modes

Treating every gap below as a defect per the audit brief — these are exactly the questions an AI support agent will be asked and, absent a stated answer, will guess at:

- **What happens if the mapped field is empty on some Items?** Not stated anywhere on this page. Verified in source (`/workspace/qrtub/src/lib/page/bindings.ts` `resolveBindingsForUrl`, and `/workspace/qrtub/src/lib/page/destination-resolver.ts` `resolveDestination`): a binding that evaluates to `undefined`, `null`, or `''` is recorded as **unresolved**, and the destination-resolution layer **discards that Destination/rule entirely** rather than inserting an empty string into the URL — for a plain (non-conditional) Destination, an empty `defaultLink` binding means `resolveDestination` returns no destination at all, which (`/workspace/qrtub/src/lib/page/public-link-resolution.tsx`) surfaces to the visitor as a **"link not ready" page**, not a broken CMMS URL. This is the single most support-relevant fact missing from the page: if `cmmsAssetID` is blank for some Items, those QR codes will show "link not ready," not open a malformed UpKeep URL. Worth noting the QRtub editor also surfaces this before deployment — `DestinationPreview.tsx` renders `bindingErrors` in the Destination preview panel — so the page could point readers to check the preview before printing.
- **No URL-encoding statement** (see §1) — a field value containing a space, `&`, or `#` breaks the generated CMMS URL silently. Sibling pages state this explicitly; this page doesn't, despite depending entirely on inserting field values into URLs.
- **No app-link fallback behavior stated** for Option 1 deep links — no mention of the 2.5-second timer, Fallback URL, or Fallback Message documented in `help/app-links.mdx`. A reader following only this page will ship a deep-link Destination with no fallback and no idea that's even configurable.
- **No plan-tier gating mentioned** — checked `/workspace/qrtub/BRAND.md`'s feature-status table for anything gating Destinations, URL Templates, or Page Mode by plan; found none. This appears to be a non-issue for this page (no tier gap exists to document), but if plan tiers do gate Page Mode or the number of Destinations, that isn't verifiable from the repo made available and should be confirmed with product before publishing, since its absence here currently reads as "available on every plan."
- **No character/length limit stated** for generated URLs. The Mitti/SafetyCulture page states "Very long deep links may not work consistently" and "keep it under ~2000 characters" in its troubleshooting section — this page's Troubleshooting section has no equivalent line, despite CMMS work-order URLs plausibly carrying several concatenated parameters (see the "Create Work Order" example, which already chains a query string).
- **Vendor deep-link support claims are asserted without a source or date.** The "Common CMMS Platforms" table states specific ✅/❌ facts about UpKeep, Fiix, Maintenance Connection, Hippo CMMS, and eMaint's deep-link support. These are claims about third-party products QRtub doesn't control and this repo can't verify either way; the only hedge is the single trailing note *after* the table ("Deep link availability changes. Check your CMMS documentation or contact support"). Given the docs style guide's own instruction to "avoid dated claims that quietly expire," a table of unlinked, unsourced vendor capability claims is a durability risk independent of QRtub's own product — worth downgrading the checkmarks to "check vendor docs" framing throughout, not just in a footnote.
- **No statement of what "Test URLs before mass deployment" (Best Practices) actually means procedurally** — no mention of the Destination preview panel (which does exist and does show binding errors, per `DestinationPreview.tsx`) as the concrete tool for this "best practice." The advice is stated with no way to act on it from the page.

### 6. Chunk integrity

Evaluating each H2 in isolation, as if only that section were retrieved:

- **Overview** — Self-contained. Fine alone.
- **Integration Approaches** — Self-contained (contains all three options with their own examples). No dangling references.
- **Example Setup: UpKeep** — **Breaks in isolation.** Uses `{{item.assetID}}` in all three code blocks, but the earlier "Option 3: URL Templates" section (a different H2 subsection) established the field name as `{{item.cmmsAssetID}}` ("Item field `cmmsAssetID`: 'PUMP-042'"). A reader (or AI agent) retrieving only "Example Setup: UpKeep" has no idea `assetID` is a fictional/example field name unrelated to the `cmmsAssetID` field named two sections earlier — and if both chunks are retrieved together, they contradict each other on what the field should be called. This is a genuine, fixable inconsistency, not just a completeness gap.
- **Multi-System Page** — Self-contained in the sense that it doesn't grammatically depend on prior text, but its content ("Combine CMMS with other systems:" + a 4-item list + "One QR code, all systems accessible") is too terse to be useful in isolation — it doesn't explain that this requires Page Mode (a Tub-level setting explained only in `help/pages-overview.mdx`, not linked from this section) or that each numbered line is a separate Destination the reader must configure individually.
- **Common CMMS Platforms** — Self-contained as a table, but isolated retrieval loses the caveat note if a chunker splits at the row boundary vs. keeping the trailing "**Note:**" line attached — a real risk since the note is a separate paragraph after the table rather than a table footnote or inline caveat.
- **Best Practices** — Self-contained but assumes the reader already knows what "Field Mapping," "your QRtub Item fields," and "your CMMS asset identifiers" mean in QRtub's data model (Item, Tub, custom fields) — reasonable for a reader of the whole site, but this section doesn't reintroduce "Item field" and would confuse someone who lands directly on this chunk via search without ever having seen `help/key-concepts.mdx`.
- **Troubleshooting** — Self-contained; the two failure categories and their bullets read fine with no external dependency. This is the best-isolated section on the page.
- **Resources** — Self-contained (a link list), though "Contact your CMMS vendor for deep link documentation" only makes sense with the rest of the page's context that deep-link support is CMMS-vendor-dependent — negligible risk since Resources sections aren't usually retrieved as an answer chunk on their own.

---

### Summary

The page's core content (three integration approaches, platform table, best practices, troubleshooting) is sound and largely non-redundant with its sibling. But it fails self-containment (no in-app procedure, no Page Mode prerequisite, no fallback mechanism), fails answer-first structure on every H2, bundles at least one clearly separable, duplicative section (Multi-System Page) and one underdeveloped vendor-specific detour (Example Setup: UpKeep), and is silent on the single most consequential edge case for this exact feature — what a scan shows when the mapped field is empty (a "link not ready" page, not a broken URL, per verified source behavior). A substantive rewrite is warranted; see `/workspace/mintlify-docs/audit/proposed/integrations__cmms-systems.md`.


**Proposed rewrite:** `audit/proposed/integrations__cmms-systems.md`

---
