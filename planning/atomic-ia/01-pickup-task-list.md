# Picking the atomic IA back up — task list

**Set 25 August 2026.** State measured, not remembered. The plan is
`00-atomic-ia-plan.md` (127 pages, 3 tabs, 19 groups); this file records what actually shipped,
what did not, and the order to do the rest in.

---

## Where the plan actually got to

**The Help tab is done.** 17 groups, and the atomic rewrite landed: median page 540 words, and
every group that was split sits in the target band — Items 428, Team 435, Print Batches 470,
Collections 480, Fields 517, Billing 537. Nav is healthy: 116 nav entries, **zero pointing at a
missing file**.

**Two tabs were never rewritten.** They are still the original long pages at the original paths:

| Tab | Shipped | Planned | Average page |
|---|---|---|---|
| Help | ~112 pages | 114 | 540 words |
| Use Cases | 5 pages | 6 | **1,211 words** |
| Integrations | 2 pages | 7 | **1,088 words** |

That is the whole 119-vs-127 gap, and it is why the docs feel finished in some places and not
others. The five `industries/*` pages and the two `integrations/*` pages are the only ones still
at legacy length.

`destinations/` is the one *split* group running long at 762 average — `device-detection` 1067,
`conditional-destinations` 906, `field-bindings` 853, `what-is-a-destination` 832. Worth a second
pass, but it is not in the same category as the two tabs above.

---

## Phase 1 — Nav accordion

**The constraint that decides the shape.** Mintlify's `expanded` property **only works on nested
groups**; its own docs say *"Top-level groups always expand."* So 17 top-level groups will always
render fully expanded, which is exactly the current problem. An accordion requires introducing a
parent layer.

- [ ] **1.1 Restructure the Help tab into ~6 top-level groups containing the existing 17 as
      nested, collapsed groups.** Proposed parents: *Getting Started* (kept flat, it is the front
      door) · *Collections & Items* (Collections, Fields, Items, Import & Export) · *Links* (Links,
      Bulk Link Operations) · *Pages & Destinations* (Pages, Destinations) · *Producing Tags*
      (Media, Print Batches, Print-First Workflow, Working with a Print Shop) · *Account & Billing*
      (Account, Team, Billing, Workspace). Sidebar goes from 116 lines to roughly 23.
- [ ] **1.2 Rename two groups while the nav is open.** **"Media" → "Tags"**, per the terminology
      decision — GLOSSARY bans Media for the physical unit. And **"Working with a Print Shop"**,
      which contains `print-shop/gang-sheets` about anodising and laser engraving; a Supplier is
      not a print shop. Long-standing item, and this is the cheap moment for it.
- [ ] **1.3 Decide the three orphan files.** `index` is the home page and correctly out of nav.
      `creating-your-first-link` and `key-concepts` are both redirect targets and both sit in the
      Getting Started group — confirm they are reachable, or that being out of nav is deliberate.
- [ ] **1.4 Verify collapsed groups do not break search or `llms.txt`.** Both should be
      structure-independent, but confirm rather than assume before merging.

## Phase 2 — Finish the Help-tab rewrite

- [ ] **2.1 Diff the plan tree against shipped files** to find the ~2 missing Help pages. Do it
      mechanically, page slug by page slug, rather than by reading both documents.
- [ ] **2.2 Second pass on the four long `destinations/` pages** (above). These were split but not
      tightened; `device-detection` at 1067 is the worst.
- [ ] **2.3 Settle the `industries/` vs `use-cases/` path question.** The tab is *named* Use Cases
      but every file lives at `industries/*`, while the plan specified `use-cases/*`. Either move
      and add redirects, or record that keeping the paths is deliberate. Do not leave it
      undecided — it is the kind of mismatch that quietly gets copied into the next tab.
- [ ] **2.4 Rewrite the five `industries/` pages atomically** and add the missing
      `use-cases/overview`. Averaging 1,211 words, they are the least atomic content on the site.
- [ ] **2.5 Held, not forgotten:** the Deployed → Installed sweep, roughly 50 references across 14
      pages plus the `print-batches/deployment-status` slug. Deliberately waiting on the app, per
      `decisions/terminology-print-batches-and-installation.md` — a doc saying "mark it Installed"
      against a button reading "Deployed" is worse than a lagging doc.

## Phase 3 — Integrations, starting with Mitti

Grounded in `../../qrtub-ops/research/mitti-qr-surface-map.md`, which mapped Mitti's five
generated code types and fourteen deep-link formats.

- [ ] **3.1 Write `integrations/overview`** — every integration is a URL recipe, not an API
      connection. This is the page that stops a reader expecting a sync, and it is the tab's
      missing front door.
- [ ] **3.2 Split `safetyculture.mdx` (1,767 words) into the planned four.** `mitti/setup`,
      `mitti/entities`, `mitti/prefilling`, `mitti/worked-examples`. Redirect the old slug — a
      SafetyCulture community post links to it.
- [ ] **3.3 Decide the file path.** The page is titled Mitti and lives at
      `integrations/safetyculture`. The split is the moment to move it, with redirects.
- [ ] **3.4 `mitti/entities` carries the access matrix.** Only issue codes work without a Mitti
      account; asset profiles and files also need the mobile app. This is the single most useful
      fact for anyone specifying a tag for a mixed audience, and it belongs in a reference table.
- [ ] **3.5 Split `cmms-systems.mdx` into the planned two** — `setup` and `worked-examples`.
- [ ] **3.6 Answer open question 8 from the plan** — CMMS gets 2 pages while Mitti gets 4. Confirm
      that asymmetry is intentional rather than incomplete.

## Phase 4 — Mitti AI prompts, and a Prompt component

Martin's finding, and the novel part of this whole integration: **Mitti's AI assistant is good at
retrieving entity IDs**, which is the tedious prerequisite for building a QRtub Destination. And it
can reference QRtub from inside Mitti. So the help page can carry prompts a user copies straight
into Mitti and gets the IDs back.

This is worth doing well because nothing else on that marketplace does it, and it removes the
step that most often stalls a first setup.

- [ ] **4.1 Verify the capability before documenting it.** What the assistant is called in-product,
      which plans have it, whether it will return template / asset / document / course IDs, and
      what a good prompt looks like versus one that returns prose. Document how it actually works —
      no drafting from a remembered demo.
- [ ] **4.2 Draft the prompts.** One per entity type, matched to the deep-link formats in the
      surface map. Each should return an ID in a form that pastes straight into a QRtub
      Destination or a URL template.
- [ ] **4.3 Build a `Prompt` component** in `/snippets/` — Mintlify's confirmed location for custom
      React components, and this repo already uses that folder for `cta` and `snippet-intro`. It
      needs a copy button, and it should read as *something to paste into an AI*, not as code.
      **Check first** whether a plain fenced code block already gives a copy button; if it does,
      the component is styling and semantics only, not clipboard logic.
- [ ] **4.4 Place the prompts on `mitti/entities`**, beside the table of what each ID is for, so
      the reader meets the shortcut at the exact moment they hit the tedious part.
- [ ] **4.5 Consider the reverse direction.** Mitti's AI referencing QRtub means a Mitti user can
      be told about QRtub inside Mitti. That is a channel observation, not a docs task — belongs in
      `../../qrtub-ops/outbound/partner-channel-playbook.md` if it holds up.

---

## Order, and why

Nav first because it is one file and it changes how everything else is read. Help second because
it is nearly finished and leaving it 95% done is the worst state. Integrations third because it is
the biggest content gap and it now has research behind it. The prompt component last, because it
needs a verification pass before a line of it is written.
