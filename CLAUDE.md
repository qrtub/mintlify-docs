# Mintlify documentation for QRtub

QRtub is a product built and owned by Australian company Teralis Pty Ltd.
The only contact email is hi@qrtub.com.

---

## Source of truth

This file covers **how to write the docs**. It deliberately does not restate product
facts, terminology, or brand voice — those live in the app repo and are maintained there:

| For | Read |
|-----|------|
| Canonical terminology, what NOT to call things | `../qrtub/GLOSSARY.md` |
| Product definition, entity model, feature status, voice, audiences, positioning | `../qrtub/BRAND.md` |
| How a feature **actually** behaves | The app source in `../qrtub/src/` |

**Never** copy those facts into this file. An earlier version of this brief duplicated the
glossary, the voice rules and the feature-status table. The copies drifted, and the docs
went on to promise features that did not exist. One source, referenced — not duplicated.

These paths assume the three-repo workspace (`qrtub`, `mintlify-docs`, `qrtub-ops` as
siblings) is open. If you only have this repo, ask rather than guess.

### The rule that matters most

**Document how the code actually works, not how it should work.** Before writing or editing
any page, read the relevant code in `../qrtub/src/`. If you cannot verify a behaviour in the
source, do not describe it — ask.

Pages have previously documented a `today` value for date comparisons, automatic URL
encoding, scan history, and Media type tracking. **None of these exist.** All were written
from assumption rather than from the code.

---

## Working relationship

- You can push back on ideas — this leads to better documentation. Cite sources and explain
  your reasoning when you do
- ALWAYS ask for clarification rather than making assumptions
- NEVER lie, guess, or make up information

## Project context

- Format: MDX files with YAML frontmatter
- Config: `docs.json` for navigation, theme, settings
- Components: Avoid Mintlify components where possible — this content will likely be
  migrated to Next.js

## Content strategy

- Document just enough for user success — not too much, not too little
- Prioritise accuracy and usability
- Make content evergreen where possible. Avoid dated claims ("through Q1 2025") that quietly
  expire
- Search for existing information before adding new content. Avoid duplication unless
  strategic
- Check existing patterns for consistency
- Start by making the smallest reasonable change

## Frontmatter requirements for pages

- `title`: clear, descriptive page title
- `description`: concise summary for SEO/navigation

Both are required on every page. Note that `sidebar_position`, `category` and `slug` appear
in some older files — Mintlify drives navigation from `docs.json`, so these are inert
leftovers from another system and should not be added to new pages.

## Writing standards

- Second-person voice ("you")
- Prerequisites at the start of procedural content
- **Verify every code example against the app source before publishing**
- Match the style and formatting of existing pages
- Include both basic and advanced use cases
- Language tags on all code blocks
- Alt text on all images
- Relative paths for internal links

## Git workflow

- NEVER use `--no-verify` when committing
- Ask how to handle uncommitted changes before starting
- Create a new branch when no clear branch exists for changes
- Commit frequently
- NEVER skip or disable pre-commit hooks

## Do not

- Skip frontmatter on any MDX file
- Use absolute URLs for internal links
- Include untested or unverified code examples
- Promise features listed as Planned in `../qrtub/BRAND.md`
- Restate feature status here — link to BRAND.md instead
- Make assumptions — always ask

---

## Technical details worth getting right

These are the details docs have most often got wrong. Verify against the source each time;
the file references below are starting points, not guarantees.

### Field bindings in URL templates

The syntax is **double braces with a namespace**:

```
https://app.example.com/inspect?id={{item.assetID}}&site={{tub.name}}
```

- `{{item.fieldName}}` for Item fields, `{{tub.fieldName}}` for Collection fields
- **The Collection binding prefix is `tub.`, not `collection.`.** Collections were renamed
  from Tubs in August 2026; the binding namespace still uses the original term. Write the
  prose as "Collection" and the binding as `tub.` — and say so on any page that shows one,
  or readers will try `collection.name` and get an empty string with no error
- **Single braces do not work.** `{assetID}` is not a binding and will be sent literally
- Values are inserted **exactly as stored** — there is no automatic URL encoding
- A missing or empty field inserts an empty string

Source: `../qrtub/src/lib/page/bindings.ts`

### Conditional visibility

Conditions use CEL (Common Expression Language). What is available in an expression:

- **Item fields** — `item.name`, `item.status`, `item.tags`, custom fields
- **Collection fields** — `tub.name` (prefix is `tub.`, see above)
- **Device fields** — `device.isMobile`, `device.isIOS`, `device.browser`
- **Time fields** — `time.hour`, `time.dayOfWeek`, `time.dayOfMonth`, `time.month`,
  `time.year`, `time.isWeekend`

**There is no current-date value and no date arithmetic.** `time` exposes the parts listed
above but not a full date, so "show when the inspection is overdue" cannot be expressed
today. Use a status field the user maintains (`item.testStatus == "expired"`) instead of
comparing dates.

An undefined identifier makes the whole condition evaluate to `false` silently — which is
why an invented value like `today` produces a rule that never fires and no error message.

Source: `../qrtub/src/lib/page/bindings.ts`, `../qrtub/src/lib/page/context.ts`

### Link URL structures

| Type | Format | Example |
|------|--------|---------|
| Random | `qrtub.com/r/{5-char}` | `qrtub.com/r/x5fgd` |
| ID-based | `qrtub.com/{id}` | `qrtub.com/cra001` |
| Custom | `qrtub.com/{custom}` | `qrtub.com/mylink` |

The domain is always lowercase `qrtub.com`. The product name is `QRtub` — see
`../qrtub/GLOSSARY.md`.

### What "integration" means here

QRtub does not have API integrations with third-party products. It builds a URL that opens
them, sometimes as an app deep link. Never write "integrates with SafetyCulture" in a way
that implies data exchange, sync, or write-back. Nothing writes data back into QRtub.

Accurate: "opens the inspection in SafetyCulture, pre-filled with this item's ID".
Not accurate: "syncs with SafetyCulture" / "test results update automatically".

---

## Writing patterns

### Problem-solution (landing pages only — see the warning under Industry pages)

```
[PROBLEM — specific and recognisable]
↓
[PAIN — what happens without a solution]
↓
[SOLUTION — how QRtub addresses it]
↓
[OUTCOME — what concretely changes]
```

### Feature description

```
[WHAT IT IS — one sentence]
[WHY IT MATTERS — the problem it solves]
[HOW IT WORKS — brief mechanics, verified against code]
[EXAMPLE — one concrete scenario]
```

### Industry pages

**An industry is an audience, not a concept, so an industry page can never be atomic on its
own.** That is the trap. The previous version of this section said *"change the nouns, not the
verbs — same capabilities, different context"*, and following it produced five pages that were
one template filled in five times: "The Challenge" ×5, "Real-World Example" ×5, "Why X Choose
QRtub" ×5. 6,522 words of which maybe 2,000 was industry-specific, and a fix to one left four
stale. Do not reintroduce that instruction.

**Write the mechanism once, as a workflow page**, and let industry pages route to it. A
compliance register is the same mechanism for an electrician, a council and an arborist — one
page, three referrers.

An industry page is a short entry point, roughly 400 words:

1. **What is being tagged**, physically and specifically
2. **The Collection shape** that fits — the fields that actually matter here
3. **The one or two things genuinely different** about this industry
4. **Links out** to the workflow pages and the atomic Help pages

Then:

- **Use industry terminology** for the customer's own domain (an electrician's "test and tag",
  a council's "assets"), but QRtub's glossary terms for QRtub's own concepts
- **Reference the real tools they use** — without implying QRtub integrates with them
- Industry pages have historically been the worst offenders for overclaiming. Check every
  capability sentence against the app source before publishing.

---

## Site structure

```
help.qrtub.com
├── /                     (homepage)
├── /<page>               How-to guides for QRtub features — root-level, no /help/ prefix
│                         (the subdomain already says "help"; repeating it in every path
│                         was redundant, dropped August 2026 — old /help/* links redirect)
├── /industries/*         Vertical pages — CURRENTLY HIDDEN FROM NAV, see planning/docs-status.md
└── /integrations/*       Guides for connecting QRtub to third-party tools
```

**Progress is tracked in `../qrtub-ops/docs-planning/docs-status.md`.** Read it before starting
docs work and update it in the same commit. It carries the decisions log, what shipped, and what
is next. The atomic IA design sits beside it in `../qrtub-ops/docs-planning/atomic-ia/`.

Planning notes deliberately live in the ops repo, not here. They are full of binding examples in
double braces and comparisons like "under 150 words", which the MDX parser reads as JSX and which
broke `mintlify broken-links` on every run. A docs repo should contain only pages.

**The Help tab nav is an accordion**: 6 top-level groups holding 15 nested collapsed groups.
Mintlify's `expanded` property only works on *nested* groups — top-level groups always expand —
so a new group belongs inside a parent, not alongside them.

There is no blog. Two stale posts were removed in August 2026; do not reintroduce the
section without a decision to maintain it.

Navigation is defined in `docs.json`. A new page is not live until it is added there.

| Page type | Purpose | Tone |
|-----------|---------|------|
| Help | Enable successful usage | Technical, clear, utility |
| Industry | Speak to a specific vertical | Industry-aware, practical |
| Integration | Connect QRtub to a third-party tool | Precise, honest about mechanism |

**Page endings:** every page closes with a short `## Related` list of 2–4 sibling pages.
**There is no CTA footer** — do not add "See plans and pricing", the support address, or the
`snippets/cta.mdx` block to a help page, and do not "restore" one you find missing.

Why: the identical CTA block appeared on all 19 pre-migration pages, so it carried no
answering signal and only diluted every page's retrieval chunk. A `## Related` list of real
sibling pages earns the same footer space by actually routing the reader onward. Commercial
CTAs live in the navbar "Get Started" button, not in page bodies.

**Do not describe QRtub as being in BETA.** The word was retired across every surface in
August 2026 — docs, marketing and the founder's LinkedIn. On a reference site it reads as a
caveat about stability; on a pitch surface it invites the reader to wait. If something is
genuinely not built, say what it is rather than reaching for the word.

---

## Prompting templates

Reusable task shapes. Each assumes you have already read `../qrtub/GLOSSARY.md`,
`../qrtub/BRAND.md`, and the relevant app source.

### Help page

```
TASK: Write a help page for [FEATURE].

FIRST: read the implementing code in ../qrtub/src/ and list what the feature
actually does. Do not proceed on assumption.

STRUCTURE:
1. What it is (1–2 sentences)
2. Prerequisites
3. Step-by-step procedure
4. Verified examples
5. Limitations — what it cannot do
6. Related pages

CONSTRAINTS:
- Every example verified against the source
- Second person, no marketing language
- title + description in frontmatter
- State limitations explicitly; silence reads as capability
```

### Industry landing page

```
TASK: Write a landing page for [INDUSTRY].

INDUSTRY CONTEXT:
- Items managed: [list]
- Pain points: [list]
- Software they already use: [list]

STRUCTURE:
1. Hero: industry-specific headline + subheadline
2. Three industry pain points
3. How QRtub addresses each
4. One detailed scenario
5. CTA

CONSTRAINTS:
- Every capability claim traceable to the app source
- Name their tools, but never imply QRtub integrates with them
- No automation, reporting, or write-back claims
- Check the result against BRAND.md "Claims That Are FALSE"
```

### FAQ answer

```
TASK: Write an FAQ answer for "[QUESTION]".

CONSTRAINTS:
- First sentence answers the question directly
- 2–4 sentences total
- If it touches a Planned feature, say so explicitly
- Verify against ../qrtub/BRAND.md feature status
```

---

## Use cases library

Recurring stories the docs draw on. Status is indicative — confirm against
`../qrtub/BRAND.md` feature status before writing, and never present a Planned use case as
something a reader can do today.

**UC-001 · Multi-system integration** — *"One QR code for every system"*
Equipment needs codes for inspection, maintenance and customer info, so it ends up wearing
three stickers. One Link with a Page carrying a Destination per system replaces them.
Example: "Start Inspection" → `iauditor://template/new_audit/xyz?asset={{item.assetID}}`,
"Log Maintenance" → `cmms.example.com/asset/{{item.equipmentID}}`. Configure the template
once; every item routes to its own pre-filled URL. **Available.**

**UC-002 · Audience routing** — *"Right info for the right person"*
Staff need operational tools; customers need support info. One Page shows every Destination
and people self-select. Conditional visibility can narrow the list by item field or device.
**Available.**

**UC-003 · Vendor lock-in prevention** — *"Switch systems, keep your codes"*
QR codes normally hardcode a vendor's URL, so changing vendor means reprinting. Codes encode
a QRtub Link instead; switching means updating Destinations, not reprinting. **Available.**

**UC-004 · Print-before-link deployment** — *"Print now, connect when ready"*
Professional printing needs bulk orders and lead time, but equipment arrives over months.
Generate Links first, print, apply during installation, connect when convenient. Mistakes
are fixed by reassigning, not reprinting. **Available.**

**UC-005 · Professional item presence** — *"Turn every item into a digital hub"*
Physical items need a digital presence — menu, WiFi, feedback form, support contact.
Item data and its Page stay linked, so an update reaches every code at once. **Available.**

**UC-006 · Asset lifecycle transfer** — *"Codes that follow the asset, not the owner"*
A manufacturer prints codes in the factory; ownership transfers to the buyer at sale.
Also covers cross-rental and contractor access. Requires cross-account transfer and granular
permissions. **Planned — always state this explicitly.**

**UC-007 · Product handoff value-add** — *"Digital tracking included"*
Codes applied at manufacture initially show warranty and setup information, then transfer to
the buyer who connects them to their own systems. Requires cross-account transfer.
**Planned — always state this explicitly.**

**UC-008 · Media as infrastructure** — *"What the code is printed on is infrastructure too"*
A metal plaque costs $50 and outlasts the equipment; a billboard costs $5,000. These are
assets with their own lifecycle, distinct from the Link they encode and the Item they
represent.

Be precise about what exists, because this page has been wrong in both directions:

- **Print batches are shipped.** Exporting a print list creates a tracked batch with a name,
  notes, tags and a photo, a Draft → Printing → Printed → Deployed lifecycle, per-code
  deployment status, the stored CSV, and archiving. See `help/print-batches.mdx`.
- **Per-item Media is not.** There is no record of what an individual code is printed on —
  no material type, cost, durability or installation location, and no inventory.

So production runs are tracked; individual pieces of media are not. Do not write "QRtub does
not track Media" as a blanket statement — an earlier version of this brief said exactly that
and the docs went on to deny a shipped feature.

---

## Before publishing: checklist

**Accuracy**
- [ ] Every capability claim verified in `../qrtub/src/`
- [ ] No Planned feature presented as available
- [ ] Code examples and field bindings tested, using `{{item.field}}` syntax
- [ ] Limitations stated where a reader would otherwise assume capability

**Terminology**
- [ ] Terms match `../qrtub/GLOSSARY.md`
- [ ] "QRtub" — capital QR, lowercase tub. The domain `qrtub.com` stays lowercase
- [ ] No capitalised "Profile Page" — the canonical noun is "Page"

**Voice**
- [ ] Matches `../qrtub/BRAND.md` — practical and quietly confident, never salesy
- [ ] Problem-focused, specific, no empty superlatives

**Structure**
- [ ] `title` and `description` in frontmatter
- [ ] Internal links relative, and their targets exist
- [ ] Added to `docs.json` navigation
